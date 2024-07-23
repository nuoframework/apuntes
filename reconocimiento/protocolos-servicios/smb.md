---
description: En esta página aprenderás a enumerar y hacer fuerza bruta ante el servicio SMB
---

# SMB

{% hint style="info" %}
SMB (Server Message Block), es un protocolo de comunicación de red utilizado para compartir archivos, impresoras y otros recursos entre dispositivos de red. Este es muy utilizado en sistemas Windows. En cambio Samba es la versión libre y de código abierto del protocolo propietario de Windows utilizado en sistemas Linux, este proporciona una manera de compartir archivos y recursos entre dispositivos de red que ejecutan sistemas operativos diferentes.
{% endhint %}

## Enumeración del servicio

Tanto la versión utilizada en Windows (SMB) como la utilizada en Linux / Unix (Samba), se puede enumerar de la misma forma, estas suelen ejecutarse en el puerto 445 o 139 bajo el nombre "microsoft-ds" o "netbios-ssn".&#x20;

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

### Crackmapexec

Con crackmapexec también podemos enumerar el servicio SMB, en este caso, especificamos que queremos enumere SMB, la IP, y haciendo uso de una Null Session, con los parámetros `-u` y `-p` sin especificar credenciales y finalmente con el parámetro `--shares` listamos los recursos compartidos

```ruby
crackmapexec smb 192.168.1.10 -u '' -p '' --shares
```

Podemos usar esta herramienta para probar una serie de contraseñas en base a un usuario o viceversa sustituyendo la contraseña por la ruta de un diccionario. Por ejemplo:

```ruby
crackmapexec smb 192.168.1.10 -u usuario -p diccionario.txt
```

{% hint style="warning" %}
**ATENCIÓN!!** Puede que los resultados no sean reales y nos den falsos negativos, por lo que se recomienda revisar los resultados con smbmap (`smbmap -H 192.168.1.10 -u usuario -p contraseña1`)
{% endhint %}

### Enum4linux

En este caso, podemos hacer una enumeración muy exustiva que incluso puede reportarnos usuarios, de manera muy sencilla con el siguiente comando:

```ruby
enum4linux 192.168.1.10
```

## Conexión a los recursos compartidos

Una vez hemos hecho el previo [reconocimiento](smb.md#enumeracion-del-servicio), y si este nos a permitido ver recursos compartidos a los que podamos acceder sin proporcionar credenciales, nos podemos conectar con smbclient de esta manera:

```perl
smbclient //127.0.0.1/nombre_recurso -N
```

{% hint style="info" %}
Para descargar un recurso a nuestra maquina, podemos hacer uso del comando `get archivo.txt` o para casos de descarga masiva el comando `mget *` el problema surge cuando tenemos que confirmar muchos archivos, para ello antes del ejecutar el comando `mget`, ejecutamos el comando`promt`
{% endhint %}

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

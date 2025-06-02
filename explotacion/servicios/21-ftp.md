---
description: En esta página aprenderás a enumerar y hacer fuerza bruta ante el servicio FTP
coverY: 0
---

# FTP

## Fuerza Bruta

{% hint style="success" %}
A la hora de emplear diccionarios se recomiendan los siguientes:

### Usuarios

```
/usr/share/wordlists/metasploit/unix_users.txt
```

### Contraseñas

```
/usr/share/wordlists/rockyou.txt
```
{% endhint %}

### Hydra

Para poder hacer fuerza bruta con hydra al servicio FTP, se pueden dar varios casos, en el caso de que tengas el usuario pero no la contraseña, deberás de usar un diccionario en este aspecto, por lo que el comando sería el siguiente

```
hydra -l user -P passwords.txt ftp://127.0.0.1 -t 15
```

En este caso con el parámetro `-l` le indicamos que sabemos el usuario y con el parámetro `-P` le indicamos la ruta donde esta el diccionario de contraseñas.

{% hint style="warning" %}
Puede que el servicio FTP no se encuentre en el puerto predefinido, en ese caso deberemos de especificar a hydra el puerto en el que está, esto se hace con el parámetro `-s`

```
hydra -l user -P passwords.txt ftp://127.0.0.1 -t 15 -s 2000
```
{% endhint %}

En el caso contrario de que no sepas el usuario pero si la contraseña, los parámetros querían así:

```
hydra -L users.txt -p Password123 ftp://127.0.0.1 -t 15
```

En este comando, le indicamos la ruta del diccionario de usuarios con el parámetro en mayúscula y una contraseña en concreto con el parámetro en minuscula. En el otro posible caso de que no tengamos ni la contraseña ni el usuario, pondremos los dos parámetros en mayúscula pasandole como valor la ruta del diccionario tanto de usuarios como de contraseñas.

```
hydra -L users.txt -P passwords.txt ftp://127.0.0.1 -t 15
```

{% hint style="info" %}
El con el parámetro`-t` le indicamos el número de hilos que queremos emplear
{% endhint %}

### Metasploit

Para esto, usaremos el modulo "FTP Authentication Scanner". Deberemos de configurar las siguientes opciones:

* USERNAME -> En el caso de que usemos un usuario concreto
* USER\_FILE -> En el caso de que usemos un diccionario de usuarios
* PASSWORD -> En el caso de que usemos una contraseña concreta
* PASS\_FILE -> En el caso de que usemos un diccionario de contraseñas
* RHOSTS -> IP de la máquina victima

### Nmap

Podemos usar nmap para hacer una pequeña fuerza bruta al servicio FTP. En este caso, usaremos el script "`ftp-brute`" pasándole como argumentos el diccionario de usuarios:

```bash
nmap --script ftp-brute --script-args userdb=/root/users -p 21 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

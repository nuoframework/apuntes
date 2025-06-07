# 🪟 Windows

{% hint style="info" %}
El Pivoting solo se podrá hacer si el host tiene más de una interfaz de red (sin contar loopback)
{% endhint %}

Para hacer pivoting en entornos Linux, deberemos explotar la maquina que tiene acceso a ambas redes.

## Metasploit

{% hint style="warning" %}
Para hacer un pivoting desde metasploit, deberemos tener una meterpreter activa en el host windows
{% endhint %}

Una vez estamos con la meterpreter activa, y vemos que la maquina tiene varias redes, por lo que tendremos que hacer pivoting entre ellas. Para ello, ponemos la sesión activa en segundo plano (CTRL + Z)  y le ponemos la "y".&#x20;

Comprobamos que la sesión está en segundo plano y activa:

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

### Escaneo de hosts

Usamos el arp scanner integrado:

```bash
use windows/gather/arp_scanner
```

Le indicamos el rango de red a escanear y el id de la sesión:

```bash
set SESSION 1

set RHOSTS 192.168.1.0/24

run
```

Nos encuentra los siguientes equipos:

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Después deberemos de enrutar el trafico, para ello empleamos un modulo de metasploit:

```bash
use multi/manage/autoroute
```

Y lo configuramos:

```
set SESSION 1
```

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Escaneo de puertos en un host

Para ello empleamos el siguiente modulo de metasploit:

```bash
use scanner/portscan/tcp
```

Lo configuramos:

{% hint style="info" %}
Por defecto el modulo hace un escaneo del puerto 1 al 10.000 (se puede modificar)
{% endhint %}

```bash
set RHOSTS 192.168.1.1
```

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

```
run
```

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Port Forwarding

Para el uso del port forwarding, haremos uso de un modulo de metasploit:

```bash
use windows/manage/portproxy

set CONNECT_ADDRESS 192.168.1.1    # IP maquina victima

set COONECT_PORT 80    # Puerto de la maquina victima

set LOCAL_ADDRESS 0.0.0.0   # IP atacante

set LOCAL_PORT  5000  # Puerto al que nos queremos traer el servicio en local

set SESSION 1    # ID de la sesion con la meterpreter
```

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

Ahora, imaginad que estamos haciendo esto en esta red:

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

En el caso de que tengamos la meterpreter en la maquina windows, primero hacemos un escaneo de hosts en la red, después hacemos un escaneo de puertos al host y finalmente hacemos el port forwarding. En el port forwarding, imagina que la maquina linux, tiene el puerto 80 (web) abierto, y lo queremos ver, enumerar... lo que hacemos es pasarnos el puerto 80 de la linux a un puerto cualquiera de la windows con la ayuda de port forwarding y desde la kali (atacante) apuntamos a la ip de la windows (192.168.1.40) y al puerto que hayamos elegido, de esta manera podemos ver el puerto de la 3 maquina:

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

# 🛜 Enumeración de Red

## Identificación del SO

### TTL

Para poder identificar el sistema operativo de un host/IP, podemos emplear una traza ICMP con la herramienta PING en la que dependiendo del TTL (Time To Live) que  obtengamos, corresponderá a un sistema u otro. Aquí las equivalencias:

| Sistema Operativo | TTL |
| ----------------- | --- |
| Linux             | 64  |
| FreeBSD           | 64  |
| MacOS             | 64  |
| Windows           | 128 |

{% hint style="info" %}
Existen herramientas como WhichSystem ([https://pastebin.com/HmBcu7j2](https://pastebin.com/HmBcu7j2)) que podemos emplear para identificar el sistema operativo de la IP
{% endhint %}

{% hint style="success" %}
Podemos comprobar si un host nos está enviado trazas ICMP (PING) con tcpdump:

```bash
sudo tcpdump -i tun0 icmp
```
{% endhint %}

### Nmap

Podemos usar nmap para aplicar un descubrimiento de SO, para ello, ejecutaremos el siguiente comando:

```bash
nmap -O 192.168.1.1
```

{% hint style="warning" %}
En el caso de que no se muestre la versión, se puede intensificar el escaneo con el parámetro `--osscan-guess`:

```bash
nmap -O --osscan-guess 192.168.1.1
```
{% endhint %}

### Sistemas Ubuntu

Además, una vez realizado la [#enumeracion-de-servicios](enumeracion-de-red.md#enumeracion-de-servicios "mention"), podremos determinar la version/codename de Linux de un host en base a la versión del servicio, para ello deberemos buscar en internet, el nombre del servicio tal cual hemos obtenido en la captura de NMAP acompañado de la palabra "launchpad"

{% hint style="warning" %}
Si tras realizar la intrusión (o antes de ella sabemos la información de la maquina) comprobamos que el codename que nos ha proporcionado la herramienta anterior no es el mismo que el del sistema operativo (lo podemos comprobar con `lsb_relase -a`), nos podemos encontrar con los siguientes casos:

* Que el servicio esté en un contenedor (p .ej: docker)
* Que sea un falso positivo
{% endhint %}

## Enumeración de equipos en la red actual

{% hint style="success" %}
Los equipos que están virtualizados, en la mac, los 4 primeros caracteres suelen coincidir con: `08:00`
{% endhint %}

{% hint style="info" %}
Para verificar que un host está activo y tenemos alcance, podemos hacer uso de la utilidad de Ping. Además, podemos especificar los paquetes que queremos enviar. Para Linux:

```bash
ping -c 4 192.168.1.1
```

Para Windows

```powershell
ping -n 4 192.168.1.1
```
{% endhint %}

### Nmap

Para este caso usaremos la herramienta **NMAP** con el siguiente comando:

<pre class="language-ruby"><code class="lang-ruby"><strong>nmap -sn 192.168.1.0/24 -oN hosts-SUBNET.txt
</strong></code></pre>

{% hint style="success" %}
Puedes saber cual es la dirección de red de la red a la que estas conectado actualmente con el comando&#x20;

```sh
ip a
```
{% endhint %}

### Fping

También tenemos otras opciones como **fping:**

```java
fping -a -g 192.168.1.0/24 2>/dev/null
```

### ARP-SCAN

```bash
arp-scan -I eth0 --localnet
```

### Netdiscover

```bash
netdiscover -i eth0 -r 192.168.1.0/24
```

## Enumeración de puertos&#x20;

En este caso, nuestro objetivo es enumerar mediante **NMAP** los puertos que están activos en una serie de maquinas. Imaginemos que hay una red con los siguientes equipos:

* 192.168.1.12
* 192.168.1.13
* 192.168.1.14
* 192.168.1.15

### TCP

Podemos hacerlo con el siguiente comando:

```ruby
sudo nmap -sS --min-rate 2000 -p- -Pn -vvv -n --open -oN puertos.txt 192.168.1.12,13,14,15
```

{% hint style="success" %}
Puedes usar la siguiente expresión regular para formatear y ver una lista de puertos y usarlo en otros comandos:

```bash
grep '^[0-9]' puertos.txt | cut -d '/' -f1 | sort -u | xargs | tr ' ' ','
```
{% endhint %}

### UDP

Con este comando podemos ver los puertos que hay por UDP:

```bash
nmap -sU --top-ports 1200 --min-rate=2000 -Pn 192.168.1.12
```

## Enumeración de servicios

Podemos usar el siguiente comando para obtener detalles sobre los servicios que corren en los puertos de las maquinas que hemos escaneado:

```java
nmap -sCV --script "vuln" --open -vvv -Pn -A -oN servicios.txt 192.168.1.12,13,14 -p22,80
```

{% hint style="info" %}
En el caso de que no se muestre la versión, se puede intensificar el escaneo con el parámetro `--version-intensity` seguido de la intensidad. 0 es baja y 9 es muy alta:&#x20;

```bash
nmap -sCV --version-intensity 9 --open -vvv -Pn -A -oN servicios.txt 192.168.1.12,13,14 -p22,80
```
{% endhint %}

### Enumeración de servicios vulnerables

Para detectar servicios vulnerables, podemos usar NMAP, además con el uso de esta herramienta, podremos ver descripcion de la vulnerabilidad, CVEs...

```bash
nmap --script "vuln" -p22,139,80 192.168.1.12
```

{% hint style="success" %}
## NSE Nmap Scripts

Podemos ver todos los scrips que tiene nmap disponibles ejecutando el siguiente comando:

```bash
ls -la /usr/share/nmap/scripts/
```

Para obtener ayuda sobre un script usamos el siguiente comando:

```bash
nmap --script-help=script-ejemplo
```

Para poder ejecutar un script  (o varios) en concreto ejecutamos nmap con el siguiente parámetro:

```bash
nmap --script=script-ejemplo,script-de-ejemplo2 192.168.1.1
```

En Nmap existen varías categorías de scripts. De hecho cada uno de los script disponibles está clasificado en cada una de ellas:

* auth
* broadcast
* brute
* default
* discovery
* dos
* exploit
* external
* fuzzer
* intrusive
* malware
* safe
* version
* vuln

\*Puedes ver más información sobre cada categoría haciendo [clic aquí](https://nmap.org/book/nse-usage.html#nse-categories)

Para hacer uso de los script que hay en una categoría, podemos usar el siguiente parámetro:

```bash
nmap --script=categoria 192.168.1.1
```

También podemos combinar características, por ejemplo, queremos saber información sobre versiones... pero que sea seguro o no muy intrusivo. Puede leer más información haciendo [clic aquí](https://nmap.org/book/nse-usage.html#nse-script-selection)
{% endhint %}

## Detección de Firewall

Esta parte es muy importante en la enumeración, puesto que herramientas como los firewalls o los IDS pueden evitar que en los escaneos no se nos reporte toda la información. Imagina que un firewall, bloquea las solicitudes ICMP Echo Reply, en este caso, al hacer un escaneo con la herramienta fping, ciertos hosts no se nos mostrarían.

En el caso de la enumeración de los puertos y servicios ocurre algo parecido, veamos un ejemplo:

Este es un host Windows (se puede ver por los servicios):

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>Captura de pantalla del escaneo de Nmap</p></figcaption></figure>

En este caso, vemos como podemos comprobar si hay un sistema de defensa en distintos puertos. Para ello, ejecutamos un escaneo con el parámetro `-sA`. En el caso de que exista un sistema de defensa, nos mostrará el puerto como "filtrado" o "filtered" y en caso contrario "unfiltered":

```bash
nmap -Pn -sA -p80,22,3389 192.168.1.1
```

Esta sería la salida de ejemplo:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Captura de pantalla del escaneo de Nmap</p></figcaption></figure>

## Evasión de sistemas IDS


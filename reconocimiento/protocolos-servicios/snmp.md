# SNMP

{% hint style="info" %}
Este puerto/servicio solo se encuentra por UDP por lo que habrá que adecuar el escaneo ([#udp](../enumeracion-de-red.md#udp "mention"))
{% endhint %}

Este protocolo se encarga de intercambiar información por red, por lo que es interesante enumerarlo correctamente

## Onesixtyone

Con esta herramienta podemos encontrar la clave de comunidad (Que es necesaria para poder seguir enumerando por este servicio) Ejecutamos el siguiente comando:

```bash
onesixtyone -c /ruta/diccionario 192.168.1.12
```

{% hint style="info" %}
El diccionario ser de tipo contraseñas como el rockyou.txt
{% endhint %}

En este caso, la clave es "security":

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

## Snmpwalk

Enumeramos con este comando/herramienta posible información relevante (dominios, IPs...)

```bash
snmpwalk -v 2c -c contraseña 192.168.1.12
```

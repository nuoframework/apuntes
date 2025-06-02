# 80 & 443 - HTTP/S

## Heartbleed Memory Leak (OpenSSL - CVE-2014-0160)

Heartbleed fue una vulnerabilidad de OpenSSL, la librería que protege muchas conexiones por Internet. Se originó en la extensión “Heartbeat” (latido del corazón), que **sirve para comprobar que una conexión TLS sigue activa**. El fallo consistía en que, al recibir un mensaje de comprobación, el servidor no verificaba correctamente cuántos bytes debía devolver. De este modo, **un atacante enviaba un mensaje pequeño pero indicaba que quería cientos de kilobytes, y el servidor acababa devolviendo fragmentos de su propia memoria**.

Esos fragmentos podían contener información muy sensible: claves privadas de certificados, contraseñas, correos electrónicos o cualquier dato en memoria en ese momento.

### Explotación

{% hint style="info" %}
Exploit: [https://github.com/vulhub/vulhub/blob/master/openssl/CVE-2014-0160/ssltest.py](https://github.com/vulhub/vulhub/blob/master/openssl/CVE-2014-0160/ssltest.py)
{% endhint %}

```bash
python3 ssltest.py <IP> -p <PUERTO>
```

{% hint style="success" %}
Para eliminar las filas con información vacía, podemos usar el siguiente comando:

```bash
python3 ssltest.py <IP> -p <PUERTO> | grep -v "00 00 00 00..."
```

_**\*IMPORTANTE**: Hay que sustituir los ceros de dentro del grep por los de la consola, porque puede que no tengan formato y no los filtre correctamente_
{% endhint %}

---
description: En esta página aprenderás a enumerar el servicio HTTP/S
cover: >-
  https://images.unsplash.com/photo-1607028227034-f0378aa1bd2c?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw1fHxnZW9tZXRyaWN8ZW58MHx8fHwxNjg5MjQxNDA2fDA&ixlib=rb-4.0.3&q=85
coverY: 58
---

# HTTP/S

## Revisión del Certificado SSL

Muchas de las veces, en el certificado, aparecen subdominios, por lo que es importante revisarlo

{% hint style="info" %}
Puedes revisar el certificado, una vez hayas entrado en la web mediante el navegador, al lado de la URL, hay un candado, si haces clic podrás ver más detalles sobre el certificado
{% endhint %}

### OpenSSL&#x20;

Podemos hacerlo desde la terminal con OpenSSL, de esta manera:

```
openssl s_client -connect website.com
```

De esta manera le indicamos que desde la visión del cliente, queremos ver la informacion de los certificados SSL de la web

### Sslscan

En algunos casos, existen vulnerabilidades (como [heartbleed](https://www.incibe.es/incibe-cert/alerta-temprana/vulnerabilidades/cve-2014-0160)...) en el certificado SSL, y este lo podemos comprobar con esta herramienta

```
sslscan website.com
```

### Nmap

Ademas de comprobarlo con sslscan, podemos hacerlo con un script que tiene nmap llamado `ssl-heartbleed.nse` :&#x20;

```
nmap --script ssl-heartbleed -p443 127.0.0.1
```

{% hint style="info" %}
#### [Vulnerabilidad Heartbleed](https://www.incibe.es/incibe-cert/alerta-temprana/vulnerabilidades/cve-2014-0160)

Esta vulnerabilidad pertenece a la libreria criptografica de OpenSSL, la cual permite hacer un volcado de parte de la memoria de la maquina, así llegando a ver archivos, cookies de sesión...

Puedes leer informacion sobre el CVE-2014-0160 (HeartBleed) en estos enlaces:

1. [https://heartbleed.com/](https://heartbleed.com/)
2. [https://www.incibe.es/incibe-cert/alerta-temprana/vulnerabilidades/cve-2014-0160](https://www.incibe.es/incibe-cert/alerta-temprana/vulnerabilidades/cve-2014-0160)
{% endhint %}

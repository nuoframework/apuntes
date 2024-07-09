---
cover: https://lab.wallarm.com/wp-content/uploads/2020/02/ssrf-min.png
coverY: 59
---

# Server-Site Request Forgery (SSRF)

## ¿Que es un SSRF?

Los ataques de tipo SSRF, consisten en hacer que un servidor web realice solicitudes HTTP descritas por el atacante en su nombre. Ejemplo:

Una tienda realiza peticiones HTTP a una URL para obtener información de stock acerca de un producto, si ese aplicativo web fuese vulnerable a SSRF, podríamos modificar el valor de la dirección, apuntando a la maquina en si misma, de manera que podamos ver información que desde fuera no se puede ver o acceder a sitios que solo desde dentro podemos ver.

## ¿Que se diferencia de un CSRF?

Esta vulnerabilidad web es muy similar a los CSRF que hacen que mediante la preparación de una URL, un usuario que realice la petición, ejecute una acción con dicha solicitud ([CSRF](cross-sise-request-forgery-csrf.md)), la diferencia esta en que en el SSRF, la petición  la realiza el propio servidor, en cambio el CSRF la petición la realiza el usuario que hace la solicitud a la URL que el atacante prepara

## Ataques contra el propio servidor

En muchas ocasiones, las empresas utilizan entornos de pre-producción o utilizan servicios internos que no salen hacia fuera de la red local. Pues los ataques SSRF permiten al atacante acceder al entorno interno realizando una petición contra el propio servidor



{% hint style="info" %}
Enlaces de Interés:

1. [https://portswigger.net/web-security/ssrf](https://portswigger.net/web-security/ssrf)
2. [https://www.youtube.com/watch?v=xQ2rivaFcsE\&ab\_channel=S4viOnLive%28BackupDirectosdeTwitch%29](https://www.youtube.com/watch?v=xQ2rivaFcsE\&ab\_channel=S4viOnLive%28BackupDirectosdeTwitch%29)
3. [https://byte-mind.net/como-funciona-la-vulnerabilidad-ssrf/](https://byte-mind.net/como-funciona-la-vulnerabilidad-ssrf/)
4. [https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery](https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery)
{% endhint %}

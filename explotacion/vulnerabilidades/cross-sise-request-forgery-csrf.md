---
cover: >-
  https://byte-mind.net/wp-content/uploads/2021/12/csrf-cross-site-request-forgery.jpg
coverY: -202
---

# Cross-Sise Request Forgery (CSRF)

## ¿Qué es un CSRF?

Un ataque CSRF o XSRF, es una vulnerabilidad web que hace que un atacante, pueda crear solicitudes que una vez ejecutadas por el usuario, se ejecutan sin que el usuario tenga intención de ello.&#x20;

## CSRF Tradicional

### ¿Como funciona?

Por ejemplo, imagina que la web es una tienda online, y eres el usuario administrador, existe una pagina para cambiar la contraseña, de normal, los datos de cambio de contraseña (la nueva contraseña) viajan por POST al servidor, pero an algunos casos, podemos transformar la solicitud a GET, si esta también funciona, tenemos el CSRF, ya que si enviamos esta URL con los valores de los parámetros en ella, si el usuario administrador (o cualquier otro) hace la solicitud con el enlace, modificará su contraseña

## XMLHttpRequest CSRF



## Flash-based CSRF



{% hint style="info" %}
Enlaces de Interés:

1. [https://portswigger.net/web-security/csrf](https://portswigger.net/web-security/csrf)
2. [https://book.hacktricks.xyz/pentesting-web/csrf-cross-site-request-forgery](https://book.hacktricks.xyz/pentesting-web/csrf-cross-site-request-forgery)
3. [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/CSRF%20Injection/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/CSRF%20Injection/README.md)
4. [https://0xffsec.com/handbook/web-applications/csrf/](https://0xffsec.com/handbook/web-applications/csrf/)
5. [https://owasp.org/www-community/attacks/csrf](https://owasp.org/www-community/attacks/csrf)
{% endhint %}

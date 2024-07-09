---
cover: https://www.vaadata.com/blog/wp-content/uploads/2021/12/SSTI_vulnerability.png
coverY: 245
---

# Server-Side Template Injection (SSTI)

## ¿Que es un SSTI?

Un SSTI consiste en la inyección de código malicioso en un campo perteneciente a una plantilla que se concatenan en la plantilla antes de renderizar el contenido visual. Es algo similar a una [SQLi](sql-injection-sqli/)

## Identificación

Cuando tenemos campos o valores modificables y queremos probar si estos son vulnerables a una SSTI o veamos que Python o Flask están activos en la web, podemos intentar usar Payloads, esta es una lista de Payloads dependiendo de que se use por detrás, aunque los más usados son los siguientes:

<pre><code>{{ 7*7 }}
<strong>{{4*4}}[[5*5]]
</strong>{{7*'7'}} would result in 7777777
</code></pre>

Payloads --> [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#detection](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#detection)

{% hint style="info" %}
Enlaces de Interés:

1. [https://portswigger.net/web-security/server-side-template-injection](https://portswigger.net/web-security/server-side-template-injection)
2. [https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)
3. [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md)
4. [https://infayer.com/archivos/803](https://infayer.com/archivos/803)
{% endhint %}

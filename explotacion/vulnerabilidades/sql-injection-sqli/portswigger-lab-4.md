---
description: >-
  Resolución del 4º laboratorio de SQL injection UNION attack, finding a column
  containing text
---

# 🧪 PortSwigger Lab 4

> Este laboratorio contiene una vulnerabilidad de inyección SQL en el filtro de categoría de producto. Los resultados de la consulta se devuelven en la respuesta de la aplicación, por lo que puede utilizar un ataque UNION para recuperar datos de otras tablas. Para construir dicho ataque, primero debe determinar el número de columnas devueltas por la consulta. Puede hacer esto usando una técnica que aprendió en un laboratorio anterior. El siguiente paso es identificar una columna que sea compatible con los datos de cadena.
>
> El laboratorio proporcionará un valor aleatorio que debe hacer que aparezca dentro de los resultados de la consulta. Para resolver el laboratorio, realice un ataque UNION de inyección SQL que devuelva una fila adicional que contenga el valor proporcionado. Esta técnica le ayuda a determinar qué columnas son compatibles con los datos de cadena.

Vemos que es vulnerable y tiene 3 columnas:&#x20;

<figure><img src="../../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

Introducimos el texto `PTGuqt` que nos indica el lab y empezamos a probar para saber que columna es la que es de tipo String:

<figure><img src="../../../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

Con esto obtenemos una fila más con el nombre que nos han indicado:

<figure><img src="../../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

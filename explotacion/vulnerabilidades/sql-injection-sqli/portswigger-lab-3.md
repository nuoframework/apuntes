---
description: >-
  Resolución del 3º laboratorio de SQLi de PortSwigger. SQL injection UNION
  attack, determining the number of columns returned by the query
cover: ../../../.gitbook/assets/Banner Gitbook sql port3.png
coverY: 0
---

# 🧪 PortSwigger Lab 3

> Este laboratorio contiene una vulnerabilidad de inyección SQL en el filtro de categoría de producto. Los resultados de la consulta se devuelven en la respuesta de la aplicación, por lo que puede utilizar un ataque UNION para recuperar datos de otras tablas. El primer paso de tal ataque es determinar el número de columnas que están siendo devueltas por la consulta. Luego usará esta técnica en laboratorios posteriores para construir el ataque completo.
>
> Para resolver el laboratorio, determine el número de columnas devueltas por la consulta realizando un ataque UNION de inyección SQL que devuelve una fila adicional que contiene valores nulos.

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Probamos a usar el `ORDER BY` para enumerar las tablas:

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Vemos que es vulnerable a una SQLi, pero necesitamos confirmar cuantas columnas tiene, para ello, seguimos aumentando el número del `ORDER BY`. Hasta que llegamos a 4 y nos da un error. En ese punto sabemos que hay 3 columnas:

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

Aunque ya sabemos cuantas columnas hay, para resolver el lab, hay que añadir una fila de valores nulos:

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

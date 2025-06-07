---
description: >-
  Resolución del 7º laboratorio de SQL injection attack, querying the database
  type and version on MySQL and Microsoft
---

# 🧪 PortSwigger Lab 7

> Este laboratorio contiene una vulnerabilidad de inyección SQL en el filtro de categoría de producto. Puede usar un ataque UNION para recuperar los resultados de una consulta inyectada.
>
> Para resolver el laboratorio, muestre la cadena de versión de la base de datos.

Comprobamos que sea vulnerable:

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Cuando intentamos hacer una inyección SQL y no obtenemos respuesta propia de ella, se recomienda usar otro tipo de comentario o bien porque el `--` está tratado o bien porque se trata de otra BBDD diferente a MySQL, por ejemplo, para el caso de MySQL, se suele usar tambien la `#`
{% endhint %}

Ahora, tratamos de averiguar que columnas admiten String (por el tipo de datos que hay en la web, podemos pensar que ambas van a ser de tipo String):

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Ahora enumeramos la versión:

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

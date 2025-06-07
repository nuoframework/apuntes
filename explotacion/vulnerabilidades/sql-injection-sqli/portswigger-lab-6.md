---
description: >-
  Resolución del 6º laboratorio de SQL injection UNION attack, retrieving
  multiple values in a single column
---

# 🧪 PortSwigger Lab 6

> Este laboratorio contiene una vulnerabilidad de inyección SQL en el filtro de categoría de producto. Los resultados de la consulta se devuelven en la respuesta de la aplicación para que pueda usar un ataque UNION para recuperar datos de otras tablas.
>
> La base de datos contiene una tabla diferente llamada `users`, con columnas llamadas `username` y `password`.
>
> Para resolver el laboratorio, realice un ataque UNION de inyección SQL que recupere todos los nombres de usuario y contraseñas, y use la información para iniciar sesión como `administrator` usuario.

Comprobamos que es vulnerable:

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
## ¿Porque `/filter?category=Pets' UNION SELECT username,password FROM users--` no funciona?

<img src="../../../.gitbook/assets/image (1).png" alt="" data-size="original">\
\
Este payload no funciona porque esta tabla, solo admite un campo de tipo String.

¿Que debemos hacer en esta situación?

Podemos mostrar en la primera columna un valor NULL y en la segunda (que es la que admite el String) mostramos los valores de usuario y contraseña.\
Estos valores, como son 2 campos en la misma columna, debemos de concatenarlos para que actúen como uno solo
{% endhint %}

Buscamos el campo que admite String:

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Tras ver que es el 2º, en el primero inyectaremos un valor nulo y en el segundo la concatenación de los valores que nos pide el lab (username y password):

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Una ves tenemos las credenciales, iniciamos sesión y resolvemos el lab:

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

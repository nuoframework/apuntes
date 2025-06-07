---
description: >-
  Resolución del 5º laboratorio de SQL injection UNION attack, retrieving data
  from other tables
---

# 🧪 PortSwigger Lab 5

> Este laboratorio contiene una vulnerabilidad de inyección SQL en el filtro de categoría de producto. Los resultados de la consulta se devuelven en la respuesta de la aplicación, por lo que puede utilizar un ataque UNION para recuperar datos de otras tablas. Para construir tal ataque, debe combinar algunas de las técnicas que aprendió en laboratorios anteriores.
>
> La base de datos contiene una tabla diferente llamada `users`, con columnas llamadas `username` y `password`.
>
> Para resolver el laboratorio, realice un ataque UNION de inyección SQL que recupere todos los nombres de usuario y contraseñas, y use la información para iniciar sesión como `administrator` usuario.

Comprobamos el número de columnas (ya lo sabemos por el enunciado):

<figure><img src="../../../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

Inyectamos una consulta SQL con SELECT en el UNION para que nos devuelva los resultados de la tabla de users:

<figure><img src="../../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

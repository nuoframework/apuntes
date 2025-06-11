---
description: >-
  Resolución del 8º laboratorio de SQL injection attack, listing the database
  contents on non-Oracle databases
cover: ../../../.gitbook/assets/Banner Gitbook sql port8.png
coverY: 0
---

# 🧪 PortSwigger Lab 8

> Este laboratorio contiene una vulnerabilidad de inyección SQL en el filtro de categoría de producto. Los resultados de la consulta se devuelven en la respuesta de la aplicación para que pueda usar un ataque UNION para recuperar datos de otras tablas.
>
> La aplicación tiene una función de inicio de sesión, y la base de datos contiene una tabla que contiene nombres de usuario y contraseñas. Debe determinar el nombre de esta tabla y las columnas que contiene, luego recuperar el contenido de la tabla para obtener el nombre de usuario y la contraseña de todos los usuarios.
>
> Para resolver el laboratorio, inicie sesión como el `administrator` usuario.

Comprobamos que sea vulnerable:


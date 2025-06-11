---
description: Resolución del 2º laboratorio de SQLi de PortSwigger
cover: ../../../.gitbook/assets/Banner Gitbook sql port2.png
coverY: 0
---

# 🧪 PortSwigger Lab 2

> Este laboratorio contiene una vulnerabilidad de inyección SQL en la función de inicio de sesión.
>
> Para resolver el laboratorio, realice un ataque de inyección SQL que inicie sesión en la aplicación como `administrator` usuario.

Interceptamos la petición de login con el Burp y modificamos la petición para que omita la contraseña:

<figure><img src="../../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

De esta forma podremos acceder con la cuenta `administrator` sin proporcionar credenciales

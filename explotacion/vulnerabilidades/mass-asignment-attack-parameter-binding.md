# Mass-Asignment Attack / Parameter Binding

## ¿Qué es un Ataque de Asignamiento Masivo?

Un ataque de asignamiento masivo, es un ataque se realiza usando parámetros adicionales en la solicitud al servidor, para que este la interprete.

<figure><img src="../../.gitbook/assets/0_NdrpE_5pAd7b5g5V.png" alt=""><figcaption></figcaption></figure>

## Escenario 1

Nos encontramos en un formulario de registro, del cual interceptamos la petición de registro:

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

El servidor nos contesta con otro formato JSON:

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

Como vemos, en la respuesta del servidor, nos devuelve parámetros que nosotros no hemos enviado. Pues en este punto, podemos intentar hacer un Ataque de Asignamiento Masivo o Parameter Binding, en este caso, de lado de nuestra petición de registro vamos a añadir el parámetro de `role` y asignarle otro valor:

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

La respuesta a esta petición modificada es exitosa, y nuestro usuario se ha registrado con rol de administrador

{% hint style="info" %}
En algunos casos, el servidor no responde con ningún parámetro nuevo, por lo que podemos usar una ataque de fuerza bruta en las cabeceras de la solicitud para determinar que nombre de párametro y que valor, nos permitirá hacer cambios.
{% endhint %}

---
description: Resolución del 1º laboratorio de SQLi de PortSwigger
cover: ../../../.gitbook/assets/Banner Gitbook sql port1.png
coverY: 0
---

# 🧪 PortSwigger Lab 1

En este laboratorio, tenemos que aplicar una inyección sql en la clausula `WHERE` que nos permita ver datos ocultos

<figure><img src="../../../.gitbook/assets/imagen (8).png" alt=""><figcaption></figcaption></figure>

En este caso nos especifica que la inyección se encuentra en la parte de categorías.

<figure><img src="../../../.gitbook/assets/imagen (2).png" alt=""><figcaption></figcaption></figure>

Como podemos ver, si aplicamos un filtro, se aplica un parametro, ahí tenemos que realizar la inyección sql:

<figure><img src="../../../.gitbook/assets/imagen (6).png" alt=""><figcaption></figcaption></figure>

Para probar si es vulnerable a SQLi, podemos hacer lo siguiente:

<figure><img src="../../../.gitbook/assets/imagen (3).png" alt=""><figcaption></figcaption></figure>

En este caso lo que hacemos es añadir una comilla simple y a continuación añadir dos guiones seguidos de un espacio y otro guion (los guiones se utilizan para comentar consultas). Nosotros suponemos que el código no esta bien sanetizado, por lo que podremos añadir una comilla y descomentar el código que haya detrás como en este ejemplo.

<figure><img src="../../../.gitbook/assets/imagen (4).png" alt=""><figcaption></figcaption></figure>

Esta es la prueba de que este parámetro es vulnerable a SQLi, pero si nos fijamos en el antes y el después:&#x20;

<div><figure><img src="../../../.gitbook/assets/imagen (9).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/imagen (1).png" alt=""><figcaption></figcaption></figure></div>

Como podemos observar, cuando hacemos la SQLi, y comentamos el posible codigo que exista detrás del parámetro, se añade un producto a la web. Esto pasa porque la consulta que se realiza por detrás es esta:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

Cuando nosotros hacemos la inyección, comentamos parte de la consulta que nos muestra solo los productos lanzados.

```sql
SELECT * FROM products WHERE category = 'Gifts'-- -' AND released = 1
```

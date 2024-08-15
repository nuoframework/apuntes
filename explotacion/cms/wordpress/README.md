# WordPress

## Fuerza Bruta para la obtención de credenciales

### WPScan

{% hint style="info" %}
Puedes encontrar más información en [#wpscan](../../../reconocimiento/cmss/enumeracion-wordpress.md#wpscan "mention")
{% endhint %}

Usando la herramienta, y con el siguiente comando podemos intentar contraseñas validas para un usuario y viceversa (usuarios validos para una contraseña) además de usar en ambos casos diccionarios:

```perl
wpscan -U usuario -P diccionario.txt --url http://192.168.1.10/
```

{% hint style="info" %}
Para variar entre el uso del diccionario en el lado del usuario, contraseña o ambos, no es necesario modificar el parámetro, solamente el valor por una palabra o una ruta
{% endhint %}

### Hydra

Con hydra podemos hacer fuerza bruta al login de wordpress. En este caso, le haremos el fuzzing a la peticion, por lo que el primer paso, es capturar la peticion que hacemos al iniciar sesion de manera erronea (podemos hacerlo con burpsuite):

<figure><img src="../../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

Ahora, con hydra usamos la siguiente sintaxis:

```python
hydra -L users.txt -P passwords.txt 127.0.0.1 http-post-form "/endpoint-del-login.php:COPIAR-PETICION-DE-INICIO-DE-SESION:mensaje de error"
```

Como podemos ver, usaremos 3 parámetros:

1. **Endpoint del login**: Por ejemplo /wp-login.php
2. **Petición de ejemplo capturada por burpsuite**: Aquí pegamos la petición que hemos capturado, en el sitio del usuario, en el caso de que usemos un diccionario (usamos la opción `-L`)  ponemos donde iría el usuario el texto "`^USER^`" para que hydra sustituya el texto por el valor de las líneas del diccionario, en el caso de que solo queramos usar un usuario, ponemos el usuario literalmente en la petición (y usamos la opción `-l`). Para la contraseña hacemos lo mismo (el parámetro de la contraseña es "^PASS^")
3. **Mensaje de error**: Deberemos poner parte del mensaje de error que nos devuelve la petición

## Explotando WordPress con temas

Una vez estemos logeados con usuario y contraseña dentro del panel de administración de wordpress, para poder insertar código malicioso en un tema nos vamos a la parte de apariencia:

<figure><img src="../../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

Una vez estemos dentro del editor de temas, buscamos por ejemplo un footer o alguna pagina o modulo, eliminamos todo y creamos un payload en php con msfvenom ([#reverse-shell-tcp-con-php](../../msfvenom.md#reverse-shell-tcp-con-php "mention")). Dicho contenido del payload, lo pegamos en el editor del tema:

<figure><img src="../../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

Ahora, accedemos a dicho recurso y se nos enviará una reverse shell

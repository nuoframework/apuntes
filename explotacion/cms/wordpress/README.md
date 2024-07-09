# WordPress

## Explotando WordPress con temas

Una vez estemos logeados con usuario y contraseña dentro del panel de administración de wordpress, para poder insertar código malicioso en un tema nos vamos a la parte de apariencia:

<figure><img src="../../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

Una vez estemos dentro del editor de temas, buscamos por ejemplo un footer o alguna pagina o modulo, eliminamos todo y creamos un payload en php con msfvenom ([#reverse-shell-tcp-con-php](../../msfvenom.md#reverse-shell-tcp-con-php "mention")). Dicho contenido del payload, lo pegamos en el editor del tema:

<figure><img src="../../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

Ahora, accedemos a dicho recurso y se nos enviará una reverse shell

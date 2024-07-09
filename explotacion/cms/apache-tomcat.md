# Apache Tomcat

## Login

### Credenciales por defecto

Apache Tomcat, por defecto monta un panel de login en el directorio "`/manager/html`" y existen credenciales por defecto:

* admin:admin
* tomcat:tomcat
* admin:\<nothing>
* admin:s3cr3t
* tomcat:s3cr3t
* admin:tomcat

{% hint style="info" %}
Puede que podemos ver la contraseña, si una vez en el panel de login, le damos a cancelar:

![](<../../.gitbook/assets/image (11).png>)

![](<../../.gitbook/assets/image (12).png>)
{% endhint %}

## Subida de archivos

En la consola de administración de Apache Tomcat, hay un sitio en el que podemos subir archivos:

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Ahora, generamos un payload [#reverse-shell-tcp-con-war](../msfvenom.md#reverse-shell-tcp-con-war "mention") y subimos el archivo

Una vez esté subido, hacemos clic en el:

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Si este payload no funciona, podemos probar con la otra opción que hay debajo
{% endhint %}

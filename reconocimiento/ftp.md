# FTP

## Enumeración del servicio

Podemos con Nmap enumerar este servicio una vez sepamos que el puerto FTP (21) esté abierto. La siguiente instruccion nos da detalles del servicio

```
nmap -sCV -p21 127.0.0.1
```

{% hint style="info" %}
Para conectarte al servicio FTP, este es el comando que debes de ejecutar:

```
ftp 127.0.0.1
```

Despues te pedirá usuario y contraseña
{% endhint %}

## Usuario Anonimo habilitado

Cuando se lanzan los scripts basicos de reconocimiento con el parametro `-sC` se lanza el script `ftp-anon.nse` y este se encagar de revisar si el usuario y contraseña Anonymous están habilitados

<figure><img src="../.gitbook/assets/image (54).png" alt=""><figcaption><p>Captura de pantalla de escaneo de Nmap</p></figcaption></figure>

{% hint style="info" %}
En los casos en los que el usuario `anonymous` está habilitado, las credenciales son las siguientes:

`anonymous:anonymous`
{% endhint %}

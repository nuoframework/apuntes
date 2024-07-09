# 🛠️ Metasploit

{% hint style="info" %}
Para iniciar Metasploit, este comando es el mejor:

```bash
sudo msfdb init && msfconsole
```
{% endhint %}

## Uso de metasploit para exploits y módulos

Para buscar un modulo en metasploit, usamos el comando "`search modulo a buscar`"

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption><p>Captura de pantalla de la búsqueda de un módulo de enumeración de usuarios de ssh en metasploit</p></figcaption></figure>

Después, se listarán los módulos que coinciden con nuestra búsqueda, y para seleccionar uno de ellos, usamos el comando "`use`" seguido del numero del modulo que vayamos a usar (P. ej: "`use 2`")

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption><p>Uso del modulo / exploit</p></figcaption></figure>

Ahora, para ver las opciones que tenemos que configurar para que el script funcione, usamos "`show options`"

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption><p>Mostrado de opciones sobre el módulo elegido</p></figcaption></figure>

{% hint style="warning" %}
Antes de continuar, debemos diferenciar dos conceptos:

* Exploit: Es el código o la técnica utilizada para aprovechar una vulnerabilidad específica en un sistema o aplicación. Su objetivo es acceder al sistema vulnerable y ejecutar el payload. (P. ej: Un exploit puede aprovechar una vulnerabilidad en un servidor web para ejecutar comandos en el servidor.)
* Payload: Es el código que se ejecuta una vez que el exploit ha logrado acceder al sistema. Su objetivo es realizar una acción específica, como abrir una sesión de shell, instalar un malware o recolectar datos. (P. ej: Un payload puede ser una reverse shell que permite al atacante obtener control remoto del sistema.)
{% endhint %}

Una vez seleccionado el exploit / módulo, deberemos de declarar una serie de valores para opciones del exploit, para ello, lo hacemos con "`set OPCION valor`" (P. ej: "`set RHOSTS 192.168.1.2`")

{% hint style="info" %}
En la mayoría de casos, nos encontraremos con algunas opciones a configurar (obligatorias):

* RHOSTS -> La IP de la máquina victima
* RPORTS -> El puerto de la maquina victima
* LHOSTS -> La IP de la máquina atacante
* LPORTS -> El puerto de la máquina atacante
{% endhint %}

Una vez hayamos puesto valores a las opciones obligatorias del módulo, ejecutamos "`run`" o "`exploit`"

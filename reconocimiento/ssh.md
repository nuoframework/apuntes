# SSH

{% hint style="info" %}
## Conexión

### Credenciales

```
ssh usuario@ip_host
```

### Clave id\_rsa

```
ssh -i /ruta/id_rsa usuario@ip_host
```

En el caso de que nos aparezca una advertencia como esta:

<img src="../.gitbook/assets/image (22).png" alt="" data-size="original">

Deberemos de modificar los permisos:

* El directorio /home/xxx/.ssh permisos (700) -> `sudo chmod 700 ~/.ssh`&#x20;
* Las Keys permisos (600) -> `sudo chmod 600 ~/.shh/`&#x20;
* La lista de host conocidos permisos (644) -> `sudo chmod 644 ~/.shh/known_hosts`\

{% endhint %}



{% hint style="success" %}
## Transferencias de archivos entre hosts por SSH con scp



### De host a victima

```perl
spc /ruta/archivo/local usuariovictima@ipvictima:/ruta/maquina/victima
```

### De victima a host

```perl
spc usuariovictima@ipvictima:/ruta/archivo/victima /ruta/maquina/host
```
{% endhint %}

## Enumeración del servicio

Podemos enumerar los detalles del servicio con nmap. El servicio SSH (Secure Shell), se encuentra en el puerto 22

```
nmap -sCV -p22 127.0.0.1
```

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption><p>Captura de pantalla de Nmap</p></figcaption></figure>

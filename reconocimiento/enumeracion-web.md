# 🌐 Enumeración Web

{% hint style="warning" %}
En el caso de encontrar un panel de login, recomendamos hacer peticiones desde la terminal con curl y usando time para determinar si hay una posible enumeración de usuarios en función de si hay diferencias de respuesta con credenciales validas o no
{% endhint %}

{% hint style="info" %}
Se recomienda revisar el archivo robots.txt
{% endhint %}

## Tecnologías

### Revisión del código fuente

La primera opción y la más tediosa es mirar el código fuente de la web (`CTRL + U`) puesto que el desarrollador se podría haber dejado comentarios o trozos de código donde aparezcan versiones de software

{% hint style="info" %}
Puede que también haya versiones detalladas en el pie de la página
{% endhint %}

### Herramientas

Para enumerar las tecnologías que tiene una web, podemos emplear la herramienta "whatweb". P. ej:

```bash
whatweb https://dominio.com/
```

Otra opción es hacer uso de extensiones del navegador como "Wappalyzer", "BuiltWith" o "What Runs"

{% embed url="https://chromewebstore.google.com/detail/whatruns/cmkdbmfndkfgebldhnkbfhlneefdaaip" %}
Instalación de la herramienta WhatRuns
{% endembed %}

{% embed url="https://chromewebstore.google.com/detail/builtwith-technology-prof/dapjbgnjinbpoindlpdmhochffioedbn" %}
Instalación de la herramienta BuiltWith
{% endembed %}

{% embed url="https://chromewebstore.google.com/detail/wappalyzer-technology-pro/gppongmhjkpfnbhagpmjfkannfbllamg" %}
Instalación de la herramienta Wappalyzer
{% endembed %}

{% hint style="info" %}
## jQuery / jQuery UI

Las versiones antiguas o desactualizadas, suelen tener vulnerabilidades como [cross-site-scripting-xss.md](../explotacion/vulnerabilidades/cross-site-scripting-xss.md "mention") y [prototype-pollution.md](../explotacion/vulnerabilidades/prototype-pollution.md "mention")
{% endhint %}

## Virtual Hosting

Puede que haya webs que utilicen virtual hosting y para identificarlo, nos podemos encontrar varios casos.

Por ejemplo, podemos encontrarnos una web que no se renderice correctamente o que no tenga acceso a ciertos recursos, si miramos el código fuente, nos daremos cuenta de que puede ocurrir porque solicita el recurso a un dominio y no a la IP desde la que estamos visitando el servicio. Otro caso es que nos encontremos un dominio en el escaneo de NMAP al servicio ([#enumeracion-de-servicios](enumeracion-de-red.md#enumeracion-de-servicios "mention"))&#x20;

Para poder ver correctamente la web, deberemos añadir este dominio e IP al archivo `/etc/hosts` por ejemplo:

{% code title="/etc/hosts" %}
```ruby
192.168.1.10    dominio.local
```
{% endcode %}

## Fuzzing/Discovery

Para poder buscar directorios en un servicio web en busca de portales de administración, usamos herramientas que nos ayudan a "sustituir" una parte de la ruta que le indiquemos por palabras de un diccionario.

{% hint style="success" %}
Es recomendable, realizar una búsqueda recursiva en los directorios más relevantes
{% endhint %}

Además, podremos emplear google hacking / dorks el cual nos ayudará con la búsqueda de directorios relevantes para la intrusión [google-hacking.md](../osint/google-hacking.md "mention")

Existen las siguientes herramientas:

{% hint style="success" %}
## Diccionarios recomendados

### eJPT

1. `/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt`

### Directorios Generales

1. `/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt`
2. `/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-big.txt`
3. `/usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt`
4. `/usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt`
5. `/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt`
6. `/usr/share/seclists/Discovery/Web-Content/raft-small-directories.txt`

#### `Lowercase`

1. `/usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt`
2. `/usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-big.txt`
3. `/usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt`
4. `/usr/share/seclists/Discovery/Web-Content/raft-large-directories-lowercase.txt`
5. `/usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-small.txt`
6. `/usr/share/seclists/Discovery/Web-Content/raft-small-directories-lowercase.txt`

### Login

1. `/usr/share/seclists/Discovery/Web-Content/Logins.fuzz.txt`
2. `/usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt`
3. `/usr/share/seclists/Discovery/Web-Content/raft-large-files.txt`
4. `/usr/share/seclists/Discovery/Web-Content/trickest-robots-disallowed-wordlists/top-10000.txt`
5. `/usr/share/seclists/Discovery/Web-Content/SVNDigger/all.txt`
6. `/usr/share/seclists/Discovery/Web-Content/raft-small-files.txt`
7. `/usr/share/seclists/Discovery/Web-Content/SVNDigger/context/log.txt`

En este directorio encontrarás diferentes archivos txt en los que dependiendo del lenguaje se hace fuzzing de uno u otro

1. `/usr/share/seclists/Discovery/Web-Content/SVNDigger/cat/Language/`

#### Lowercase

1. `/usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt`
2. `/usr/share/seclists/Discovery/Web-Content/raft-large-files-lowercase.txt`
3. `/usr/share/seclists/Discovery/Web-Content/raft-small-files-lowercase.txt`

### Subdominos

1. `/usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt`
2. `/usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt`
3. `/usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt`
4. `/usr/share/seclists/Discovery/DNS/n0kovo_subdomains.txt`
5. `/usr/share/seclists/Discovery/DNS/namelist.txt`
6. `/usr/share/seclists/Discovery/DNS/dns-Jhaddix.txt`
7. `/usr/share/seclists/Discovery/DNS/bug-bounty-program-subdomains-trickest-inventory.txt`
8. `/usr/share/seclists/Discovery/DNS/combined_subdomains.txt`


{% endhint %}

### Nmap

```bash
nmap -sCV --script=http-enum IP
```

### Wfuzz

Para hacer busqueda de directorios:

```perl
wfuzz -c -t 200 -hc=404 -w /ruta/diccionario https://web.com/FUZZ
```

{% hint style="info" %}
Con la palabra "FUZZ" le indicamos a la herramientas donde queremos sustituir las palabras del diccionario
{% endhint %}

Para hacer busqueda de subdominios:

```bash
wfuzz -c -t 200 -hc=404 -w /ruta/diccionario -H "Host: FUZZ.dominio.com" -u 192.168.1.12
```

### Gobuster

Para realizar una búsqueda de directorios:

```perl
gobuster dir -t 150 -w /ruta/diccionario -u http://dominio.com/
```

Subdominios:

```bash
gobuster vhost -t 100 -u dominio.com --append-domain -w /ruta/diccionario
```

Para buscar archivos:

```bash
gobuster dir -t 150 -w /ruta/diccionario -u http://dominio.com/ -x txt,php,py,sh,html
```

### Dirb

```bash
dirb http://dominio.com/
```

### Dirsearch

```bash
dirsearch -u http://dominio.com/
```

### Ffuf

```bash
ffuf -u http://dominio.com/FUZZ -c -t 150 -w /ruta/diccionario
```

Subdominios:

```bash
ffuf -H  "Host : FUZZ.domino.com" -u https://domino.com -w /Users/tanish/Documents/SecLists-master/Discovery/DNS/bitquark-subdomains-top100000.txt -fs 0,4605
```

## Subdominios

Además de poder enumerar subdominios usando la fuerza bruta, podemos usar una herramienta muy conocida que funciona muy bien para esto, puesto que recopila diferentes fuentes como DNSDumpster, NetCraft...

{% hint style="success" %}
Esta herramienta también puede realizar ataques de fuerza bruta
{% endhint %}

[https://github.com/aboul3la/Sublist3r](https://github.com/aboul3la/Sublist3r)

Este es un ejemplo de escaneo:

```bash
python3 sublist3r.py -d domino.com -v -o subdominos-dominio.txt
```



## WAF (Web Application Firewall)

Podemos detectar el uso de un WAF en una web, tras hacer diferentes escaneos y ver ciertos resultados de error, pero también podemos usar herramientas de detección.

Una de ellas es `wafw00f`:

[https://github.com/EnableSecurity/wafw00f](https://github.com/EnableSecurity/wafw00f)

Este es un ejemplo en el cual la web se encuentra con el WAF de CloudFlare:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

```bash
wafw00f https://dominio.com -a -v
```

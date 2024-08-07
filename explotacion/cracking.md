# 🔬 Cracking

## Archivos ZIP

Cuando nos enfrentamos a un archivo comprimido cifrado, podremos crackearlo usando John The Ripper.

{% hint style="warning" %}
John The Ripper, crackea hashes y no archivos, por lo que deberemos de extraer el hash del archivo comprimido, y este es el que será crackeado por JTR. Para extraer el hash del archivo, ejecutamos este comando:

```bash
zip2john archivo.zip > hash-archivo
```
{% endhint %}

Una vez tengamos el hash del archivo, ejecutamos este comando:

```bash
john --wordlist=/ruta/diccionario hash-archivo
```

## Base de Datos de KeePass

Cuando nos enfrentamos a una base de datos de keepass, podremos crackearla usando John The Ripper.

{% hint style="info" %}
John The Ripper, crackea hashes y no archivos, por lo que deberemos de extraer el hash del archivo comprimido, y este es el que será crackeado por JTR. Para extraer el hash del archivo, ejecutamos este comando:

```
keepass2john archivo.kdbx > hash-archivo
```
{% endhint %}

Una vez tengamos el hash del archivo, ejecutamos este comando:

```bash
john --wordlist=/ruta/diccionario hash-archivo
```

## SSH id\_rsa

En el caso de que tengamos a nuestra disposición el archivo id\_rsa (la clave privada) de usuarios, podemos crackearlo con herramientas con el objetivo de obtener el usuario y contraseña validos para el servicio ssh:

```bash
ssh2john id_rsa > hash-id-rsa
```

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Así luce el hash:

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

Ejecutamos john:

```bash
john hash_idrsa -w=/ruta/diccionario
```

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

Como vemos obtenemos la contraseña, por lo que podremos iniciar sesión con el usuario, el archivo id\_rsa y la contraseña que hemos crackeado:

```perl
ssh -i id_rsa usuario@ip
```

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## Hashes

### John The Ripper

Cuando nos encontramos credenciales en forma de hash, también podemos realizar fuerza bruta para descubrir la contraseña real:

En primer lugar dejamos el hash en un archivo, y después lo crackeamos con este comando:

```bash
john --wordlist=/ruta/diccionario hash-archivo
```

{% hint style="warning" %}
En el caso de que nos de un error y no craquee el hash, es debido a que debemos especificarle el tipo de hash que es.

Para saber el hash al que nos enfrentamos, acudimos a "Hash Identifier" (herramienta de kali) y le pasamos el hash:

![](<../.gitbook/assets/image (20).png>)

Ahora, ejecutamos este comando, donde MD5 será el tipo de hash:

```bash
john --format=Raw-MD5 --wordlist=/ruta/diccionario hash-archivo
```
{% endhint %}

### Hashcat

Otra forma de craquear un hash es con hashcat, para ello empleamos el siguiente comando:

```bash
hashcat -m 0 -a 0 hash.txt /ruta/diccionario
```

En el parámetro `-m`, deberemos indicar el tipo de hash que queremos craquear, cada hash tiene un numero asignado, (por lo que lo podemos buscar en [https://hashcat.net/wiki/doku.php?id=example\_hashes](https://hashcat.net/wiki/doku.php?id=example\_hashes) o ejecutando `hashcat -h`) y en el parámetro `-a` indicar el tipo de ataque:

```
- [ Attack Modes ] -

  # | Mode
 ===+======
  0 | Straight
  1 | Combination
  3 | Brute-force
  6 | Hybrid Wordlist + Mask
  7 | Hybrid Mask + Wordlist
  9 | Association
```

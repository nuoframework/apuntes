# Terminal

Después de realizar una intrusión y tener acceso a una terminal victima, se recomienda aplicar un tratamiento a la terminal, para ello, ejecutaremos los siguientes comandos:

```bash
script /dev/null -c bash        

# Otra opción es hacer este 👆 comando con python
# python3 -c "import pty;pty.spawn('/bin/bash')"

CTRL + Z

stty raw -echo; fg

reset xterm

export TERM=xterm

stty rows 44 columns 184
```

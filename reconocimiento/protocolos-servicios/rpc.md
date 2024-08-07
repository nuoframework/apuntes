# RPC

## Enumeración de usuarios

Para enumerar sin credenciales y haciendo uso de una Null Session, ejecutamos el siguiente comando:

```bash
rpcclient -U "" -N 192.168.1.12
```

Si conseguimos entrar y obtener un prompt, podremos ejecutar los siguientes comandos:

* `srvinfo`  -> Obtiene información del sistema operativo
* `querydispinfo`  -> Enumera usuarios validos
* `enumdomusers`  -> Enumera usuarios validos
* `lookupnames username` -> Enumera el SID completo de un usuario (es necesario especificar el usuario)

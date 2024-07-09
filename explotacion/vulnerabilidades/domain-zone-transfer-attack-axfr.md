# Domain Zone Transfer Attack (AXFR)

## ¿Que son los Ataques de Transferencia de Zona?

Los Ataques de Transferencia de Zona o AXFR, son ataques que se realizan a los DNS que permiten al atacante obtener información sensible sobre los dominios de una entidad.

## ¿Cómo funciona?

El ataque AXFR se lleva a cabo enviando una solicitud de transferencia de zona desde un servidor DNS falso a un servidor DNS legítimo. Esta solicitud se realiza utilizando el protocolo de transferencia de zona DNS (AXFR), que es utilizado por los servidores DNS para transferir registros DNS de un servidor a otro.

## ¿Cómo se realiza este ataque?

Para poder hacer un ataque de transferencia de zona completa, podemos usar la herramienta `dig`:

```sh
dig @IP-SERVER DOMAIN AXFR
```

Ej:

```sh
dig @127.0.0.1 domain.com AXFR 
```

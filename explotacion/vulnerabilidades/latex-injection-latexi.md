# LaTeX Injection (LaTeXi)

## ¿Que es una Inyección LaTeX?

Esta es una vulnerabilidad que ocurre en aplicaciones utilizan LaTeX para generar documentos. Esta vulnerabilidad permite insertar código LaTeX malicioso en dichos documentos que se ejecuta cuando este se compila (se genera el documento)

## ¿Cómo funciona?

Pues en estos casos, al atacante se le permite insertar texto para generar dicho documento, pues si el código no esta correctamente sanitizado, este podría permitir cierto código LaTeX.

Si se da este escenario, el atacante podría usar código como este para leer archivos:

```latex
\input{/etc/passwd}
```

O estos para ejecutar comandos:

```latex
\immediate\write18{whoami}
```

```latex
\pdfshellescape{ls}
```

{% hint style="warning" %}
Para ejecutar comandos en la maquina, es necesario que una función propietaria de LaTeX (write18) esté habilitada, ya que es la que permite ejecutar código.

Esto se puede ver desde la maquina victima dependiendo el modo que esté activado
{% endhint %}

{% hint style="info" %}
Enlaces de Interés:

1. [https://book.hacktricks.xyz/pentesting-web/formula-doc-latex-injection](https://book.hacktricks.xyz/pentesting-web/formula-doc-latex-injection)
2. [https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/LaTeX%20Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/LaTeX%20Injection)
3. [https://www.elladodelmal.com/2022/08/ataques-de-hacking-contra-compiladores.html](https://www.elladodelmal.com/2022/08/ataques-de-hacking-contra-compiladores.html)
{% endhint %}

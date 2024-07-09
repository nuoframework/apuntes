---
description: En esta página aprenderás a enumerar el CMS Joomla
cover: >-
  https://images.unsplash.com/photo-1650951478024-a701c28d5a9b?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxqb29tbGF8ZW58MHx8fHwxNjg5MzMyMjM1fDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Enumeración Joomla

## Enumeración de la Versión

Para saber que version de Joomla esta corriendo, lo podemos ver mediante la herramienta WhatWeb o mediante la extensión Wappalyzer entre otras

{% hint style="info" %}
Para usar Wappalyzer debes de instalar la extensión:\
\
[Navegadores basados en Chromium](https://chrome.google.com/webstore/detail/wappalyzer-technology-pro/gppongmhjkpfnbhagpmjfkannfbllamg?hl=es)

[Navegadores basados en Mozilla Firefox](https://addons.mozilla.org/es/firefox/addon/wappalyzer/?utm\_source=addons.mozilla.org\&utm\_medium=referral\&utm\_content=search)
{% endhint %}

### WhatWeb

Para identificar que CMS corre en la web, lo podemos hacer mediante este comando:

```
whatweb http://127.0.0.1
```

{% hint style="warning" %}
No siempre es posible ver la versión de los CMS
{% endhint %}

## Escaneo y recolección de información

### JoomScan

Esta es una herramienta que actua como escaner y que nos permite analizar webs que estén montadas con Joomla.

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

```
perl joomscan.pl -u https://website.com
```

Esta herramienta es capaz de detectar y enumerar componentes vulnerables de Joomla, es capaz de detectar si se está implementando firewall, enumera vulnerabilidades...

{% hint style="info" %}
Puedes descargar la herramienta desde su repositorio oficial: [https://github.com/OWASP/joomscan](https://github.com/OWASP/joomscan)
{% endhint %}

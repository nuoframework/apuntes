---
description: En esta página aprenderás a enumerar el CMS Magento
cover: >-
  https://images.unsplash.com/photo-1652156752342-c786f1e1d1ce?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxtYWdlbnRvfGVufDB8fHx8MTY4OTMzODk0Mnww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Enumeración Magento

## Enumeración de la Versión

Para saber que version de Magento esta corriendo, lo podemos ver mediante la herramienta WhatWeb o mediante la extensión Wappalyzer entre otras

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

### Magescan

Esta herramienta nos permite escanear el CMS y buscar en el: vulnerabilidades comunes en Magento, incluyendo problemas con permisos de archivos, errores de configuración y vulnerabilidades conocidas en extensiones populares de Magento.

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

```
php magescan.phar scan:all https://website.com
```

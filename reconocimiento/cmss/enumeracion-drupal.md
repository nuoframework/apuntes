---
description: En esta página aprenderás a enumerar el CMS Drupal
cover: >-
  https://images.unsplash.com/photo-1650951476924-5842963e3249?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxkcnVwYWx8ZW58MHx8fHwxNjg5MzM2MjYxfDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Enumeración Drupal

## Enumeración de la Versión

Para saber que version de Drupal esta corriendo, lo podemos ver mediante la herramienta WhatWeb o mediante la extensión Wappalyzer entre otras

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

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
No siempre es posible ver la versión de los CMS
{% endhint %}

## Escaneo y recolección de información

### Droopescan

Esta herramienta nos permite escanear el CMS

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

```
droopescan scan drupal --url https://website.com
```

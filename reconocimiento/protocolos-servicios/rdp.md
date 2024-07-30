# RDP

{% hint style="info" %}
Para acceder a un host por RPD con credenciales, usaremos la herramienta `xfreerdp` usaremos el siguiente comando:

```bash
xfreerdp /u:usuario /p:contraseña /v:192.168.1.12:3389
```
{% endhint %}

## Enumeración del servicio

Podemos comprobar si RDP se ejecuta en un puerto en especifico diferente al habitual mediante el siguiente modulo de metasploit:

<pre class="language-perl"><code class="lang-perl">msfconsole
<strong>
</strong><strong>use auxiliary/scanner/rdp/rdp_scanner
</strong>
set RHOSTS 192.68.1.1

set RPORT 3333

exploit
</code></pre>

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

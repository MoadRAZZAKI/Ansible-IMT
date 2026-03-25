# Challenge atelier 06 : Configuration de base

On commence tout d'abord par le démarrage des VMs à l’aide de la commande :

```bash
vagrant up
```

<img width="561" height="95" alt="conf1" src="https://github.com/user-attachments/assets/4a7f2cf2-8ee8-42cf-a61f-26ba2c4a9cf6" />

Après le démarrage, nous avons vérifié l’état des machines avec la commande:

```bash
vagrant status
```

Toutes les machines sont en état running comme montre la figure ci-dessous.

<img width="662" height="354" alt="conf2" src="https://github.com/user-attachments/assets/bdb09302-c628-4e3c-90aa-6d19069d2ea8" />

On se connecte par la suite à notre VM control et on midifie fichier ```/etc/hosts``` afin de permettre la résolution des noms des machines cibles.

<img width="645" height="131" alt="conf3" src="https://github.com/user-attachments/assets/93bbda28-0ae5-44da-81ea-959ca9f6ae66" />

on récupère ensuite les clés publiques des machines target avec la commande ```bash ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts``` et on génère une paire des clés SSH

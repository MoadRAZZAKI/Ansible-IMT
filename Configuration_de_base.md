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

on récupère ensuite les clés publiques des machines target avec la commande ```bash ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts``` et on génère une paire des clés SSH. Après on a copié cette clé sur chaque machine cible avec la commande : 

```bash
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```

La figure ci-dessous montre le processus complet : 

<img width="915" height="665" alt="conf4" src="https://github.com/user-attachments/assets/1c60d59d-9f84-4fcb-aadd-4e898d55ed85" />

Une fois que la configartion SSH est accomplie, on passe à l'installation du package ansible avec la commande : 

```bash
sudo apt install -y ansible
```

<img width="473" height="96" alt="conf5" src="https://github.com/user-attachments/assets/2d3ad0a3-4914-4981-b855-c927647751dd" />




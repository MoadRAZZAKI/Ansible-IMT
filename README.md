# Ansible-IMT


## Prise en main et premier test

Pour tester le bon fonctionnement de Vagrant et de VirtualBox, on va lancer un premier cluster de quatre machines virtuelles Alpine Linux :

![alt text](image.png)


## Une box pour chaque distribution

On fait une box pour chaque distribution, on récupère les box Vagrant des quatre principales distributions que l'on peut être emmener à utiliser dans un contexte professionnel

![alt text](image-1.png)

## Installer Ansible sur le Control Host

Sur la machine virtuelle Rocky Linux, nous commençons par démarrer la VM : 

<img width="877" height="241" alt="image" src="https://github.com/user-attachments/assets/5c270f68-2e7f-4ab0-8497-6c8476c15415" />


Puis nous nous y connectons à l’aide de la commande vagrant ssh rocky : 


<img width="764" height="117" alt="image" src="https://github.com/user-attachments/assets/17bb1abd-6532-47e5-a3c7-7fe3a98eb37b" />



Par défaut, seuls les dépôts officiels de la distribution sont activés, ce que l’on peut vérifier avec la commande dnf repolist :

<img width="726" height="96" alt="image" src="https://github.com/user-attachments/assets/24488b26-2bb4-4739-be3a-a0023d94186a" />




Afin de pouvoir installer certains logiciels supplémentaires comme Ansible, nous ajoutons le dépôt tiers EPEL en installant le paquet epel-release avec sudo dnf install -y epel-release. L’utilisation de ce dépôt nécessite également l’activation du dépôt officiel Code Ready Builder , ce qui est fait avec la commande sudo crb enable.

<img width="956" height="274" alt="image" src="https://github.com/user-attachments/assets/fc48eb28-40ac-429f-8bbe-a4e356f5525b" />



Une fois ces dépôts configurés, Ansible peut être installé à l’aide de sudo dnf install -y ansible. 

<img width="963" height="175" alt="image" src="https://github.com/user-attachments/assets/c18c96af-4362-4bad-b706-2acee59be254" />


Enfin, nous vérifions que l’installation s’est correctement déroulée en affichant la version du logiciel avec ansible --version.

<img width="956" height="223" alt="image" src="https://github.com/user-attachments/assets/889457f2-b5c5-40c3-948c-0605b7b2f352" />





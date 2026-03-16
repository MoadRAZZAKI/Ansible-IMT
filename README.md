# Ansible-IMT


## Prise en main et premier test

Pour tester le bon fonctionnement de Vagrant et de VirtualBox, on va lancer un premier cluster de quatre machines virtuelles Alpine Linux :

![alt text](image.png)


## Une box pour chaque distribution

On fait une box pour chaque distribution, on récupère les box Vagrant des quatre principales distributions que l'on peut être emmener à utiliser dans un contexte professionnel

![alt text](image-1.png)

## Installer Ansible sur le Control Host

### Rocky Linux

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

ensuite , on quitte et on supprime la vm : 

<img width="547" height="102" alt="image" src="https://github.com/user-attachments/assets/e18873ed-4c4b-4abe-bf25-aa9fb8fff7e0" />


### Challenge 1 : 


On démarre la VM ubuntu depuis le répertoire atelier-01 : 


<img width="841" height="251" alt="image" src="https://github.com/user-attachments/assets/44d1a268-4681-4b4c-aef2-9a492078d737" />


Ensuite, on se connecte à la VM avec un ssh : 

<img width="720" height="343" alt="image" src="https://github.com/user-attachments/assets/fda28ca9-f5a4-4cf5-bc6b-64f6f630d6c9" />

Une fois connecté à la VM , on rafraichit les informations sur les paquets :

<img width="874" height="440" alt="image" src="https://github.com/user-attachments/assets/0998fe12-4ecb-456f-b1c9-6f99ac87469a" />

On cherche le paquet ansible :

<img width="870" height="172" alt="image" src="https://github.com/user-attachments/assets/01ce2b60-ba66-449f-90f8-43eb8083d804" />


il faut ensuite installer le paquet officiel :

<img width="919" height="282" alt="image" src="https://github.com/user-attachments/assets/af4bdf60-bb7a-4c89-b50b-2c6ef28140ae" />


Pour enfin verfier la version , on utilise la commande suivante : 


<img width="957" height="177" alt="image" src="https://github.com/user-attachments/assets/7bd3fb8e-48ec-4bb6-9b6d-5aea41597cf4" />


 
On se déconnecte de la vm et on la supprime : 

<img width="698" height="167" alt="image" src="https://github.com/user-attachments/assets/0ebf220c-7aea-4515-8dad-fd7a65011a69" />



### Challenge 2 : 



On démarre la VM ubuntu depuis le répertoire atelier-01 : 


<img width="841" height="251" alt="image" src="https://github.com/user-attachments/assets/44d1a268-4681-4b4c-aef2-9a492078d737" />


Ensuite, on se connecte à la VM avec un ssh : 

<img width="720" height="343" alt="image" src="https://github.com/user-attachments/assets/fda28ca9-f5a4-4cf5-bc6b-64f6f630d6c9" />

Une fois connecté à la VM , on rafraichit les informations sur les paquets :

<img width="874" height="440" alt="image" src="https://github.com/user-attachments/assets/0998fe12-4ecb-456f-b1c9-6f99ac87469a" />

cette fois , on ajoute le dépôt PPA Ansible : 

<img width="954" height="333" alt="image" src="https://github.com/user-attachments/assets/9e67e712-f9ca-41e3-8040-10b1efb3d62b" />


On rafraichit à nouveau la liste des paquets : 

<img width="755" height="173" alt="image" src="https://github.com/user-attachments/assets/8dd3439d-3ee5-4b4b-bbdc-be16f54733af" />

On installe Ansible depuis le dépôt PPA


<img width="957" height="287" alt="image" src="https://github.com/user-attachments/assets/1f83544f-cfd1-4f9e-ab80-439299bba5d6" />



On vérifie l'installation et on affiche la version

<img width="959" height="189" alt="image" src="https://github.com/user-attachments/assets/13fa78c1-8e79-4b50-b654-513b3740dcb8" />


et enfin on se déconnecte de la vm et on la supprime : 

<img width="698" height="167" alt="image" src="https://github.com/user-attachments/assets/0ebf220c-7aea-4515-8dad-fd7a65011a69" />

### Challenge 3 : 

Depuis le répertoire de l’atelier, on démarre la VM : 


![alt text](image-2.png)


on se connecte à la VM :

![alt text](image-3.png)


Ensuite , on vérifie la version de python , et on installe le gestionnaire de paquets python : 


![alt text](image-4.png)


![alt text](image-5.png)


Rocky Linux possède déjà le module venv intégré à Python.

Donc, on crée l’environnement virtuel, et on l'active :

![alt text](image-6.png)


Le terminal change :


![alt text](image-7.png)


Après avoir installé ansible, on vérifie l'installation :

![alt text](image-8.png)

## Authentification sur les Target Hosts



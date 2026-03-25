# Challenge Atelier 14 : Les variables


L’objectif de cet atelier est de configurer la communication entre un nœud de contrôle (control) et plusieurs machines cibles (target01, target02, target03) à l’aide d’Ansible, puis de vérifier cette communication avec le module ping.

On commence tout d'abord par le démarrage des VMs à l’aide de la commande :

```bash
vagrant up
```

<img width="629" height="94" alt="var1" src="https://github.com/user-attachments/assets/bf631a15-2856-4440-be01-5ae6ca8af2ac" />

Après le démarrage, nous avons vérifié l’état des machines avec la commande:

```bash
vagrant status
```

Toutes les machines sont en état running comme montre la capture.

<img width="658" height="220" alt="var2" src="https://github.com/user-attachments/assets/8071400d-b047-471f-a326-245660c4d7c8" />

On se connecte par la suite à notre VM Ansible et on accède au répertoire de notre atelier :

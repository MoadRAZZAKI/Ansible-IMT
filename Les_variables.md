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

<img width="769" height="129" alt="var3" src="https://github.com/user-attachments/assets/66a1e660-a82a-4c1e-8a20-54f8acc901dd" />

On commence tout d'abord par créer notre premier playbook qui affiche le contenu des variables ```mycar``` et ```mybike``` en mode ```debug```. Voici le contenu :

```bash
---  # myvars1.yml

- hosts: localhost
  gather_facts: false

  vars:
    mycar: Audi
    mybike: S1000R

  tasks:
    - debug:
        msg: " My car is: {{mycar}}, My Bike is: {{mybike}}"
```

On peut remarquer dans la figure ci-dessous que notre playbook a été bien éxecuté :

<img width="904" height="249" alt="var4" src="https://github.com/user-attachments/assets/1d780447-a8d2-49c9-8822-6ef0c56c7b22" />

Par la suite, voici le résultat du remplacement des variables en uilisant l'option ```-e``` : 

<img width="901" height="728" alt="vars5" src="https://github.com/user-attachments/assets/9d6ad4eb-30c0-49df-b154-2513edc7d259" />

Après on passe à notre deuxième playbook ```myvars2.yml``` qui fait essentiellement la même chose que ```myvars1.yml``` qui utilise le module ```set_fact```. Voici le playbook:

```bash
---  # myvars2.yml

- hosts: localhost
  gather_facts: false


  tasks:
    - name: Setting variables
      set_fact:
        mycar: Dodge
        mybike: Z900

    - debug:
	msg: " My car is: {{mycar}}, My Bike is: {{mybike}}"
```

Voici ensuite le résultat de l'exécution avec les extra vars.

<img width="901" height="903" alt="vars4" src="https://github.com/user-attachments/assets/8e989144-97b9-431a-a69f-12c97501314d" />

Pour la partie suivante, afin d'afficher le contenu des deux variables ```mycar``` et ```mybike``` mais sans les définir on commence par créer notre répertoire group_vars. Ensuite on a élaboré le fichier ```all.yml``` qui contient nos variables et leurs valeurs respectives.

<img width="661" height="43" alt="vars6" src="https://github.com/user-attachments/assets/7e3c4fe5-974a-4e24-99f0-2330eb2c644d" />

Voici le fichier ```all.yml``` et notre playbook

```bash
---  # group_vars/all.yml


mycar: VW
mybike: BMW
```

```bash
---  # myvars3.yml

- hosts: all      
  gather_facts: false

  tasks:
    - name: Display variables
      debug:
	msg: " My car is: {{mycar}}, My Bike is: {{mybike}}"
```

Voici le résultat de l'exécution : 

<img width="903" height="560" alt="vars8" src="https://github.com/user-attachments/assets/3b26b5f4-30d5-4a07-916b-d4403b0dbf9a" />

Ensuite, afin de remplacer VW et BMW par Mercedes et Honda sur l'hôte ```target02```, on commence par créer le répertoire ```host_vars``` et on créé notre fichier ```target02.yml```. 
Voici son contenu:

```bash
---  # group_vars/all.yml

mycar: Mercedes
mybike: Honda
```

Voici le résultat de l'éxecution de notre playbook : 

<img width="905" height="446" alt="vars8" src="https://github.com/user-attachments/assets/0b289b2c-2d9d-4835-b2da-af48f0562279" />




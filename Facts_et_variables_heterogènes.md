# Challenge atelier 16 : Facts et variables implicites
On commence tout d'abord par le démarrage des VMs à l’aide de la commande :

```bash
vagrant up
```
<img width="804" height="135" alt="facts1" src="https://github.com/user-attachments/assets/11f6853f-7700-4f3c-b351-a1ecab1b5074" />


Après le démarrage, nous avons vérifié l’état des machines avec la commande:

```bash
vagrant status
```

Toutes les machines sont en état running comme montre la capture.
<img width="903" height="199" alt="facts2" src="https://github.com/user-attachments/assets/01d563e3-703c-4a55-b354-8253a0c079ab" />

On se connecte par la suite à notre VM Ansible et on accède au répertoire de notre atelier :

<img width="907" height="149" alt="facts3" src="https://github.com/user-attachments/assets/272867fd-b937-42b2-839d-5c21ebe81953" />

Nous avons utilisé le module setup pour afficher les facts, qui sont des informations système automatiquement collectées par Ansible.
<img width="903" height="274" alt="facts4" src="https://github.com/user-attachments/assets/2b58800c-e4a8-4008-8a39-df5d0ba6e8af" />

Au début on a eu une erreur de dépassement de caractères dans le playbook à cause du longueur de notre message. Pour respecter les bonnes pratiques YAML, nous avons utilisé une chaîne multilignes avec ```>-```, ce qui améliore la lisibilité sans modifier le résultat.

<img width="904" height="306" alt="facts5" src="https://github.com/user-attachments/assets/a02b29af-8fc4-4e60-8a09-d08bc3d810fe" />


Voici la version finale de notre premier playbook:

```yaml
---  # pkg-info.yml

- hosts: all

  tasks:
    - name: Display package manager
      debug:
        msg: >-
          Le host {{ inventory_hostname }}
          (OS: {{ ansible_distribution }})
          utilise : {{ ansible_pkg_mgr }}
```

voici le résultat de l'éxecution de notre premier playbook.

<img width="910" height="534" alt="facts6" src="https://github.com/user-attachments/assets/80017a6d-79bd-4d7f-a058-f6d97ecc0c23" />

Par la suite, afin d'afficher la version de Python installée sur nos targets, on a élaboré ce playbook qui utilise le fact ```{{ansible_python_version}}```.

```yaml
---  # python-info.yml

- hosts: all

  tasks:
    - name: Display Python version
      debug:
	msg: "Host {{inventory_hostname}} runs Python version {{ansible_python_version}}."
```

Voici le résultat de l'éxecution du deuxième playbook.

<img width="903" height="534" alt="facts7" src="https://github.com/user-attachments/assets/6a1cbfb9-7af7-4970-a894-99d9faa630e5" />

Après, on a élaboré notre dérnier playbook ```dns-info.yml``` pour afficher le(s) serveur(s) DNS utilisé(s) en utilisant le fact ```ansible_dns.nameservers```:

```yaml
---  # dns-info.yml

- hosts: all

  tasks:
    - name: Display DNS servers
      debug:
        msg: >-
          Host {{inventory_hostname}}
          uses DNS servers:
          {{ansible_dns.nameservers}}
```
Voici le résultat d'éxecution du playbook et la vérification du bon fonctionnement avec le module ```setup```.

<img width="985" height="502" alt="facts8" src="https://github.com/user-attachments/assets/3d3579de-0dd8-439e-ae36-b57740fcba5e" />
<img width="990" height="561" alt="facts9" src="https://github.com/user-attachments/assets/d8e5038b-b589-417b-aef6-643e17b15eac" />


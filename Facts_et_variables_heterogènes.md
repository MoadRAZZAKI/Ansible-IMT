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

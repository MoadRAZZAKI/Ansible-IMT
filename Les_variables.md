# Challenge atelier 10 : Un serveur web simple

On commence tout d'abord par le démarrage des VMs à l’aide de la commande :

```bash
vagrant up
```
<img width="583" height="95" alt="hand1" src="https://github.com/user-attachments/assets/72c4b94b-4236-4d57-b142-6e9e8738b016" />

Après le démarrage, nous avons vérifié l’état des machines avec la commande:

```bash
vagrant status
```
Toutes les machines sont en état running comme montre la capture.

<img width="697" height="204" alt="hand2" src="https://github.com/user-attachments/assets/ccc693f8-96c4-4a31-9259-c4eb4744e84e" />

On se connecte par la suite à notre VM Ansible et on accède au répertoire de notre atelier :

<img width="779" height="204" alt="hand3" src="https://github.com/user-attachments/assets/105b270d-01cc-431c-94cb-a4898c67d205" />

Par la suite, on passe à la création de notre playbook. Au premier moment, le playbook sert juste à l'installation du paquet ```chrony``` et l'activation et le démarrage du service ```chronyd``` afin de jeter un oeil sur la configuration de base.

```yaml
---  # apache-rocky.yml

- hosts: redhat

  tasks:

    - name: Install Chrony
      dnf:
        name: chrony

    - name: Start & enable Chrony
      service:
        name: chronyd
        state: started
        enabled: true
```


Voici le résultat de l'éxecution du playbook : 

<img width="1044" height="546" alt="hand5" src="https://github.com/user-attachments/assets/25b4d8f8-3f51-4058-8a88-e966b8b56e75" />

Voici un bout du fichier de configuration de base de chrony :

<img width="662" height="491" alt="hand4" src="https://github.com/user-attachments/assets/c0dcceec-3ef6-4661-86c1-64b6e4cff83b" />

Ensuite, on passe à l'ajout des deux nouvelles tâches qui servent à charger notre nouvelle configuration désirée dans le fichier ```/etc/chrony.conf``` et redémarrer les service ```chronyd``` afin de charger cette dernière. Voici la version finale de notre playbook:


```yaml
--  # apache-rocky.yml

- hosts: redhat

  tasks:

    - name: Install Chrony
      dnf:
	name: chrony

    - name: Start & enable Chrony
      service:
	name: chronyd
        state: started
        enabled: true

    - name: Chrony custom config
      copy:
	dest: /etc/chrony.conf
        content: |
          # /etc/chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony

    - name: Restart Chrony
      service:
	name: chronyd
        state: restarted

```

On peut remarquer que la syntaxe est bien correcte et notre playbook a été bien exécuté comme montre la figure ci-dessous :

<img width="903" height="814" alt="hand6" src="https://github.com/user-attachments/assets/8bd579c6-6f0e-42be-8a26-a6ccca960205" />

Notre idempotence est bien assuré aussi après une autre éxecution comme montre la figure ci-dessous. 


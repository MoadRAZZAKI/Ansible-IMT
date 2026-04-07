# Challenge atelier 18 : Jinja & Templates
On commence tout d'abord par le démarrage des VMs à l’aide de la commande :

```bash
vagrant up
```
<img width="602" height="146" alt="jinja1" src="https://github.com/user-attachments/assets/1143f93e-8def-4a87-8ad7-c2b6fd73c551" />

Après le démarrage, nous avons vérifié l’état des machines avec la commande:

```bash
vagrant status
```
Toutes les machines sont en état running comme montre la figure ci-dessous:

<img width="987" height="236" alt="jinja2" src="https://github.com/user-attachments/assets/048960d9-8d37-42c0-8a32-08c3d54a9dc4" />

On se connecte par la suite à notre VM Ansible et on accède au répertoire de notre atelier :

<img width="984" height="199" alt="jinja3" src="https://github.com/user-attachments/assets/015060a7-44e0-4b61-8162-0dce17f6faf0" />

Une fois que nos machines sont dérmarrées et fonctionnelles, on commence par créer notre fichier de template jinja2 afin de générer dynamiquement le fichier de configration ```chrony.conf``` sur les targets.
Voici le fichier ```chrony.conf.j2``` situé dans le répertoire ```templates/```:

```jinja
{# templates/chrony.conf.j2 #}

# {{ chrony_conf_path }}

server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```

Une fois notre fichier de template est élaboré, on est passé à la création de notre playbook ```chrony.yaml``` qui permet de configurer automatiquement Chrony sur plusieurs machines. Il adapte le chemin du fichier et le service selon le système grâce à ```set_fact```, déploie la configuration via notre template ```Jinja2,``` puis redémarre le service uniquement si le fichier est modifié grâce à un ```handler```.
Voici le contenu de notre playbook : 

```yaml
---  # chrony.yml

- hosts: all

  tasks:

    - name: Set Chrony parameters for Debian/Ubuntu
      set_fact:
        chrony_package: chrony
        chrony_service: chrony
        chrony_conf_path: /etc/chrony/chrony.conf
      when: ansible_os_family == "Debian"

    - name: Set Chrony parameters for Rocky Linux
      set_fact:
        chrony_package: chrony
        chrony_service: chronyd
        chrony_conf_path: /etc/chrony.conf
      when: ansible_distribution == "Rocky"

    - name: Set Chrony parameters for SUSE Linux
      set_fact:
        chrony_package: chrony
        chrony_service: chronyd
        chrony_conf_path: /etc/chrony.conf
      when: ansible_distribution == "openSUSE Leap"

    - name: Install Chrony
      package:
        name: "{{ chrony_package }}"
        state: present

    - name: Deploy chrony configuration (template)
      template:
        src: chrony.conf.j2
        dest: "{{ chrony_conf_path }}"
        mode: '0644'
      notify: Restart Chrony

    - name: Start & enable Chrony
      service:
        name: "{{ chrony_service }}"
        state: started
        enabled: true

  handlers:

    - name: Restart Chrony
      service:
        name: "{{ chrony_service }}"
        state: restarted
```


Voici le résultat de l'éxecution et la vérification de l'existence du fichiers de configuration sur nos targets:
<img width="1106" height="986" alt="jinja4" src="https://github.com/user-attachments/assets/c44a349f-a247-49ad-8bcf-780bcbdeed9a" />
<img width="665" height="985" alt="jinja5" src="https://github.com/user-attachments/assets/8759b2fe-b769-4c06-99f1-1e640f6a0505" />




Une fois les tests terminés et les résultats validés, on proçède au nettoyage de notre environnement de travail afin de libérer les ressources.
<img width="624" height="203" alt="jinja6" src="https://github.com/user-attachments/assets/0fcbdac9-a9b8-4107-a0de-ee4f61e043a8" />

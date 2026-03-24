# Challenge atelier 10 : Un serveur web simple
On commence tout d'abord par le démarrage des VMs à l’aide de la commande :

```bash
vagrant up
```

<img width="899" height="450" alt="Capture d’écran du 2026-03-24 11-47-30" src="https://github.com/user-attachments/assets/9b447456-8395-4e32-bb97-788ae64b8079" />

Après le démarrage, nous avons vérifié l’état des machines avec la commande:

```bash
vagrant status
```

Toutes les machines sont en état running comme montre la capture.
<img width="659" height="198" alt="status" src="https://github.com/user-attachments/assets/912a33f3-0b41-4131-b074-3715c5a9cbb6" />

On se connecte par la suite à notre VM Ansible et on accède au répertoire de notre atelier :

<img width="708" height="270" alt="Capture d’écran du 2026-03-24 11-58-41" src="https://github.com/user-attachments/assets/b184bccf-935d-46ce-98b5-70b3a52994f3" />







Dans le répoertoire playbooks, on créé les playbooks respectifs à chaque distribution:

## Debian

 - Ce playbook permet d’installer et configurer un serveur Apache sur une machine Debian.

 - Le playbook dédié à Debian commence par mettre à jour la liste des paquets avec apt, puis installe le serveur Apache. Il démarre ensuite le service et l’active au démarrage. Enfin, une page web personnalisée est déployée dans le répertoire /var/www/html comme demandé dans le challenge.


```yaml

---  # apache-debian.yml

- hosts: debian

  tasks:

    - name: Update package information
      apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install Apache
      apt:
	name: apache2

    - name: Start & enable Apache
      service:
	name: apache2
        state: started
        enabled: true

    - name: Install custom web page
      copy:
	dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Apache on Debian</title>
            </head>
            <body>
              <h1>Apache web server running on Debian Linux</h1>
            </body>
          </html>

...

```

 - Ci-dessous, des captures d'écran montrant l’exécution du playbook et le bon fonctionnement du serveur web sur Debian :

<img width="918" height="577" alt="apache-debian2" src="https://github.com/user-attachments/assets/673c852e-0f09-452e-b5c9-b56572fb2f98" />
<img width="945" height="197" alt="apache-debian3" src="https://github.com/user-attachments/assets/876602f4-1c91-4bd0-86d6-04b8c1c1ca16" />

## Rocky Linux

 - Ce playbook configure un serveur Apache sur une machine Rocky Linux.

 - Il utilise le gestionnaire de paquets dnf pour installer Apache, appelé ici httpd. Une fois installé, le service est démarré et activé. Une page HTML personnalisée est ensuite copiée dans le dossier /var/www/html, ce qui permet de tester facilement que le serveur web fonctionne correctement.

```yaml
---  # apache-rocky.yml

- hosts: rocky

  tasks:

    - name: Install Apache
      dnf:
	name: httpd

    - name: Start & enable Apache
      service:
	name: httpd
        state: started
        enabled: true

    - name: Install custom web page
      copy:
	dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Apache on Rocky</title>
            </head>
            <body>
              <h1>Apache web server running on Rocky Linux</h1>
            </body>
          </html>

...
```

 - Ci-dessous, des captures d'écran montrant l’exécution du playbook et le bon fonctionnement du serveur web sur Rocky :
<img width="905" height="350" alt="suse1" src="https://github.com/user-attachments/assets/dcecc3eb-d35a-433c-9737-9ec567f29761" />
<img width="790" height="233" alt="suse2" src="https://github.com/user-attachments/assets/59781bff-7934-4b07-9865-e616a9dab5dd" />



## SUSE Linux

 - Ce playbook permet de déployer Apache sur une machine SUSE Linux.

 - L’installation se fait via le gestionnaire zypper, avec le paquet apache2. Le service est ensuite démarré et activé pour être lancé automatiquement. Ensuite une page web personnalisée est ajoutée dans le répertoire /srv/www/htdocs, qui correspond au dossier par défaut pour les fichiers web sur SUSE.
   
```yaml

---  # apache-suse.yml

- hosts: suse

  tasks:

    - name: Install Apache
      zypper:
	name: apache2

    - name: Start & enable Apache
      service:
	name: apache2
        state: started
        enabled: true

    - name: Install custom web page
      copy:
	dest: /srv/www/htdocs/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Apache on SUSE</title>
            </head>
            <body>
              <h1>Apache web server running on SUSE Linux</h1>
            </body>
          </html>

...

```

 - Ci-dessous, des captures d'écran montrant l’exécution du playbook et le bon fonctionnement du serveur web sur SUSE :

<img width="905" height="350" alt="suse1" src="https://github.com/user-attachments/assets/2cc72d55-68a7-4ab2-b4ff-e89b95459699" />
<img width="790" height="233" alt="suse2" src="https://github.com/user-attachments/assets/84157d56-df2d-440a-a01a-2ad8e706c0c1" />

Une fois les tests terminés et les résultats validés, on proçède au nettoyage de notre environnement de travail afin de libérer les ressources.

```bash
vagrant destroy -f
```

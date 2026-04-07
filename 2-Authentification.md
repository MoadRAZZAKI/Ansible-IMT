# Challenge Atelier 03 : Authentification

L’objectif de cet atelier est de configurer la communication entre un nœud de contrôle (control) et plusieurs machines cibles (target01, target02, target03) à l’aide d’Ansible, puis de vérifier cette communication avec le module ping.

On commence tout d'abord par le démarrage des VMs à l’aide de la commande :

```bash
vagrant up
```

<img width="848" height="148" alt="AUTH1" src="https://github.com/user-attachments/assets/8a86ce5a-4cc4-418b-9b1e-d612253033c1" />

Après le démarrage, nous avons vérifié l’état des machines avec la commande:

```bash
vagrant status
```

Toutes les machines sont en état running comme montre la capture.

<img width="670" height="197" alt="Auth2" src="https://github.com/user-attachments/assets/9ffc9eaa-c948-46b5-b1ec-6fa87f0c3424" />

Nous nous connectons ensuite à notre machine virtuelle "control" et vérifions l'installation d'Ansible.

<img width="717" height="467" alt="AUTH3" src="https://github.com/user-attachments/assets/28981580-5d06-4c0f-ad78-cb783c8bd1e5" />

On passe après à l'ajout de nos machines "target" dans le fichier ```bash /etc/hosts``` afin de faire la résolution DNS.

<img width="572" height="169" alt="auth4" src="https://github.com/user-attachments/assets/4902f54b-69e2-43f3-b346-99aaef841793" />

Nous avons vérifié la connectivité entre les machines avec la commande :

```bash 
for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
```

Les résultats montrent que toutes les machines sont joignables.
<img width="805" height="305" alt="auth5" src="https://github.com/user-attachments/assets/aa34a072-2a7d-48a9-9a3b-23a7addceced" />

Les clés SSH des machines cibles ont été récupérées avec :

```bash 
ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
```
<img width="803" height="97" alt="auth6" src="https://github.com/user-attachments/assets/e30324b4-4cc9-4bdd-8629-d12825744cee" />

Ensuite, une paire de clés SSH a été générée sur le Control Host comme montre la figure ci-dessous :

<img width="707" height="378" alt="auth7" src="https://github.com/user-attachments/assets/94bf750e-91d1-469f-9eb0-d38eef23f686" />

Par la suite on a copié cette clé sur chaque machine cible:

```bash 
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```
<img width="902" height="693" alt="auh8" src="https://github.com/user-attachments/assets/5a41b361-7eef-43f3-966f-61fa24a971d4" />

Cela permet une authentification sans mot de passe.

on a effectué un test après afin de vérifier la communication sans préciser l'utilisateur avec la commande :

```bash 
ansible all -i target01,target02,target03 -m ping
```

Voici le résultat après le test de communication avec le module ping d'Ansible.

<img width="723" height="418" alt="auth9" src="https://github.com/user-attachments/assets/9663c012-5641-4dbb-9c87-e8d000266dad" />

Une fois les tests terminés et les résultats validés, on proçède au nettoyage de notre environnement de travail afin de libérer les ressources.

<img width="510" height="218" alt="auth11" src="https://github.com/user-attachments/assets/98e89b99-953b-49a7-92b9-49871d98232e" />



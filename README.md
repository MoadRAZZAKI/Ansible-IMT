# Projet Ansible - Ateliers pratiques

## Objectif du projet
Ce projet a pour objectif de mettre en pratique les concepts fondamentaux d’Ansible à travers une série d’ateliers.  
Il permet de découvrir l’automatisation de la configuration des systèmes, la gestion de plusieurs machines et l’utilisation de playbooks pour déployer des services.

---

## Binôme
- El Houssein MOUSSA
- Moad RAZZAKI

---

## Organisation du projet
Chaque atelier est documenté dans un fichier Markdown dédié contenant :
- les objectifs
- les commandes utilisées
- les playbooks réalisés
- les résultats obtenus

---

## Liste des ateliers

- [Installation - Prise en main](./1-Installation-Prise-en-main.md)  
  -> Découvrir Ansible, installer l’environnement et exécuter les premières commandes.

- [Authentification](./2-Authentification.md)  
  -> Mettre en place l’authentification SSH entre le control node et les machines cibles.

- [Configuration de base](./3-Configuration_de_base.md)  
  -> Comprendre la structure d’un projet Ansible et configurer les premiers playbooks.

- [Idempotence](./4-Idempotence.md)  
  -> Garantir que les playbooks peuvent être exécutés plusieurs fois sans effet indésirable.

- [Un serveur web simple](./5-Un_serveur_web_simple.md)  
  -> Déployer et configurer automatiquement un serveur web avec Ansible.

- [Les handlers](./6-Les_handlers.md)  
  -> Déclencher des actions (ex: redémarrage de service) uniquement en cas de modification.

- [Les variables](./7-Les_variables.md)  
  -> Utiliser les variables pour rendre les playbooks dynamiques.

- [Les variables enregistrées](./8-Les_variables_enregistrées.md)  
  -> Récupérer et exploiter les résultats des tâches avec `register`.

- [Facts et variables hétérogènes](./9-Facts_et_variables_heterogènes.md)  
  -> Utiliser les facts et adapter les configurations selon les systèmes.

- [Cibles hétérogènes](./10-Cibles_hétérogènes.md)  
  -> Gérer différents systèmes (Debian, Rocky, SUSE…) avec Ansible.

- [Jinja Templates](./11-Jinja_Templates.md)  
  -> Générer des fichiers dynamiques avec des templates Jinja2.


---

## Technologies utilisées
- Ansible
- Vagrant
- Linux (Rocky, Debian, SUSE, Ubuntu)

---

## Conclusion
Ce projet nous a permis de comprendre le fonctionnement d’Ansible et de maîtriser l’automatisation des tâches sur plusieurs systèmes.  
Nous avons appris à utiliser des variables, des templates, des handlers et à gérer des environnements hétérogènes de manière efficace.

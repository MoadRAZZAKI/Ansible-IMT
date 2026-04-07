# Projet Ansible - IMT

## Description
Ce dépôt contient une série d’ateliers pratiques réalisés dans le cadre de la formation IMT.  
L’objectif est de maîtriser les concepts fondamentaux d’Ansible à travers des cas concrets d’automatisation.

Le projet couvre l’installation, la configuration, la gestion de machines multiples ainsi que le déploiement automatisé de services.

---

## Auteurs

Ce projet a été réalisé en binôme par :

- El Houssein MOUSSA  
- Moad RAZZAKI  

Dans le cadre de la formation IMT.

---

## Objectifs pédagogiques
- Comprendre le fonctionnement d’Ansible  
- Automatiser la configuration de systèmes Linux  
- Gérer plusieurs machines via un inventaire  
- Écrire et structurer des playbooks  
- Utiliser des variables, handlers et templates  
- Gérer des environnements hétérogènes  

---

## Contenu du projet

### Ateliers

- [Installation - Prise en main](./1-Installation-Prise-en-main.md)  
- [Authentification](./2-Authentification.md)  
- [Configuration de base](./3-Configuration_de_base.md)  
- [Idempotence](./4-Idempotence.md)  
- [Un serveur web simple](./5-Un_serveur_web_simple.md)  
- [Les handlers](./6-Les_handlers.md)  
- [Les variables](./7-Les_variables.md)  
- [Les variables enregistrées](./8-Les_variables_enregistrées.md)  
- [Facts et variables hétérogènes](./9-Facts_et_variables_heterogènes.md)  
- [Cibles hétérogènes](./10-Cibles_hétérogènes.md)  
- [Jinja Templates](./11-Jinja_Templates.md)  

Chaque atelier est documenté dans un fichier dédié.

---

## Technologies utilisées
- Ansible  
- Vagrant  
- Linux (Debian, Rocky, Ubuntu, SUSE)  

---

## Prérequis
- Ansible (version récente recommandée)  
- Vagrant  
- VirtualBox ou équivalent  

---

## Installation

```bash
git clone https://github.com/MoadRAZZAKI/Ansible-IMT.git
cd Ansible-IMT
vagrant up

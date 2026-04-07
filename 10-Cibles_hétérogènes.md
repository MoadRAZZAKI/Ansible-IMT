# Challenge – Atelier 17 : Gestion de cibles hétérogènes avec Ansible
## Déploiement de Chrony sur des distributions Linux multiples

---

## 1. Objectifs de l'atelier

Cet atelier a pour objectifs de :

- Maîtriser l'**exécution conditionnelle** dans les playbooks Ansible via le paramètre `when`
- Comprendre comment gérer des **cibles hétérogènes** (distributions Linux différentes)
- Mettre en œuvre deux approches de complexité croissante pour déployer un service sur plusieurs distributions
- Appliquer ces compétences à un cas concret : la synchronisation NTP avec **Chrony**

---


## 2. Application pratique : déploiement de Chrony

### 2.1 Identification des paramètres par distribution

Chrony est le client NTP de référence sur les distributions Linux modernes. Ses paramètres diffèrent selon la distribution :

| Distribution   | Paquet  | Service   | Fichier de configuration      |
|----------------|---------|-----------|-------------------------------|
| Debian         | `chrony` | `chrony`  | `/etc/chrony/chrony.conf`     |
| Ubuntu         | `chrony` | `chrony`  | `/etc/chrony/chrony.conf`     |
| Rocky Linux    | `chrony` | `chronyd` | `/etc/chrony.conf`            |
| SUSE  | `chrony` | `chronyd` | `/etc/chrony.conf`            |

### 2.2 Configuration à déployer

La configuration NTP suivante doit être installée sur l'ensemble des cibles :

```
# chrony.conf
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```

---

## 3. Playbook 1 – Méthode « gros sabots » (`chrony-01.yml`)

### Principe

Cette approche utilise les **modules natifs** de gestion de paquets (`apt`, `dnf`, `zypper`) et duplique chaque tâche pour chaque distribution. C'est la méthode la plus directe mais aussi la moins maintenable.

### Code complet

```yaml
---  # chrony-01.yml

- hosts: all

  tasks:

    - name: Update package cache on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Debian/Ubuntu
      apt:
        name: chrony
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Rocky Linux
      dnf:
        name: chrony
      when: ansible_distribution == "Rocky"

    - name: Install Chrony on SUSE Linux
      zypper:
        name: chrony
      when: ansible_distribution == "openSUSE Leap"

    - name: Install chrony configuration on Debian/Ubuntu
      copy:
        dest: /etc/chrony/chrony.conf
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        mode: '0644'
      when: ansible_os_family == "Debian"

    - name: Install chrony configuration on Rocky Linux
      copy:
        dest: /etc/chrony.conf
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        mode: '0644'
      when: ansible_distribution == "Rocky"

    - name: Install chrony configuration on SUSE Linux
      copy:
        dest: /etc/chrony.conf
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        mode: '0644'
      when: ansible_distribution == "openSUSE Leap"

    - name: Start & enable Chrony on Debian/Ubuntu
      service:
        name: chrony
        state: started
        enabled: true
      when: ansible_os_family == "Debian"

    - name: Start & enable Chrony on Rocky Linux
      service:
        name: chronyd
        state: started
        enabled: true
      when: ansible_distribution == "Rocky"

    - name: Start & enable Chrony on SUSE Linux
      service:
        name: chronyd
        state: started
        enabled: true
      when: ansible_distribution == "openSUSE Leap"

...
```

### Analyse

**Les avantages :**
- Simple à comprendre et à lire tâche par tâche
- Aucune abstraction nécessaire

**Les inconvenients :**
- Chaque tâche est dupliquée autant de fois qu'il y a de distributions
- Ajouter une nouvelle distribution impose de modifier chaque bloc de tâches
- Le playbook grossit rapidement et devient difficile à maintenir

---

## 4. Playbook 2 – Méthode avec variables et module générique (`chrony-02.yml`)

### Principe

Cette approche définit en amont trois variables (`chrony_package`, `chrony_service`, `chrony_confdir`) via `set_fact`, selon la distribution détectée. Le reste du playbook est alors **entièrement générique** et utilise le module `package` plutôt que `apt`, `dnf` ou `zypper`.

### Code complet

```yaml
---  # chrony-02.yml

- hosts: all

  tasks:

    - name: Set Chrony parameters for Debian/Ubuntu
      set_fact:
        chrony_package: chrony
        chrony_service: chrony
        chrony_confdir: /etc/chrony/chrony.conf
      when: ansible_os_family == "Debian"

    - name: Set Chrony parameters for Rocky Linux
      set_fact:
        chrony_package: chrony
        chrony_service: chronyd
        chrony_confdir: /etc/chrony.conf
      when: ansible_distribution == "Rocky"

    - name: Set Chrony parameters for SUSE Linux
      set_fact:
        chrony_package: chrony
        chrony_service: chronyd
        chrony_confdir: /etc/chrony.conf
      when: ansible_distribution == "openSUSE Leap"

    - name: Update package cache on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install Chrony
      package:
        name: "{{ chrony_package }}"

    - name: Install chrony configuration
      copy:
        dest: "{{ chrony_confdir }}"
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
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

...
```

### Analyse

**Points positifs :**
- Les tâches fonctionnelles (`install`, `copy`, `service`) ne sont écrites **qu'une seule fois**
- L'ajout d'une nouvelle distribution ne nécessite qu'un nouveau bloc `set_fact`
- Le handler utilise directement `{{ chrony_service }}` sans condition supplémentaire
- Le code est plus court, plus lisible et plus maintenable

**Limites :**
- Nécessite de comprendre le mécanisme `set_fact` et les variables
- Légèrement plus abstrait pour un débutant

---

## 5. Comparaison des deux approches

| Critère                        | chrony-01 (gros sabots) | chrony-02 (variables)  |
|--------------------------------|-------------------------|------------------------|
| Lisibilité immédiate           | ✅ Bonne                | ⚠️ Abstraite           |
| Maintenabilité                 | ❌ Faible               | ✅ Bonne               |
| Ajout d'une distribution       | ❌ Modifier N tâches    | ✅ 1 bloc `set_fact`   |
| Répétition du code             | ❌ Élevée               | ✅ Minimale            |
| Utilisation du module générique| ❌ Non                  | ✅ Oui (`package`)     |
| Handler propre                 | ❌ Complexe             | ✅ Simple              |

---

## 6. Vérification du déploiement

### Contrôle syntaxique avant exécution

```bash
yamllint chrony-01.yml
```
<img width="468" height="23" alt="image" src="https://github.com/user-attachments/assets/3071ec35-0144-4e21-8cf8-03fcad44df02" />


```bash
yamllint chrony-02.yml
```
<img width="464" height="23" alt="image" src="https://github.com/user-attachments/assets/83aad924-204f-4fff-8e45-d7cb71b57437" />




```bash
ansible-playbook --syntax-check chrony-01.yml
```
<img width="685" height="56" alt="image" src="https://github.com/user-attachments/assets/ab4544e7-611a-4846-b6cf-032b60d8a1bd" />


```bash
ansible-playbook --syntax-check chrony-02.yml
```

<img width="694" height="62" alt="image" src="https://github.com/user-attachments/assets/8d83985a-caa5-489e-ac8f-cbd4c75c99bf" />


### Exécution des playbooks

```bash
ansible-playbook chrony-01.yml
```
<img width="902" height="591" alt="image" src="https://github.com/user-attachments/assets/5839eeb8-2ab9-4867-bd90-a75e26504284" />


```bash
ansible-playbook chrony-02.yml
```
<img width="901" height="622" alt="image" src="https://github.com/user-attachments/assets/001f1d2a-0c90-47f2-b8c7-d4566863a20e" />



### Vérification depuis le Control Host

```bash
# Pour vérifier la synchronisation NTP sur toutes les cibles, on lance la commande suivante :
ansible all -a "chronyc tracking"
```

<img width="728" height="609" alt="image" src="https://github.com/user-attachments/assets/85db5c69-e656-4683-ad1a-32778c36f674" />

```bash
# Pour vérifier le statut du service par famille de distribution, on utilise les commandes suivantes : 
ansible debian,ubuntu -a "systemctl status chrony"
ansible rocky,suse -a "systemctl status chronyd"
```

<img width="902" height="593" alt="image" src="https://github.com/user-attachments/assets/d0364968-697f-47b2-88ec-15a760e8ac12" />

<br>

<br>


<img width="909" height="495" alt="image" src="https://github.com/user-attachments/assets/169dfb07-7204-426c-b706-b80a9d5132fd" />

<br>

<img width="897" height="474" alt="image" src="https://github.com/user-attachments/assets/c6782576-cf03-4639-bf76-1c85ba94b83e" />

<img width="903" height="446" alt="image" src="https://github.com/user-attachments/assets/2c8d92c0-5090-443b-a307-5210ab7dbd21" />



```bash
# Pour vérifier la configuration déployée
ansible debian,ubuntu -a "cat /etc/chrony/chrony.conf"
ansible rocky,suse -a "cat /etc/chrony.conf"
```


<img width="794" height="388" alt="image" src="https://github.com/user-attachments/assets/bd5308e9-a536-46db-9d18-3d55dd7a2721" />

<img width="713" height="394" alt="image" src="https://github.com/user-attachments/assets/e87ba406-2b87-4b34-b92a-43110e978604" />



---

## 7. Conclusion
L’atelier 17 nous a permis de comprendre l’évolution naturelle dans la façon d’écrire des playbooks Ansible pour des environnements hétérogènes. Une approche rapide et peu structurée peut fonctionner au départ, mais elle devient vite difficile à maintenir lorsque la complexité augmente. 
À l’inverse, l’utilisation de variables comme set_fact nous a montré l’importance de bien séparer les éléments spécifiques à chaque distribution de la logique générique, ce qui constitue une base solide pour des playbooks plus propres, modulaires et évolutifs, notamment avec l’utilisation des rôles Ansible.

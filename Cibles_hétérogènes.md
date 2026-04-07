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
| openSUSE Leap  | `chrony` | `chronyd` | `/etc/chrony.conf`            |

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

## 7. Comparaison des deux approches

| Critère                        | chrony-01 (gros sabots) | chrony-02 (variables)  |
|--------------------------------|-------------------------|------------------------|
| Lisibilité immédiate           | ✅ Bonne                | ⚠️ Abstraite           |
| Maintenabilité                 | ❌ Faible               | ✅ Bonne               |
| Ajout d'une distribution       | ❌ Modifier N tâches    | ✅ 1 bloc `set_fact`   |
| Répétition du code             | ❌ Élevée               | ✅ Minimale            |
| Utilisation du module générique| ❌ Non                  | ✅ Oui (`package`)     |
| Handler propre                 | ❌ Complexe             | ✅ Simple              |

---

## 8. Vérification du déploiement

### Contrôle syntaxique avant exécution

```bash
yamllint chrony-01.yml
yamllint chrony-02.yml
ansible-playbook --syntax-check chrony-01.yml
ansible-playbook --syntax-check chrony-02.yml
```

### Exécution des playbooks

```bash
ansible-playbook chrony-01.yml
ansible-playbook chrony-02.yml
```

### Vérification depuis le Control Host

```bash
# Vérifier la synchronisation NTP sur toutes les cibles
ansible all -a "chronyc tracking"

# Vérifier le statut du service par famille de distribution
ansible debian,ubuntu -a "systemctl status chrony"
ansible rocky,suse -a "systemctl status chronyd"

# Vérifier la configuration déployée
ansible debian,ubuntu -a "cat /etc/chrony/chrony.conf"
ansible rocky,suse -a "cat /etc/chrony.conf"
```

---

## 9. Conclusion

Cet atelier illustre une progression naturelle dans l'écriture de playbooks Ansible pour environnements hétérogènes. La méthode « gros sabots » permet de démarrer rapidement mais atteint vite ses limites dès que le nombre de distributions ou de tâches augmente. L'approche par variables (`set_fact`) offre une meilleure séparation entre la **connaissance spécifique à chaque distribution** et la **logique d'installation générique**, ce qui constitue le fondement des rôles Ansible.

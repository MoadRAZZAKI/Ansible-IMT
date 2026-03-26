## Exercice 1 : 

Écrivez un playbook kernel.yml qui affiche les infos détaillées du noyau sur tous vos Target Hosts. Utilisez la commande uname -a et le module debug avec le paramètre msg.


Affichage des informations du noyau : 


```
---  # kernel.yml

- hosts: all
  gather_facts: false

  tasks:

    - name: Get kernel info
      command: uname -a
      changed_when: false
      register: kernel_info

    - debug:
        msg: "{{ kernel_info.stdout_lines }}"

...


```



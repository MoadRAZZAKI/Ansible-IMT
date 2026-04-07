## Exercice 1 : 

Écrivez un playbook kernel.yml qui affiche les infos détaillées du noyau sur tous vos Target Hosts. Utilisez la commande uname -a et le module debug avec le paramètre msg :


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


<img width="1145" height="603" alt="image" src="https://github.com/user-attachments/assets/f0f11314-58ef-47c5-be66-37806e1af3bb" />


## Exercice 2 : 

Essayez d'obtenir le même résultat en utilisant le paramètre var du module debug : 


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
        var: kernel_info.stdout_lines

...

```


<img width="1137" height="589" alt="image" src="https://github.com/user-attachments/assets/b089516f-2a1d-48a7-80fc-d752f5c94a24" />


## Exercice 3 : 

Écrivez un playbook packages.yml qui affiche le nombre total de paquets RPM installés sur les hôtes rocky et suse (rpm -qa | wc -l) :


Affichage du nombre de paquets RPM :

```


---  # packages.yml

- hosts: rocky,suse
  gather_facts: false

  tasks:

    - name: Get total RPM packages installed
      shell: rpm -qa | wc -l
      changed_when: false
      register: rpm_count

    - debug:
        msg: "Nombre de paquets RPM installés : {{ rpm_count.stdout }}"

...



```


<img width="1066" height="356" alt="image" src="https://github.com/user-attachments/assets/e197b37a-a306-45aa-9cef-129cdc5a06d7" />



Enfin , on quitte le control host , et on supprimer les VM :

<br>

<img width="558" height="290" alt="image" src="https://github.com/user-attachments/assets/5a2a1502-c107-456a-b262-0953ac23071d" />


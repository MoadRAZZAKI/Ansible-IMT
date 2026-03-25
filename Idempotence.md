## Installation des paquets

<br>

```
ansible all -m package -a "name=tree state=present"
ansible all -m package -a "name=git state=present"
ansible all -m package -a "name=nmap state=present"
```

Premier lancement → changed: true (orange) : les paquets sont installés.

<br>

<img width="902" height="287" alt="image" src="https://github.com/user-attachments/assets/400fe889-23dd-4179-8442-054e4a5f16c4" />

<br>

Deuxième lancement → changed: false (vert) : les paquets sont déjà présents, rien à faire.

<br>

<img width="716" height="388" alt="image" src="https://github.com/user-attachments/assets/ef8074c8-06ab-4e2b-83a6-992ff7198bbb" />

## Désinstallation des paquets 


```
ansible all -m package -a "name=tree state=absent"
ansible all -m package -a "name=git state=absent"
ansible all -m package -a "name=nmap state=absent"
```


Premier lancement → changed: true (orange) : les paquets sont supprimés.

<br>

<img width="709" height="383" alt="image" src="https://github.com/user-attachments/assets/ed7c3d9c-615f-4577-a641-75d08ed077c1" />

<br>

Deuxième lancement → changed: false (vert) : les paquets sont déjà absents, rien à faire.


<br>

<img width="801" height="340" alt="image" src="https://github.com/user-attachments/assets/98ff094e-bb8d-4201-8c94-a8c6794f7dce" />

<br>


## Copie du fichier /etc/fstab vers les cibles


```

ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"

```


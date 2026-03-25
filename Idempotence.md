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

<br>


Premier lancement → changed: true (orange) : le fichier est copié vers les cibles.

<br>

![alt text](image-9.png)


<br>

Deuxième lancement → changed: false (vert) : le fichier est déjà présent et identique, rien à faire.


<br>

![alt text](image-10.png)

<br>



## Suppression du fichier /tmp/test3.txt


<br>

```
ansible all -m file -a "path=/tmp/test3.txt state=absent"

```

<br>

Premier lancement → changed: true (orange) : le fichier est supprimé.

<br>

![alt text](image-11.png)



<br>

Deuxième lancement → changed: false (vert) : le fichier est déjà absent, rien à faire.

<br>


![alt text](image-12.png)


<br>

## Affichage de l'espace disque


```

ansible all -m command -a "df -h /"

```

<br>

![alt text](image-13.png)

<br>


![alt text](image-14.png)

Le module command affiche toujours changed: true, même au deuxième lancement. Contrairement aux modules package, file ou copy, il ne peut pas déterminer si une commande brute a modifié l'état du système. 
Ce comportement montre très bien pourquoi les modules dédiés sont préférables : ils garantissent l'idempotence, ce que le module command ne fait pas.


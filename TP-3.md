# **Exercice 1. Gestion des utilisateurs et des groupes**

1 - Pour ajouter les groupes dev et infra: **sudo groupadd dev** et **sudo groupadd infra**

2 - Afin de créer les utilisateurs en spécifiant la création de leur dossier personnel et avec bash pour shell, on execute les commandes:
* **sudo useradd -m -s /bin/bash alice**
* **sudo useradd -m -s /bin/bash bob**
* **sudo useradd -m -s /bin/bash charlie**
* **sudo useradd -m -s /bin/bash dave**

3 - On ajoute les utilisateurs dans les nouveaux groupes crées:
* **sudo usermod -a -G dev alice**
* **sudo usermod -a -G dev bob**
* **sudo usermod -a -G dev dave**
* **sudo usermod -a -G infra bob**
* **sudo usermod -a -G infra charlie**
* **sudo usermod -a -G infra dave**

4 - Une premire façon d'afficher les membres du groupe infra est de taper la commande **getent group infra** (on peut l'améliorer: **getent group infra | cut -d: -f4**).
Une seconde manière d'obtenir ceci peut se faire avec **cat /etc/group | grep 'dev'**; on peut encore améliorer cette commande afin qu'elle soit plus précise avec **cat /etc/group | grep -w 'dev' | cut -d: -f4**.

5 - dev devient le groupe propriétaire des répertoires /home/alice et /home/bob grâce aux commandes **sudo chown :dev /home/alice** et **sudo chown :dev /home/bob**. infra devient le groupe propriétaire des répertoires /home/charlie et /home/dave grâce aux commandes **sudo chown :dev /home/charlie** et **sudo chown :dev /home/dave**.

6 - On peut remplacer le groupe primaire des nouveaux utilisateurs que l'on a précédemment crées :
* **sudo usermod -g dev alice**
* **sudo usermod -g dev bob**
* **sudo usermod -g infra charlie**
* **sudo usermod -g infra dave**

7 - Création des dossiers : **sudo mkdir /home/dev**, **sudo mkdir /home/infra**.
![résultat](img/TP3_Q7-1.jpg)

Ensuite, on va changer le groupe propoiétaire de ces dossiers: **sudo chgrp dev dev** et **sudo chgrp infra infra**.
![résultat](img/TP3_Q7-2.jpg)

Par la suite, on spécifie le droit d'écriture: **sudo chmod g+w dev** et **sudo chmod g+w infra**.
Les groupes propriétaires des dossiers dev et infra ont de ce fait la permission d'écrire dans ces dossiers.
![résultat](img/TP3_Q7-3.jpg)
Maintenant, les membres de dev ont la permission d'écrire dans ce dossier et les membres d'infra ont la permission d'écrire dans le dossier infra.

On aurait également pu utiliser la commande **sudo chmod 774 dev**.

8 - 

9 - Avec la commande **sudo su alice** oui, car la commande est executée en tant qu'administrateur. Autrement, sans le sudo, cela n'est pas possible car il faudrait alors taper le mot de passe d'alice, que l'on ne possède pas.

10 - 

11 - Via la commande **id alice**, on obtient son uid (1002) et son gid (1002).

12 - Une première façon de trouver l'utilisateur dont l'uid est 1003 est possible avec la commande **id 1003**. On voir alors apparaitre un résultat similaire à la question précédente. On y voit donc le nom de l'utilisateur.
Une seconde façon grâce à **getent passwd 1003**.

13 - L'id du groupe dev est 1002. On peut obtenir ce résultat avec **getent group dev** ou en executant **id [nom d'un utilisateur appartenant au groupe dev]** (dans l'affichage du résultat de cette commande on peut y voir l'id du group).

14 - Simplement avec **getent group 1002**, ce qui donne donc le groupe dev.

15 - **sudo gpasswd -d charlie infra**; -d spécifie la suppression de l'utilisateur charlie du groupe infra. Il ne peut donc plus écrire dans le dossier infra (cela dépend des permissions attribuées précédemment) car il ne fait plus parti du groupe infra.

16 - chage -l dave

17 - L'interpréteur de commandes (Shell) de l'utilisateur root est /bin/bash. On peut le savoir en tapant la commande :
**cat /etc/passwd | grep root | cut -d: -f 7**

18 - L'utilisateur nobody est un pseudo utilisateur. Il représente l'utilisateur avec le moins de permissions possibles. Il n'a également aucun interpréteur de commandes assigné. Selon *Linux Standard Base*, nobody est utilisé par NFS.

19 - Selon *linuxtricks.fr*, par défaut, le mot de passe est concervé pour une durée de 15 minutes. Il est possible de forcer sudo à oublier notre mot de passe avec **sudo -K**.
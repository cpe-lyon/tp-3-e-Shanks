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

16 - chage -l dave (sudo chage -E 2021-06-01 -M 90 -W 14 -I 30 dave)

17 - L'interpréteur de commandes (Shell) de l'utilisateur root est /bin/bash. On peut le savoir en tapant la commande :
**cat /etc/passwd | grep root | cut -d: -f 7**

18 - L'utilisateur nobody est un pseudo utilisateur. Il représente l'utilisateur avec le moins de permissions possibles. Il n'a également aucun interpréteur de commandes assigné. Selon *Linux Standard Base*, nobody est utilisé par NFS.

19 - Selon *linuxtricks.fr*, par défaut, le mot de passe est concervé pour une durée de 15 minutes. Il est possible de forcer sudo à oublier notre mot de passe avec **sudo -K**.

# **Exercice 2. Gestion des permissions**

1 - **mkdir test**, **touch fichier**.
Sur le dossier test, pour l'utilisateur propriétaire et le groupe propriétaire les droits sont les mêmes, c'est-à-dire la permission de lire, écrire et exécuter. Pour les autres, c'est seulement le droit de lire et d'exécuter.
Sur le fichier fichier, pour l'utilisateur propriétaire et le groupe propriétaire les droits sont les mêmes, c'est-à-dire la permission de lire et écrire. Pour les autres, c'est seulement le droit de lire.

2 - On retire tous les droits sur ce fichier avec la commande **chmod a-rwx fichier**. Même en étant root, il nous est impossible de modifier le fichier.

3 - **chmod u+wx fichier**, **echo "echo Hello" > fichier**.
Les droits sont respéctés ????????????????

4 - **cat fichier** => permission denied
**sudo cat fichier** => affiche bien le contenu
On peut en déduire que root à toutes les permissions sur les fichiers mais pas sur les dossiers????????????????

5 - Dans le dossier test, on se retire le droit de lecture sur ce dossier via **chmod u-r ../test**. On ne peut ni afficher le contenu du répertoire ni exécuter le fichier ficher. Cela peut s'expliquer par le fait que comme nous n'avons plus la permission de lire ce dossier mais que, comme nous sommes déjà à l'intérieur nous n'avons plus les autres permissions ce qui nous empêche d'agir.
On rétablie le droit en lecture: **chmod u+r ../test**.

6 - Dans le dossier test:
* création du fichier: **touch nouveau**
* création du répertoire: **mkdir sstest**
* suppression des droits en écriture: **chmod a-w ../test nouveau**
* **echo "test modification" > nouveau**: permission denied
* on rétablie le droit en écriture pour le dossier test: **chmod a+w ../test**
* **echo "bonjour" > nouveau**: permission denied
* **rm nouveau**: "remove write-protected regular empty file 'nouveau'?"; on peut donc supprimer en tapant "y".
On peut alors déduire que ????????????????

7 - Une fois positionné dans le répertoire personnel avec **cd ~**, on retire le droit d'exécution au dossier test via **chmod a-x test**. Après cette commande, il est impossible de se déplacer dans le répertoire test, d'y créer un nouveau fichier ou d'en modifier un. On peut en déduire que chaque action faite sur ce dossier n'est pas réalisable sans le droit d'exécution.

8 - Rétablissement du droit d'exécution: **chmod a+x test**. On se positionne à l'intérieur puis on lui retir de nouveau ce droit: **chmod a-x ../test**. Une fois cette commande exécutée, on ne peut plus rien faire: plus afficher le contenu d'un fichier, d'en exécuter un, d'afficher le contenu du dossier courant, de se déplacer dans le dossier sstest. Les droits que l'on possède sur le répertoire courant ????????????????
On peut cependant retourner dans le répertoire parent, ceci est du au fait ????????????????

9 - **chmod a+x test**. Pour attribuer au fichier fichier les droits suffisants pour qu'une autre personne de notre groupe puisse y accéder en lecture mais pas en écriture, on tape la commande **chmod g+r test/fichier** (si le droit d'exécution et d'écriture est déjà attribué au fichier, alors on les retire avec **chmod g-wx fichier**.

10 - On retire les permissions pour tout le monde sauf pour nous: **umask g-rwx** et **umask o-rx** (on peut vérifier avec umask -S).
Pour tester nos modifications, on créer un nouveau fichier et un nouveau dossier. On peut effectivement agir sur eux. Maintenant on se place sur le compte d'alice, par exemple, **sudo su alice**. Et en effet, on ne peut plus agir sur le nouveau fichier et le nouveau dossier en tant qu'une autre personne que le créateur du fichier et du dossier.

11 - Même processus que la question précédente: **umask g+rx** et **umask o+rx**. Toutefois, ce n'est toujours pas possible ????????????????

12 - c deja le cas ????????????????

13 - Création d'un fichier fic pour l'exercice: **touch fic**
* **chmod u=rx,g=wx,o=r fic** => **chmod 534 fic**
* **chmod uo+w,g-rx fic** => **chmod 602 fic**
* **chmod 653 fic** => **chmod u=rw,g=rx,o=wx fic** ou **chmod u-x, g+r, o+w fic**
* **chmod u+x,g=w,o-r fic** => **chmod 574 fic**

14 - On peut afficher les droits sur le programme passwd via la commande **stat -c %A /usr/bin/passwd**. On remarque que l'utilisateur propriétaire (root) possède un droit "s" que les autres n'ont pas. Cela signifie que root possède SUID permission sur ce programme. C'est-à-dire que seul root peut exécuter ce programme.

15 - (tuto en ligne)

16 - (tuto en ligne)

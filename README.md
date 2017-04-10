# syncable-linux
Rework every linux app to make them synchronization friendly


## EN
Coming soon ... please stay tuned :-)


## FR

### Pourquoi ?

Partager un environnement entier de travail en se basant uniquement sur les contenus (fichiers).  
Aller-retours en ligne/hors-ligne transparents ou du moins facilités.  
Plusieurs sources peuvent interagir sur le même contenu.  
Possibilité d'interagir partiellement sur le contenu.  
Restauration sans heurts en cas de problème.  

### Syncrhonization friendly?

Chaque logiciel devrait être jugé (pour la synchronisation) selon les critères suivants :

1. identification constante des fichiers contenant le travail et la configuration
2. format de fichier ouvert et si possible standard (afin que d'autres logiciels interagissent)
3. détection des changements et rechargement du contenu pendant le travail
4. aide à la résolution de conflits

### Solutions de mise en oeuvre

- Utiliser des contenus/fichiers imuables (dont le contenu ne change pas). S'il y a un changement, alors cela produit un nouveau fichier. Problème : augmente potentiellement/temporairement l'espace de stockage utilisé.
- Utilser des répertoires contenant les différentes versions des fichiers, et des liens symboliques pointant vers la dernière version. (Comme la gestion des vhost Apache Httpd sur Debian).
- Déconnecter l'interface des logiciels, de leur système de stockage, afin de permettre de greffer un intermédiaire (s'occupant de la synchronisation, ou bien de changer le format de stockage).

### Autre approche

Utiliser un système de fichier sous-jacent :

- qui permette la synchronisation avec de multiples sources/destinations
- avec une synchronisation éventuellement partielle des contenus. 

### Exemples

**Logiciel de traitement de texte sur 2 ordinateurs**  
L'utilisateur possède 2 ordinateurs O1 et O2.  
Via le logiciel, il produit un fichier F sur O1.  
Sur O2 il récupère automatique le fichier F.  
Il modifie F sur O2 (avec le même logiciel ou un autre) ce qui donne F'.  
De manière transparente F est mis à jour sur O1 et devient F'.  
Le logiciel (toujours ouvert) sur O1 propose alors à l'utilisateur :  

- d'ignorer les modifications de la mise à jour (écraser F' pour garder F)
- de remplacer son document par celui à jour (garder F')
- de voir les différences (entre F et F')
- d'essayer d'intégrer automatiquement (fusionner) les modifications de l'un à l'autre (donnant F")
- de conserver les deux versions et de les marquer comme "en conflit" (nommage de fichier)

**Boite mail sur 2 ordinateurs et un smartphone**  
L'utilisateur U possède 2 ordinateurs O1 et O2 et un smartphone S.  
L'ordinateur O1 récupère les mails via IMAP (depuis le serveur) et les stockent au format Maildir (un fichier par mail).  
Les nouveaux mails se propagent via synchronisation de fichier vers O2 et S (sans qu'ils aient besoin de se connecter au serveur en IMAP).  
S ne peut pas contenir toute la boite mail alors ne sont stockés que les 90 derniers jours de mail (peut en récupérer plus à la demande).  
Ensuite 01 est éteint et O2 déconnecté.  
S récupère alors les nouveaux mail via IMAP et les stockent dans sa boite mail partielle.  
Les nouveaux mails se propageront à O1 lorsqu'il sera allumé.  
O2 a été reconnecté entre temps, et a déjà récupéré les nouveaux mails via IMAP, faisant doublons avec ceux du téléphone.  
Puisque le format Maildir est utilsé (un fichier par mail), le logiciel de synchronisation peut alors :  

- identifier les doublons entre ceux du smartphone et ceux de l'ordinateur
- fusionner de manière transparente les nouveaux mails du smartphone avec ceux déjà récupérés en IMAP

Ainsi les 3 environnements sont en permanence synchronisés les uns par rapports aux autres, selon le contenu qui les intéressent.


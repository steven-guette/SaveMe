# <font color="orange">Save</font>Me

***Ce petit programme clé en main permet d'effectuer des sauvegardes de vos fichiers sensibles que ce soit en local 
ou via un protocole SSH.***

***Il s'agit d'un programme destiné aux systèmes GNU/Linux.***

***En outre, si vous êtes sur une distribution qui n'est pas basée sur Debian, vous devrez probablement installer les dépendances manuellement.***

## <font color="orange">Configuration d'un nouveau backup</font>
Pour configurer un nouveau backup, il vous suffit d'ajouter un fichier dans `SaveMe/repositories/` 
qui contiendra toute la configuration du backup en question.

Petit plus, vous y trouverez déjà un fichier de configuration nommé `example` que vous pouvez copier en éditant le nom, 
puis vous n'aurez plus qu'à modifier son contenu selon vos besoins.

Son contenu est bien commenté, cela ne devrait pas vous prendre plus de quelques minutes pour configurer un nouveau backup.

### <font color="red">Important</font>
**Pour des raisons de sécurité et de compatibilité, il est impératif que le répertoire SaveMe
soit situé dans /root et que les scripts qu'il contient soient utilisés en tant que root.**

## <font color="orange">Les outils disponibles</font>
*Vous trouverez ci-dessous la liste de tous les scripts de type utilitaire que vous pourrez utiliser afin de gérer vos différents backups. 
Pour détailler leur utilisation, je me servirais du fichier example.
Chaque script prend en paramètre le nom du fichier de configuration que vous ciblez.*

### <font color="darkorange">save</font>
```Bash
./save example
```
Le script *save* permet d'effectuer une nouvelle sauvegarde dans le backup ciblé.
Si vous venez de créer le fichier de configuration et que le backup n'existe pas encore, 
il sera créer et sa première archive sera ensuite générée.

Au besoin, vous pouvez directement automatiser le tout depuis une tâche cron, 
à moins de vouloir générer un backup ponctuel, il n'est pas nécessaire d'utiliser ce script en direct.
***

### <font color="darkorange">upgrade</font>
```Bash
./upgrade example 
```
Le script *upgrade* permet de faire évoluer un backup initialement configuré sans cryptage, en un backup cryptée.
Si vous modifiez la valeur de *encryption* dans le fichier de configuration sans avoir fait évoluer votre backup avant,
vos sauvegardes à venir échoueront et retourneront des logs indiquant qu'il ne s'agit pas d'un backup crypté.
***

### <font color="darkorange">downgrade</font>
```Bash
./downgrade example 
```
Identique à *upgrade*, mais cette fois-ci permet de faire évoluer un backup crypté en un backup non crypté.
***

### <font color="darkorange">restore</font>
```Bash
./restore example [nom_archive]
```
Le script *restore* permet de restaurer vos données sur une archive spécifique.
Si vous ne fournissez pas le nom d'une archive en second paramètre, il vous proposera d'effectuer une restauration à partir de la dernière archive.
Si vous refusez la restauration à partir de la dernière archive, il vous fournira la liste de vos archives disponibles.
***

### <font color="darkorange">getinfos</font>
```Bash
./getinfos example [nom_archive]
```
Le script *getinfos* permet d'afficher les détails du backup ainsi que ceux d'une archive en particulier.
***

### <font color="darkorange">getlist</font>
```Bash
./getlist example [nom_archive]
```
Le script *getlist* permet d'afficher la liste des archives disponibles dans un backup, ou la liste du contenu d'une archive en particulier.
***

### <font color="darkorange">getrepos</font>
```Bash
./getrepos
```
Le script *getrepos* permet d'afficher la liste de tous vos fichiers de configuration.
***

### <font color="darkorange">remove</font>
```Bash
./remove example [nom_archive]
```
Le script *remove* permet de supprimer une archive ou un backup complet, soyez conscient qu'aucune restauration ne sera disponible.

## <font color="orange">Les petits plus</font>
*Pour terminer la présentation de **SaveMe**, voici quelques informations complémentaires.*
***

### <font color="darkorange">Les logs</font>
Lors de chaque sauvegarde des logs seront générés dans `SaveMe/logs` :
- **backup-<nom_du_fichier_de_config>.log** contiendra le résultat de la sauvegarde, que ce soit un succès ou un échec ;
- **errors-<nom_du_fichier_de_config>.log** contiendra les potentielles erreurs qui ont forcé l'annulation de la sauvegarde.

Les logs de type *backup* sont purgés automatiquement, en revanche les logs de type *errors* resteront jusqu'à ce que vous les supprimiez manuellement.
***

### <font color="darkorange">Rapports par mail</font>
Vous avez la possibilité de recevoir les rapports de vos sauvegardes directement par email.
Pour ce faire, il vous suffira de renseigner les informations du fichier de configuration en conséquence.

Les seuls prérequis pour l'utiliser sont d'avoir préalablement installé et configuré un module smtp de votre choix sur votre système.

Le script qui gère l'envoi des mails est configuré pour ssmtp.

**Si vous utilisez un autre paquet pour la gestion de vos mails, il vous faudra suivre les étapes suivantes :**
1. Ouvrez `SaveMe/includes/mail-report` avec l'éditeur de texte de votre choix.
2. Modifier la ligne suivante selon vos besoins :

```Bash
echo -e "Subject: $mail_header\nMIME-Version: 1.0\nContent-Type: text/html\n\n$mail_content" | /usr/lib/sendmail "$mailto"
```
***

### <font color="darkorange">Dépendances nécessaires</font>
Si vous êtes sur la distribution Debian ou sur une autre distribution basée dessus, l'installation des paquets se fera automatiquement.

Le cas échéant, voici la liste des paquets nécessaires au bon fonctionnement du programme :
```Bash
borgbackup gawk grep dpkg
```
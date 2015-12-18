Installation d'apache2
======================

Il y a plusieurs méthode pour installer un serveur apache2 sur un serveur debian:

Installation de LAMP
--------------------

LAMP (Linux Apache MySQL PHP), est un serveur apache préconfiguré pour l'utilisation de PHP et MySQL.
Pour installer un LAMP, on utiliser la commande suivante (le ^ n'est pas une faute de frappe):

    sudo apt-get install lamp-server^

Si vous tenter d'accéder à votre serveur sur le port 80 (mettre l'ip du serveur dans un navigateur ou localhost si vous êtes en local), une page avec marqué "It works" devrait s'afficher.

Installation avec les paquets
-----------------------------

La seconde méthode est d'installer tout les paquets qui composent LAMP séparément.
Pour cela, on peut exécuter la commande suivante:

    sudo apt-get install apache2 php5 mysql-server libapache2-mod-php5 php5-mysql

Cette commande installe respectivement apache, php5, mysql, le mod_php5 pour apache et l'intégration de mysql avec php.

Configuration
=============

Structure 
---------

Les fichiers de configuration d'apache se trouvent dans le répertoire /etc/apache2 qui est structuré comme cesi:

- apache2.conf: Fichier de configuration général d'apache
- conf.d: Répertoire contenant des fichiers de configuration complémentaire pour apache. Tout le contenu du répertoire est inclut par dans la configuration par défault d'apache (cf apache2.conf)
- envvars: Fichier qui contient la définition des variables d'environements d'apache
- magic: Fichier de configuration du mod magic
- mod-available: Répertoire qui contient les configurations des mods disponibles pour apache
- mod-enabled: Répertoire qui contient les configurations activés pour apache (c'est une liste de liens symboliques qui pointent sur mod-available). Si on regarde dans le fichier de configuration d'apache, on remarque que le repertoire mod-enabled est inclut
- ports.conf: Fichier de configuration qui défini les ports d'écoute du serveur (par défault 80 et 443 si le mod_ssl est activé)
- sites-available: Répertoire qui contient les configurations disponibles de vos différents sites
- sites-enabled: Répertoire qui contient les configurations de site activés

Pour faire simple, le fichier apache2.conf contient les configurations général d'apache, et inclut d'autres fichiers de configuration plus spécifiques.

Les commandes
-------------

Voici quelques commandes utile:

service apache2 start / stop / restart # Démarrer, arrêter ou redémarrer le serveur apache

a2enmod / a2dismod # Activer ou désactiver un mod apache
a2ensite / a2dissite # Activer ou désactiver un site

Les vhosts
----------

Le virtual hosting (hébergement virtuel) est le fait de créer plusieurs sites web distincts sur une même instance de serveur apache. On pourra par exemple avoir http://site1.com et http://site2.com qui pointent sur la même instance d'apache.

Il y a 4 méthodes pour distinguer les vhosts:

- vhost par adresse: La machine a plusieurs adresses ip et chacune de ces adresses pointent sur un site distinct
- vhost par nom: On se base sur le nom dans la requette http pour distinguer le site (ex: site1.com)
- vhost par ports: On se base sur le port d'écoute pour distinguer le site (ex: 80, 443 etc)
- Mixe: On peut mixer les 3 différentes méthodes pour distinguer les sites (ex: ip+port, nom + port etc)

Par défault, apache écoute le port 80 et 443 si le ssl est activé (cf ports.conf). Pour écouter sur d'autre ports, il faut utiliser la directive "Listen" suivit du port que l'on souhaite écouter.

La configuration du vhost par défault se situe dans sites-available/default.

On crée un vhost à l'aide de la directive:

    <VirtualHost ip:port>
	  DocumentRoot /path/to/site/directory
	  ServerName server1
	</VirturalHost>

Si on regarde le fichier sites-available/default, on remarque que le vhost est configuré pour accepter les requêtes de toutes les ips de la machine qui sont émises sur le port 80.
Le DocumentRoot pointe sur /var/www. Si on va dans ce répertoire, on trouve le fichier html avec le code du "It works".


Exercices
=============

- Chaque nouveau vhost devra être configuré dans des fichiers séparés et placé dans le répertoire sites-available.
- Changer votre fichier hosts (/etc/hosts), pour diriger les noms de domaine sur votre ip.
- Redémarrer apache à chaque modification de la configuration (sudo service apache2 restart).
- N'oubliez pas d'activer vos site (a2ensite <nom du site>).

Exercice 1
----------

Créer un vhost repondant au nom de domaine site1.local et renvoyant une page index.html avec de l'html basique (Un hello world par exemple).
Le repertoire racine de ce vhost sera /www/site1.

Renommer le fichier index.html en index.php, et y ajouter un bout de php (par exemple <?php print "Ceci est du texte &eacute;crit en php"; ?>).
Tester pour voir si le fichier PHP est bien interprété.

Directives à utiliser:
ServerName, DocumentRoot, VirtualHost

Exercice 2
----------

Créer un nouveau vhost avec le nom de domaine site2.local et le port 8080 et pointant sur le répertoire /www/site2.
Dans ce vhost, indexer uniquement les fichier qui s'appel accueil.html.
Créer le fichier accueil.html avec de l'html pour tester.

Directives à utiliser:
Listen, DirectoryIndex

Exercice 3
----------

Dans cet exercice, nous allons faire un systeme de release grâce aux liens symbolique.

Dans votre répertoire personnel (~), créer un repertoire site3, avec dedans 2 répertoires: release1, release2
Ajouter dans chaque répertoire un fichier index.html avec un contenu différent.

Créer le répertoire /www/site3. Dans ce répertoire, créer un lien symbolique vers le répertoire release1 avec pour nom current (avec la commande ln -s <cible> <nom>).
Ce qui donne l'arborescence suivante: /www/site3/current

Créer un nouveau vhost avec le non de dommaine site3.local pointant sur le repertoire site3/current. Ajouter la possibilité de suivre les liens symbolique (Directive Option)

Tester que c'est bien le fichier index.html du répertoire release1 qui est appelé.

Mainenant, nous allons changer le lien symbolique pour pointer sur le repertoire release2.

Vous pouvez maintenant changer le site juste en changeant le lien symbolique.

Nous allons maintenant créer un repertoire partager à toutes les release (Pour y mettre des images par exemple).

Dans le répertoire ~/site3 ajouter un répertoire shared avec des fichiers dedans.
Dans les répertoires release1 et release2, créer un lien symbolique vers le fichier shared.
Maintenant, tester d'accéder à ce réperoire depuis le navigateur (http://site3.local/shared).
Normalement, peut importe la release, le dossier contient les mêmes images.

Directives à utiliser: Option, FollowSymLink

Exercice 4
----------

Pour cet exercice, nous allons reprendre le vhost de l'exercice 3.

Nous allons pour cela créer un alias qui fera pointer l'adresse site4.local sur ce vhost. Pour cela, utiliser la directive ServerAlias.

Nous allons empêcher le listing du répertoire shared.
Pour cela, il faut créer un portion de configuration avec la directive Directory.
Dans cet portion de configuration, nous allons enlever les Options Index et FollowSymLinks (avec un -).

Créer un lien symbolique de le repertoire shared pour être sûr qu'on ne puisse pas y accéder.
Essayer d'accéder au répertoire shared pour voir si il y a bien une erreur.

Directives à utiliser: ServerAlias, Option, Directory

Exercice 5
----------

Pour cet exercice, nous allons garder le même vhost que l'exercice précédant.

Le but de cet exercice est de créer des pages d'erreurs personnalisées.
Pour cela nous allons utiliser la directive ErrorDocument.

Pour l'erreur 403 (forbidden), mettre juste le texte "Vous ne pouvez pas accéder à cette page.".
Pour l'erreur 404 (Page not found), afficher le contenu d'une page HTMl error404.html.

Pour l'erreur 404 uniquement dans le répertoire shared, afficher le texte "Vous n'êtes pas autorisé à accéder au répertoire partagé.".

Directives à utiliser: Directory, ErrorDocument

Exercice 6
----------

Pour cet exercice, nous allons garder le même vhost que l'exercice précédant.

Le but de cet exercice est de manipuler les fichier logs de votre vhost.

À l'aide de la directive ErrorLog, changer l'emplacement du log d'erreur pour le rediriger vers /var/log/apache2/exo3.error.log.

Vous pouvez changer le format des erreurs avec la directive ErrorLogFormat

Directives à utiliser: ErrorLog, ErrorLogFormat


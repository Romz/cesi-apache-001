Installation d'apache2
======================

Il y a plusieurs méthode pour installer un serveur apache2 sur un serveur debian:

Installation de LAMP
--------------------

LAMP (Linux Apche MySQL PHP), est un serveur apache préconfiguré pour l'utilisation de PHP et MySQL.
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
=========

Chaque nouveau vhost devra être configuré dans des fichiers séparés et placé dans le répertoire sites-available.
N'oubliez pas de changer votre fichier hosts pour faire pointer les noms sur votre ip local (Il se trouve ici: /etc/hosts).
Vous pouvez trouver la liste des directives [ici](http://httpd.apache.org/docs/2.4/mod/directives.html)

Exo 1:
------
Changer la page par défault index.html.

Exo 2:
------
Changer en index.php et mettre un bout de php (ex: print "test";).

Exo 3:
------
Créer un nouveau vhost basé sur le nom site1.com pointant sur le répertoire /var/www2/ en indexant uniquement les fichiers index.html.

Tip: Utiliser la directive DirectoryIndex

Exo 4:
------
Créer un nouveau répertoire dans votre répertoire personnel : ~/sites/ avec un fichier index.html dedans.
Ajouter un lien symbolique vers ce nouveau répertoire dans /var/www2/.
Modifier la configuration du vhost pour pouvoir accéder à ce répertoire.

Tip: Utiliser la directive Options

Exo 5:
------
Ajouter un nouveau répertoire /var/www2/override/.
Modifier la configuration du vhost pour empêcher l'indexation des fichiers d'index et de suivre les liens symbolique, uniquement pour le répertoire override.

Tip: Directive <Directory>

Exo 6:
------
Ajouter un nouveau vhost à l'écoute sur le port 8080 pointant vers le répertoire /var/www3/.
Changer la page d'erreur 404 pour pointer vers un fichier error404.html.

Exo 7:
------
Modifier la configuration des 2 vhosts pour mettre un fichier de log d'erreur dédié (<servername>.error.log).


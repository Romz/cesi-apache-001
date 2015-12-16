Les fichiers .htaccess
====================
Les fichiers .htaccess sont des fichiers de configuration d'apache que l'on place dans un répertoire d'un site pour ajouter des options à la configuration de base du site. Cette configuration sera valable pour le répertoire et tous les sous dossiers ne contenant pas de fichier .htaccess.
Pour que le fichier .htaccess soit interprété, il faut notifier dans la configuration du server que l'on autorise l'override de la configuration avec la directive AllowOverride.
Vous pouvez trouver la documentation d'utilisation du fichier .htaccess [ici](http://httpd.apache.org/docs/current/howto/htaccess.html)

Url rewrite
===========

L'url rewrite, comme son nom l'indique, permet de réécrire les url de votre site à la volé. On pourra par exemple transformer l'url site.com?categorie=apache&article=6 par site.com/apache/5 ou encore site.com/apache-5.html

La doc du mod_rewrite se trouve [ici](http://httpd.apache.org/docs/current/fr/mod/mod_rewrite.html)
Une introduction au mod_rewrite et aux expressions régulières [ici](http://httpd.apache.org/docs/current/fr/rewrite/intro.html)


Pour cela, il faut activer le mod_rewrite d'apache.
Il faut ensuite ajouter dans la configuration du site les directives suivantes:

    RewriteEngine On
	Options FollowSymLinks

Voici un exemple de fichier .htaccess avec une rêgle simple de rewrite:

    <IfModule mod_rewrite.c>
      Options +FollowSymlinks
	  RewriteEngine on
	  RewriteBase /
	  RewriteRule ^torewrite.html$ rewrited.html [R]
	</IfModule>

Si on essaye d'accéder au fichier torewrite.html, l'url sera réécrit pour afficher rewrited.html

Exercices
=========

Exercice 1
----------
Forcer le www avant le nom de domaine (si on accede a site1.com, rediriger sur www.site1.com). Rediriger avec le code 301

Tips:
- Faire une condition pour tester si le www est présent
- Utiliser le flag R pour le redirect avec le code
- Utiliser la variable d'environnement HTTP_HOST

Exercice 2
----------

Créer un fichier index.php dans un de vos site.
Ce fichier devra afficher les paramètre GET category et l'id de l'article. Voici le code: 

    <?php print $_GET['category']; ?>
	<?php print $_GET['articleId']; ?>

Réécrivez l'url pour qu'elle ai la forme suivante: {baseurl}/{category}/{articleId}

Exercice 3
----------

Empêcher l'utilisation des images depuis un autre site

Tips:
- Utiliser la variable d'environnement HTTP_REFERER
- Utiliser le flag F pour interdir l'accès

Exercice 4
----------

Forcer l'utilisation de l'https pour le site

Tips:
- Utiliser les variable d'environnement SERVER_PORT, HTTP_HOST et REQUEST_URI

Exercice 5
----------
Reprendre la règle de l'exo 4 mais forcer l'https que pour les pages admin.
Exemple d'url d'admin: site1.com/admin/article.html.


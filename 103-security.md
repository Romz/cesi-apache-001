Authentification basique et htpasswd
====================================

[Doc apache Authentification](http://httpd.apache.org/docs/2.2/howto/auth.html)

Control d'accès
===========

Exercice 1
----------

Créer un vhost security.

Ajouter un fichier index.html avec du contenu.
Créer un répertoire admin avec un fichier un fichier html.

Limiter l'accès du repertoire admin qu'à l'IP 127.0.0.1

Créer un dossier blacklisted, et empêcher des ip d'y accéder (Vous pouvez utiliser les ips de vos voisins).

Directives utilisés:
Directory, Order, Deny, Allow

Exercice 2
----------

Reprednre le vhost de l'exercice1.
Ajouter un fichier no-download.txt.
Interdir le téléchargement de se fichier.

Directives à utiliser:
Files, Order, Deny

Exercice 3
----------

Nous allons reprendre le vhost de l'exercice précédant.

Ajouter un repertoire auth, qu'on ne pourra accéder que si on est authentifié.

Créer un fichier de mot de passe à l'aide de la commande htpasswd.

Ajouter l'authentification pour accéder au répertoire

Directives à utiliser:
AuthType, AuthName, AuthBasicProvider, AuthUserFile, Require

HTTPS
=====
[Doc SSL](https://httpd.apache.org/docs/2.4/fr/ssl/ssl_howto.html)

Exo
---
Mettre en place l'https sur le serveur.

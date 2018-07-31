# Angular mise en production avec Apache2 tutoriel
  
Une fois votre application terminé en local lancé la commande suivante :  
``ng build --prod``  
vous pouvez aussi définir un base path avec le flag suivant :  
``ng build --prod --base-href /``  
  
## Etape 1 Sur votre serveur
Près requis :  
apache2  
``apt-get install apache2``  
  
le module mod_rewrite et pres installer avec apache2 visible ici.  
``ls  -l  /usr/lib/apache2/modules/``  
  
## Etape 2 Activer le mod_rewrite
lancée la commande suivante pour activé les modules :  
``a2enmod rewrite``  
  
Ajouter a la fin de la configuration apache apache.conf  
``vim /etc/apache2/apache2.conf``  
  
Ajouter le code suivant :  
```
<ifModule mod_rewrite.c>
	RewriteEngine On  
</ifModule>
```  
Une fois ceci effectuer relancer le service apache2  
``service apache2 restart``  
  
## Etape 3 configuration des sites
Rendez vous dans le dossier ``/etc/apache2/sites-available/``  
  
Ouvrez le fichier **000-default.conf** avec votre éditeur de texte favoris  
  
Mon dossier de destination ici et ``/var/www/html/jguyet``  
  
Ajouter la partie entre  
``#Partie a ajouter ->``  
et  
``#Fin de partie a ajouter``  
  
```
<VirtualHost *:80>
	ServerName www.jguyet.com

	DocumentRoot /var/www/html/jguyet

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

	#Partie a ajouter ->
	<Directory /var/www/html/jguyet>
		RewriteEngine on

        # Don't rewrite files or directories
        RewriteCond %{REQUEST_FILENAME} -f [OR]
        RewriteCond %{REQUEST_FILENAME} -d
        RewriteRule ^ - [L]

        # Rewrite everything else to index.html
        # to allow html5 state links
        RewriteRule ^ index.html [L]
	</Directory>
	#Fin de partie a ajouter
</VirtualHost>
```  
## Partie final
  
Copier le contenue du résultat de la précédente commande ``ng build --prod``  
qui a créer le dossier **dist** copier ceci dans votre dossier de destination sur votre serveur mon dossier de destination ici et ``/var/www/html/jguyet``  
  
`` ls dist `` résultat :  
``portail``  
je renome mon dossier portail en jguyet et le deplace sur mon serveur dans le dossier ``/var/www/html``  
  
ps :  
Conseil ne garder pas le path /var/www trop connue et souvent exploiter.  
  
Merci d'avoir lus.  
Jeremy Guyet.  

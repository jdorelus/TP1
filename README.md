# Procédure d'utilisation du serveur Apache
## Son travail consiste à établir une connexion entre un serveur et les navigateurs des visiteurs du site web tout en délivrant des fichiers entre eux (structure client-serveur)



Afin d'utiliser Apache2, nous avons besoin des services suivants sur le serveur : 

- Mettre à jour le serveur Ubuntu
- LAMP SERVER
- Ajuster le firewall

Une config master contenant les fichiers de configurations par défaut d'Apache a été créer sur Github.
Une branch dev servant à effectuer les ajustements nécessaire aux fichiers de configuration a aussi été créer sur Github.

Sous Ubuntu, vous trouverez la configuration d’Apache dans le répertoire  /etc/apache2/  . La configuration est ensuite découpée en un grand nombre de fichiers. Le fichier principal se nomme  apache2.conf  . Comme c’est souvent le cas pour les fichiers de configuration, les lignes commençant par un  #  sont des commentaires et ce fichier est abondamment commenté. Il y a beaucoup de paramètres comme :

- User ${APACHE_RUN_USER}
- Group ${APACHE_RUN_GROUP}

qui définissent l’utilisateur et le groupe sous lesquels tourne Apache. Vous voyez que les valeurs sont des variables. La valeur réelle est configurée à part dans le fichier  "envvars" , c’est dans ce fichier que vous pourrez la changer.

Vous pouvez définir le fichier de log utilisé pour stocker les logs d’erreur Apache (directive  ErrorLog), le niveau de verbosité des logs (LogLevel) et différents formats de logs définis par la directive  LogFormat. Pour cette dernière, différents formats sont préconfigurés, leur nom se trouve en second paramètre en fin de ligne. 

# Configurez le site web sous Apache

Apache est capable de gérer plusieurs sites web sur la même machine, c’est ce qu’on appelle des hôtes virtuels ou virtual hosts. Dans la requête que fera le client sur la machine, un entête HTTP précisera si le client veut plutôt consulter toto.com ou tata.org qui sont tous les deux hébergés sur la machine. La configuration de chaque virtual host se fait dans un fichier  *.conf  dans  /etc/apache2/sites-available/  . Créez donc le fichier  /etc/apache2/sites-available/01-www.example.com.conf

Tout d’abord, remarquez que la configuration d’un virtual host est comprise dans des balises “type HTML”  <VirtualHost *:80></VirtualHost>  . Le  *:80  de la balise ouvrante indique que le virtual host peut être utilisé sur le port 80 sur toutes les interfaces. Voici ensuite le détail des directives utilisées :

#### ServerName  : précise l’hôte pour lequel cette configuration sera utilisée. Il ne peut y avoir qu’un  ServerName  par VirtualHost

#### ServerAlias  : il ne peut y avoir qu’un ServerName mais par contre, vous pouvez compléter par autant de ServerAlias que vous voulez séparés par des espaces

#### ServerAdmin  : indique une adresse mail de contact et qui peut être affichée sur certains messages d’erreur

#### DocumentRoot  : c’est la racine de l’arborescence de votre site web, là où vous mettrez tous vos fichiers

## Pour pouvoir tester votre site web, il vous reste à :

1- créer la racine de votre arborescence et y déposer un premier fichier .html  :

  $ sudo mkdir /var/www/html/www.example.com/
  $ sudo cp /var/www/html/index.html /var/www/html/www.example.com

2- activez la configuration de votre VirtualHost :

  $ sudo a2ensite 01-www.example.com
  Cette commande crée automatiquement le lien symbolique du répertoire sites-enabled vers le répertoire sites-available.
  
3-echargez maintenant la configuration d’Apache pour prendre en compte vos modifications :

  $ sudo systemctl reload apache2
  
Maintenant si vous ajoutez www.example.com en alias à votre serveur dans le fichier /etc/hosts de votre client.

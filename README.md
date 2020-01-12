# GLPI deploy with Docker

Deploy and run GLPI (any version) with Docker.

Install latest version by default but you can specify the version you want by passing

You can:
- link to an existing database.
- or create a new one easily with docker-compose.

## Deploy GLPI only (no database)

```docker run -it -d -p 80:80 pamtrak06/glpi:9.4.5```

## Deploy with docker-compose

You can deploy GLPI + database by creating 2 files:
- **docker-compose.yml**
- **glpi.env**

### docker-compose.yml

```yml
glpi:
  image: pamtrak06/glpi:9.4.5
  ports:
    - "8090:80"
  links:
    - mysql:db
  env_file:
    - ./glpi.env

mysql:
  image: mariadb
  env_file:
    - ./glpi.env
```

### glpi.env

```env
MYSQL_ROOT_PASSWORD=rootpasswd
MYSQL_DATABASE=glpi
MYSQL_USER=glpi
MYSQL_PASSWORD=glpipaswd
GLPI_SOURCE_URL=https://forge.glpi-project.org/attachments/download/2020/glpi-0.85.4.tar.gz
```

### Run docker-compose

```bash
docker-compose build
docker-compose up
```

### Configure new database

Access your container with HTTP.
Use infos you setup in glpi.env file

![alt tag](https://raw.githubusercontent.com/driket/docker-glpi/master/doc/glpi-db-setup.png)

## FAQ

### Do I have to use Mariadb?

Nope, you can replace with mysql image in docker-compose.yml if prefer

### How to make my database persistent?

Check docker-compose.sample.yml.

Basically, you need to create a data container that won't be destroyed at each deployment.

### How can I install a different version of GLPI?

- Choose a version at: https://forge.glpi-project.org/projects/glpi/files
- Copy URL and paste it in glpi.env:

```
GLPI_SOURCE_URL=https://forge.glpi-project.org/attachments/download/2020/glpi-0.85.4.tar.gz
```

- Run ```docker-compose build```
- Run ```docker-compose up```

## Tests compatibilités

- Tests effectués	Résultats
- Test du Parseur PHP	La version de PHP est au minimum 5.6.0 - Parfait !
- Test des sessions	Le suport des sessions est disponible - Parfait !
- Test de l'utilisation de Session_use_trans_sid	Ok - Les sessions fonctionnent (pas de problèmes de trans_id) - Parfait !
- Test de l'extension mysqli	l'extension mysqli est installée
- Test de l'extension ctype	l'extension ctype est installée
- Test de l'extension fileinfo	l'extension fileinfo est installée
- Test de l'extension json	l'extension json est installée
- Test de l'extension mbstring	l'extension mbstring est installée
- Test de l'extension iconv	l'extension iconv est installée
- Test de l'extension zlib	l'extension zlib est installée
- Test de l'extension curl	l'extension curl est installée
- Test de l'extension gd	l'extension gd est installée
- Test de l'extension simplexml	l'extension simplexml est installée
- Test de l'extension xml	l'extension xml est installée
- Test de l'extension ldap	l'extension ldap est installée
- Test de l'extension Zend OPcache	l'extension Zend OPcache est installée
- Test de l'extension exif	l'extension exif est installée
- Test de l'extension imap	l'extension imap est manquante
- Test de l'extension APCu	l'extension APCu est manquante
- Test de l'extension xmlrpc	l'extension xmlrpc est manquante
- Test de l'extension CAS	l'extension CAS est manquante
- Test de la mémoire allouée	Mémoire allouée > 64 Mio - Parfait !
- Test d'écriture des fichiers de journal	Un fichier a été créé - Parfait !
- Test d'écriture du fichier de configuration	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture de fichiers documents	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Vérification des droits d'écriture du fichier de sauvegarde	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture des fichiers de sessions	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture des fichiers des actions automatiques	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Vérification des droits d'écriture des fichiers graphiques	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture des fichiers de verrouillage	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture des documents des plugins	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture des fichiers temporaires	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture des fichiers de cache	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture de fichiers RSS	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture des fichiers téléchargés	Un fichier et un dossier ont été créés et supprimés - Parfait !
- Test d'écriture de fichiers photos	Un fichier et un dossier ont été créés et supprimés - Parfait !
- L'accès web au répertoire des fichiers est protégé	L'accès web au répertoire des fichiers est protégé


## Password
Les identifiants et mots de passe par défaut sont :

-    glpi/glpi pour le compte administrateur
-    tech/tech pour le compte technicien
-    normal/normal pour le compte normal
-    post-only/postonly pour le compte postonly

## Post install

    Pour des raisons de sécurité, veuillez changer le mot de passe par défaut pour le(s) utilisateur(s) : glpi post-only tech normal
    Pour des raisons de sécurité, veuillez supprimer le fichier : install/install.php

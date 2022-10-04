# Exercice 12 - Docker  : volume et redirection de port

- Évaluation : formative
- Durée estimée : 4 heures
- Système d'exploitation : Ubuntu 20.04 Lts Client 

Note : créez répertoire Exercice12 puis un répertoire par exercice.

## Exercice 1 - Busybox

### Exercice 1.1 - Réseau par défaut (bridge)

- Ouvrez deux terminaux, powershell ou bash, et sur chacun, lancez un conteneur de l'image "busybox" en mode intéractif.
- Trouvez l'adresse IP de chaque conteneur
- Effectuez une requête ICMP d'un conteneur vers l'autre. Qu'observez-vous ?
- Essayez de faire une requête ICMP vers "www.google.com". Qu'observez-vous ?
- Quittez les conteneurs
- Nettoyez vos conteneurs en supprimant ceux créés dans cet exercice.

### Exercice 1.2 - Réseau none

- Lancez un conteneur de l'image "busybox" avec l'option "--network none"
- Essayez de déduire l'adresse IP du conteneur. Qu'observez-vous ?
- Essayez de faire une requête ICMP vers "www.google.com". Qu'observez-vous ?
- Nettoyez vos conteneurs en supprimant ceux créés dans cet exercice.

### Exercice 1.3 - Réseau host ([Linux seulement](https://docs.docker.com/network/host/))

- Lancez un conteneur de l'image "busybox" avec l'option "--network host"
- Essayez de déduire l'adresse IP du conteneur. Qu'observez-vous ?
- Nettoyez vos conteneurs en supprimant ceux créés dans cet exercice.

## Exercie 2 - Nginx - réseau

Dans cet exercice, vous allez devoir utiliser l'image "nginx" afin de créer un serveur web ([Documentation Nginx](https://hub.docker.com/_/nginx)).

### Exercice 2.1 - Nginx - premiers tests

- Sur un premier terminal, lancez en arrière plan (-d) l'image "nginx" sans spécifier de redirection de port avec le nom "mon_nginx" (--name)
- Faite un "docker inspect mon_nginx" afin de trouver l'adresse du conteneur
- Sur un autre terminal, lancez l'image "browsh/browsh" en mode intéractif (-it) ([Documentation browsh/browsh](https://hub.docker.com/r/browsh/browsh)).
- Faites un "Ctrl-l" et tapez l'adresse IP de votre serveur web. Le site par défaut d'Nginx devrait s'afficher. Pour quitter faites un "Ctrl-q" (pour les autres commandes faites un "F1").
- À partir de votre navigateur favori, essayer de naviguer l'adresse IP que vous aviez. Essayez avec l'adresse locale.
- Qu'observez-vous et pourquoi ?
- Nettoyez vos conteneurs en supprimant ceux créés dans cet exercice.

### Exercice 2.2 - Nginx - liaison

- Recréé un conteneur, mais cette fois-ci, faites une liaison entre le port 1234 de l'hôte vers le port 80 du conteneur.
- Refaites les tests de l'exercice précédent
- Nettoyez vos conteneurs en supprimant ceux créés dans cet exercice.

## Exercice 3 - Ngnix - volume

- Créez vous un répertoire temporaire et créez un fichier "index.html". Dans ce fichier, écrivez un code html simple afin d'afficher "Un fichier de mon hôte servi à partir d'un conteneur"
- Par défaut, l'image "Nginx" utilise le répertoire "/usr/share/nginx/html" comme base de recherche de ses fichiers statiques. Vous allez devoir faire un montage afin que votre répertoire soit monté vers ce chemin au moment du démarrage du conteneur
- Validez que vous voyez bien votre page
- Supprimez votre conteneur et lancez en un nouveau en faisant maintenant un montage en lecture seule (:ro : voir documentation)
- Validez que le tout fonctionne toujours
- Nettoyez vos conteneurs en supprimant ceux créés dans cet exercice.

## Exercice 4 - Application MVC core

### Exercice 4.1 - Création d'un projet MVC en .Net core

- Au besoins, procéder à l'installation : 
Installing .NET 6 on Ubuntu 22.04 (Jammy) https://github.com/dotnet/core/issues/7699


- À partir d'un terminal et de la commande dotnet, créez une application :

  - Type : mvc
  - Nom : webapp
- Modifiez le fichier "Views/Home/Index.cshtml" pour y ajouter votre prénom / nom
- Testez que l'application fonctionne localement

<details>
- Création de l'application MVC

```bash
dotnet new mvc -au None -n webapp
cd webapp/
dotnet run
```
- Testez l'application localement: Ouvrez un navigateur avec localhost et le port mentionné pour le dotnet run. 
</details>

### Exercice 4.2 - Création d'un compte docker hub

- Allez sur le site de docker hub et créez-vous un compte
- Créez un dépot nommé "webapp"

### Exercice 4.2 - Création d'une image

- Placez vous à la racine du projet
- Créez le fichier Dockerfile avec le contenu suivant :

```Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
WORKDIR /app

COPY *.csproj ./
RUN dotnet restore

COPY . ./
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "webapp.dll"]
```

- Assurez-vous de bien comprendre chaque ligne du fichier Dockerfile
- Construisez l'image "<votre_docker_id>/webapp" avec la version "latest"

```bash 
docker build --tag [votre_docker_id]/webapp:latest .
```
- Exécutez un conteneur à partir de cette image en liant le port 80 du conteneur au port 5000 de l'hôte
- Validez que le tout fonctionne (attention vous allez être en http)

### Exercice 4.3 - Publication

- Sur un terminal faites un "docker login" et tapez vos informations de connexion
- Faites un "docker push <votre_docker_id>/webapp:latest". La première fois que vous allez effectuer cette commande, il va vous demander vos informations de connexion.
- Allez dans votre compte sur docker hub et validez que votre image est bien présente.
- Supprimez votre version locale
- Démarrez un conteneur à partir de l'image qui est sur docker hub et validez que tout fonctionne
- Demandez le nom de l'image d'un de vos collègues ou utilisez mon image "jpduches/webapp"
- Démarrez un conteneur avec cette image et validez que tout fonctionne
- Nettoyez vos conteneurs en supprimant ceux créés dans cet exercice.

## Exercice 5 - MySQL

### Exercice 5.1 - MySQL - Base

- Si ce n'est déjà fait sur votre environnement docker, installez MySQL Workbench (https://dev.mysql.com/downloads/file/?id=497518)
- Lancez un conteneur à partir de l'image mysql :
  - Nom : mysql
  - Spécifiez que le container doit être détaché (-d)
  - Fournissez la valeur "Passw0rd" pour la variable d'environnement "MYSQL_ROOT_PASSWORD" (Voir utilisation du -e dans la documentation)
  - Liez le port 3306 local vers le port 3306 du container
- Avec MySQL Workbench, validez que le tout marche :
  - Connectez-vous
  - Créez la base de données "test" (create database test;)
  - Validez que la base de données est bien existantes (show databases;)
- Arrêtez et supprimez le conteneur "mysql"
- Recréez un nouveau conteneur mysql et connectez-vous de nouveau à MySQL et listez les bases de données. Que constatez-vous et pourquoi ?
- Supprimez votre conteneur

<details>
    <summary>Solution</summary>

```bash
docker run -d --rm --name mysql -e MYSQL_ROOT_PASSWORD=Passw0rd -p 3306:3306 mysql
```

</details>

### Exercice 5.2 - MySQL - Volume

- Créez vous un répertoire qui va contenir vos données pour MySQL
- Réutilisez votre ligne de commande pour créer un conteneur MySQL avec les mêmes caractéristiques que précédemment et ajoutez le montage d'un volume qui lie le répertoire de données que vous venez de créer au répertoire "/var/lib/mysql"
- Avec MySQL Workbench, validez que le tout marche :
  - Connectez-vous
  - Créez la base de données "test" (create database test;)
  - Validez que la base de données est bien existantes (show databases;)
- Arrêtez et supprimez le conteneur "mysql"
- Recréez un nouveau conteneur mysql et connectez-vous de nouveau à MySQL et listez les bases de données. Que constatez-vous et pourquoi ?
- Supprimez votre conteneur

<details>
    <summary>Solution</summary>

```bash
docker run -d --rm --name mysql -e MYSQL_ROOT_PASSWORD=Passw0rd -p 3306:3306 -v /Users/pfl/tmp/msyql:/var/lib/mysql mysql
```

</details>

## Exercice 6 - Wordpress - On fait parler plusieurs conteneurs ?

### Exercice 6.1 - Wordpress - Version manuelle

- Créez-vous un nouveau répertoire pour stocker une autre installation de bases de données
- Créez un nouveau conteneur avec les options précédentes en modifiant le chemin du montage ainsi qu'en ajoutant les variables d'environnement suivantes :
  - MYSQL_DATABASE=wordpress
  - MYSQL_USER=wordpress
  - MYSQL_ROOT_PASSWORD=Passw0rd
  - MYSQL_PASSWORD=Passw0rd
- Avec MySQL Workbench, essayez de vous connecter à la base de données wordpress avec le nom d'utilisateur "wordpress" et le mot de passe "Passw0rd"
- Cherchez l'adresse IP du conteneur mysql
- Créez un conteneur à partir de l'image wordpress en liant le port locale 8080 au port 80 et avec les variables d'environnement suivantes :
  - WORDPRESS_DB_HOST=172.17.0.2
  - WORDPRESS_DB_USER=wordpress
  - WORDPRESS_DB_PASSWORD=Passw0rd
  - WORDPRESS_DB_NAME=wordpress
  - WORDPRESS_TABLE_PREFIX=wp_
- Testez votre site web et vérifiez que cela fonctionne
- Que pensez-vous de toutes ces manipulations ?

<details>
    <summary>Solution</summary>

```bash
docker run -d --rm --name mysql -e MYSQL_ROOT_PASSWORD=Passw0rd -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=Passw0rd -p 3306:3306 -v /Users/pfl/tmp/msyql:/var/lib/mysql mysql:5.7

docker run --rm --name wordpress -d -e WORDPRESS_DB_HOST=172.17.0.2 -e WORDPRESS_DB_USER=wordpress -e WORDPRESS_DB_PASSWORD=Passw0rd -e WORDPRESS_DB_NAME=wordpress -e WORDPRESS_TABLE_PREFIX=wp_ -p 8080:80 wordpress
```

</details>


## Pour vérification
Remettre une capture d’écran de votre site WordPress et du ps de votre contenueur dans l'espace travaux, exercice 12 sur LÉA.
N'oubliez pas, je dois pouvoir identifier votre VMs.

### Exercice 6.2 - Wordpress - Un début d'orchestration ?

- Lisez et exécutez les instructions présentes à la page suivante : https://docs.docker.com/compose/wordpress/
- Quels sont les avantages d'un tel système ?



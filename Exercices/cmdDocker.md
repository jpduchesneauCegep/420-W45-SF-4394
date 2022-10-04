# Voici une liste des commandes de base de Docker 


## Commandes de gestion  :
```bash
$docker container ls                       # List all running containers
$docker container stop <hash>              # Gracefully stop the specified container
$docker stop $(docker ps -a -q)
$docker container kill <hash>              # Force shutdown of the specified container
$docker container rm <hash>                # Remove specified container from this machine
$docker container rm $(docker container ls -a -q)   # Remove all containers
$docker image ls -a                        # List all images on this machine
$docker image rm <image id>                # Remove specified image from this machine
$docker image rm $(docker image ls -a -q)  # Remove all images from this machine
$docker login                              # Log in this CLI session using your Docker credentials
$docker tag <image> username/repository:tag  # Tag <image> for upload to registry
$docker push username/repository:tag      # Upload tagged image to registry
$docker run username/repository:tag       # Run image from a registry


```


## Création d'images
```bash
$docker build -t vieux/apache:2.0 .
$docker build -t friendlyhello .             # Create image using this directory's Dockerfile
$docker build -f /path/to/a/AutreFichier .   # Utilisez un autre fichier que Dockerfile.
```

## Démarrer les conteneurs : 

```bash
$docker run --name test1 -it busybox
$docker run -p 4000:80 friendlyhello          # Run "friendlyname" mapping port 4000 to 80
$docker run -d -p 4000:80 friendlyhello        # Same thing, but in detached mode
$docker run -p 3307:3306 -e MYSQL_ROOT_PASSWORD=Passw0rd -d --name MonMySQL  mysql:latest
# Rentrer dans le container avec un shell :
$docker container exec -it mon-container bash 
```
## Gestion des réseaux : 
```bash
$docker network ls                          # Lister les réseaux de Docker
$docker network inspect <NETWORK_NAME>      # Avoir les informations détaillées sur un réseau.
                                            # Renvoi aussi les conteneurs qui sont liés par ce réseau.
$docker network create <NETWORK_NAME>       # Création d'un nouveau réseau auquel des nouveaux contenurs pourront s'attacher. 
                                            # Le driver par défaut est bridge
# Pour utiliser un nouveau réseau :
$docker container run --name webserver --network new-network -d nginx
# Pour déplacer un conteneur vers un réseau : 
$docker network connect <NETWORK_NAME> <CONTAINER_NAME>
# Pour le déconnecter :
$docker network disconnec <CONTAINER_NAME>
```
**Attention : Pour utiliser le service DNS de Docker, il faut nécessairement créer de nouveaux réseaux. Le réseau par défaut bridge n'est pas géré par le service DNS de Docker.**

## Utilisation des volumes :

```bash
$docker volume ls # Lister les volumes
$docker volume create <VOLUME_NAME>
$docker volume rm <VOLUME_NAME/ID>
$docker volume inspect <VOLUME_NAME/ID>
# Exemple d'utilisation d'un volume (Bind Mounting) sur Linux
$docker container run \
    -v /tmp/volumeNginx:/usr/share/nginx/html \
    -p 11234:80 -d --name monNginxLinux nginx
# Exemple d'utilisation d'un volume (Bind Mounting) sur Windows
$docker container run \
    -v d:\\DockerVolume\\VolumeNginx:/usr/share/nginx/html \
    -p 1234:80 -d --name -d monNginxWin nginx

$docker container run -v d:\\DockerVolume\\VolumeMySQL:/var/lib/mysql \
    --rm -d --name MonMysql -e MYSQL_ROOT_PASSWORD=Passw0rd \
    -p 3307:3306 mysql
#En lecture seule
$docker run \
    -v /tmp/volumeNginx:/usr/share/nginx/html:ro \
    -p 1234:80 --name monNginxLinux nginx
## Atention ##
$docker volume prune #Attention : supprimer tous les volumes qui ne sont pas utilisés par Docker. Donc disparition des données.

```

## Création de WordPress :
```bash
$docker run -d --rm --name mysql \
    -e MYSQL_ROOT_PASSWORD=Passw0rd -e MYSQL_DATABASE=wordpress \
    -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=Passw0rd \
    -p 3306:3306 -v /Users/pfl/tmp/msyql:/var/lib/mysql mysql
    
# Attention : la version latest de mysql provoque des erreurs de connexions. Pour faciliter votre apprentsisage, utiliser plutôt mysql:5.7

$docker run --rm --name wordpress -d \
    -e WORDPRESS_DB_HOST=172.17.0.2 -e WORDPRESS_DB_USER=wordpress \
    -e WORDPRESS_DB_PASSWORD=Passw0rd -e WORDPRESS_DB_NAME=wordpress \
    -e WORDPRESS_TABLE_PREFIX=wp_ -p 8080:80 wordpress
```

# Docker-Compose
```bash
$docker-compose --help         # pour voir les autres commandes
$docker-compose up -d          # mode détaché
$docker-compose ls
$docker-compose top            # affiche les processus en cours d'exécution sur mes conteneurs`
$docker-compose run web env    # voir les variables d'environnements du conteneur web
$docker-compose stop           # arrêter vos services
$docker-compose down           # tout démonter, en supprimant entièrement les conteneurs
$docker-compose down --volumes # supprimer également les données.
```

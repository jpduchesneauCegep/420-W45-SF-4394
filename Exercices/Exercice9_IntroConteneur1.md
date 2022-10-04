# Exercice 9 - Docker : Prise en main des conteneurs docker

- Évaluation : formative
- Durée estimée : 2 heures
- Système d'exploitation : Ubuntu Client 


## Préambule - testez votre installation

### Sous Windows :

- Sous Windows : utilisez le terminal Windows, lancez une console Ubuntu 
  - Testez la commande "docker". Si le programme ne se lance pas, installez-le (package docker)
  - Si vous avez un qui parle de l'intégration de WSL
    - Ouvrez une console powershell, et tapez "wsl -l -v". Si votre "Ubuntu" utilise la version 1, tapez la ligne de commande suivante "wsl --set-version Ubuntu 2" (L'opération dure quelques minutes)
    - À partir de la barre d'état système (system tray), sélectionnez l'icône de docker en faisant un clic droit, puis choisissez "Settings".
    - Une fois le panneau de configuration de docker ouvert, sélectionnez "Ressources" puis "WSL Integration". Si l'option "Ubuntu" n'est pas activée, activez-la et cliquez sur "Apply & Restart"
    - Une fois appliquée, retestez
  

  <details>
    <summary>Qu'est-ce que WSL</summary>
Depuis la version 2.2.0.3 de Docker Desktop pour Windows, Docker offre la possibilité d’utiliser WSL2 (Windows Subsystem for Linux 2) afin d’éviter l’obligation de créer une machine virtuelle dans Hyper-V. Avant de pouvoir dire à Docker d’utiliser ce système, il est nécessaire de configurer Windows pour activer WSL2.
La version minimum de Windows 10 pour utiliser WSL 2 est Windows 10 Build 18917.
</details>

### Sous Linux : 
- Sous Linux, lancez un terminal
  - Testez la commande "docker". Si le programme ne se lance pas, installez le package docker

<details>

   <summary>Installation de docker</summary>


```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

</details>

**Si aucun message d'erreur ne s'affiche, vous pouvez débuter l'exercice 1.**


## Exercice 1 - Hello world docker !

- À partir d'un terminal ouvert précédemment, vous allez lancer votre premier conteneur à partir de l'image docker "hello-world" :
  - Faites un "docker image ls". Que constatez-vous ?
  - Tapez la ligne de commande "docker run hello-world"
  - Recherchez la signification des dernières lignes sur la documentation en ligne de docker
- Faites un "docker ps". Voyez-vous quelque chose ?
- Faites un "docker ps -a". Que constatez-vous ?
- Affichez la liste des couches de commandes qui ont été appliquées à l'image "hello-world" :
  - Cherchez la bonne commande dans la documentation de docker dans la section "image"
  - Vous devriez avoir un affichage similaire au suivant :


```bash
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
bf756fb1ae65        8 months ago        /bin/sh -c #(nop)  CMD ["/hello"]               0B                  
<missing>           8 months ago        /bin/sh -c #(nop) COPY file:7bf12aab75c3867a…   13.3kB        
```


- Toujours à partir de l'option image de la commande docker et de la documentation, cherchez comment supprimer l'image hello-world. Vous devriez rencontrer une erreur pourquoi ?
- À partir de l'affichage obtenu à partir de la commande "docker ps -a", cherchez la suite de commandes à exécuter pour supprimer le conteneur et l'image
- Validez que l'image n'est plus présente


<details>
    <summary>Affichage de la liste des couches</summary>


```bash
docker image history hello-world
```


</details>


## Exercice 2 - Utilisation de l'image du serveur Web Nginx avec Docker


- Sous Windows : utilisez le terminal Windows, lancez une console Ubuntu ; sous Linux lancez un terminal
- Taper la commande docker suivante :
  
  ```bash
  docker container run --publish 80:80 nginx
  ```

- - **Attention !** Si vous utilisez votre serveur Ubuntu et que le service nginx de l'exercice précédent est toujours en marche, vous allez rencontrer ce message&nbsp;:
- - ```Error starting userland proxy: listen tcp4 0.0.0.0:80: bind: address already in use.```
- - Vous devez simplement arrêter le service précédent :
- - ```sudo systemctl stop nginx```
- - Maintenant, essayez à nouveau.


- Cette commande démarre un nouveau conteneur utilisant nginx téléchargée depuis DockerHub.
- La partie **publish 80:80** expose le port local 80 de la machine hôte, et redirige tout le trafic à l'application exécutable dans un comeneur sur sont port 80.
- Vérifier dans le navigateur l'URL localhost, vous devriez avoir la page de nginx.
- Regarder votre terminale, chaque tentative de connexion est affichée.
- Faite ctrl+c pour reprendre la main de votre terminale.
<details>
Sur Linux ctrl+C éteint le conteneur. Sur Windows il fonctionne toujours.
</details>
- Pour exécuter le conteneur en arrière-plan, utilisez l'option "--detach" :
  
  ```bash
  docker container run --publish 80:80 --detach nginx
  ```
- Vérifier dans le navigateur l'URL localhost, vous devriez avoir la page de nginx.
- Vous ne voyez plus les tentatives de connexion sur le serveur Web.
- Faites un "docker container ls". Voyez-vous quelque chose ?
- Faites un "docker info". Vous voyez les images et les conteneurs.



## Exercice 3 - Manipulation des conteneurs 


- Pour voir tous les conteneurs, faites la commande suivante :
  ```bash
  docker container ls -a
  ```
  - Vous devrez pouvoir voir votre conteneur son id, l'image d'origine ainsi que  d'autres informations. 
  - Le dernier champ "Names est un nom aléatoire donné par Docker lors de la création, car vous ne l'aviez pas spécifié.
  - Au niveau du statut que remarquez-vous ?
  - Si vous faite la même commande, mais sans l'option -a vous allez voir que les conteneurs démarrés.
  - Démarrez un conteneur avec son  propre nom :
   ```bash
  docker container run --publish 8080:80 --detach --name nginx_webserver nginx
  ```
  - À partir de l'aide, trouvez la signification de l'option --publish 8080:80. 
  ```bash
  docker container run --help
  ```
  - Consulter aussi la documentation en ligne : https://docs.docker.com/engine/reference/commandline/run/


  - Faite un "doker container ls -a" est vous allez voir les deux conteneurs nginx.
  - Vérifier les deux serveurs dans le navigateur avec les URL suivants :
    localhost et localhost:8080


  - Pour arrêter un conteneur : 
  ```bash
  docker container stop <name/id>
   ```
  - Pour supprimer un conteneur vous utiliser la cmd docker conteneur rm name ou id
  ```bash
  docker container rm nginx_webserver 
  ```
  - Faite a nouveau "doker container ls -a" est vous allez voir le changement.


  - Démarrez plusieurs serveurs de la façon suivante :
   ```bash
  docker container run --publish 8081:80 --detach --name nginx_webserver1 nginx
  docker container run --publish 8082:80 --detach --name nginx_webserver2 nginx
  docker container run --publish 8083:80 --detach --name nginx_webserver3 nginx
  ```
  - Un petit truc pour supprimer tous les conteneurs (Linux ou Git Bash sur Windows) :
   ```bash
   docker container stop $(docker ps -a -q)
   docker container rm $(docker ps -a -q)
   ou
   docker container rm -f $(docker ps -a -q)
  ``` 
    - Pratiquez les commandes suivantes sur les conteneurs : 
   ```bash
   docker container run --publish 80:80 --detach --name nginx_webserver nginx
    ```
    - Allez actualiser l'URL dans le navigateur
   ```bash 
   docker container logs nginx_webserver
   docker container top nginx_webserver
  ``` 
  - Vous avez donc accès au log est aux processus du conteneur.

## Pour vérification
Remettre une capture d’écran des deux dernières commandes dans l'espace travaux, exercice 9 sur LÉA.
N'oubliez pas, je dois pouvoir identifier votre VMs.

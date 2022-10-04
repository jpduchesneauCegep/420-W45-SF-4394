# Cours 9 : 15 juin 2022

## Théorie : Architecture informatique    


## Exercice 8 - Installation d'un environnement de tests sur site
    - Attention à vos mot de passe.
    - Script d'espace
    - Les fichiers de configuration 
        - commentaire vs déclaration
    Exemple : 
    - Éditez le fichier pour qu'il correspondent aux lignes suivantes :
```bash
server {
        listen 80;
        server_name test.example.com;
        root /var/www/html;
        index index.php;



    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
    }
}
```
    - Le ficher info.php

## TP1 : Procédure d'installation d'un serveur
   
    - 3 x 1 heure en classe.
    - Objectif :
        - Dépôt git
        - Markdown
        - Procédure pourquoi ?

    - https://classroom.github.com/a/NVbaQ6QW

## Exercice 7 Administration système;
    - 2 heures
    - Aucune remise, mais très important.
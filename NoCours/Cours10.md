# Cours 10 : 17 juin 2022

## Information sur l'examen du 23 juin :

- Je fournis la VMs pour l'examen

- partie 1 – Question de cours (8 pt) ~ 30 mins
- partie 2 - Adminstration système (8,5 points) ~ 1h
- partie 3 - Gestion des utilisateurs et manipulations du système de fichiers (8,5 points) ~ 30 mins

Pour un total de 25 points qui représente 15% de la session.

---
Aujourd'hui :

## Retour sur l'exercice 8

- Localhost et votre Ip la différence.
- La version de Php
        - c'Est le CGI(voir note de référence) 

        ```bash
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
         }
        ```
        
 - systemctl restart/reload nginx quant l'utiliser.

## Docker 
##  Exercice 9 - Docker : Prise en main des conteneurs Docker

---
Références :
 - CGI : https://fr.wikipedia.org/wiki/Common_Gateway_Interface
 - Nginx : https://nginx.org/en/docs/
 - fichier hosts : https://fr.wikipedia.org/wiki/Hosts
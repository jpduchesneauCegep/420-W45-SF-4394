# Exercice 2 – Installation d'un serveur Ubuntu

### Informations
- Évaluation : formative
- Type de travail : individuel
- Durée : 1 heures
- Système d'exploitation : Linux Ubuntu serveur 22.04
- Environnement : virtuel, vsphere.

### Objectifs :

Cet exercice a pour objectifs :
- D'installer un environnement de test

Dans cet exercice, vous allez vous installer un environnement de test. Vous allez utiliser un serveur Linux Ubuntu.

## Pour vérification

Vous devez remettre dans un document un comparatif entre la VM cliente et la VM serveur au niveau suivant : 
| Item |    Valeur Client   | Valeur serveur
| ---- |    -------------   | ---------------
Processeur |           |
Mémoire vive |
Nb de processus |
% utilisation des partitions

De plus, vous devez mettre dans votre document une capture d’écran de votre serveur avec un shell d’ouvert ayant les commandes suivantes exécutées à l'intérieur :

```bash
# Les informations à l'ouverture de la session 
git version
docker --version
docker-compose --version
```
## Partie 1 : Installation d’Ubuntu 
Dans cette partie, vous allez installer un serveur Ubuntu selon les spécifications données.

### Étape 1 : Installation
a - En utilisant l’ISO ubuntu-22.04-live-server-amd64.iso, créez une machine virtuelle selon les spécifications suivantes :

    Dossier dans vSphere : DFC DS/VM DFC/E22_4393_420W44_ITV_JPD/
    Nom de la VM : E22_4393_420W44_Ub_srv_Initiale_#Matricule
    CPU : 2
    Mémoires : 2 Go
    Disque dur : 2 disques, 20 Go chacun en partitionnement dynamique 
    Carte réseau : VM DFC2 (réseau 10.100.2.0)
    CD/DVD : ISO ubuntu-22.04-live-server-amd64.iso
    

b -	Une fois la VM créée, lancez la VM et installez le serveur Ubuntu selon les spécifications suivantes :
    
    Pour vous déplacer dans les fenêtres, utiliser votre touche tabulation. Pour sélectionner un item, appuyez sur la touche Entrée(enter). Pour cocher un item cliqué sur la barre d'espacement.

    - Clavier : French (Canada) ou English (US)
    - Type d’installation : Ubuntu Server
    - Connexions réseau : DHCPv4
    - Configurer le proxy : Laissez vide et appuyer Terminé.
    - Miroir d'archive Ubuntu : ne rien changer et appuyer sur Terminé.

    - Configuration de stockage guidée :
        Utiliser un disque entier
             Laissé Set up this disk as an LMV group

            Confirmer l'action même s’il est en rouge.

    - Configuration profil :
        
        Attention pour le nom d'utilisateur et mot de passe je vous recommande d'utiliser le même que votre compte principal sur votre client.

        Votre nom : ;-)
        Le nom de la machine : srv-web-[matricule]
        Nom d'utilisateur : a votre chois
        Mot de passe : a votre chois

   - Configuration SHH : Cochez Installer le serveur OpenSSH 
      (cliquez sur la barre d'espace)
        N'importez pas la clé SSH.  

    -  Featured Server Snaps ne cocher rien et cliqué sur Terminé

    - Patientez! L'INSTALLATION EST EN COURS.
 
 
Une fenêtre vous proposant de redémarrer des services s'affichera. Laissez les services déjà cochés et continuez. Enfin, redémarrez votre serveur&nbsp;:
```
reboot
```

## Partie 2 : Première utilisation de votre machine

Démarrez votre VM et connectez-vous.

**Remise** : Garder une copie de l'écran d'accueil pour votre remise.

### Mise à jour de votre Ubuntu :

Si vous ne l'avez pas encore fait, lors de la connexion, faites une mise à jour de votre système : 

```
sudo apt update
sudo apt ugrade
```


### Vérification des partitions et du système

- Ouvrez un terminal et tapez les commandes demandées :

```
df -h
```
**Remise** : Prenez en note la valeur importante pour votre remise.

Ici il y a deux partitions qui nous intéressent davantage :

```bash
# La partition LVM probablement : 
/dev/mapper/vgubuntu-root
# La partition de boot/efi probablement :
/dev/sda2
```
Remarquer les valeurs d'utilisation.



### Gestion des partitions, la commande lsblk :

Un périphérique de bloc est un fichier faisant référence à un périphérique. Les périphériques peuvent être des disques durs, des disques SDD, des disques RAM, etc. Les fichiers de périphérique de bloc se trouvent dans le répertoire /dev.

-  Taper la commande suivante, et avec la page man vérifier les informations données sur la commande.

```bash
lsblk
# sda, ce sont les données sur votre premier disque dur.

#sdb, ce sont les données sur votre deuxième disque dur non utilisé. Nous allons le configurer dans un autre exercice.
```
![Lsblk](images/lsblk.jpg)



### Mémoire RAM, processeur et processus

Dans un terminal, taper la commande top
```
top 
```
**Remise** : Prenez en note la valeur importante pour votre remise.

Pour quitter top tapez q.

## Partie 3 : Ajout de services et configurations

Dans cette partie, vous allez ajouter les logiciels wget, curl, git, Visual Studio Code et Docker et faire quelques configurations supplémentaires.


### Étape 2 : Ajout de logiciels de base

a- Nous allons installer les outils de base wget, curl et git et vim.

```bash
sudo apt install wget curl git vim -y
```

### Étape 3: Ajout de Docker
a.	Il existe plusieurs manières d’installer Docker. Nous allons utiliser le script officiel de Docker pour l’installer (vous pouvez consulter le script à https://get.docker.com).

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
sudo usermod -aG docker VotreNomUtilisateur
sudo docker version 
```
Avec ces commandes, vous avez installé Docker, vous avez ajouté votre utilisateur au groupe Docker (ça évite de toujours utiliser sudo devant vos commandes Docker, par contre ce n’est pas une bonne habitude de sécurité) et vous avez vérifié que Docker est bien installé.

Vous devez relancer votre session  pour que votre utilisateur soit inclus dans le groupe docker.

b.	Plusieurs des exercices se feront sous Docker. Vous pouvez également installer Docker sous votre Windows ou Mac.
Docker sous Windows : https://docs.docker.com/desktop/windows/install/.

Docker sous MAC : https://docs.docker.com/desktop/mac/install/ .

c.	Nous allons également installer Docker Compose.

Pour Docker Compose, aller au site Web https://docs.docker.com/compose/install/. Sous «Install Compose», cliquer sur Linux et suivre les instructions d’installation.


## Lancez les commandes nécessaires à votre remise :

```bash
# Les informations à l'ouverture de la session 
git version
docker --version
docker compose version
```
## Compétences développées


00Q1 - Effectuer l’installation et la gestion d’ordinateur :

    2 installer le système d’exploitation.
    3 Installer des applications
    4 Effectuer des tâches de gestion du système d’exploitation.

00SF - Évaluer des composants logiciels et matériels.

    1 Rechercher des composants logiciels et matériels.
    2 Formuler des avis sur les composants logiciels et matériels.

Note : les compétences sont développées en partie.

## Références
- Ubuntu : https://ubuntu.com/download/desktop

- LVM : https://access.redhat.com/documentation/fr-fr/red_hat_enterprise_linux/6/html/logical_volume_manager_administration/index

- zsh : http://https://kifarunix.com/install-and-setup-zsh-and-oh-my-zsh-on-ubuntu-20-04/ 

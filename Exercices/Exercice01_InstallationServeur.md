# Exercice 1 – Installation du serveur Ubuntu

### Informations
- Évaluation : formative
- Type de travail : individuel
- Durée : 2 heures
- Système d'exploitation : Linux Ubuntu 22.04
- Environnement : virtuel, vsphere.

### Objectifs :

Cet exercice a pour objectifs :
- D'intaller un environnement de travail

Dans cet exercice, vous allez vous installer un environnement de travail pour la majorité des exercices et des travaux pratiques du cours. Vous allez utiliser un client Linux Ubuntu comme étant un client et un serveur.

## Partie 1 : Installation d’Ubuntu 
Dans cette partie, vous allez installer un serveur Ubuntu selon les spécifications données.
### Étape 1 : Installation
a - En utilisant l’ISO ubuntu-22.04-desktop-amd64.iso, créez une machine virtuelle selon les spécifications suivantes :

    Dossier dans vSphere : DFC DS/VM DFC/E22_4393_420W44_ITV_JPD/
    Nom de la VM : E22_4393_420W44_Ub_Cli_Initiale_#Matricule
    CPU : 2
    Mémoires : 4 Go
    Disque dur : 2 disques, 20 Go chacun en partitionnement dynamique 
    **Attention**: Vous devrez terminer la configuration et la modifier avant le démarrage pour ajouter le deuxième disque dur.
    Carte réseau : VM DFC2 (réseau 10.100.2.0)
    CD/DVD : ISO ubuntu-22.04-desktop-amd64.iso
    Carte vidéo : Activer le support 3D
À la maison : Utiliser les mêmes informations pour les CPUs et le disque. Vous pouvez utiliser la version d’Ubuntu que vous désirez, mais au minimum 20.04.

b -	Une fois la VM créée, lancez la VM et installez le serveur Ubuntu selon les spécifications suivantes :

    Clavier : French (Canada) ou English (US)
    Type d’installation : Minimal
    Type d'installation : Fonctions avancés et utiliser LVM pour la nouvelle installation d'Ubuntu. Acceptez le partititionnement proposé, sélectionnez le premier disque et continuer.
    Utilisateur : à votre choix, sinon user01
 

## Partie 2 : Première utilisation de votre machine

Démarrez votre VM et connectez-vous.

### Mise à jour de votre Ubuntu :

Si vous ne l'avez pas encore fait, lors de la connection, faite une mise à jour de votre système : 

```
sudo apt update
sudo apt upgrade
```
**Question** : Qu'elle est la différence entre ces deux commandes ?


### Générer une clé SSH

Générer une clé SSH sur votre machine local et vas vous permettre d'utiliser la clé générée pour se connecter à un serveur distant, comme par exemple GitHub

Pour ce faire taper la commande suivate à la racine de votre dossier usager. 

```bash
#Vérifier que vous êtes à la racine de votre $USER
pwd
#Sortie attendu votre répertoire usager:
/home/jpduches
# Générer la clé SSH en changeant le courriel pour le votre :
ssh-keygen -t ed25519 -C "email@example.com"
# Une fois que vous aurez entré cette commande, quelques questions apparaîtront:
Enter file in which to save the key (/home/xxxxx/.ssh/id_rsa):
```
En général, il est recommandé de laisser tel quel (appuyez sur ENTRÉE sans taper quoi que ce soit) afin que le générateur de clé puisse créer la clé à l’emplacement par défaut. Les questions qui apparaîtront ensuite :
```
Enter passphrase (empty for no passphrase):
```
et
```
Enter
```
Pour des raisons de commodité, nous laissons ceux-ci vides. De cette façon, après avoir configuré la clé avec votre serveur distant, vous n’aurez pas besoin d’utiliser un mot de passe pour vous connecter. Vous entrerez ssh utilisateur@ipduserveur et serez immédiatement connecté. Mais si vous avez besoin de plus de sécurité, vous pouvez entrer une phrase secrète (passphrase) dans cette section. Si vous choisissez cette option, vous devrez entrer le mot de passe chaque fois que vous vous connectez au serveur distant.

L’image générée est l’empreinte digitale de la clé.

**IMPORTANT! Il y a deux clés qui ont été créées ici ( PRIVEE et PUBLIQUE ): id_ed25519 et id_ed25519.pub. Prenez soin du fichier nommé id_rsa (c’est la clé privée ), gardez la sur votre ordinateur et ne le donnez à personne. Ils sont situé dans le dossier caché .ssh de votre répertoire home**

```bash
ls -al .ssh
cat .ssh/id_rsa.pub
```

L’autre fichier, id_ed25519.pub est la clé publique, elle doit être envoyé  sur votre serveur distant. Par exemple, dans votre profil GitHub dans Setting -> SSH and GPG Keys -> New SSH Key. Dans ce cas vous faite une copie du contenu id_rsa.pub et vous le coller dans l'espace dans le champs prévu à cette effet  (key) et donnez un nom significatif à votre clé dans Tile:

![Git Hub SSH](images/GitHubSSH.jpg)

Par la suite, vous pourrez vous connectez avec le mode ssh à tous vos dépôt github.

### Vérification des partitions et du système

- Ouvrez un terminal et taper les commandes demandées :

```
df -h
```
**Question** : À quoi sert la commande df et son paramètre -j ?

Utiliser la page man pour répondre

```
man df
```

Cette commande vous permet d’avoir l’espace utilisé et disponible pour chaque partition du système de fichier qui est monté.

Vous devriez pouvoir identifier vos partitions créées lors de l’installation. Le paramètre -h permet l’affichage à la puissance 1024 (exemple 1023M ; 1024 donnant 1Go.).

Ici il y a deux partition qui nous intéresse d'avantage :

```bash
# La partition LVM probablement : 
/dev/mapper/vgubuntu-root
# La partition de boot/efi probablement :
/dev/sda2
```
Remarqué les valeurs d'utilisation.



### Gestion des partitions, la commande lsblk :

Un périphérique de bloc est un fichier faisant référence à un périphérique. Les périphériques peuvent être des disques durs, des disques SDD, des disques RAM, etc. Les fichiers de périphérique de bloc se trouvent dans le répertoire /dev.

-  taper la commande suivante, et avec la page man vérifer les informations données sur la commande.

```bash
lsblk
# sda, ce sont les données sur votre premier disque dur.

#sdb, ce sont les données sur votre deuxième disque dur non utilisé. Nous allons le configurer dans un autre exercice.
```
![Lsblk](images/lsblk.jpg)

Il est également possible d’avoir des informations en mode graphique. Pour ce faire, utiliser l’outil disk. 

— Cliquez sur chaque partition pour pouvoir accéder aux informations. Vérifier les points de montage.


### Mémoire RAM, processeur et processus

Dans un terminal, taper la commande top
```
top 
```
La commande top vous permet d’avoir une vue dynamique en temps réel du processus, de la mémoire et du processeur.

Pour quittez top taper q.

## Partie 3 : Ajout de services et configurations

Dans cette partie, vous allez ajouter les logiciels wget, curl, git, Visual Studio Code et Docker et faire quelques configurations supplémentaires.


### Étape 2 : Ajout de logiciels de base

a- Nous allons installer les outils de base wget, curl et git. Pour ceux qui le désire, vous pouvez également ajouter vim.

```bash
sudo apt install wget curl git vim -y
```

b. Installer Visual Studio Code : https://code.visualstudio.com/
suivez les instruction pour Linus Ubuntu.

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

### Étape 4 : Configuration supplémentaire

Nous allons faire une configuration supplémentaire, on va installer le shell zshell, un shell plus fonctionnel que le bash.

**Note** : cette configuration supplémentaire est optionnelle.
a.	Installer le shell zsh et vérifier son installation.

```bash
sudo apt install zsh -y
zsh --version
```

b.	Changez votre shell par défaut pour zsh.

```bash
chsh -s $(which zsh)
```


Pour voir le changement, vous devez ouvrir une nouvelle fenêtre de commandes. À la fenêtre ci-dessous, répondez 2 pour peupler le fichier ~/.zshrc.

[![Image](images/zsh1.png)]

c.	Pour pouvoir personnaliser l’apparence vous devez installer oh-my-zsh.

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

La configuration de oh-my-zsh se fait par le fichier $HOME/.zshrc et les fichiers de configurations se trouve sous $HOME/.oh-my-zsh/. Vous pouvez installer le thème que vous voulez, voir le lien de référence.

d.	Un thème intéressant que je recommande est Powerlevel10k. Vous devez installer les polices du thème avant d’installer le thème.

```bash
cd $HOME/Downloads

wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf

wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf

wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf

wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf

```
Pour installer les polices, vous ouvrez le répertoire (dossier) Downloads et vous double-cliquez sur chacune des polices.

Voici les commandes pour installer le thème.

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc
```

## Pour vérification
Remettre une capture d’écran de votre Linux avec un shell d’ouvert ayant les commandes suivantes exécutées à l'intérieur :
```bash
git version
docker --version
docker compose version

**Attention** Si vous déposé plus d'une version, je ne corrige que le dernier dépôt fait sur LÉA dans travaux.

```
## Compétences développées


00Q1 - Effectuer l’installation et la gestion d’ordinateur :

    2 installer le système d’exploitation.
    3 Installer des applications
    4 Effectuer des tâches de gestion du système d’exploitation.

00SF - Évaluer des composants logiciels et matériels

    1 Rechercher des composants logiciels et matériels.
    2 Formuler des avis sur les composants logiciels et matériels.

Note : les compétences sont développées en partie.

## Références
- Ubuntu : https://ubuntu.com/download/desktop

- LVM : https://access.redhat.com/documentation/fr-fr/red_hat_enterprise_linux/6/html/logical_volume_manager_administration/index

- zsh : https://kifarunix.com/install-and-setup-zsh-and-oh-my-zsh-on-ubuntu-20-04/

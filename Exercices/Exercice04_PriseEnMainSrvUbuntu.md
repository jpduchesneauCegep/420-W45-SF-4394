# Exercice 4 - prise en main de votre serveur

- Évaluation : formative
- Type de travail : individuel
- Durée estimée : 1 heure
- Système d'exploitation : Linux Ubuntu serveur 22.04
- Environnement : virtuel, vsphere.

## Prise en main de votre serveur

 Quand vous vous connectez à votre serveur, certaines [informations](images/Connexion.png) importantes vous sont fournies. l'adresse IP est l'une de plus importante. Notez là.


Lors de l'installation, vous avez coché oui à l'installation d'un serveur SSH.
SSH signifie Secure SHell. C’est un protocole qui permet de faire des connexions sécurisées (i.e. chiffrées) entre un serveur et un client SSH.

<blockquote>Celui-ci va permettre aux utilisateurs d'accéder au système à distance, en rentrant leur login et leur mot de passe (ou avec un mécanisme de clefs).

**Le système de clefs de SSH**

SSH utilise la cryptographie asymétrique RSA ou DSA. En cryptographie asymétrique, chaque personne dispose d’un couple de clefs : une clé publique et une clef privée. La clé publique peut être librement publiée tandis que la clef privée doit rester secrète. La connaissance de la clef publique ne permet pas d’en déduire la clé privée.

Un serveur SSH dispose d’un couple de clefs RSA stocké dans le répertoire /etc/ssh/ et généré lors de l’installation du serveur. Le fichier ssh_host_rsa_key
contient la clef privée et a les permissions 600. Le fichier ssh_host_rsa_key.pub contient la clef publique et a les permissions 644.


**Voici les étapes de l’établissement d’une connexion SSH**

1. Le serveur envoie sa clef publique au client. Celui-ci vérifie qu’il s’agit bien de la clef du serveur, s’il l’a déjà reçue lors d’une connexion précédente.
2. Le client génère une clef secrète et l’envoie au serveur, en chiffrant l’échange avec la clef publique du serveur (chiffrement asymétrique). Le serveur déchiffre cette clef secrète en utilisant sa clé privée, ce qui prouve qu’il est bien le vrai serveur.
3. Pour le prouver au client, il chiffre un message standard avec la clef secrète et l’envoie au client. Si le client retrouve le message standard en utilisant la clef secrète, il a la preuve que le serveur est bien le vrai serveur. 
4. Une fois la clef secrète échangée, le client et le serveur peuvent alors établir un canal sécurisé grâce à la clef secrète commune (chiffrement symétrique).
5. Une fois que le canal sécurisé est en place, le client va pouvoir envoyer au serveur le login et le mot de passe de l’utilisateur pour vérification. La canal sécurisé reste en place jusqu’à ce que l’utilisateur se déconnecte.


Il y a donc trois contraintes majeures pour garder un système sécurisé après avoir installé un serveur SSH :

1. avoir un serveur SSH à jour au niveau de la sécurité, ce qui doit être le cas si vous
faites consciencieusement les mises à jour de sécurité en suivant la procédure;
2. que les mots de passe de TOUS les utilisateurs soient suffisamment complexes
pour résister à une attaque en force brute ;
3. surveiller les connexions en lisant régulièrement le fichier de log /var/log/auth.log.

Source : Les citations sur SSH proviennent de : Formation Debian GNU/Linux ECP, janvier 2013, document PDF.</blockquote>



<i>Nous aborderons la notion de sécurité et la lecture des logs plus tard.</i>

## L’établissement d’une connexion SSH

**Connexion depuis votre poste de développeur**


Utilisez votre machine Ubuntu client (poste de développeur) pour établir une connexion avec votre serveur.

- Dans un terminal sur le poste client taper la commande :
```bash
$ssh {adresse ip du serveur}  [-l votre nom d'usager sur le serveur] [-p port]
```
- Remarquez l'échange de la clé lors de la première connexion.
- Vérifier par quelques commandes que vous êtes bien sur le serveur : 
```bash
$uname
$ip a
$ss -tuap
$netstat -tap
$df
$pwd
$top
```
- Taper les mêmes commandes dans un autre terminal, mais cette fois pour votre poste client. 
- Au besoin, vérifier le manuel de chaque commande (man).

## Faire les mises à jour 

<blockquote>
L’instruction **apt update** va rechercher les mises à jour disponibles pour votre système et vos programmes installés en se basant sur les sources définies dans /etc/apt/source.list. Un fichier d’index est créé pour lister les mises à jour disponibles. Il servira de référence pour l’installation de nouvelles mises à jour.

L’option **apt upgrade** installe les mises à jour identifiée avec apt update sans supprimer les paquets installés. S’il y a de nouvelles dépendances à installer, les paquets peuvent être installés ou non selon le type de commande utilisée apt, apt-get ou aptitude.

Les options **apt dist-upgrade** ou **full-upgrade** sont identiques, utiliser l’une ou l’autre revient donc au même.

Ces deux options agissent plus « intelligemment » que la fonction upgrade. En plus de mettre à jour les paquets existants, elles vont également être en mesure de gérer les dépendances. Si de nouveaux paquets doivent être installés pour satisfaire des dépendances, ils le seront. Ceux qui ne sont plus utiles, sont supprimés et les paquets essentiels ou requis, sont installés. Les paquets les plus importants sont traités en priorité.

**Dois-je utiliser apt upgrade ou apt full-upgrade?**

Dans un environnement hautement critique et qui doit rester stable, la commande upgrade est plus sûr. Si vous désirez faire une opération de mise à jour courante et minorer le risque, il en est de même. Seuls les paquets actuellement installés sont traités. Les risques de dysfonctionnement suite à la suppression ou l’installation d’un nouveau paquet sont réduits.

Dans la plupart des autres cas, l’option dist-upgrade ou full-upgrade est à privilégier car vous obtiendrez toutes les dernières mises à jour sur votre système et du noyau. Lorsque vous souhaitez changer de version majeure de distribution (par exemple pour passer de Debian 8 à Debian 9), c’est cette commande que vous devez utiliser.

Dans tous les cas, je vous conseille vivement de réaliser une sauvegarde avant toute mise à jour et de réaliser des essais au préalable sur un environnement de test.
Source : [https://www.lecoindunet.com/difference-apt-update-upgrade-full-upgrade](https://www.lecoindunet.com/difference-apt-update-upgrade-full-upgrade) </blockquote>

- Tapez les commandes suivantes sur votre serveur : 
```bash
$sudo apt update
$sudo apt upgrade
```
- Tapez les commandes suivantes sur poste client : 

```bash
$sudo apt update
$sudo apt full-upgrade
```

## Vérifier les logs du serveur


- Tapez la commande suivante sur votre  poste client et sur le serveur pour observer les connexions.


```bash
$tail /var/log/auth.log
```
### Vérifier des droits sur des fichiers et des répertoires


Pour comprendre les droits : [https://doc.ubuntu-fr.org/droits](https://doc.ubuntu-fr.org/droits), nous y reviendrons dans un prochain cours.
- Vérifier les droits sur le fichier des logs :


```bash
$ls -l /var/log/auth.log
```
- Garder les informations pour la comparer plus tard. Notez les droits de cette façon :


    **exemple appliqué sur /var/log/auth.log**


     -rw-r----- syslog adm


   - ici on a fichier, première lettre (-)
   - lire, écrire, null pour le propriétaire
   - lecture seulement pour le groupe
   - et aucun droit pour les autres
   - Le propriétaire est syslog et le groupe adm
   


- Comparer les droits avec ceux des fichiers suivants :


```bash
$ls -l /etc/passwd
$ls -l /etc/shadow
$ls -l /home/{votre usager}
```
Vous pouvez visualiser mes résultats [ici](images/droit.png).

**Question** : Selon-vous, pourquoi seule root et le groupe shadow on le droit lecture sur /etc/shadow et que tous le monde peux lire /etc/passwd ?

## Pour vérification
Remettre une capture d’écran de la commande suivante, et ce pour les deux VMS (dans un seul fichier) de l'espace travaux, exercice 4 sur LÉA.

```bash
$ls -l /var/log/auth.log
```


Je dois pouvoir vous identifier sur la capture.
Exemple de capture acceptable client: 

![Capture acceptable](images/captureAcceptable.jpg)

Exemple de capture acceptable serveur: 

![Capture acceptable](images/captureAcceptableSrv.jpg)




**Fin exercice 4**

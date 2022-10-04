# Exercice 3 – Gestion des disques LVM

### Informations
- Évaluation : formative
- Type de travail : individuel
- Durée : 1 heure
- Système d'exploitation : Linux Ubuntu client 22.04 et Ubuntu serveur 22.04
- Environnement : virtuel, vSphere.

### Objectifs :

Cet exercice a pour objectifs :
- Pouvoir ajouter des disques durs dans LVM

Vous devez réaliser cet exercice sur le client et sur le serveur.


## Partie 1 : Vérification de vos disques

Vérifier l'état de votre système avant de débuter et garder une trace des informations 

```bash
echo --- avant modification --- > FichierDesTraces.txt
date >> FichierDesTraces.txt
df -H >> FichierDesTraces.txt
lsblk >> FichierDesTraces.txt
cat /etc/fstab >> FichierDesTraces.txt
```

## Partie 2 : État du stokage LVM

Taper les commandes suivantes et garder une trace des informations :

```bash
echo --- Avant modification --- > FichierLVM.txt
date >> FichierLVM.txt
sudo pvs >> FichierLVM.txt
sudo vgs >> FichierLVM.txt
sudo lvs >> FichierLVM.txt
```

## Partie 3 : Modification de votre stokage LVM

D'abord il faut ajouter le disque sdb dans le "physiqual volume" : 
```bash
sudo pvcreate /dev/sdb # Si sdb est bien le nouveau disque vérifier avec les commandes de la partie 2
sudo pvs
```

Par la suite, nous pouvons ajouter le disque dans le "volume group" :

Pour le client :
```bash
sudo vgextend vgubuntu /dev/sdb # Attention: prenez le nom que la commande sudo vgs pour vous renvoyer et le bon nom de disque.
sudo vgs  
```
Pour le serveur :
```bash
sudo vgextend ubuntu-vg /dev/sdb
sudo vgs  
```
Maintenant, nous allons étendre le "logical volume" :

Pour le client :
```bash
sudo lvextend -l +100%FREE /dev/vgubuntu/root # ici j'utilise le nom complet de la partition logique renvoyé par la commande df.
```
Pour le serveur :
```bash
sudo lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```
Vérifier le résultat et placer l'information dans votre fichier : 
```bash
echo --- Après modification --- >> FichierLVM.txt
date >> FichierLVM.txt
sudo pvs >> FichierLVM.txt
sudo vgs >> FichierLVM.txt
sudo lvs >> FichierLVM.txt
```

## Partie 4: Ajuster votre fichier /etc/fstab

Débutons par une vérifications de l'état du fichier /etc/fstab avec la commande df :

```bash
df -H 
```
Remarqué l'entré pour votre partition lvm dont le point de montage est / (root). C'est à dire celle que nous avons étendu.
Elle n'est pas étendu.

Pour que le système de fichiers utilise la totalité des de l'espace disponibles, exécutez :
sudo resize2fs /dev/VG1/LV1
```bash
sudo resize2fs /dev/mapper/vgubuntu-root #Le nom de la partition monté dans le fstab.
```
Le resize2fs est un utilitaire en ligne de commande qui vous permet de redimensionner des systèmes de fichiers ext2, ext3 ou ext4. 

Note : L'extension d'un système de fichiers est une opération à risque modéré. Il est donc recommandé de sauvegarder l'intégralité de votre partition pour éviter toute perte de données.


 
## Pour vérification
Remettre une capture d’écran des trois commandes suivantes, et ce pour les deux VMS (dans un seul fichier) de l'espace travaux, exercice 3 sur LÉA.

```bash
sudo pvs 
sudo vgs 
sudo lvs 
```
## Compétences développées


00Q1 - Effectuer l’installation et la gestion d’ordinateur :

    2 installer le système d’exploitation.
    4 Effectuer des tâches de gestion du système d’exploitation.

00SF - Évaluer des composants logiciels et matériels.

    1 Rechercher des composants logiciels et matériels.
    2 Formuler des avis sur les composants logiciels et matériels.

Note : les compétences sont développées en partie.

## Référence

- LVM : https://access.redhat.com/documentation/fr-fr/red_hat_enterprise_linux/6/html/logical_volume_manager_administration/index


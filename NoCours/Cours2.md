# Cours 2 : 20 mai 2022

## État de la situation exercice 1 ?

- 19 VMs de réalisé - moins prof = 17
- Nb étudiant : 20
- Exemple machine : 
  - E22_4393_420W44_Ub_Cli_JCC_2291719 en état de rédémarage.
  - E22_4393_420W44_Ub_Cli_DC_#2092367 Mal nommé et toujours active.

- Importance de la nonemclature

<details>
 
Exemple de code PowerShell

```PowerShell
$NomVMAbrege = "E22_4393_420W44_Ub_Cli*"
Write-host 'Nombre de Vms pour le cours :'(Get-VM -Name $NomVMAbrege).count 
Get-VM -Name $NomVMAbrege| Stop-vm
Get-vm -Name $NomVMAbrege| REMOVE-VM -DeletePermanently
```
</details>

## Réponse aux questions exercice 1

- Qu'elle est la différence entre ces deux commandes (sudo apt update et sudo apt upgrade) ? 
       <details>  

        **apt udpdate** : télécharge les informations sur les paquets depuis toutes les sources configurées (c'est-à-dire les sources configurées dans /etc/apt/sources.list). C'est ainsi que votre système sait quels paquets sont disponibles pour une mise à jour, et où récupérer ces logiciels.
        
        **apt upgrade** : peut alors agir sur ces informations et mettre à niveau tous les paquets installés vers leurs dernières versions. Cette commande ne met à niveau que les paquets déjà installés ; elle n'installe pas de nouveaux paquets à moins qu'ils ne soient nécessaires pour résoudre les dépendances. apt upgrade ne supprime pas non plus de paquets. Si un paquet doit être supprimé pour terminer une mise à niveau, la commande ignorera simplement cette mise à niveau et laissera vos paquets actuels intacts.

        

- À quoi sert la commande df et son paramètre -h ?
        <details>  
        
        Cette commande vous permet d’avoir l’espace utilisé et disponible pour chaque partition du système de fichier qui est monté.
       

## Théorie 

## Exercice 2 et 3

## Lecture recommandé : 
    - https://fr.wikipedia.org/wiki/YAML
# Exercice 10 - Docker : Dockerfile

- Évaluation : formative
- Durée estimée : 2 heures
- Système d'exploitation : Ubuntu Client 

## Exercice 1 - Créez le programme hello-world

- Sous Windows :  lancez une console Ubuntu.
- Sous Linux lancer un terminal.
- Vérifiez si le programme g++ est installé (g++ --help)
- S'il n'est pas installé, l'installer (package g++)
- Créez le fichier "premier-programme.cpp" avec le contenu suivant :

```cpp
#include <iostream>

int main(int argc, char** argv) {
    std::cout << "Bonjour à tous" << std::endl;
}
```

- Pour compiler votre programme, utilisez la commande ```g++ premier-programme.cpp -o premier-programme -static ```

- Créez dans le même répertoire que votre fichier "premier-programme" précédemment compilé, le fichier "Dockerfile" avec le contenu suivant :
  
  ```dockerfile
  FROM scratch
  COPY premier-programme /
  ENTRYPOINT ["/premier-programme"]
  ```

- À partir du fichier et de la documentation, décrivez ce que fait le fichier Dockerfile
- Tapez la commande ```docker build --tag premier-programme:latest .```
- Vérifiez que l'image a bien été créée
- Lancez un conteneur qui utilise votre image
- Modifiez le programme précédent pour qu'il corresponde au listage suivant :

```cpp
#include <iostream>

int main(int argc, char** argv) {
    std::cout << argv[1] << std::endl;
}
```

- Modifiez votre Dockerfile afin que par défaut, le programme reçoive "!!Bonjour à tous !!" en paramètre.
    <details>
    <summary>Modification du Dockerfile </summary>
    
    ```dockerfile
    FROM scratch
    COPY premier-programme /
    ENTRYPOINT ["/premier-programme"]
    cmd [ "Bonjour à tous" ]
     ```
    </details>
- Reconstruisez votre image de conteneur
- Essayez de lancer un nouveau conteneur en utilisant les mêmes lignes de commandes que précédemment
- Essayez de lancer un nouveau conteneur en utilisant en ajoutant à la fin la chaîne de caractères ```"Un paramètre qui vient de l'extérieur du conteneur" ```

    <summary>Installation de g++</summary>

```bash
apt install g++
```

</details>

## Exercice 2 - Programme C#

- À partir d'un terminal, placez-vous dans un nouveau répertoire "premier-conteneur-csharp"
- À partir du programme dotnet, créez un projet .Net core de type console
- Testez manuellement votre programme

<details>
    <summary>Installation dotnet, création et compilation du programme en local</summary>

```bash   
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-6.0
    
dotnet new console -o premier-conteneur-csharp
cd premier-conteneur-csharp
dotnet run
```

</details>

- Pour compiler votre programme, nous allons utiliser ici une image docker contenant le SDK de dotnet 6.0 Nous allons ensuite copier le résultat dans une nouvelle image qui ne contient que l'environnement d'exécution de dotnet 6.0 Pour cela, placez-vous dans le répertoire du projet et créez le fichier Dockerfile suivant :

```Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /source

COPY . .
RUN dotnet publish -c release -o /app

FROM mcr.microsoft.com/dotnet/runtime:6.0
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["./premier-conteneur-csharp"]
```

- Créez l'image docker "premier-programme-csharp"
- Testez votre image
- Essayez de comprendre les lignes du fichier Dockerfile en cherchant dans la documentation


## Pour vérification
Remettre une capture d’écran de la création et du test de l'image dans l'espace travaux, exercice 10 sur LÉA.
N'oubliez pas, je dois pouvoir identifier votre VMs.


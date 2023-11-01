# Créer une image native de l'application Helidon MP

## Présentation

Dans cet exercice, vous allez apprendre à configurer l'environnement sur votre machine linux locale.

Temps estimé : 10 minutes

### Objectifs

*   Configurez le JDK et le Maven requis.
*   Téléchargez le code source de l'application.

### Prérequis

*   Vous devez disposer d'un IDE pour modifier, créer et exécuter le projet.

## Tâche 1 : télécharger la version Maven et JDK requise

1.  Dans votre IDE, ouvrez une console/un terminal.
    
2.  Copiez les commandes suivantes et collez-les dans le terminal. Il télécharge la version requise de JDK et de Maven et définit la variable PATH de sorte qu'elle utilise le Maven et le JDK requis.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.tar.gz
        tar -xzvf jdk-19_linux-x64_bin.tar.gz</copy>
        

## Tâche 2 : télécharger le code source Helidon

1.  Copiez les commandes suivantes et collez-les dans le terminal pour télécharger le code source de l'application helidon.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  Copiez et collez la commande suivante pour décompresser le fichier _helidon-levelup-2023-main.zip_.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

Vous pouvez maintenant _passer à l'exercice suivant_.

## Accusés de réception

*   **Auteur** - Joe DiPol
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, février 2023
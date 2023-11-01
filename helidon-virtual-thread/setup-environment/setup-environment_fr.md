# Configurer l'environnement

## Présentation

Cet atelier vous guide tout au long des étapes de configuration de l'environnement requis pour l'atelier.

[Revue de processus Lab1](videohub:1_far2bboa)

Temps estimé : 10 minutes

### A propos de l'éditeur de code

L'éditeur de code vous permet de modifier et de déployer du code pour divers services OCI directement à partir de la console OCI. Vous pouvez désormais mettre à jour les scripts et les workflows de service sans avoir à basculer entre la console et vos environnements de développement locaux. Cela facilite la création rapide de prototypes de solutions cloud, l'exploration de nouveaux services et l'exécution rapide de tâches de codage.

L'intégration directe de l'éditeur de code à Cloud Shell vous permet d'accéder à l'image native d'entreprise GraalVM et à JDK 17 (Java Development Kit) préinstallés dans Cloud Shell.

### Objectifs

*   Configurer l'éditeur de code
*   Téléchargez la version Maven et JDK requise
*   Télécharger le code source Helidon

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.
*   Vous devez utiliser Firefox comme navigateur pour ouvrir l'éditeur de code.

## Tâche 1 : configurer l'éditeur de code

1.  Dans la console cloud, cliquez sur l'icône Editeur de code comme indiqué. ![Editeur de code](images/code-editor.png)
    
2.  Dans l'éditeur de code, cliquez sur Terminal -> New Terminal. ![Ouvrir un terminal](images/open-terminal.png)
    

## Tâche 2 : télécharger la version Maven et JDK requise

1.  Copiez les commandes suivantes et collez-les dans le terminal. Il télécharge la version requise de JDK et Maven.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/archive/jdk-19.0.2_linux-x64_bin.tar.gz
        tar -xzvf jdk-19.0.2_linux-x64_bin.tar.gz</copy>
        

## Tâche 3 : télécharger le code source Helidon

1.  Copiez les commandes suivantes et collez-les dans le terminal pour télécharger le code source de l'application helidon.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  Copiez et collez la commande suivante pour décompresser le fichier _helidon-levelup-2023-main.zip_.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

Vous pouvez maintenant _passer à l'exercice 2_.

## Accusés de réception

*   **Auteur** - Joe DiPol
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, février 2023
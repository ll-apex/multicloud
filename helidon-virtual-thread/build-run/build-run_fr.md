# Création et exécution de l'application Helidon Nima et réactive

## Présentation

Cet atelier vous guide tout au long du processus de création et d'exécution des applications Nima et Helidon réactives dans l'éditeur de code Oracle au sein d'Oracle Cloud Infrastructure.

[Revue de processus Lab2](videohub:1_e88ydqwt)

Temps estimé : 15 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Créer, exécuter et tester l'application Helidon Nima
*   Créer, exécuter et tester l'application Helidon Reactive
*   Analyser la simplicité de l'application Nima comparativement à l'application Réactive

### Prérequis

Pour exécuter cet exercice, vous devez disposer des éléments suivants :

*   Compte Oracle Cloud
*   Terminé l'atelier 1, qui installe le kit JDK et maven requis.

## Tâche 1 : créer et exécuter l'application Nima

1.  Cliquez sur _Fichier_ -> _Ouvrir_ dans l'éditeur de code. ![Ouvrir un projet](images/open-project.png)
    
2.  Sélectionnez le dossier _helidon-levelup-2023-main_ et cliquez sur _Ouvrir_. ![Projet Helidon](images/helidon-project.png)
    
    > Ignorez les avertissements/problèmes, remarquez dans l'éditeur de code lors de l'ouverture de ce dossier.
    
3.  Ouvrez un nouveau terminal, puis copiez et collez la commande suivante pour définir la variable PATH et la variable JAVA\_HOME.
    
        <copy>PATH=~/jdk-19.0.2/bin:~/apache-maven-3.8.3/bin:$PATH
        JAVA_HOME=~/jdk-19.0.2</copy>
        
4.  Copiez la commande suivante et exécutez-la dans le terminal pour vérifier que les versions JDK et Maven requises sont configurées correctement.
    
        <copy>mvn -v</copy>
        
    
    Le résultat obtenu est semblable au résultat suivant :
    
        $ mvn -v 
        Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
        Maven home: /home/ankit_x_pa/apache-maven-3.8.3
        Java version: 19.0.2, vendor: Oracle Corporation, runtime: /home/ankit_x_pa/jdk-19.0.2
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "4.14.35-2047.520.3.1.el7uek.x86_64", arch: "amd64", family: "unix"
        $ 
        
    
    > _Vous n'utiliserez ce terminal que pour créer et exécuter l'application, car elle dispose des versions JDK et Maven requises._
    
5.  Copiez et collez la commande suivante pour créer l'application nima.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-blocking ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/nima/target/example-nima-blocking.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  7.562 s
        [INFO] Finished at: 2023-02-27T11:42:52Z
        [INFO] ------------------------------------------------------------------------
        
6.  Copiez et collez la commande suivante dans le terminal pour exécuter la version bloquante nima de l'application :
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.27 11:43:18.589 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:43:18.902 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:43:19.700 [0x314d2373] http://127.0.0.1:33193 bound for socket '@default'
        2023.02.27 11:43:19.703 [0x314d2373] direct writes
        2023.02.27 11:43:19.722 Helidon Níma 4.0.0-ALPHA5
        
7.  Notez le numéro de port sur lequel le serveur est exécuté (reportez-vous à l'entrée de journal pour @default). Par exemple, dans notre sortie, le numéro de port est 33193. De même, recherchez le numéro de port de votre serveur.
    
8.  Pour ouvrir un nouveau terminal, cliquez sur _Terminal_ -> _New Terminal_. Nous allons utiliser ce terminal pour exécuter les commandes _curl_. ![Nouveau terminal](images/new-terminal.png)
    
9.  Copiez et collez la commande suivante dans le nouveau terminal. N'oubliez pas de remplacer _`<port>`_ par le port de votre serveur.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        curl http://localhost:33193/one
        remote_1
        $
        
10.  Copiez et collez la commande suivante dans le nouveau terminal. N'oubliez pas de remplacer _`<port>`_ par le port de votre serveur.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ curl http://localhost:33193/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > Notez l'ordre des résultats. Comme suggéré par le nom, cette demande appelle une ressource distante plusieurs fois dans l'ordre.
    
11.  Copiez et collez la commande suivante dans le nouveau terminal. N'oubliez pas de remplacer _`<port>`_ par le port de votre serveur.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ curl http://localhost:33193/parallel
        Combined results: [remote_21, remote_18, remote_12, remote_13, remote_14, remote_15, remote_16, remote_17, remote_19, remote_20]
        $
        
    
    > Notez l'ordre des résultats. Comme suggéré par le nom, cette demande appelle une ressource distante plusieurs fois en parallèle.
    
12.  Appuyez sur _Ctrl + C_ dans le terminal où la commande \*java -jar \* est en cours d'exécution pour arrêter le serveur.
    

## Tâche 2 : créer et exécuter l'application réactive

1.  Copiez et collez la commande suivante pour créer l'application nima.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-reactive ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/reactive/target/example-nima-reactive.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  6.196 s
        [INFO] Finished at: 2023-02-27T11:51:17Z
        [INFO] ------------------------------------------------------------------------
        $
        
2.  Copiez et collez la commande suivante dans le terminal pour exécuter la version réactive de l'application :
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.27 11:51:26.227 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:51:26.723 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:51:27.286 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.27 11:51:27.618 Channel '@default' started: [id: 0x53fadd43, L:/0.0.0.0:45765]
        
3.  Notez le numéro de port sur lequel le serveur est exécuté (reportez-vous à l'entrée de journal pour @default). Par exemple, dans notre sortie, le numéro de port est 45765. De même, recherchez le numéro de port de votre serveur.
    
4.  Revenez au terminal que nous avons ouvert dans la tâche 1 pour exécuter la commande curl.
    
5.  Copiez et collez la commande suivante dans le nouveau terminal. N'oubliez pas de remplacer _`<port>`_ par le port de votre serveur.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ curl http://localhost:45765/one
        remote_1
        $
        
6.  Copiez et collez la commande suivante dans le nouveau terminal. N'oubliez pas de remplacer _`<port>`_ par le port de votre serveur.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ curl http://localhost:45765/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > Notez l'ordre des résultats. Comme suggéré par le nom, cette demande appelle une ressource distante plusieurs fois dans l'ordre.
    
7.  Copiez et collez la commande suivante dans le nouveau terminal. N'oubliez pas de remplacer _`<port>`_ par le port de votre serveur.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit :
    
        $ curl http://localhost:45765/parallel
        Combined results: [remote_17, remote_16, remote_13, remote_20, remote_12, remote_19, remote_18, remote_14, remote_21, remote_15]
        $
        
    
    > Notez l'ordre des résultats. Comme suggéré par le nom, cette demande appelle une ressource distante plusieurs fois en parallèle.
    
8.  Appuyez sur _Ctrl + C_ dans le terminal où la commande \*java -jar \* est en cours d'exécution pour arrêter le serveur.
    

## Tâche 3 : Analyser la simplicité de l'application Nima

**Blocage vs réactif**

Comparons les implémentations entre Níma (blocage) et Helidon SE (réactif) pour la même tâche.

*   Les deux implémentations exécutent des appels REST à l'aide de Helidon WebClient
*   L'implémentation du blocage est simple à suivre :
    *   Le flux d'exécution est reflété par l'ordre des instructions dans le code.
    *   Il n'y a pas d'appels de bibliothèque complexes ou riches en sémantique
    *   Le débogage est simple
*   Les versions réactives nécessitent une bonne compréhension des bibliothèques réactives
    *   Y compris Multi, flatMap, gestion des erreurs, etc.
    *   Le flux de contrôle n'est plus évident en raison de l'utilisation de gestionnaires réactifs
    *   Une compréhension de la capacité de flatMap à activer/interdire la simultanéité d'accès aux données est requise
    *   Le débogage est plus difficile

1.  Ouvrez le fichier _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ pour voir comment les endpoints sont implémentés dans la version nima de l'application. ![Bloc Nima](images/nima-block.png)
    
2.  Ouvrez le fichier _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ pour voir comment les adresses sont implémentées dans la version réactive de l'application. ![Service réactif](images/reactive-service.png)
    
3.  Dans la section _OPEN EDITORS_, cliquez avec le bouton droit de la souris sur le fichier _BlockingService.java_ et sélectionnez _Sélectionner pour comparaison_. ![Sélectionner une comparaison](images/select-compare.png)
    
4.  Cliquez avec le bouton droit de la souris sur le fichier _ReactiveService.java_ et sélectionnez _Comparer avec la sélection_. ![Comparer les éléments sélectionnés](images/compare-selected.png)
    
5.  Voyez que le code réactif est plus compliqué que le blocage (fil virtuel) ![Comparer le code](images/compare-code.png)
    
    > Vérifiez les méthodes un, séquencées et parallèles dans BlockingService et ReactiveService respectivement. Voyez si vous comprenez comment ils fonctionnent !
    

Vous pouvez maintenant _passer à l'exercice suivant_.

## Accusés de réception

*   **Auteur** - Joe DiPol
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, février 2023
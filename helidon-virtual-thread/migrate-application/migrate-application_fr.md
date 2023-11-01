# Créer une application Helidon 3, puis la migrer vers Helidon 4

## Présentation

Dans cet exercice, vous allez commencer par une application Helidon 3 exécutée sur notre serveur Web réactif d'origine basé sur Netty. Vous allez ensuite migrer l'application vers Helidon 4 exécutée sur le nouveau Helidon Nima WebServer à l'aide de threads virtuels.

[Revue de processus Lab4](videohub:1_zr1m00ba)

Temps estimé : 20 minutes

### Objectifs

*   Générez, créez et exécutez une application Helidon MicroProfile à l'aide de Helidon Starter.
*   Migrer l'application Helidon 3 Microprofile vers Helidon 4

### Prérequis

*   Compte Oracle Cloud

## Tâche 1 : créer une application Helidon 3 et créer l'application

1.  Copiez l'URL ci-dessous et collez-la dans le navigateur pour ouvrir la page du projet Helidon.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  Sous Générer votre projet, sélectionnez _Helidon MP_ en tant que saveur Helidon, puis cliquez sur _Suivant_.
    
3.  Pour Type d'application, sélectionnez _Démarrage rapide_, puis cliquez sur _Suivant_.
    
4.  Pour Media Support, sélectionnez _Jackson_, puis cliquez sur _Next_.
    
5.  Pour Personnaliser le projet, sélectionnez les valeurs par défaut et cliquez sur _Téléchargements_. Cela apparaîtra dans une fenêtre, enregistrez ce fichier _myproject.zip_ à l'emplacement de votre choix. Dans le reste de cet atelier, le nom _myproject_ sera utilisé. Si vous choisissez un autre nom, modifiez-le respectivement.
    
6.  Revenez à l'éditeur de code, dans HELIDON-LEVELUP-2023-MAIN, puis cliquez sur _docs_. ![Sélectionner des documents](images/select-docs.png)
    
7.  Cliquez sur _Fichier_ -> _Télécharger des fichiers_ et sélectionnez _myproject.zip_ à partir de l'emplacement où vous avez enregistré ce fichier précédemment, puis cliquez sur _Ouvrir_. ![Charger des fichiers](images/upload-files.png)
    
8.  Le fichier _myproject.zip_ apparaît sous le dossier _docs_. ![Fichier Zip](images/zip-file.png)
    
9.  Copiez et collez la commande suivante pour décompresser le fichier.
    
        <copy>cd ~/helidon-levelup-2023-main/docs/
        unzip myproject.zip</copy>
        
10.  A partir du dossier myproject, exécutez la commande suivante pour créer le projet. Utilisez le terminal, où vous avez défini les variables PATH et JAVA\_HOME.
    
        <copy>cd myproject
        mvn clean package</copy>
        
    
    > La mention _BUILD SUCCESS_ doit apparaître à la fin de l'exécution de cette commande.
    
11.  Copiez et collez la commande suivante dans le terminal pour exécuter cette application. Une sortie similaire à celle illustrée dans la capture d'écran ci-dessous apparaît.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    La sortie doit ressembler à ce qui suit :
    
        $ java -jar target/myproject.jar
        2023.02.28 03:48:36 INFO io.helidon.common.LogConfig Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:48:39 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:48:40 INFO io.helidon.webserver.NettyWebServer Thread[#31,nioEventLoopGroup-2-1,10,main]: Channel '@default' started: [id: 0x5b66bd2a, L:/0.0.0.0:8080]
        2023.02.28 03:48:41 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 4816 milliseconds (since JVM startup).
        2023.02.28 03:48:41 INFO io.helidon.common.HelidonFeatures Thread[#32,features-thread,5,main]: Helidon MP 3.1.2 features: [CDI, Config, Health, JAX-RS, Metrics, Open API, Server]
        
12.  Revenez au terminal, à partir duquel vous exécutez les commandes curl et exécutez les commandes suivantes pour vérifier l'application :
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
13.  Arrêtez l'application _myproject_ en saisissant `Ctrl + C` dans le terminal où la commande "java -jar target/myproject.jar" est exécutée.
    
14.  Modifiez le fichier _src/main/java/com/example/myproject/GreetResource.java_, recherchez la méthode _createResponse(String who)_ et ajoutez la ligne suivante comme indiqué dans la capture d'écran.
    
        <copy>System.out.println("Running on thread " + Thread.currentThread());</copy>
        
    
    ![modifier l'application](images/modify-application.png)
    
15.  Reconstruisez, exécutez et exercez l'application comme décrit aux étapes 10, 11 et 12.
    
16.  Consultez la sortie du serveur dans le terminal sur lequel vous avez démarré le serveur. Notez que le thread est nommé helidon-server-n. Il s'agit d'un thread de plate-forme traditionnel dans un pool de threads créé par Helidon pour gérer les demandes JAX-RS. La sortie du serveur se présente comme suit :
    
        Running on thread Thread[#25,helidon-server-1,5,server]
        
17.  Arrêtez l'application _myproject_ en saisissant `Ctrl + C` dans le terminal où la commande "java -jar target/myproject.jar" est exécutée.
    

## Tâche 2 : migration de l'application Helidom MicroProfile vers Helidon 4

1.  Pour myproject, ouvrez le fichier _pom.xml_ et remplacez le pom parent _3.1.1_ par _4.0.0-ALPHA5_.
    
        <parent>
            <groupId>io.helidon.applications</groupId>
            <artifactId>helidon-mp</artifactId>
            <version>4.0.0-ALPHA5</version>
            <relativePath/>
        </parent>
        
    
    ![Version de Pom](images/pom-version.png)
    
2.  Modifiez src/main/resources/logging.properties et remplacez _io.helidon.common.HelidonConsoleHandler_ par _io.helidon.logging.jul.HelidonConsoleHandler_. ![Modification de la journalisation](images/edit-logging.png)
    
3.  Votre application a été migrée vers Helidon 4. Copiez et collez la commande suivante pour créer l'application.
    
        <copy>mvn clean package -DskipTests</copy>
        
4.  Copiez et collez la commande suivante pour exécuter l'application.
    
        <copy>java --enable-preview  -jar target/myproject.jar</copy>
        
    
    Vous obtenez une sortie semblable à ce qui suit.
    
        $ java --enable-preview  -jar target/myproject.jar
        2023.02.28 03:56:17 INFO io.helidon.logging.jul.JulProvider Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:56:21 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] http://0.0.0.0:8080 bound for socket '@default'
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] direct writes
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Started all channels in 19 milliseconds. 5131 milliseconds since JVM startup. Java 19.0.2+7-44
        2023.02.28 03:56:22 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 5144 milliseconds (since JVM startup).
        2023.02.28 03:56:22 INFO io.helidon.common.features.HelidonFeatures Thread[#28,features-thread,5,main]: Helidon MP 4.0.0-ALPHA5 features: [CDI, Config, Health, Metrics, Open API, Server, WebServer]
        
5.  Examinez la sortie du serveur et notez que le thread est désormais un VirtualThread.
    
6.  Arrêtez l'application _myproject_ en saisissant `Ctrl + C` dans le terminal où la commande "java --enable-preview -jar target/myproject.jar" est exécutée.
    

Félicitations, vous avez terminé l'atelier sur les threads virtuels Helidon.

## Accusés de réception

*   **Auteur** - Joe DiPol
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, février 2023
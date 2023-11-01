# Erstellen Sie eine Helidon 3-Anwendung, und migrieren Sie sie dann zu Helidon 4

## Einführung

In dieser Übung beginnen Sie mit einer Helidon 3-Anwendung, die auf unserem ursprünglichen reaktiven Webserver basierend auf Netty ausgeführt wird. Anschließend migrieren Sie die Anwendung mit virtuellen Threads zu Helidon 4, die auf der neuen Helidon Nima WebServer ausgeführt wird.

[Durchlauf durch Lab4](videohub:1_zr1m00ba)

Geschätzte Zeit: 20 Minuten

### Ziele

*   Generieren, erstellen und führen Sie eine Helidon MicroProfile-Anwendung mit Helidon Starter aus.
*   Helidon 3 Microprofile-Anwendung zu Helidon 4 migrieren

### Voraussetzungen

*   Oracle Cloud-Account

## Aufgabe 1: Helidon 3-Anwendung erstellen und Anwendung erstellen

1.  Kopieren Sie die folgende URL, und fügen Sie sie in den Browser ein, um die Helidon-Projektseite zu öffnen.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  Wählen Sie unter "Projekt generieren" die Option _Helidon MP_ als Helidon-Aroma aus, und klicken Sie auf _Weiter_.
    
3.  Wählen Sie unter "Anwendungstyp" die Option _Schnellstart_ aus, und klicken Sie auf _Weiter_.
    
4.  Wählen Sie für Media Support _Jackson_ aus, und klicken Sie auf _Weiter_.
    
5.  Wählen Sie unter "Projekt anpassen" die Standardwerte aus, und klicken Sie auf _Downloads_. Dies wird in einem Fenster angezeigt. Speichern Sie diese _myproject.zip_ im gewünschten Speicherort. Im Rest dieses Workshops wird der Name _myproject_ verwendet. Wenn Sie einen anderen Namen wählen, ändern Sie den Namen.
    
6.  Gehen Sie zurück zum Codeeditor, in HELIDON-LEVELUP-2023-MAIN, und klicken Sie auf _docs_. ![Dokumente auswählen](images/select-docs.png)
    
7.  Klicken Sie auf _Datei_ -> _Dateien hochladen_, und wählen Sie _myproject.zip_ aus dem Speicherort, in dem Sie diese Datei zuvor gespeichert haben. Klicken Sie dann auf _Öffnen_. ![Dateien hochladen](images/upload-files.png)
    
8.  Die Datei _myproject.zip_ wird im Ordner _docs_ angezeigt. ![ZIP-Datei](images/zip-file.png)
    
9.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Datei zu entpacken.
    
        <copy>cd ~/helidon-levelup-2023-main/docs/
        unzip myproject.zip</copy>
        
10.  Führen Sie im Ordner "myproject" den folgenden Befehl aus, um das Projekt zu erstellen. Verwenden Sie das Terminal, in dem Sie die Variablen PATH und JAVA\_HOME festgelegt haben.
    
        <copy>cd myproject
        mvn clean package</copy>
        
    
    > Am Ende der Ausführung dieses Befehls sollte _BUILD SUCCESS_ angezeigt werden.
    
11.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um diese Anwendung auszuführen. Sie sehen eine Ausgabe ähnlich der im folgenden Screenshot.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ java -jar target/myproject.jar
        2023.02.28 03:48:36 INFO io.helidon.common.LogConfig Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:48:39 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:48:40 INFO io.helidon.webserver.NettyWebServer Thread[#31,nioEventLoopGroup-2-1,10,main]: Channel '@default' started: [id: 0x5b66bd2a, L:/0.0.0.0:8080]
        2023.02.28 03:48:41 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 4816 milliseconds (since JVM startup).
        2023.02.28 03:48:41 INFO io.helidon.common.HelidonFeatures Thread[#32,features-thread,5,main]: Helidon MP 3.1.2 features: [CDI, Config, Health, JAX-RS, Metrics, Open API, Server]
        
12.  Gehen Sie zurück zum Terminal, von dem aus Sie die curl-Befehle ausführen und die folgenden Befehle ausführen, um die Anwendung zu prüfen:
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
13.  Stoppen Sie die Anwendung _myproject_, indem Sie `Ctrl + C` in das Terminal eingeben, in dem der Befehl "java -jar target/myproject.jar" ausgeführt wird.
    
14.  Bearbeiten Sie die Datei _src/main/java/com/example/myproject/GreetResource.java_, suchen Sie die Methode _createResponse(String who)_, und fügen Sie die folgende Zeile hinzu (siehe Screenshot).
    
        <copy>System.out.println("Running on thread " + Thread.currentThread());</copy>
        
    
    ![Anwendung ändern](images/modify-application.png)
    
15.  Erstellen Sie die Anwendung wie in den Schritten 10, 11 und 12 beschrieben neu, führen Sie sie aus, und führen Sie sie aus.
    
16.  Prüfen Sie die Serverausgabe im Terminal, in dem Sie den Server gestartet haben. Beachten Sie, dass der Thread den Namen helidon-server-n trägt. Dies ist ein herkömmlicher Plattform-Thread in einem Threadpool, der von Helidon zur Verarbeitung von JAX-RS-Anforderungen erstellt wurde. Die Serverausgabe sieht in etwa wie folgt aus:
    
        Running on thread Thread[#25,helidon-server-1,5,server]
        
17.  Stoppen Sie die Anwendung _myproject_, indem Sie `Ctrl + C` in das Terminal eingeben, in dem der Befehl "java -jar target/myproject.jar" ausgeführt wird.
    

## Aufgabe 2: Helidom MicroProfile-Anwendung zu Helidon 4 migrieren

1.  Öffnen Sie für myproject die Datei _pom.xml_, und ändern Sie den übergeordneten Pom von _3.1.1_ in _4.0.0-ALPHA5_.
    
        <parent>
            <groupId>io.helidon.applications</groupId>
            <artifactId>helidon-mp</artifactId>
            <version>4.0.0-ALPHA5</version>
            <relativePath/>
        </parent>
        
    
    ![Pom-Version](images/pom-version.png)
    
2.  Bearbeiten Sie src/main/resources/logging.properties und ändern Sie _io.helidon.common.HelidonConsoleHandler_ in _io.helidon.logging.jul.HelidonConsoleHandler_. ![Logging bearbeiten](images/edit-logging.png)
    
3.  Ihre Anwendung wurde jetzt zu Helidon 4 migriert! Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Anwendung zu erstellen.
    
        <copy>mvn clean package -DskipTests</copy>
        
4.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Anwendung auszuführen.
    
        <copy>java --enable-preview  -jar target/myproject.jar</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt.
    
        $ java --enable-preview  -jar target/myproject.jar
        2023.02.28 03:56:17 INFO io.helidon.logging.jul.JulProvider Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:56:21 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] http://0.0.0.0:8080 bound for socket '@default'
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] direct writes
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Started all channels in 19 milliseconds. 5131 milliseconds since JVM startup. Java 19.0.2+7-44
        2023.02.28 03:56:22 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 5144 milliseconds (since JVM startup).
        2023.02.28 03:56:22 INFO io.helidon.common.features.HelidonFeatures Thread[#28,features-thread,5,main]: Helidon MP 4.0.0-ALPHA5 features: [CDI, Config, Health, Metrics, Open API, Server, WebServer]
        
5.  Prüfen Sie die Serverausgabe, und beachten Sie, dass der Thread jetzt VirtualThread ist.
    
6.  Stoppen Sie die Anwendung _myproject_, indem Sie `Ctrl + C` in das Terminal eingeben, in dem der Befehl "java --enable-preview -jar target/myproject.jar" ausgeführt wird.
    

Herzlichen Glückwunsch, Sie haben den Helidon Virtual Thread Workshop abgeschlossen.

## Danksagungen

*   **Autor** - Joe DiPol
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Feb 2023
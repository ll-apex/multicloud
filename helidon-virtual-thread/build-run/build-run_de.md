# Erstellen und Ausführen der Helidon Nima- und Reactive-Anwendung

## Einführung

In dieser Übung werden Sie durch den Prozess zum Erstellen und Ausführen der Nima- und Reactive Helidon-Anwendungen im Oracle Code Editor in Oracle Cloud Infrastructure geführt.

[Durchlauf durch Lab2](videohub:1_e88ydqwt)

Geschätzte Zeit: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Helidon Nima-Anwendung erstellen, ausführen und testen
*   Erstellen, Ausführen und Testen der Helidon Reactive-Anwendung
*   Analyse der Einfachheit der Nima-Anwendung im Vergleich zur Reactive-Anwendung

### Voraussetzungen

Um diese Übung ausführen zu können, benötigen Sie:

*   Oracle Cloud-Account
*   Abgeschlossene Übung 1, in der das erforderliche JDK und maven installiert werden.

## Aufgabe 1: Nima-Anwendung erstellen und ausführen

1.  Klicken Sie im Codeeditor auf _Datei_ -> _Öffnen_. ![Projekt öffnen](images/open-project.png)
    
2.  Wählen Sie den Ordner _helidon-levelup-2023-main_, und klicken Sie auf _Öffnen_. ![Helidon-Projekt](images/helidon-project.png)
    
    > Ignorieren Sie die Warnungen/Probleme, die Sie beim Öffnen dieses Ordners im Codeeditor bemerken.
    
3.  Öffnen Sie ein neues Terminal, kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Variable PATH und JAVA\_HOME festzulegen.
    
        <copy>PATH=~/jdk-19.0.2/bin:~/apache-maven-3.8.3/bin:$PATH
        JAVA_HOME=~/jdk-19.0.2</copy>
        
4.  Kopieren Sie den folgenden Befehl, und führen Sie ihn im Terminal aus, um zu prüfen, ob die erforderliche JDK- und Maven-Version ordnungsgemäß konfiguriert ist.
    
        <copy>mvn -v</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ mvn -v 
        Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
        Maven home: /home/ankit_x_pa/apache-maven-3.8.3
        Java version: 19.0.2, vendor: Oracle Corporation, runtime: /home/ankit_x_pa/jdk-19.0.2
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "4.14.35-2047.520.3.1.el7uek.x86_64", arch: "amd64", family: "unix"
        $ 
        
    
    > _Sie verwenden nur dieses Terminal zum Erstellen und Ausführen der Anwendung, da sie die erforderliche JDK- und Maven-Version aufweist._
    
5.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die nima-Anwendung zu erstellen.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-blocking ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/nima/target/example-nima-blocking.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  7.562 s
        [INFO] Finished at: 2023-02-27T11:42:52Z
        [INFO] ------------------------------------------------------------------------
        
6.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um die blockierende nima-Version der Anwendung auszuführen:
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.27 11:43:18.589 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:43:18.902 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:43:19.700 [0x314d2373] http://127.0.0.1:33193 bound for socket '@default'
        2023.02.27 11:43:19.703 [0x314d2373] direct writes
        2023.02.27 11:43:19.722 Helidon Níma 4.0.0-ALPHA5
        
7.  Notieren Sie sich die Portnummer, auf der der Server ausgeführt wird (siehe Logeintrag für @default). Beispiel: In unserer Ausgabe lautet die Portnummer 33193. Finden Sie auch Ihre Server-Portnummer heraus.
    
8.  Um ein neues Terminal zu öffnen, klicken Sie auf _Terminal_ -> _Neues Terminal_. In diesem Terminal führen wir _curl_\-Befehle aus. ![Neues Terminal](images/new-terminal.png)
    
9.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das neue Terminal ein. Vergessen Sie nicht, _`<port>`_ durch Ihren Serverport zu ersetzen.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        curl http://localhost:33193/one
        remote_1
        $
        
10.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das neue Terminal ein. Vergessen Sie nicht, _`<port>`_ durch Ihren Serverport zu ersetzen.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ curl http://localhost:33193/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > Beachten Sie die Reihenfolge der Ergebnisse. Wie vom Namen vorgeschlagen, ruft diese Anforderung eine Remoteressource mehrmals nacheinander auf.
    
11.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das neue Terminal ein. Vergessen Sie nicht, _`<port>`_ durch Ihren Serverport zu ersetzen.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ curl http://localhost:33193/parallel
        Combined results: [remote_21, remote_18, remote_12, remote_13, remote_14, remote_15, remote_16, remote_17, remote_19, remote_20]
        $
        
    
    > Beachten Sie die Reihenfolge der Ergebnisse. Wie vom Namen vorgeschlagen, ruft diese Anforderung eine Remoteressource mehrmals parallel auf.
    
12.  Drücken Sie _Strg + C_ im Terminal, in dem der Befehl \*java -jar \* ausgeführt wird, um den Server zu stoppen.
    

## Aufgabe 2: Reaktive Anwendung erstellen und ausführen

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die nima-Anwendung zu erstellen.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-reactive ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/reactive/target/example-nima-reactive.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  6.196 s
        [INFO] Finished at: 2023-02-27T11:51:17Z
        [INFO] ------------------------------------------------------------------------
        $
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um die reaktive Version der Anwendung auszuführen:
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.27 11:51:26.227 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:51:26.723 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:51:27.286 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.27 11:51:27.618 Channel '@default' started: [id: 0x53fadd43, L:/0.0.0.0:45765]
        
3.  Notieren Sie sich die Portnummer, auf der der Server ausgeführt wird (siehe Logeintrag für @default). Beispiel: In unserer Ausgabe lautet die Portnummer 45765. Finden Sie auch Ihre Server-Portnummer heraus.
    
4.  Gehen Sie zurück zum Terminal, das in Aufgabe 1 geöffnet wurde, um den curl-Befehl auszuführen.
    
5.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das neue Terminal ein. Vergessen Sie nicht, _`<port>`_ durch Ihren Serverport zu ersetzen.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ curl http://localhost:45765/one
        remote_1
        $
        
6.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das neue Terminal ein. Vergessen Sie nicht, _`<port>`_ durch Ihren Serverport zu ersetzen.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ curl http://localhost:45765/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > Beachten Sie die Reihenfolge der Ergebnisse. Wie vom Namen vorgeschlagen, ruft diese Anforderung eine Remoteressource mehrmals nacheinander auf.
    
7.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das neue Terminal ein. Vergessen Sie nicht, _`<port>`_ durch Ihren Serverport zu ersetzen.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ curl http://localhost:45765/parallel
        Combined results: [remote_17, remote_16, remote_13, remote_20, remote_12, remote_19, remote_18, remote_14, remote_21, remote_15]
        $
        
    
    > Beachten Sie die Reihenfolge der Ergebnisse. Wie vom Namen vorgeschlagen, ruft diese Anforderung eine Remoteressource mehrmals parallel auf.
    
8.  Drücken Sie _Strg + C_ im Terminal, in dem der Befehl \*java -jar \* ausgeführt wird, um den Server zu stoppen.
    

## Aufgabe 3: Die Einfachheit der Nima-Anwendung analysieren

**Blockieren vs. Reaktiv**

Vergleichen wir die Implementierungen zwischen Níma (Blockieren) und Helidon SE (Reaktiv) für dieselbe Aufgabe.

*   Beide Implementierungen führen REST-Aufrufe mit dem Helidon WebClient aus
*   Die Implementierung der Blockierung ist einfach zu befolgen:
    *   Der Ausführungsablauf spiegelt sich in der Anweisungsreihenfolge im Code wider
    *   Es gibt keine Aufrufe komplizierter oder semantisch reicher Bibliotheksaufrufe
    *   Debugging ist einfach
*   Reaktive Versionen erfordern ein gutes Verständnis von reaktiven Bibliotheken
    *   Einschließlich Multi, flatMap, Fehlerbehandlung usw.
    *   Der Kontrollfluss ist aufgrund der Verwendung von reaktiven Handlern nicht mehr offensichtlich
    *   Ein Verständnis der Fähigkeit von flatMap zum Aktivieren/Ablehnen des gleichzeitigen Zugriffs ist erforderlich
    *   Debugging ist schwieriger

1.  Öffnen Sie die Datei _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_, um zu sehen, wie Endpunkte in der nima-Version der Anwendung implementiert werden. ![Nima Block](images/nima-block.png)
    
2.  Öffnen Sie die Datei _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_, um zu sehen, wie Endpunkte in der reaktiven Version der Anwendung implementiert werden. ![Reaktiver Service](images/reactive-service.png)
    
3.  Klicken Sie im Abschnitt _OPEN EDITORS_ mit der rechten Maustaste auf die Datei _BlockingService.java_, und wählen Sie _Für Vergleich auswählen_ aus. ![Vergleich auswählen](images/select-compare.png)
    
4.  Klicken Sie mit der rechten Maustaste auf die Datei _ReactiveService.java_, und wählen Sie _Mit Auswahl vergleichen_. ![Auswahl vergleichen](images/compare-selected.png)
    
5.  Sehen Sie, dass reaktiver Code komplizierter ist als das Blockieren (virtueller Thread) ![Code vergleichen](images/compare-code.png)
    
    > Prüfen Sie die Methoden eins, Sequenz und parallel in BlockingService bzw. ReactiveService. Sehen Sie, ob Sie verstehen, wie sie funktionieren!
    

Sie können jetzt _mit der nächsten Übung fortfahren_.

## Danksagungen

*   **Autor** - Joe DiPol
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Feb 2023
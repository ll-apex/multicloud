# Helidon-Anwendung entwickeln

## Einführung

In dieser Übung werden Sie durch die Schritte zum Erstellen einer Helidon-MP-Anwendung geführt.

Geschätzte Zeit: 15 Minuten

### Über Produkt/Technologie

Helidon ist einfach zu bedienen, mit Tools und Beispielen, um Sie schnell zum Laufen zu bringen. Da Helidon nur eine Sammlung von Bibliotheken ist, die auf einem schnellen Netty-Kern ausgeführt werden, gibt es keinen zusätzlichen Overhead oder Bloat. Helidon unterstützt MicroProfile und stellt vertraute APIs wie JAX-RS, CDI und JSON-P/B bereit. Unsere MicroProfile-Implementierung wird auf unserer schnellen Helidon Reactive WebServer ausgeführt. Die Reactive WebServer bietet ein modernes, funktionales Programmiermodell und läuft auf Netty. Der Helidon WebServer ist leicht, flexibel und reaktiv und bietet eine einfache und schnelle Grundlage für Ihre Microservices.

Mit Unterstützung für Health Checks, Metriken, Tracing und Fehlertoleranz bietet Helidon alles, was Sie zum Schreiben cloudfähiger Anwendungen benötigen, die in Prometheus, Jaeger/Zipkin und Kubernetes integriert sind.

### Helidon-CLI

Mit der Helidon-CLI können Sie ganz einfach ein Helidon-Projekt erstellen, indem Sie aus einer Gruppe von Archetypen auswählen. Es unterstützt auch eine Entwicklerschleife, die eine kontinuierliche Kompilierung und einen Anwendungsneustart durchführt, sodass Sie problemlos über Quellcodeänderungen iterieren können.

Die CLI wird zur einfachen Installation als eigenständige ausführbare Datei (kompiliert mit GraalVM) verteilt. Es ist derzeit als Download für Linux, Mac und Windows verfügbar. Laden Sie einfach die Binärdatei herunter, installieren Sie sie an einem Ort, auf den Sie von Ihrem PATH zugreifen können, und Sie können loslegen.

### Ziele

*   Helidon-CLI installieren
*   Erstellen Sie einen von MicroProfile unterstützten Microservice namens "Helidon Greeting".
*   Ausführen und Ausführen der Helidon-Gruß-App
*   Zustands- und Metrikdaten anzeigen
*   Neue Funktionalität zur Anwendung hinzufügen

### Voraussetzungen

*   Helidon erfordert Java 11+
*   Maven 3.6.x

> **Achtung: Verwenden Sie die Version 3.8.x aufgrund eines bekannten Problems mit der Anwendungserstellung nicht!**

*   Java und `mvn` befinden sich in Ihrem Pfad.
*   Windows-Benutzer benötigen auch die Visual C++ Redistributable Runtime.  
    Weitere Informationen finden Sie unter [Helidon unter Windows](https://helidon.io/docs/v2/#/about/04_windows).

## Aufgabe 1: Helidon-CLI installieren

1.  Helidon-CLI installieren
    
    Für macOS:
    
        <copy>
        curl -O https://helidon.io/cli/latest/darwin/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Unter Linux:
    
        <copy>
        curl -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Unter Windows:
    
        <copy>
        PowerShell -Command Invoke-WebRequest -Uri "https://helidon.io/cli/latest/windows/helidon.exe" -OutFile "C:\Windows\system32\helidon.exe"
        </copy>
        

## Aufgabe 2: Helidon-Grußanwendung erstellen

1.  Geben Sie in der Konsole Folgendes ein:
    
        <copy>helidon init --version 2.4.0 </copy>
        
    
    > Um potenzielle Probleme zu vermeiden, definieren Sie die spezifische Helidon-Version, die für die Umgebung dieser Übung getestet wurde.
    
2.  Für diese Demo erstellen wir einen von MicroProfile unterstützten Microservice. Wählen Sie daher die Option **(2)** für **Helidon MP Flavor**:
    
        Helidon flavor
        (1) SE 
        (2) MP 
        Enter selection (Default: 1): 2
        
3.  Wählen Sie für die meisten Funktionen die Option **(2) Schnellstart** und dann **Eingabetaste** für die Standardantworten aus. Beachten Sie, dass Sie unterschiedliche Standard-Package- und Projektgruppennamen haben können, da der BS-Benutzername verwendet wird. Notieren Sie sich den Packagenamen. Sie müssen ihn beim Erstellen einer neuen Java-Klasse verwenden.
    
        Select archetype
        (1) bare | Minimal Helidon MP project suitable to start from scratch 
        (2) quickstart | Sample Helidon MP project that includes multiple REST operations 
        (3) database | Helidon MP application that uses JPA with an in-memory H2 database 
        Enter selection (Default: 1): 2
        Project name (Default: quickstart-mp): 
        Project groupId (Default: me.user-helidon): 
        Project artifactId (Default: quickstart-mp): 
        Project version (Default: 1.0-SNAPSHOT): 
        Java package name (Default: me.user_name.mp.quickstart): 
        Switch directory to /home/user/quickstart-mp to use CLI
        
        Start development loop? (Default: n):
        $
        
    
    > Übernehmen Sie für die **Entwicklungsschleife** vorerst den Standardwert (**n**). Die Entwicklungsschleife wird später in dieser Übung gestartet.
    
    Sie haben jetzt ein voll funktionsfähiges Microservice-Maven-Projekt:
    

        quickstart-mp
        ├── Dockerfile
        ├── Dockerfile.jlink
        ├── Dockerfile.native
        ├── README.md
        ├── app.yaml
        ├── pom.xml
        └── src
            ├── main
            │   ├── java
            │   │   └── me
            │   │       └── buzz
            │   │           └── mp
            │   │               └── quickstart
            │   │                   ├── GreetResource.java
            │   │                   ├── GreetingProvider.java
            │   │                   └── package-info.java
            │   └── resources
            │       ├── META-INF
            │       │   ├── beans.xml
            │       │   ├── microprofile-config.properties
            │       │   └── native-image
            │       │       └── reflect-config.json
            │       └── logging.properties
            └── test
                └── java
                    └── me
                        └── buzz
                            └── mp
                                └── quickstart
                                    └── MainTest.java
    
    

## Aufgabe 3: Helidon-Grußanwendung ausführen

1.  Navigieren Sie von derselben Konsole/dem gleichen Terminal zum Verzeichnis quickstart-mp, und führen Sie die folgenden Befehle aus:
    
        <copy> cd quickstart-mp
        </copy>
        
2.  Mit JDK11+
    
        <copy>
        mvn package
        java -jar target/quickstart-mp.jar
        </copy>
        

### Anwendung ausführen

1.  Öffnen Sie einen neuen Terminal/eine neue Konsole, und führen Sie die folgenden Befehle aus, um die Anwendung zu prüfen.
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        

### Zustands- und Metrikdaten prüfen

1.  Führen Sie im selben Terminal/in derselben Konsole die folgenden Befehle aus, um den Zustand und die Metriken zu prüfen:
    
        <copy>
        curl -s -X GET http://localhost:8080/health
        </copy>
        {"outcome":"UP",...
        . . .
        
    
        # Prometheus Format
        <copy>
        curl -s -X GET http://localhost:8080/metrics
        </copy>
        # TYPE base:gc_g1_young_generation_count gauge
        . . .
        
    
        # JSON Format
        <copy>
        curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
        </copy>
        {"base":...
        . . .
        
2.  Stoppen Sie die Anwendung _quickstart-mp_, indem Sie `Ctrl + C` in das Terminal eingeben, in dem der Befehl "java -jar target/quickstart-mp.jar" ausgeführt wird.
    

## Aufgabe 4: Anwendung ändern

1.  Öffnen Sie Ihre bevorzugte IDE, und navigieren Sie zur Datei **microprofile-config.properties**.
    
    ![Konfigurationsdatei](images/config.jpg)
    
2.  Navigieren Sie in der Konsole/dem Terminal zum Projektordner, und geben Sie Folgendes ein:
    
        <copy>helidon dev</copy>
        
    
    > Dadurch wird die in der vorherigen Aufgabe erwähnte **Entwicklungsschleife** gestartet.
    
3.  Ändern Sie die Eigenschaft _app.greeting_ in "Hallo Oracle", und speichern Sie die Datei.
    
        <copy>app.greeting=Hello Oracle</copy>
        
    
    ![HelidonDev](images/properties.jpg)
    
    > Wenn Sie eine Datei ändern, erkennt die **Helidon-CLI**, dass eine Änderung vorliegt, kompiliert die App neu und führt sie erneut aus. Da Helidon klein ist, passiert alles schnell.
    
4.  Geben Sie in der Konsole/dem Terminal Folgendes ein:
    
        <copy>curl -X GET http://localhost:8080/greet</copy>
        
    
    Das Ergebnis sollte sein:
    
        {"message":"Hello Oracle World!"}
        
    
    > Stoppen Sie unbedingt die Entwicklungsschleife mit `CTRL+C`
    
5.  Gehen Sie zurück zum Projektordner, und öffnen Sie die Datei **GreetResource.java**.
    
    > Sie können sehen, dass es sich um reinen MicroProfile-kompatiblen Code handelt:
    
    ![ModifyJava](images/greet-resource.jpg)
    
6.  Erstellen Sie einen neuen Endpunkt, der Hilfe für verschiedene Begrüßungen in verschiedenen Sprachen bietet. Um diese neue Funktionalität zu erstellen, erstellen Sie eine neue Klasse namens **GreetHelpResource** mit dem folgenden Code:
    
        <copy>
        package me.user_name.me.quickstart;
        import java.util.Arrays;
        import java.util.List;
        import java.util.logging.Logger;
        
        import javax.enterprise.context.ApplicationScoped;
        import javax.ws.rs.GET;
        import javax.ws.rs.Path;
        
        import org.eclipse.microprofile.metrics.annotation.Counted;
        
        @ApplicationScoped
        @Path("/help")
        public class GreetHelpResource {
        
            Logger LOGGER = Logger.getLogger(GreetHelpResource.class.getName());
        
            @GET
            @Path("/allGreetings")
            @Counted(name = "helpCalled", description = "How many time help was called")
            public String getAllGreetings(){
                LOGGER.info("Help requested!");
                return Arrays.toString(List.of("Hello","Привет","Hola","Hallo","Ciao","Nǐ hǎo", "Marhaba","Olá").toArray());
            }
        }
        </copy>
        
    
    > Die Klasse hat nur eine Methode _getAllGreetings_, die eine Liste mit Begrüßungen in verschiedenen Sprachen zurückgibt. Achten Sie beim Kopieren des Codes darauf, den erforderlichen Paketnamen über der Klasse hinzuzufügen.
    
7.  Erstellen Sie die Anwendung, und führen Sie sie aus:
    
        <copy>
        mvn package -DskipTests
        java -jar target/quickstart-mp.jar
        </copy>
        
8.  Führen Sie den folgenden Befehl aus, und achten Sie auf die Ergebnisse:
    
        <copy>curl http://localhost:8080/help/allGreetings</copy>
        
    
    Das erwartete Ergebnis:
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
9.  Schauen Sie sich die Metriken an und Sie werden sehen, dass ein neuer Zähler erschienen ist. Wenn dieser Endpunkt aufgerufen wird, erhöht sich der Wert:
    
        curl http://localhost:8080/metrics
        
        ...
        # TYPE application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total counter
        # HELP application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total How many time help was called
        application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total 1
        ...
        
10.  In der Konsole wird nun die INFO-Logzeile zu diesem Aufruf angezeigt:
    
        INFO me.buzz.mp.quickstart.GreetHelpResource Thread[helidon-4,5,server]: Help requested!
        
    
    Der neue Endpunkt wurde hinzugefügt.
    
    ![NewEndpoint](images/logs-output.jpg)
    
    > Die Arbeit mit Helidon und seinen Tools ist einfach und schnell!
    
11.  Lassen Sie das Terminal/die Konsole geöffnet, und fahren Sie mit der Verrazzano-Installationsübung fort.
    

## Danksagungen

*   **Autor** - Dmitry Aleksandrov
*   **Mitwirkende** - Maciej Gruszka, Ankit Pandey
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, März 2023
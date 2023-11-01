# Generieren Sie ein Helidon MP-Projekt, und führen Sie das Projekt im Codeeditor aus

## Einführung

In dieser Übung werden Sie durch die Schritte zum Erstellen einer Helidon-MP-Anwendung geführt.

Geschätzte Zeit: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Helidon MP-Projekt generieren und Projekt im Codeeditor ausführen](videohub:1_22nv8v4q)

### Über Produkt/Technologie

Helidon ist einfach zu bedienen, mit Tools und Beispielen, um Sie schnell zum Laufen zu bringen. Da Helidon nur eine Sammlung von Bibliotheken ist, die auf einem schnellen Netty-Kern ausgeführt werden, gibt es keinen zusätzlichen Overhead oder Bloat. Helidon unterstützt MicroProfile und stellt vertraute APIs wie JAX-RS, CDI und JSON-P/B bereit. Unsere MicroProfile-Implementierung wird auf unserer schnellen Helidon Reactive WebServer ausgeführt. Die Reactive WebServer bietet ein modernes, funktionales Programmiermodell und läuft auf Netty. Der Helidon WebServer ist leicht, flexibel und reaktiv und bietet eine einfache und schnelle Grundlage für Ihre Microservices.

Mit Unterstützung für Health Checks, Metriken, Tracing und Fehlertoleranz bietet Helidon alles, was Sie zum Schreiben cloudfähiger Anwendungen benötigen, die in Prometheus, Jaeger/Zipkin und Kubernetes integriert sind.

### Über Helidon Project Starter

Project Starter ist eine neue Web-UI zum Erstellen von Helidon-Projekten. Es ist sehr anpassbar und bietet verschiedene Optionen, mit denen Benutzer Helidon-Funktionen auswählen können, die sie dem Projekt hinzufügen möchten. Endbenutzer können Projekte entsprechend ihren spezifischen Anforderungen generieren. Weitere Informationen erhalten Sie, wenn Sie auf [Helidon Starter](https://helidon.io/starter) klicken.

### Info über Codeeditor

Mit dem Codeeditor können Sie Code für verschiedene OCI-Services direkt über die OCI-Konsole bearbeiten und bereitstellen. Sie können jetzt Serviceworkflows und Skripte aktualisieren, ohne zwischen der Konsole und Ihren lokalen Entwicklungsumgebungen wechseln zu müssen. So können Sie schnell Prototypen von Cloud-Lösungen erstellen, neue Services erkunden und schnelle Codierungsaufgaben ausführen.

Durch die direkte Integration des Codeeditors mit Cloud Shell können Sie auf das in Cloud Shell vorinstallierte GraalVM Enterprise Native Image und JDK 17 (Java Development Kit) zugreifen.

### Informationen zu OCI Cloud Shell

[OCI Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm) ist ein browserbasiertes Terminal, auf das über die Oracle Cloud-Konsole zugegriffen wird. Sie bietet Zugriff auf eine Linux-Shell mit einer vorab authentifizierten OCI-Befehlszeilenschnittstelle (CLI) und vorinstallierten Entwicklertools sowie 5 GB Speicher.

Ab Version 22.2.0 sind GraalVM Enterprise JDK 17 und Native Image in Cloud Shell vorinstalliert.

> GraalVM Enterprise ist ohne zusätzliche Kosten in Oracle Cloud Infrastructure verfügbar.

### Ziele

*   Erstellen Sie einen von MicroProfile unterstützten Microservice namens Helidon Greeting-Anwendung
*   Helidon-Anwendung im Codeeditor öffnen
*   Standard-JDK in der cloud shell ändern
*   Erforderliche Maven konfigurieren
*   Ausführen und Ausführen der Helidon-Gruß-App

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.

## Aufgabe 1: Helidon-Projekt mit Projektstarter generieren

1.  Kopieren Sie die folgende URL, und fügen Sie sie in den Browser ein, um die Helidon-Projektseite zu öffnen.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  Wählen Sie unter "Projekt generieren" die Option _Helidon MP_ als Helidon-Aroma aus, und klicken Sie auf _Weiter_.
    
3.  Wählen Sie unter "Anwendungstyp" die Option _Schnellstart_ aus, und klicken Sie auf _Weiter_.
    
4.  Wählen Sie für die Medienunterstützung _JSON-B_ aus, und klicken Sie auf _Weiter_.
    
5.  Wählen Sie unter "Projekt anpassen" die Standardwerte aus, und klicken Sie auf _Downloads_. Dies wird in einem Fenster angezeigt. Speichern Sie diese _myproject.zip_ im gewünschten Speicherort. Im Rest dieses Workshops wird meinProjektname verwendet. Wenn Sie einen anderen Namen wählen, ändern Sie bitte den Namen.
    

## Aufgabe 2: Helidon-Projekt lokal erstellen und ausführen

1.  Klicken Sie in der Cloud-Konsole auf das Symbol _Entwicklertools_ (siehe Abbildung) und dann auf _Codeeditor_. ![Codeeditor](images/code-editor.png)
    
2.  Klicken Sie im Codeeditor auf _Terminal_ -> _Neues Terminal_. ![Terminal öffnen](images/open-terminal.png)
    
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um den Ordner _myproject_ zu erstellen. Hier wird die Standarddatei myproject.zip heruntergeladen.
    
        <copy>mkdir -p ~/helidon-project/myproject</copy>
        
4.  Klicken Sie im Codeeditor auf _Datei_ -> _Öffnen_. ![Ordner öffnen](images/open-folder.png)
    
5.  Wählen Sie den Ordner _helidon-project_ aus, und klicken Sie auf _Öffnen_. ![Helidon öffnen](images/open-helidon.png)
    
6.  Im Codeeditor wird das _HELIDON-PROJECT_ im EXPLORER-Fenster angezeigt. Hier wird der Ordner _myproject_ angezeigt, und klicken Sie darauf. Klicken Sie jetzt auf _Datei_ -> _Dateien hochladen..._, geben Sie den Speicherort an, in den Sie das Projekt heruntergeladen haben, wählen Sie die ZIP-Datei aus, und klicken Sie auf _Öffnen_. ![Mein Projekt](images/myproject.png) ![Dateien hochladen](images/upload-files.png)
    
7.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Datei _myproject.zip_ zu dekomprimieren.
    
        <copy>cd ~/helidon-project/myproject
        unzip myproject.zip
        </copy>
        
8.  Blenden Sie den Ordner _myproject_ ein, um die Projektstruktur anzuzeigen. ![Projekt einblenden](images/expand-project.png)
    
9.  Zur Ausführung dieses Projekts werden Maven 3.8+ und JDK 17+ verwendet. In der Oracle Cloud stehen Ihnen verschiedene JDK zur Verfügung. Hier wählen Sie GraalVM JDK aus. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um Ihr Standard-JDK zu kennen.
    
        <copy>csruntimectl java list</copy>
        
    
    ![JDK auflisten](images/list-jdk.png)
    
    > Das JDK mit \* _Sternchen_ am Anfang ist Ihr Standard-JDK. Wenn Sie ein anderes JDK haben, dann graalvmeejdk, ändern Sie die JDK-Standardversion, indem Sie den folgenden Befehl ausführen. Bitte verwenden Sie die gezeigte Version von graalvmeejdk, da es sich von der im Befehl angezeigten unterscheiden kann.
    
        <copy>csruntimectl java set graalvmeejdk-17</copy>
        
10.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um den erforderlichen Maven zu konfigurieren.
    
        <copy>cd ~/helidon-project/myproject/
        wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
        tar -xzvf apache-maven-3.8.8-bin.tar.gz
        PATH=~/helidon-project/myproject/apache-maven-3.8.8/bin:$PATH</copy>
        
    
    ![Maven konfigurieren](images/configure-maven.png)
    
11.  Um zu prüfen, ob Sie die korrekte Version von JDK und Maven wie unten gezeigt haben, führen Sie den folgenden Befehl im Terminal aus.
    
        <copy>mvn -v</copy>
        
    
    ![Voraussetzung prüfen](images/verify-prerequisite.png)
    
12.  Führen Sie im Ordner _myproject_ den folgenden Befehl aus, um das Projekt zu erstellen.
    
        <copy>cd ~/helidon-project/myproject/myproject
        mvn clean package</copy>
        
    
    ![Projekt erstellen](images/build-project.png)
    
    > Am Ende der Ausführung dieses Befehls sollte _BUILD SUCCESS_ angezeigt werden.
    
13.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um diese Anwendung auszuführen. Die Ausgabe wird ähnlich wie im folgenden Screenshot angezeigt.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    ![Projekt ausführen](images/run-project.png)
    

> Beachten Sie, dass die Startzeit 4822 Millisekunden beträgt. Diese Zeit wird später mit der ausführbaren nativen Imagedatei verglichen.

14.  Öffnen Sie ein neues Terminal/eine neue Konsole im Codeeditor, und führen Sie die folgenden Befehle aus, um die Anwendung zu prüfen:
    
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
        
15.  _Stoppen Sie die Anwendung **myproject**, indem Sie `Ctrl + C` IN das Terminal eingeben, IN dem der Befehl "java -jar target/myproject.jar" ausgeführt wird_. ES IST SEHR WICHTIG, SONSTIG WERDEN SIE IM LABELRECHT GESICHTEN.
    

## Danksagungen

*   **Autor** - Dmitry Aleksandrov
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, April 2023
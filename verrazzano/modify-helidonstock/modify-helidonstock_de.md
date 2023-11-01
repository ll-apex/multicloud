# Buchanwendung von Bobby ändern und neues Anwendungskomponentenbild erstellen

## Einführung

In dieser Übung ändern wir die Anwendung "bobbys-helidon-stock-application" mit der _Cloud Shell_. Später erstellen wir ein neues Docker-Image für bobbys-helidon-stock-application. Dieses bobbys-helidon-stock-application-Bild ist eine Komponente für die Bobby's Books-Anwendung.

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Bobbys-helidon-stock-application ändern.
*   Erstellen Sie ein neues Docker-Image für bobbys-helidon-stock-application.

### Voraussetzungen

Sie benötigen einen Texteditor, in dem Sie die Befehle und URLs einfügen und entsprechend Ihrer Umgebung ändern können. Anschließend können Sie die geänderten Befehle zum Ausführen in die _Cloud Shell_ kopieren und einfügen.

## Aufgabe 1: Bobbys-helidon-stock-application ändern

1.  Wählen Sie die Registerkarte "Bobby's Book" aus, klicken Sie auf _Books_, und klicken Sie dann auf das Bild für das Buch _The Hobbit_ (siehe Abbildung):
    
    ![Klicken Sie auf "Bücher".](images/clickbooks.png " ")
    
    Der Buchname wird im Format _Der Hobbit_ angezeigt (siehe Abbildung).
    
    ![Der Hobbit](images/thehobbit.png " ")
    
2.  Wir möchten den Buchnamen in Großbuchstaben (THE HOBBIT) umwandeln. Wir müssen den Quellcode für die Bobby's Books-Anwendung herunterladen. Stellen Sie sicher, dass Sie sich im Home-Ordner befinden. Kopieren Sie die folgenden Befehle, und fügen Sie sie in die _Cloud Shell_ ein.
    
        <copy>cd ~
        git clone -b v0.16.0 https://github.com/verrazzano/examples.git</copy>
        
    
    ![Repository kopieren](images/clonerepository.png " ")
    
3.  Um die Dateien in der Bobby's Book-Anwendung anzuzeigen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>ls -la ~/examples/bobs-books/</copy>
        
    
    Die Ausgabe sollte ungefähr wie folgt aussehen: `bash $ ls -la ~/examples/bobs-books/ total 16 drwxr-xr-x. 5 ankit_x_pa oci 100 Mar 21 12:14 . drwxr-xr-x. 7 ankit_x_pa oci 4096 Mar 21 12:14 .. drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobbys-books drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobs-bookstore-order-manager -rw-r--r--. 1 ankit_x_pa oci 942 Mar 21 12:14 README.md drwxr-xr-x. 4 ankit_x_pa oci 72 Mar 21 12:14 roberts-books $`
    
4.  Nun werden wir Änderungen in der entsprechenden JAVA\_FILE vornehmen. Um die Datei zu öffnen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>vi ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/src/main/java/org/books/bobby/BookResource.java</copy>
        
    
    ![Datei öffnen](images/openfile.png " ")
    
5.  Drücken Sie _i_, um den Code zu ändern. Um eine neue Zeile in Zeile 84 hinzuzufügen, kopieren Sie die folgende Zeile, und fügen Sie sie wie dargestellt in Zeile 84 ein:
    
        <copy>optional.get().setTitle(optional.get().getTitle().toUpperCase());</copy>
        
    
    ![Linie einfügen](images/insertline.png " ")
    
6.  Drücken Sie _Esc_ und dann _:wq_, um die Änderungen zu speichern.
    
    ![Speichern Sie die Änderungen](images/savechanges.png " ")
    

## Aufgabe 2: Neues Docker-Image für die bobbys-helidon-stock-application erstellen

1.  Da wir ein neues Docker-Image für _bobbys-helidon-stock-application_ erstellen, das Abhängigkeiten von _bobbys-coherence-application_ aufweist, führen wir einen _Maven_\-Befehl aus, um das vorhandene bobbys-coherence-application-Archiv zu bereinigen und ein neues bobby-coherence-application-Archiv in einem lokalen Maven-Repository zu kompilieren, zu erstellen, zu verpacken und zu installieren. Um in den Verzeichnisordner _bobbys-coherence_ zu wechseln und das Anwendungsarchiv _bobbys-coherence_ in einem lokalen Maven-Repository zu kompilieren, zu erstellen und zu installieren, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>
        cd ~/examples/bobs-books/bobbys-books/bobbys-coherence/
        mvn clean install
        </copy>
        
    
    Die resultierende Ausgabe ähnelt der folgenden (abgekürzt, dass nur Start und Ende der Ausgabe angezeigt werden): `bash $ mvn clean install [INFO] Scanning for projects... [INFO] [INFO] ------------< io.verrazzano.example.books:bobbys-coherence >------------ [INFO] Building bobbys-coherence 1.0-SNAPSHOT [INFO] --------------------------------[ jar ]--------------------------------- [INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ bobbys-coherence --- [[INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/target/bobbys-coherence.jar to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.jar [INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/pom.xml to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.pom [INFO] ------------------------------------------------------------------------ [INFO] BUILD SUCCESS [INFO] ------------------------------------------------------------------------ [INFO] Total time: 8.887 s [INFO] Finished at: 2023-03-22T04:09:11Z [INFO] ------------------------------------------------------------------------ $`
    
2.  Da wir _bobbys-helidon-stock-application_ geändert haben, müssen wir diese Anwendung kompilieren, erstellen und verpacken. Um in das Verzeichnis _bobbys-helidon-stock-application_ zu wechseln und _bobbys-helidon-stock-application_ in eine JAR-Datei zu verpacken, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein. Im zweiten Bild wird die Erstellung der Datei `bobbys-helidon-stock-application.JAR` unter `~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target` angezeigt.
    
        <copy>cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/
        mvn clean package
        </copy>
        
    
    The resulting output is similar to the following (abbreviated to show only the start and end of output): \`\`\`bash $ cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/ $ mvn clean package \[INFO\] Scanning for projects... \[INFO\] ------------------------------------------------------------------------ \[INFO\] Detecting the operating system and CPU architecture \[INFO\] ------------------------------------------------------------------------
    
         [INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ bobbys-helidon-stock-application ---
         [INFO] Building jar: /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target/bobbys-helidon-stock-application.jar
         [INFO] ------------------------------------------------------------------------
         [INFO] BUILD SUCCESS
         [INFO] ------------------------------------------------------------------------
         [INFO] Total time:  10.106 s
         [INFO] Finished at: 2023-03-22T04:11:38Z
         [INFO] ------------------------------------------------------------------------
         $
         ```
        
3.  Wir werden ein Docker-Image für bobby-stock-helidon-application erstellen, aber diese Anwendung verwendet eine bestimmte Version von JDK und wir möchten die Docker-Dateien, die das neue Image erstellen, nicht ändern. Daher laden wir das erforderliche JDK herunter. Um die erforderliche JDK-Version herunterzuladen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2_linux-x64_bin.tar.gz</copy>
        
    
    Die Ausgabe sollte wie folgt aussehen: \`\`bash $ wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz --2023-03-22 04:20:16-- https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz Resolving download.java.net (download.java.net)... 2.23.220.73 Verbindung zu download.java.net (download.java.net)|2.23.220.73|:443... connected. HTTP-Anforderung gesendet, auf Antwort wartet... 200 OK Länge: 198606200 (189M) \[application/x-gzip\] Speichern in: 'openjdk-14.0.2\_linux-x64\_bin.tar.gz'
    
         100%[==============================================================================================================================>] 198,606,200  194MB/s   in 1.0s   
        
         2023-03-22 04:20:17 (194 MB/s) - ‘openjdk-14.0.2_linux-x64_bin.tar.gz’ saved [198606200/198606200]
         $
         ```
        
4.  Wir erstellen ein Docker-Image, das wir in Übung 6 in die Oracle Cloud Container Registry hochladen. Um den Dateinamen zu erstellen, benötigen wir die folgenden Informationen:
    
    *   Mandanten-Namespace
    *   Endpunkt für die Region
5.  Um den Namespace des Mandanten zu suchen, klicken Sie wie gezeigt auf das Symbol _Benutzer_ -> _Mandant_. In den **Object Storage-Einstellungen** finden Sie den Namespace. Kopieren und speichern Sie es irgendwo in Ihrer Textdatei, da wir es auch in Übung 6 verwenden werden.
    
    ![Mandantennamensraum kopieren](images/copy-tenancynamespace.png " ")
    
6.  Suchen Sie den _Endpunkt für Ihre Region_. Weitere Informationen finden Sie in der Tabelle, die unter dieser URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) dokumentiert ist. Im dargestellten Beispiel lautet der Endpunkt für die Region _UK South (London)_ (als Regionsname), und der Endpunkt ist _lhr.ocir.io_. Suchen Sie den Endpunkt für Ihren eigenen _Regionsnamen_, und speichern Sie ihn in der Textdatei. Sie werden es auch für das nächste Labor benötigen.
    
    ![Endpunkte](images/end-point.png " ")
    
    > Jetzt haben Sie sowohl den Mandanten-Namespace als auch den Endpunkt für Ihre Region.
    
7.  Jetzt haben Sie sowohl den Mandanten-Namespace als auch den Endpunkt für Ihre Region. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihren Texteditor ein. Ersetzen Sie dann **`END_POINT_OF_YOUR_REGION`** durch den Endpunkt Ihres Regionsnamens, **`NAMESPACE_OF_YOUR_TENANCY`** durch den Namespace Ihres Mandanten und **`your_first_name`** durch Ihren Vornamen in Kleinbuchstaben.
    
        <copy>docker build --force-rm=true -f Dockerfile -t END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0 .</copy>
        
    
    ![Docker Build](images/docker-build.png " ")
    
    ![Bild erstellt](images/image-created.png " ")
    
    > Beispiel: In meinem Fall lautet der Befehl `docker build --force-rm=true -f Dockerfile -t lhr.ocir.io/tenancynamespace/helidon-stock-application-ankit`.
    
    Dadurch wird das Docker-Image erstellt, das in Übung 6 in das Oracle Cloud Container Registry-Repository übertragen wird. Sie müssen den ersetzten vollständigen Imagenamen **`END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0`** in den Text editor.In Übung 6 kopieren. Wenn Sie das Repository erstellen müssen, müssen Sie ihm den Namen **`helidon-stock-application-your_first_name`** geben.
    
    Lassen Sie die _Cloud Shell_ geöffnet. Sie benötigen sie für die nächste Übung.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
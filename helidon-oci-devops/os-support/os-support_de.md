# Helidon-Anwendung ändern, um Object Storage-Unterstützung hinzuzufügen

## Einführung

In dieser Übung wird gezeigt, **wie Sie Object Storage-Zugriff über die Helidon-Anwendung hinzufügen**. Dazu ersetzen Sie eine Variable, die zum Speichern des Begrüßungsworts verwendet wird, durch ein Objekt, das jetzt zum neuen Begrüßungswortcontainer wird und **aus einem Objektspeicher-Bucket abgerufen** wird. Da das Objekt dauerhaft gespeichert wird, überlebt der letzte Begrüßungswortwert die Neustarts der Anwendung. Ohne diese Änderung und mit dem Grußwort im Speicher über die Variable wird das Grußwort auf einen Standardwert zurückgesetzt, wenn die Anwendung neu gestartet wird.

Geschätzte Zeit: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Object Storage-Unterstützung](videohub:1_p5v2wehm)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Ändern Sie die Helidon-Anwendung, um ihre Integration mit OCI-Services wie Object Storage anzuzeigen
*   Erfolgreiche Object Storage-Integration prüfen

### Voraussetzung

*   Free Tier-(Testversion), kostenpflichtiger oder LiveLabs Cloud-Account für Oracle

## Aufgabe 1: Helidon-Anwendung für Object Storage-Integration ändern

1.  Stellen Sie sicher, dass **oci.bucket.name** in **~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties** richtig konfiguriert ist, was bereits in Schritt 5 von Übung 2/Aufgabe 3 festgelegt sein sollte.
    
2.  Klicken Sie im **Codeeditor** auf den Dateinamen **`pom.xml`** unter _~/OCI-mp/server/_, um ihn zu öffnen und die **Object Storage-OCI-SDK**\-Abhängigkeit innerhalb der **dependencies**\-Klausel hinzuzufügen, wie unten gezeigt.
    
        <copy><dependency>
                    <groupId>com.oracle.oci.sdk</groupId>
                    <artifactId>oci-java-sdk-objectstorage</artifactId>
        </dependency></copy>
        
    
    ![Abhängigkeit hinzufügen](images/add-dependency.png)
    
    > Stellen Sie sicher, dass die Einrückung korrekt ist.
    
3.  Wir ändern die Datei _~/oci-mp/server/src/main/java/ocw/hol/mp/oci/server/GreetingProvider.java_, um den **Object Storage**\-Zugriff hinzuzufügen. Im Interesse der Zeit haben wir den Quellcode für diese Änderung. Kopieren Sie den folgenden Code, und fügen Sie ihn ein, um diese Datei mit den erforderlichen Änderungen zu aktualisieren. Im _Hinweis_ unten werden die Änderungen beschrieben, die in dieser Datei vorgenommen werden.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/source/GreetingProvider.java server/src/main/java/ocw/hol/mp/oci/server/</copy>
        
    
    ![Objektspeicher hinzugefügt](images/os-added.png)
    
    > **Bitte lesen Sie:-**
    
    *   Fügen Sie den Parameter _ObjectStorage objectStorageClient_ aus dem Argumentabschnitt des Konstruktors hinzu. Da dies Teil der Annotation _@Injected_ ist, wird der Parameter automatisch von Helidon verarbeitet und so festgelegt, dass er den Client enthält, der für die Kommunikation mit dem Object Storage-Service verwendet werden kann, ohne dafür mehrere Zeilen **OCI-SDK**\-Code hinzufügen zu müssen.
    *   Fügen Sie aus dem Argumentabschnitt desselben Konstruktors die Datei **ConfigProperty** hinzu, die einen Wert aus einer Eigenschaft **oci.bucket.name** in der Konfiguration extrahiert. Dies wurde zuvor während des anfänglichen Anwendungssetups in **microprofile-config.properties** aufgefüllt, als ein Utilityskript namens **`update_config_values.sh`** aus dem Repository-Verzeichnis **`devops_helidon_to_instance_ocw_hol`** ausgeführt wurde.
    *   Rufen Sie mit der Methode **getNamespace() Object Storage SDK** den Object Storage-Namespace ab, da er später zum Abrufen oder Speichern eines Objekts verwendet wird:
    
        <copy>public GreetingProvider(@ConfigProperty(name = "app.greeting") String message,
                            ObjectStorage objectStorageClient,
                            @ConfigProperty(name = "oci.bucket.name") String bucketName) {
        try {
            this.bucketName = bucketName;
            GetNamespaceResponse namespaceResponse =
                    objectStorageClient.getNamespace(GetNamespaceRequest.builder().build());
            this.objectStorageClient = objectStorageClient;
            this.namespaceName = namespaceResponse.getValue();
            LOGGER.info("Object storage namespace: " + namespaceName);
            setMessage(message);
        } catch (Exception e) {
            LOGGER.warning("Error invoking getNamespace from Object Storage: " + e);
        }
        }     </copy>
        
    
    *   Direkt unter der Klasse GreetingProvider wurde die Variablendeklaration entfernt:
    
        <copy>private final AtomicReference<String> message = new AtomicReference<>();</copy>
        
    
    *   Es wurde durch lokale globale Variablen wie unten ersetzt. _LOGGER_ wird für das Logging verwendet, während _objectStorageClient_, _namespaceName_, _bucketName_ und _objectName_ für _Object Storage-SDK_\-Aufrufe verwendet werden.
    
        <copy>private static final Logger LOGGER = Logger.getLogger(GreetingProvider.class.getName());
        private ObjectStorage objectStorageClient;
        private String namespaceName;
        private String bucketName;
        private final String objectName = "hello.txt";</copy>
        
    
    *   Der Inhalt von _getMessage()_ wurde durch den folgenden Code ersetzt. Dadurch wird die **getObject() SDK**\-Methode verwendet, um das Begrüßungswort aus dem aus dem Bucket abgerufenen Objekt **hello.txt** abzurufen.
    
        <copy>try {
        GetObjectResponse getResponse =
                objectStorageClient.getObject(
                        GetObjectRequest.builder()
                                .namespaceName(namespaceName)
                                .bucketName(bucketName)
                                .objectName(objectName)
                                .build());
        return new String(getResponse.getInputStream().readAllBytes());
        } catch (Exception e) {
            LOGGER.warning("Error invoking getObject from Object Storage: " + e);
            return "Hello-Error";
        }</copy>
        
    
    *   Der Inhalt der void-Methode **setMessage(String message)** wurde durch den folgenden Code ersetzt. Mit der **putObject() SDK**\-Methode wird das Objekt **hello.txt** mit dem Begrüßungswort im Bucket gespeichert.
    
        <copy>try {
        byte[] contents = message.getBytes();
        PutObjectRequest putObjectRequest =
                PutObjectRequest.builder()
                        .namespaceName(namespaceName)
                        .bucketName(bucketName)
                        .objectName(objectName)
                        .putObjectBody(new ByteArrayInputStream(message.getBytes()))
                        .contentLength(Long.valueOf(contents.length))
                        .build();
        objectStorageClient.putObject(putObjectRequest);
        } catch (Exception e) {
            LOGGER.warning("Error invoking putObject from Object Storage: " + e);
        }</copy>
        
    
    *   Der Import wurde entfernt (**import java.util.concurrent.atomic.AtomicReference;**) und durch diese neuen Importe ersetzt, um den gesamten neuen Code zu unterstützen, der hinzugefügt wurde.
    
        <copy>import java.io.ByteArrayInputStream;
        import java.util.logging.Logger;
        
        import com.oracle.bmc.objectstorage.ObjectStorage;
        import com.oracle.bmc.objectstorage.requests.GetNamespaceRequest;
        import com.oracle.bmc.objectstorage.requests.GetObjectRequest;
        import com.oracle.bmc.objectstorage.requests.PutObjectRequest;
        import com.oracle.bmc.objectstorage.responses.GetNamespaceResponse;
        import com.oracle.bmc.objectstorage.responses.GetObjectResponse;</copy>
        

## Aufgabe 2: Helidon-Anwendungscodeänderung per Push übertragen und Pipeline DevOps auslösen

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, **um die Änderung festzuschreiben und zu übertragen**.
    
        <copy>git add .
        git status
        git commit -m "Add support Object Storage integration"
        git push</copy>
        
    
    > Die Pipeline wird durch diesen Git-Push ausgelöst.
    
2.  Warten Sie, bis der **DevOps-Lebenszyklus** abgeschlossen ist, indem Sie die Build- und Deployment-Pipelinelogs überwachen. ![Deployment-Lauf](images/deployment-run.png)
    

## Aufgabe 3: Erfolgreiche Object Storage-Integration prüfen

Testen Sie mit curl, und prüfen Sie, ob dem **Bucket** ein neues **hello.txt**\-Objekt hinzugefügt wurde. Überprüfen Sie, ob die Größe des Objekts der Größe des Begrüßungsworts entspricht. Beispiel: Wenn das Begrüßungswort _Hallo_ lautet, muss die Größe **5** lauten. Wenn das Begrüßungswort _Hola_ lautet, muss die Größe **4** lauten.

1.  Richten Sie den Deployment-Knoten **`PUBLIC_IP`** als Umgebungsvariable ein.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um eine **GET**\-Anforderung zu erstellen. Sie erhalten eine ähnliche Ausgabe wie unten gezeigt.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  Klicken Sie in **Cloud Console** auf _Hamburger-Menü_ -> _Speicher_ -> _Buckets_ (siehe Abbildung). ![Bucket-Menü](images/bucket-menu.png)
    
4.  Wählen Sie das richtige Compartment aus, und klicken Sie auf **app-bucket-helidonocw-hol-string**, um den **Bucket** zu öffnen. ![Compartment auswählen](images/select-compartment.png)
    
5.  Stellen Sie sicher, dass der Bucket jetzt das Objekt **hello.txt** mit einer Größe von **5 Byte** enthält, da das Begrüßungswort **Hallo** lautet. Sie können das Objekt auch herunterladen und prüfen, ob der Inhalt tatsächlich _Hallo_ ist. ![Größe prüfen](images/verify-size.png)
    
    > Lassen Sie diese Seite geöffnet, da wir diese Seite aktualisieren, sobald wir das Begrüßungswort im nächsten Schritt ändern.
    
6.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um das Begrüßungswort **Hallo** durch **Hola** zu ersetzen.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
7.  Prüfen Sie, ob das Objekt **hello.txt** des Buckets jetzt eine Größe von **4 Byte** hat, weil das Begrüßungswort durch **Hola** ersetzt wird. Sie können das Objekt auch herunterladen und prüfen, ob der Inhalt in **Hola** geändert wurde. ![Hola-Größe](images/hola-size.png)
    
8.  Starten Sie die Anwendung mit dem Tool **restart.sh** neu, um zu demonstrieren, dass der Wert des Begrüßungsworts erhalten bleibt, während es im **Object Storage** persistiert wird.
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/restart.sh</copy>
        Created private.key and can be used to ssh to the deployment instance by running this command: "ssh -i private.key opc@xx.xx.xx.xx"
        FIPS mode initialized
        The authenticity of host 'xx.xx.xx.xx (xx.xx.xx.xx)' can't be established.
        ECDSA key fingerprint is SHA256:hJl8axCNhFcILDo+AwxMkodxhY+UxRD40d1ans83GTg.
        ECDSA key fingerprint is SHA1:IBUhyn05DaIs60GAQsruVXajhym.
        Are you sure you want to continue connecting (yes/no)? yes
        Warning: Permanently added 'xx.xx.xx.xx' (ECDSA) to the list of known hosts.
        Starting oci-mp-server.jar
        Helidon app is now running with pid 264792!
        Cleaning up ssh private.key
        
    
    ![Anwendung neu starten](images/restart-application.png)
    
    > Geben Sie _Ja_ ein, wenn Sie nach **Möchten Sie wirklich mit der Verbindung fortfahren (ja/nein)?** gefragt werden.
    
9.  Rufen Sie die Standardanfrage "Hello World" auf, und **beachten Sie, dass das Begrüßungswort immer noch "Hola" lautet**.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
10.  Aktualisieren Sie die Seite "Bucket" in der **Cloud-Konsole**, und Sie werden feststellen, dass die Größe immer noch **4** beträgt. Dadurch wird bestätigt, dass das Begrüßungswort weiterhin **Hola** lautet. ![Persistenz prüfen](images/verify-persistence.png)
    

**Herzlichen Glückwunsch!** Sie haben den Workshop abgeschlossen. Wenn Sie möchten, können Sie mit **Übung 6** fortfahren. Dadurch werden **alle Ressourcen gelöscht**, die während dieses Workshops erstellt wurden.

## Weitere Informationen

*   [Helidon OCI-Integration](https://helidon.io/docs/v3/#/mp/integrations/oci)

## Danksagungen

*   **Autor** - Keith Lustria
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
# Helidon-Anwendungsimage erstellen und an Oracle Cloud Container Registry übertragen

## Einführung

In dieser Übung erstellen Sie ein _natives_ Docker-Image mit Ihrer Helidon-Anwendung und pushen dieses Image in ein Repository in der Oracle Cloud Container Registry.

Geschätzte Zeit: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Helidon-Anwendungsimage erstellen und in Oracle Cloud Container Registry übertragen](videohub:1_mh1brw5t)

### Ziele

*   Erstellen und verpacken Sie Ihre Anwendung mit Docker.
*   Generieren Sie ein Authentifizierungstoken für die Anmeldung bei der Oracle Cloud Container Registry.
*   Übertragen Sie das Docker-Image der Helidon-Anwendung an Ihr Oracle Cloud Container Registry-Repository.

### Voraussetzungen

*   Die Helidon-Anwendung, die Sie in der vorherigen Übung erstellt haben
*   Docker
*   Oracle Cloud-Account

## Aufgabe 1: Docker-Image der Helidon-Anwendung erstellen

Wir erstellen ein Docker-Image, das Sie in die Oracle Cloud Container Registry hochladen, die zu Ihrem OCI-Account gehört. Dazu müssen Sie einen Bildnamen erstellen, der Ihre Registrierungskoordinaten widerspiegelt.

Sie benötigen die folgenden Informationen:

*   Regionsname
*   Mandanten-Namespace
*   Endpunkt für die Region
    
    > Kopieren Sie diese Informationen in eine Textdatei, damit Sie sie in der gesamten Übung referenzieren können.
    

1.  Suchen Sie den _Regionsnamen_.  
    Ihr _Regionsname_ befindet sich oben rechts in der Oracle Cloud-Konsole. In diesem Beispiel wird der Name _UK South (London)_ angezeigt. Ihre können anders sein.
    
    ![Container Registry](images/region-name.png)
    
2.  Suchen Sie den _Mandanten-Namespace_.  
    Öffnen Sie in der Konsole das Navigationsmenü, und klicken Sie auf **Entwicklerservices**. Klicken Sie unter **Container und Artefakte** auf **Container Registry**.
    
    ![Mandanten-Namespace](images/container-registry.png)
    
    > Der Mandanten-Namespace wird im Compartment aufgeführt. Kopieren und in einer Textdatei speichern. Diese Informationen verwenden Sie auch in der nächsten Übung. ![Mandanten-Namespace](images/name-space.png)
    
3.  Suchen Sie den _Endpunkt für Ihre Region_.  
    Weitere Informationen finden Sie in der Tabelle, die unter dieser URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) dokumentiert ist. Im dargestellten Beispiel lautet der Endpunkt für die Region _UK South (London)_ (als Regionsname), und der Endpunkt ist _lhr.ocir.io_. Suchen Sie den Endpunkt für Ihren eigenen _Regionsnamen_, und speichern Sie ihn in der Textdatei. Sie werden es auch für das nächste Labor benötigen.
    
    ![Endpunkte](images/end-points.png)
    
    > Jetzt haben Sie sowohl den Mandanten-Namespace als auch den Endpunkt für Ihre Region.
    
4.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihre Textdatei ein. Ersetzen Sie dann _`ENDPOINT_OF_YOUR_REGION`_ durch den Endpunkt Ihres Regionsnamens, _`NAMESPACE_OF_YOUR_TENANCY`_ durch den Namespace Ihres Mandanten und _`your_first_name`_ durch den Vornamen Ihres Mandanten.
    
    > Dadurch wird ein vollständiger Build im Docker-Container ausgeführt. Bei der ersten Ausführung wird es eine Weile dauern, da alle Maven-Abhängigkeiten heruntergeladen und in einer Docker-Ebene gecacht werden. Nachfolgende Builds werden viel schneller sein, solange Sie die Datei pom.xml nicht ändern. Wenn der Pom geändert wird, werden die Abhängigkeiten erneut heruntergeladen.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0 -f Dockerfile.native .</copy>
        
    
    Wenn der Befehl bereit ist, führen Sie ihn im Terminal im Codeeditor aus dem Verzeichnis _`~/helidon-project/myproject/myproject`_ aus. Der Build führt am Ende zu folgendem Ergebnis:
    
        $ docker build -t lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 -f Dockerfile.native .
        
        [1/7] Initializing...                                                                                   (15.7s @ 0.14GB)
        Version info: 'GraalVM 22.3.0 Java 17 CE'
        Java version info: '17.0.5+8-jvmci-22.3-b08'
        C compiler: gcc (redhat, x86_64, 11.3.1)
        Garbage collector: Serial GC
        2 user-specific feature(s)
        - io.helidon.integrations.graal.mp.nativeimage.extension.WeldFeature
        - io.helidon.integrations.graal.nativeimage.extension.HelidonReflectionFeature
        2023.04.06 05:41:01 INFO io.helidon.common.LogConfig Thread[main,5,main]: Logging at initialization configured using classpath: /logging.properties
        [2/7] Performing analysis...  [**********]                                                             (202.8s @ 1.92GB)
        18,812 (92.77%) of 20,278 classes reachable
        27,564 (63.52%) of 43,392 fields reachable
        87,900 (62.22%) of 141,268 methods reachable
        1,068 classes,   565 fields, and 6,864 methods registered for reflection
            65 classes,    70 fields, and    58 methods registered for JNI access
            6 native libraries: dl, m, pthread, rt, stdc++, z
        [3/7] Building universe...                                                                              (27.6s @ 3.15GB)
        [4/7] Parsing methods...      [*****]                                                                   (22.5s @ 3.00GB)
        [5/7] Inlining methods...     [***]                                                                     (11.9s @ 1.84GB)
        [6/7] Compiling methods...    [************]                                                           (156.5s @ 3.05GB)
        [7/7] Creating image...                                                                                 (15.6s @ 2.44GB)
        35.03MB (45.80%) for code area:    57,947 compilation units
        39.02MB (51.01%) for image heap:  477,987 objects and 128 resources
        2.44MB ( 3.19%) for other data
        76.49MB in total
        ------------------------------------------------------------------------------------------------------------------------
        Top 10 packages in code area:                               Top 10 object types in image heap:
        1.63MB sun.security.ssl                                     7.71MB byte[] for code metadata
        1.20MB com.sun.media.sound                                  4.60MB java.lang.Class
        1.17MB java.util                                            3.93MB java.lang.String
        822.87KB java.lang.invoke                                     3.41MB byte[] for java.lang.String
        717.54KB com.sun.crypto.provider                              3.22MB byte[] for general heap data
        517.57KB io.helidon.config                                    1.58MB com.oracle.svm.core.hub.DynamicHubCompanion
        510.02KB java.util.concurrent                                 1.13MB byte[] for reflection metadata
        481.49KB jdk.proxy4                                           1.03MB byte[] for embedded resources
        474.98KB java.lang                                          915.61KB java.util.HashMap$Node
        468.42KB com.sun.org.apache.xerces.internal.impl            781.21KB java.lang.String[]
        26.70MB for 671 more packages                                9.83MB for 4584 more object types
        ------------------------------------------------------------------------------------------------------------------------
                                31.0s (6.6% of total time) in 59 GCs | Peak RSS: 4.80GB | CPU load: 1.60
        ------------------------------------------------------------------------------------------------------------------------
        Produced artifacts:
        /helidon/target/myproject (executable)
        /helidon/target/myproject.build_artifacts.txt (txt)
        ========================================================================================================================
        Finished generating 'myproject' in 7m 48s.
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  08:04 min
        [INFO] Finished at: 2023-04-06T05:48:33Z
        [INFO] ------------------------------------------------------------------------
        Removing intermediate container e400c5c6897b
        ---> 20099e4619d6
        Step 10/15 : RUN echo "done!"
        ---> Running in a8eddd448e48
        done!
        Removing intermediate container a8eddd448e48
        ---> ebfd3064dc68
        Step 11/15 : FROM scratch
        ---> 
        Step 12/15 : WORKDIR /helidon
        ---> Running in 46be56a98462
        Removing intermediate container 46be56a98462
        ---> eaf15b746a1c
        Step 13/15 : COPY --from=build /helidon/target/myproject .
        ---> a69ac5933048
        Step 14/15 : ENTRYPOINT ["./myproject"]
        ---> Running in 71633a601e7f
        Removing intermediate container 71633a601e7f
        ---> cd9f22bfa4b3
        Step 15/15 : EXPOSE 8080
        ---> Running in 4b9763eb49fa
        Removing intermediate container 4b9763eb49fa
        ---> aa8b6e7b04c0
        Successfully built aa8b6e7b04c0
        Successfully tagged lhr.ocir.io/lrv4zdykjqrj/myproject-ankit:1.0
        
5.  Dadurch wird das Docker-Image erstellt, das Sie in Ihrem lokalen Repository einchecken können.
    
        $ docker images
        REPOSITORY                     TAG IMAGE ID        CREATED           SIZE
        lhr.ocir.io/tn/myproject-ankit 1.0 aa8b6e7b04c0 About a minute ago   80.2MB
        
    
    Kopieren Sie den ersetzten vollständigen Bildnamen `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0` in Ihren Texteditor, da Sie ihn später benötigen.
    
6.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um das Docking-Image in Cloud Shell des Codeeditors auszuführen.
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    ![Docking-Run](images/docker-run.png)
    
7.  Öffnen Sie einen neuen Terminal/eine neue Konsole, und führen Sie die folgenden Befehle aus, um die Anwendung zu prüfen.
    
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
        

## Aufgabe 2: Authentifizierungstoken für die Anmeldung bei Oracle Cloud Container Registry generieren

In diesem Schritt generieren wir ein _Authentifizierungstoken_, das wir zur Anmeldung bei der Oracle Cloud Container Registry verwenden.

1.  Wählen Sie in der oberen rechten Ecke das Benutzersymbol und dann _Mein Profil_ aus.
    
    ![Mein Profil](images/my-profile.png " ")
    
2.  Scrollen Sie nach unten, und wählen Sie _Authentifizierungstoken_ aus.
    
    ![Authentifizierungstoken](images/auth-token.png " ")
    
3.  Klicken Sie auf _Token generieren_.
    
    ![Token generieren](images/generate-token.png " ")
    
4.  Kopieren Sie _`myproject-your_first_name`_, fügen Sie es in das Feld _Beschreibung_ ein, und klicken Sie auf _Token generieren_.
    
    ![Token erstellen](images/token-create.png " ")
    
5.  Wählen Sie unter "Generiertes Token" die Option _Kopieren_ aus, und fügen Sie es in den Texteditor ein. Wir können es später nicht kopieren. Klicken Sie dann auf _Schließen_.
    
    ![Token kopieren](images/copy-token.png " ")
    

## Aufgabe 3: Docker-Image der Helidon-Anwendung (myproject) in das Container Registry Repository übertragen

1.  In Aufgabe 1 dieser Übung haben Sie eine URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) geöffnet, den Endpunkt für Ihren Regionsnamen bestimmt und in eine Textdatei kopiert. In unserem Beispiel lautet der Regionsname UK South (London). Sie benötigen diese Informationen für diese Aufgabe. ![Endpunkt](images/end-points.png)
    
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in die Textdatei ein. Ersetzen Sie dann `ENDPOINT_OF_REGION_NAME` durch den Endpunkt Ihrer Region.
    
    > In unserem Beispiel lautet der Regionsname _UK South (London)_, und der Endpunkt lautet _lhr.ocir.io_. Sie benötigen Ihre spezifischen Informationen für diese Aufgabe.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  Im vorherigen Schritt haben Sie auch den Mandanten-Namespace bestimmt. Geben Sie den Benutzernamen wie folgt ein: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Ersetzen Sie `NAMESPACE_OF_YOUR_TENANCY` durch den Namespace Ihres Mandanten
    *   Ersetzen Sie `YOUR_ORACLE_CLOUD_USERNAME` durch Ihren Oracle Cloud-Accountbenutzernamen, kopieren Sie den ersetzten Benutzernamen aus Ihrer Textdatei, und fügen Sie ihn in die _Cloud Shell_ ein.
    *   Kopieren Sie für das Kennwort das Authentifizierungstoken aus Ihrer Textdatei (oder wo auch immer Sie es gespeichert haben).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Navigieren Sie zurück zur Container Registry. Öffnen Sie in der Konsole das Navigationsmenü, und klicken Sie auf **Entwicklerservices**. Klicken Sie unter **Container und Artefakte** auf **Container Registry**. ![Container Registry](images/container-registry.png)
    
5.  Wählen Sie das Compartment, und klicken Sie auf **Repository erstellen**. ![Repository-Erstellen](images/repository-create.png)
    
6.  Wählen Sie das Compartment aus, und geben Sie _`myproject-your_first_name`_ als Repository-Namen ein. Wählen Sie dann "Zugriff" als **Öffentlich** aus, und klicken Sie auf **Repository erstellen**.
    
    ![Repository-Beschreibung](images/describe-repository.png)
    
7.  Nachdem das Repository _`myproject-your_first_name`_ erstellt wurde, können Sie es in der Repository-Liste und den zugehörigen Einstellungen prüfen.
    
    ![Namespace prüfen](images/verify-namespace.png)
    
8.  Um das Docker-Image in das Repository in der Oracle Cloud Container Registry zu übertragen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die Textdatei ein. Ersetzen Sie dann `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/myproject-your\_first\_name:1.0 durch den vollständigen Docker-Imagenamen, den Sie zuvor gespeichert haben.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    Das Ergebnis sollte wie folgt aussehen:
    
        $ docker push lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 The push refers to repository [lhr.ocir.io/tenancynamespace/myproject-ankit]
        0bf9e88b7284: Pushed 
        0acc08535a89: Pushed 
        1.0: digest: sha256:3e8cc0ff23d256499dff8d907150b639925687aed0c41008cbe1794bc02433e2 size: 735
        $
        
9.  Nachdem der Befehl _docker push_ erfolgreich ausgeführt wurde, blenden Sie das Repository _`myproject-your_first_name`_ ein, und Sie werden feststellen, dass ein neues Image in dieses Repository hochgeladen wurde.
    
    ![Bild hochgeladen](images/verify-push.png)
    

## Danksagungen

*   **Autor** - Dmitry Aleksandrov
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, April 2023
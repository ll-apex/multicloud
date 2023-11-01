# Helidon-Anwendungsimage per Push an Oracle Cloud Container Registry übergeben

## Einführung

In dieser Übung erstellen Sie ein Docker-Image mit Ihrer Helidon-Anwendung und pushen dieses Image in ein Repository in der Oracle Cloud Container Registry.

Geschätzte Zeit: 10 Minuten

### Ziele

*   Erstellen und verpacken Sie Ihre Anwendung mit Docker.
*   Generieren Sie ein Authentifizierungstoken für die Anmeldung bei der Oracle Cloud Container Registry.
*   Übertragen Sie das Docker-Image der Helidon-Anwendung an Ihr Oracle Cloud Container Registry-Repository.

### Voraussetzungen

*   Die Helidon-Anwendung, die Sie in der vorherigen Übung erstellt haben
*   Docker
*   Oracle Cloud-Account

## Aufgabe 1: Docker-Image der Helidon-Anwendung erstellen

Wir bereiten zunächst das Docker-Image vor, das Sie für die Bereitstellung auf Verrazzano verwenden.

Wir erstellen ein Docker-Image, das Sie in die Oracle Cloud Container Registry hochladen, die zu Ihrem OCI-Account gehört. Dazu müssen Sie einen Bildnamen erstellen, der Ihre Registrierungskoordinaten widerspiegelt.

Sie benötigen die folgenden Informationen:

*   Mandanten-Namespace
*   Endpunkt für die Region

1.  Um den Namespace des Mandanten zu suchen, klicken Sie wie gezeigt auf das Symbol _Benutzer_ -> _Mandant_. In den **Object Storage-Einstellungen** finden Sie den Namespace. Kopieren und speichern Sie sie in Ihrer Textdatei, da wir sie später auch verwenden.
    
    ![Mandantennamensraum kopieren](images/copy-tenancynamespace.png " ")
    
2.  Suchen Sie den _Endpunkt für Ihre Region_. Weitere Informationen finden Sie in der Tabelle, die unter dieser URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) dokumentiert ist. Im dargestellten Beispiel lautet der Endpunkt für die Region _UK South (London)_ (als Regionsname), und der Endpunkt ist _lhr.ocir.io_. Suchen Sie den Endpunkt für Ihren eigenen _Regionsnamen_, und speichern Sie ihn in der Textdatei. Sie werden es auch für das nächste Labor benötigen.
    
    ![Endpunkte](images/end-point.png " ")
    
    > Jetzt haben Sie sowohl den Mandanten-Namespace als auch den Endpunkt für Ihre Region.
    
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihren Texteditor ein. Ersetzen Sie dann _`ENDPOINT_OF_YOUR_REGION`_ durch den Endpunkt Ihres Regionsnamens, _`NAMESPACE_OF_YOUR_TENANCY`_ durch den Namespace Ihres Mandanten und _`your_first_name`_ durch den Vornamen Ihres Mandanten.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0 .</copy>
        
    
    Wenn der Befehl bereit ist, führen Sie ihn in der Cloud Shell aus dem Verzeichnis _`~/quickstart-mp/`_ aus. Der Build führt zu folgendem Ergebnis:
    
        $ cd ~/quickstart-mp/
        $ docker build lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0 .
        > docker pull lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        [+] Building 107.5s (19/19) FINISHED                                                                                                            
        => [internal] load build definition from Dockerfile                                                                                       0.1s
        => => transferring dockerfile: 785B                                                                                                       0.1s
        => [internal] load .dockerignore                                                                                                          0.1s
        => => transferring context: 48B                                                                                                           0.0s
        => [internal] load metadata for docker.io/library/openjdk:11-jre-slim                                                                     3.7s
        => [internal] load metadata for docker.io/library/maven:3.6-jdk-11                                                                        2.8s
        => [auth] library/openjdk:pull token for registry-1.docker.io                                                                             0.0s
        => [auth] library/maven:pull token for registry-1.docker.io                                                                               0.0s
        => [stage-1 1/4] FROM docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527      33.3s
        => => resolve docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527               0.0s
        => => sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527 549B / 549B                                                 0.0s
        => => sha256:f3cdb8fd164057f4ef3e60674fca986f3cd7b3081d55875c7ce75b7a214fca6d 1.16kB / 1.16kB                                             0.0s
        => => sha256:9c9e40a31d4fa290f933d76f3b0a4183ba02a7298a309f6bfa892d618e7196ef 7.56kB / 7.56kB                                             0.0s
        => => sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717 31.36MB / 31.36MB                                          18.6s
        => => sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251 1.58MB / 1.58MB                                             1.6s
        => => sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c 211B / 211B                                                 0.7s
        => => sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358 47.13MB / 47.13MB                                          24.4s
        => => extracting sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717                                                  7.7s
        => => extracting sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251                                                  0.3s
        => => extracting sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c                                                  0.0s
        => => extracting sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358                                                  5.7s
        => [build 1/7] FROM docker.io/library/maven:3.6-jdk-11@sha256:1d29ccf46ef2a5e64f7de3d79a63f9bcffb4dc56be0ae3daed5ca5542b38aa2d            0.0s
        => [internal] load build context                                                                                                          0.1s
        => => transferring context: 13.99kB                                                                                                       0.1s
        => CACHED [build 2/7] WORKDIR /helidon                                                                                                    0.0s
        => [build 3/7] ADD pom.xml .                                                                                                              0.1s
        => [build 4/7] RUN mvn package -Dmaven.test.skip -Declipselink.weave.skip                                                                91.7s
        => [stage-1 2/4] WORKDIR /helidon                                                                                                         0.8s
        => [build 5/7] ADD src src                                                                                                                0.1s
        => [build 6/7] RUN mvn package -DskipTests                                                                                               10.8s
        => [build 7/7] RUN echo "done!"                                                                                                           0.5s
        => [stage-1 3/4] COPY --from=build /helidon/target/quickstart-mp-ankit.jar ./                                                                   0.1s
        => [stage-1 4/4] COPY --from=build /helidon/target/libs ./libs                                                                            0.1s
        => exporting to image                                                                                                                     0.1s
        => => exporting layers                                                                                                                    0.1s
        => => writing image sha256:587a079ad854fc79e768acda11fc05dd87d37013261249e778e80749798c2837                                               0.0s
        => => naming to lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0                                                                           0.0s
        
4.  Dadurch wird das Docker-Image erstellt, das Sie in Ihrem lokalen Repository einchecken können.
    
        $ docker images
        
        REPOSITORY                                                                           TAG                               IMAGE ID       CREATED         SIZE
        lhr.ocir.io/tenancynamespace/quickstart-mp-ankit                                               1.0                               587a079ad854   5 minutes ago   243MB
        
    
    Kopieren Sie den ersetzten vollständigen Bildnamen `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-ankit:1.0` in Ihre Textdatei, da Sie ihn später benötigen.
    

## Aufgabe 2: Authentifizierungstoken für die Anmeldung bei Oracle Cloud Container Registry generieren

In diesem Schritt generieren wir ein _Authentifizierungstoken_, das wir zur Anmeldung bei der Oracle Cloud Container Registry verwenden.

1.  Wählen Sie in der oberen rechten Ecke das Benutzersymbol und dann _Mein Profil_ aus.
    
    ![Mein Profil](images/my-profile.png)
    
2.  Scrollen Sie nach unten, und wählen Sie _Authentifizierungstoken_ aus.
    
    ![Authentifizierungstoken](images/auth-token.png)
    
3.  Klicken Sie auf _Token generieren_.
    
    ![Token generieren](images/generate-token.png)
    
4.  Kopieren Sie _`quickstart-mp-your_first_name`_, fügen Sie es in das Feld "Beschreibung" ein, und klicken Sie auf _Token generieren_.
    
    ![Token erstellen](images/create-token.png)
    
5.  Klicken Sie unter "Generiertes Token" auf _Kopieren_, und fügen Sie es in den Texteditor ein. Sie können es später nicht kopieren. Stellen Sie daher sicher, dass Sie eine Kopie dieses Tokens gespeichert haben. Klicken Sie anschließend auf _Schließen_.
    
    ![Token kopieren](images/copy-token.png)
    

## Aufgabe 3: Docker-Image der Helidon-Anwendung (quickstart-mp) per Push an das Container Registry Repository übergeben

1.  In Aufgabe 1 dieser Übung haben Sie eine URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) geöffnet, den Endpunkt für Ihren Regionsnamen bestimmt und in einen Texteditor kopiert. In unserem Beispiel lautet der Regionsname UK South (London). Sie benötigen diese Informationen für diese Aufgabe. ![Endpunkt](images/end-point.png)
    
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in den Texteditor ein. Ersetzen Sie dann `ENDPOINT_OF_REGION_NAME` durch den Endpunkt Ihrer Region.
    
    > In unserem Beispiel lautet der Regionsname _UK South (London)_, und der Endpunkt lautet _lhr.ocir.io_. Sie benötigen Ihre spezifischen Informationen für diese Aufgabe.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  Im vorherigen Schritt haben Sie auch den Mandanten-Namespace bestimmt. Geben Sie den Benutzernamen wie folgt ein: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Ersetzen Sie _`NAMESPACE_OF_YOUR_TENANCY`_ durch den Namespace Ihres Mandanten
    *   Ersetzen Sie _`YOUR_ORACLE_CLOUD_USERNAME`_ durch Ihren Benutzernamen für den Oracle Cloud-Account, kopieren Sie den ersetzten Benutzernamen aus Ihrem Texteditor, und fügen Sie ihn in die _Cloud Shell_ ein.
    *   Kopieren Sie das Authentifizierungstoken für das Kennwort aus Ihrem Texteditor (oder wo auch immer Sie es gespeichert haben).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Navigieren Sie zurück zur Container Registry. Öffnen Sie in der Konsole das Navigationsmenü, und klicken Sie auf **Entwicklerservices**. Klicken Sie unter **Container und Artefakte** auf **Container Registry**. ![Container Registry](images/container-registry.png)
    
5.  Wählen Sie das Compartment aus, und klicken Sie auf **Erstellen**. ![Repository-Erstellen](images/repository-create.png)
    
6.  Wählen Sie das Compartment aus, und geben Sie _`quickstart-mp-your_first_name`_ als Repository-Namen ein. Wählen Sie dann "Zugriff" als **Öffentlich** aus, und klicken Sie auf **Repository erstellen**.
    
    ![Repository-Beschreibung](images/describe-repository.png)
    
7.  Nachdem das Repository _`quickstart-mp-your_first_name`_ erstellt wurde, können Sie es in der Repository-Liste und den zugehörigen Einstellungen prüfen.
    
    ![Namespace prüfen](images/verify-namespace.png)
    
8.  Um das Docker-Image in das Repository in der Oracle Cloud Container Registry zu übertragen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihren Texteditor ein. Ersetzen Sie dann `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`quickstar-mp-your_first_name`:1.0 durch den vollständigen Docker-Imagenamen, den Sie zuvor gespeichert haben.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0</copy>
        
    
    Das Ergebnis sollte wie folgt aussehen:
    
        $ docker push lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        The push refers to a repository [lhr.ocir.io/tenancynamespace/quickstart-mp-ankit]
        0795b8384c47: Pushed
        131452972f9d: Pushed
        93c53f2e9519: Pushed
        3b78b65a4be9: Pushed
        e1434e7d0308: Pushed
        17679d5f39bd: Pushed
        300b011056d9: Pushed
        1.0: digest: sha256:355fa56eab185535a58c5038186381b6d02fd8e0bcb534872107fc249f98256a size: 1786
        
9.  Nachdem der Befehl _docker push_ erfolgreich ausgeführt wurde, blenden Sie das Repository _`quickstart-mp-your_first_name`_ ein, und Sie werden feststellen, dass ein neues Image in dieses Repository hochgeladen wurde.
    
10.  Lassen Sie die _Cloud Shell_\- und Container Registry-Repository-Seite geöffnet. Sie benötigen sie für die nächsten Übungen.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
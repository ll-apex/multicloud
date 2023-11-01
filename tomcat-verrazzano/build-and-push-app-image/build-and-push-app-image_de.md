# Tomcat-Anwendungsimage in Oracle Cloud Container Registry übertragen

## Einführung

In dieser Übung erstellen Sie ein Docker-Image mit Ihrer Tomcat-Anwendung und übertragen dieses Image in ein Repository in der Oracle Cloud Container Registry.

Geschätzte Zeit: 10 Minuten

### Ziele

*   Erstellen und verpacken Sie Ihre Anwendung mit Docker.
*   Generieren Sie ein Authentifizierungstoken für die Anmeldung bei der Oracle Cloud Container Registry.
*   Übertragen Sie das Docker-Image der Tomcat-Anwendung per Push an das Oracle Cloud Container Registry-Repository.

### Voraussetzungen

*   Docker
*   Oracle Cloud-Account

## Aufgabe 1: Quellcode der Anwendung herunterladen

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um den Quellcode für diesen Workshop herunterzuladen.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/5DhoxZ22MseWaoSjG3EoBRf6DF5JbQ1QyR7tHa2qem-7IHb7nNEymF1ZSm2eA1ix/n/lrv4zdykjqrj/b/ankit-bucket/o/tomcat-examples.zip >~/tomcat-examples.zip</copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um den Quellcode zu entpacken und das aktuelle Verzeichnis in den Anwendungsordner zu ändern.
    
        <copy>unzip ~/tomcat-examples.zip
        cd ~/tomcat-examples/</copy>
        

## Aufgabe 2: Docker-Image der Tomcat-Anwendung erstellen

Wir bereiten zunächst das Docker-Image vor, das Sie für die Bereitstellung auf Verrazzano verwenden.

Wir erstellen ein Docker-Image, das Sie in die Oracle Cloud Container Registry hochladen, die zu Ihrem OCI-Account gehört. Dazu müssen Sie einen Bildnamen erstellen, der Ihre Registrierungskoordinaten widerspiegelt.

Sie benötigen die folgenden Informationen:

*   Mandanten-Namespace
    
*   Endpunkt für die Region
    
    > Kopieren Sie diese Informationen in eine Textdatei, damit Sie sie in der gesamten Übung referenzieren können.
    

1.  Um den Namespace des Mandanten zu suchen, klicken Sie wie gezeigt auf das Symbol _Benutzer_ -> _Mandant_. In den **Object Storage-Einstellungen** finden Sie den Namespace. Kopieren und speichern Sie sie in Ihrer Textdatei, da wir sie später auch verwenden.
    
    ![Mandantennamensraum kopieren](images/copy-tenancynamespace.png " ")
    
2.  Öffnen Sie eine URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab), und bestimmen Sie den Endpunkt für Ihren Regionsnamen, und kopieren Sie ihn in eine Textdatei. In unserem Beispiel lautet der Regionsname UK South (London). Sie benötigen diese Informationen für diese Aufgabe. ![Endpunkt](images/end-point.png)
    
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihren Texteditor ein. Ersetzen Sie dann _`ENDPOINT_OF_YOUR_REGION`_ durch den Endpunkt Ihres Regionsnamens, _`NAMESPACE_OF_YOUR_TENANCY`_ durch den Namespace Ihres Mandanten und _`your_first_name`_ durch den Vornamen Ihres Mandanten.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-your_first_name:v1 .</copy>
        
    
    Wenn der Befehl bereit ist, führen Sie ihn in der Cloud Shell aus dem Verzeichnis `~/tomcat-examples/` aus. Der Build führt zu folgendem Ergebnis:
    
        $ cd ~/tomcat-examples/
        $ $ docker build -t lhr.ocir.io/tenancy-namespace/tomcat-example-ankit:v1 .
        Sending build context to Docker daemon  1.097MB
        Step 1/10 : FROM tomcat:8.0-alpine
        Trying to pull repository docker.io/library/tomcat ... 
        8.0-alpine: Pulling from docker.io/library/tomcat
        4fe2ade4980c: Pull complete 
        6fc58a8d4ae4: Pull complete 
        7d9bd64c803b: Pull complete 
        a22aedc5ac11: Pull complete 
        5bde63ae3587: Pull complete 
        69cb0c9b940a: Pull complete 
        Digest: sha256:d02a16c0147fcae13d812fa670a4b3c9944f5328b10a5a463ad697d2aa5bb063
        Status: Downloaded newer image for tomcat:8.0-alpine
        ---> 624fb61775c3
        Step 2/10 : LABEL maintainer="ankit.x.pandey@oracle.com"
        ---> Running in 20cc23726499
        Removing intermediate container 20cc23726499
        ---> 50245c696fb6
        Step 3/10 : ADD sample-webapp.war /usr/local/tomcat/webapps/
        ---> 727c55f91bb5
        Step 4/10 : RUN mkdir /data
        ---> Running in f3129a859e11
        Removing intermediate container f3129a859e11
        ---> 9ce0f5674f51
        Step 5/10 : ADD jmx_prometheus_javaagent-0.17.0.jar /data/jmx_prometheus_javaagent-0.17.0.jar
        ---> f03cc9ee1bee
        Step 6/10 : ADD prometheus-jmx-config.yaml /data/prometheus-jmx-config.yaml
        ---> 50c51ae6a148
        Step 7/10 : ENV JAVA_OPTS="-javaagent:/data/jmx_prometheus_javaagent-0.17.0.jar=8088:/data/prometheus-jmx-config.yaml"
        ---> Running in 5e9effd5d494
        Removing intermediate container 5e9effd5d494
        ---> 85ca06fcd965
        Step 8/10 : EXPOSE 8088
        ---> Running in 795325f82526
        Removing intermediate container 795325f82526
        ---> 19dfc6fd903c
        Step 9/10 : EXPOSE 8080
        ---> Running in 43be96f20275
        Removing intermediate container 43be96f20275
        ---> 7d9bcaa7a271
        Step 10/10 : CMD ["catalina.sh", "run"]
        ---> Running in 3e25cd78ab88
        Removing intermediate container 3e25cd78ab88
        ---> 516065fe1bf5
        Successfully built 516065fe1bf5
        Successfully tagged lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        $
        
4.  Dadurch wird das Docker-Image erstellt, das Sie in Ihrem lokalen Repository einchecken können.
    
        $ docker images
        REPOSITORY                           TAG IMAGE ID      CREATED       SIZE
        lhr.ocir.io/name/tomcat-example-ankit v1 516065fe1bf5 2 minutes ago  147MB
        tomcat                       8.0-alpine  624fb61775c3 4 years ago    147MB
        
    
    Kopieren Sie den ersetzten vollständigen Bildnamen `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1` in Ihren Texteditor, da Sie ihn später benötigen.
    

## Aufgabe 3: Authentifizierungstoken für die Anmeldung bei Oracle Cloud Container Registry generieren

In diesem Schritt generieren wir ein _Authentifizierungstoken_, das wir zur Anmeldung bei der Oracle Cloud Container Registry verwenden.

1.  Wählen Sie in der oberen rechten Ecke das Benutzersymbol und dann _Mein Profil_ aus.
    
    ![Mein Profil](images/my-profile.png " ")
    
2.  Scrollen Sie nach unten, und wählen Sie _Authentifizierungstoken_ aus.
    
    ![Authentifizierungstoken](images/auth-token.png " ")
    
3.  Klicken Sie auf _Token generieren_.
    
    ![Token generieren](images/generate-token.png " ")
    
4.  Kopieren Sie _`tomcat-example-your_first_name`_, fügen Sie es in das Feld _Beschreibung_ ein, und klicken Sie auf _Token generieren_.
    
    ![Token erstellen](images/token-create.png " ")
    
5.  Wählen Sie unter "Generiertes Token" die Option _Kopieren_ aus, und fügen Sie es in den Texteditor ein. Wir können es später nicht kopieren. Klicken Sie dann auf _Schließen_.
    
    ![Token kopieren](images/copy-token.png " ")
    

## Aufgabe 4: Docker-Image der tomcat-Anwendung in das Container Registry Repository übertragen

1.  In Aufgabe 2 dieser Übung haben Sie eine URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) geöffnet, den Endpunkt für Ihren Regionsnamen bestimmt und in eine Textdatei kopiert. In unserem Beispiel lautet der Regionsname UK South (London). Sie benötigen diese Informationen für diese Aufgabe. ![Endpunkt](images/end-point.png)
    
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in den Texteditor ein. Ersetzen Sie dann `ENDPOINT_OF_REGION_NAME` durch den Endpunkt Ihrer Region.
    
    > In unserem Beispiel lautet der Regionsname _UK South (London)_, und der Endpunkt lautet _lhr.ocir.io_. Sie benötigen Ihre spezifischen Informationen für diese Aufgabe.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  Im vorherigen Schritt haben Sie auch den Mandanten-Namespace bestimmt. Geben Sie den Benutzernamen wie folgt ein: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Ersetzen Sie `NAMESPACE_OF_YOUR_TENANCY` durch den Namespace Ihres Mandanten
    *   Ersetzen Sie `YOUR_ORACLE_CLOUD_USERNAME` durch den Benutzernamen Ihres Oracle Cloud-Accounts, kopieren Sie den ersetzten Benutzernamen aus Ihrem Texteditor, und fügen Sie ihn in die _Cloud Shell_ ein.
    *   Kopieren Sie das Authentifizierungstoken für das Kennwort aus Ihrem Texteditor (oder wo auch immer Sie es gespeichert haben).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Navigieren Sie zurück zur Container Registry. Öffnen Sie in der Konsole das Navigationsmenü, und klicken Sie auf **Entwicklerservices**. Klicken Sie unter **Container und Artefakte** auf **Container Registry**. ![Container Registry](images/container-registry.png)
    
5.  Wählen Sie das Compartment, und klicken Sie auf **Repository erstellen**. ![Repository-Erstellen](images/repository-create.png)
    
6.  Wählen Sie das Compartment aus, und geben Sie _`tomcat-example-your_first_name`_ als Repository-Namen ein. Wählen Sie dann "Zugriff" als **Öffentlich** aus, und klicken Sie auf **Erstellen**.
    
    ![Repository-Beschreibung](images/describe-repository.png)
    
7.  Um das Docker-Image in das Repository in der Oracle Cloud Container Registry zu übertragen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihren Texteditor ein. Ersetzen Sie dann `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`tomcat-example-your_first_name`:1.0 durch den vollständigen Docker-Imagenamen, den Sie zuvor gespeichert haben.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1</copy>
        
    
    Das Ergebnis sollte wie folgt aussehen:
    
        $ docker push lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        The push refers to repository [lhr.ocir.io/tenancynamespace/tomcat-example-ankit]
        4b193f4c616d: Pushed 
        0469528628db: Pushed 
        cce8193c4190: Pushed 
        ca36c0db4673: Pushed 
        0136a6a85859: Pushed 
        98a0db77a14c: Pushed 
        9072514c7af0: Pushed 
        f6146a44a7d3: Pushed 
        0c3170905795: Pushed 
        df64d3292fd6: Pushed 
        v1: digest: sha256:65b562a7117870540f1807e0d796fe964e6428bda0ae290b8a6389bf637d1aba size: 2405
        $ 
        
8.  Nachdem der Befehl _docker push_ erfolgreich ausgeführt wurde, blenden Sie das Repository _`tomcat-example-ankit:v1`_ ein, und Sie werden feststellen, dass ein neues Image in dieses Repository hochgeladen wurde.
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
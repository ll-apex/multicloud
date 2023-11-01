# Springboot-Anwendungsimage an Oracle Cloud Container Registry übertragen

## Einführung

In dieser Übung erstellen Sie ein Docker-Image mit Ihrer Springboot-Anwendung und übertragen dieses Image in ein Repository in der Oracle Cloud Container Registry.

Geschätzte Zeit: 10 Minuten

### Ziele

*   Erstellen und verpacken Sie Ihre Anwendung mit Docker.
*   Generieren Sie ein Authentifizierungstoken für die Anmeldung bei der Oracle Cloud Container Registry.
*   Senden Sie das Docker-Image der Springboot-Anwendung per Push an das Oracle Cloud Container Registry-Repository.

### Voraussetzungen

*   Docker
*   Oracle Cloud-Account

## Aufgabe 1: Anwendungsquellcode und erforderliches JDK herunterladen

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um den Quellcode für diesen Workshop herunterzuladen.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/EeNNvCVLObhXjHIwu5M-J_zbSXJsbLop6wcsFFnDfneY3zqbAFfdKikZe-Q0GT3I/n/lrv4zdykjqrj/b/ankit-bucket/o/springboot-app.zip >~/springboot-app.zip</copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um den Quellcode zu entpacken und das aktuelle Verzeichnis in den Anwendungsordner zu ändern.
    
        <copy>unzip ~/springboot-app.zip
        cd ~/springboot-app/</copy>
        
3.  Wir erstellen ein Docker-Image für die Springboot-Anwendung. Diese Anwendung verwendet jedoch eine bestimmte Version von JDK, und wir möchten die Docker-Dateien, die das neue Image erstellen, nicht ändern. Daher laden wir das erforderliche JDK herunter. Um die erforderliche JDK-Version herunterzuladen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die Cloud Shell ein.
    
        <copy>wget https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz</copy>
        
4.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Springboot-Anwendung zu erstellen.
    
        <copy>mvn clean package</copy>
        

## Aufgabe 2: Docker-Image der Springboot-Anwendung erstellen

Wir bereiten zunächst das Docker-Image vor, das Sie für die Bereitstellung auf Verrazzano verwenden.

Wir erstellen ein Docker-Image, das Sie in die Oracle Cloud Container Registry hochladen, die zu Ihrem OCI-Account gehört. Dazu müssen Sie einen Bildnamen erstellen, der Ihre Registrierungskoordinaten widerspiegelt.

Sie benötigen die folgenden Informationen:

*   Mandanten-Namespace
    
*   Endpunkt für die Region
    
    > Kopieren Sie diese Informationen in eine Textdatei, damit Sie sie in der gesamten Übung referenzieren können.
    

1.  Um den Namespace des Mandanten zu suchen, klicken Sie wie gezeigt auf das Symbol _Benutzer_ -> _Mandant_. In den **Object Storage-Einstellungen** finden Sie den Namespace. Kopieren und speichern Sie sie in Ihrer Textdatei, da wir sie später auch verwenden.
    
    ![Mandantennamensraum kopieren](images/copy-tenancynamespace.png " ")
    
2.  Suchen Sie den _Endpunkt für Ihre Region_.  
    Weitere Informationen finden Sie in der Tabelle, die unter dieser URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) dokumentiert ist. Im dargestellten Beispiel lautet der Endpunkt für die Region _UK South (London)_ (als Regionsname), und der Endpunkt ist _lhr.ocir.io_. Suchen Sie den Endpunkt für Ihren eigenen _Regionsnamen_, und speichern Sie ihn in der Textdatei.
    
    ![Endpunkte](images/end-points.png)
    
    > Jetzt haben Sie sowohl den Mandanten-Namespace als auch den Endpunkt für Ihre Region.
    
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihre Textdatei ein. Ersetzen Sie dann _`ENDPOINT_OF_YOUR_REGION`_ durch den Endpunkt Ihres Regionsnamens, _`NAMESPACE_OF_YOUR_TENANCY`_ durch den Namespace Ihres Mandanten und _`your_first_name`_ durch den Vornamen Ihres Mandanten.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-your_first_name:v1 .</copy>
        
    
    Wenn der Befehl bereit ist, führen Sie ihn in der Cloud Shell aus dem Verzeichnis `~/springboot-app/` aus. Der Build führt zu folgendem Ergebnis:
    
        $ cd ~/springboot-app/
        $ docker build -t lhr.ocir.io/tenancy-namespace/springboot-ankit:v1 .
        Sending build context to Docker daemon  206.7MB
        Step 1/14 : FROM ghcr.io/oracle/oraclelinux:7-slim AS build_base
        Trying to pull repository ghcr.io/oracle/oraclelinux ... 
        7-slim: Pulling from ghcr.io/oracle/oraclelinux
        6cb086706000: Pull complete 
        Digest: sha256:4353fdc8664c386c0a443eb40b10a7662b4eb8d6eb5d6dcefe218e9783132c71
        Status: Downloaded newer image for ghcr.io/oracle/oraclelinux:7-slim
        ---> 1d56b1a0fd84
        Step 2/14 : RUN yum update -y && yum-config-manager --save --setopt=ol7_ociyum_config.skip_if_unavailable=true     && yum install -y tar unzip gzip     && yum clean all; rm -rf /var/cache/yum     && mkdir -p /license
        ---> Running in 92c013a8e84f
        Loaded plugins: ovl
        No packages marked for update
        Loaded plugins: ovl
        ===================================== main =====================================
        [main]
        alwaysprompt = True
        assumeno = False
        assumeyes = False
        autocheck_running_kernel = True
        autosavets = True
        bandwidth = 0
        bugtracker_url = https://linux.oracle.com
        cache = 0
        cachedir = /var/cache/yum/x86_64/7Server
        check_config_file_age = True
        clean_requirements_on_remove = False
        ----------------------------------------------------------------------
        Step 3/14 : ENV JAVA_HOME=/usr/java
        ---> Running in 96b99fba7e50
        Removing intermediate container 96b99fba7e50
        ---> 1cc6c7a63b89
        Step 4/14 : ENV PATH $JAVA_HOME/bin:$PATH
        ---> Running in 4a88eb052547
        Removing intermediate container 4a88eb052547
        ---> 48e7fa9b7b0c
        Step 5/14 : ARG JDK_BINARY="${JDK_BINARY:-openjdk-11_linux-x64_bin.tar.gz}"
        ---> Running in e922e8b35bfd
        Removing intermediate container e922e8b35bfd
        ---> 6888b690a4b0
        Step 6/14 : COPY ${JDK_BINARY} jdk.tar.gz
        ---> e9c5ffd0f2a5
        Step 7/14 : ENV JDK_DOWNLOAD_SHA256=3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e
        ---> Running in a7a89b057f25
        Removing intermediate container a7a89b057f25
        ---> 12ecfaa002bf
        Step 8/14 : RUN set -eux     echo "Checking JDK hash";     echo "${JDK_DOWNLOAD_SHA256} *jdk.tar.gz" | sha256sum --check -;     echo "Installing JDK";     mkdir -p "$JAVA_HOME";     tar xzf jdk.tar.gz --directory "${JAVA_HOME}" --strip-components=1;     rm -f jdk.tar.gz;
        ---> Running in a6a590adeee6
        + echo '3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e *jdk.tar.gz'
        + sha256sum --check -
        jdk.tar.gz: OK
        + echo 'Installing JDK'
        + mkdir -p /usr/java
        Installing JDK
        + tar xzf jdk.tar.gz --directory /usr/java --strip-components=1
        + rm -f jdk.tar.gz
        Removing intermediate container a6a590adeee6
        ---> 1f37a6cb044d
        Step 9/14 : COPY LICENSE.txt /license/
        ---> dc69f24f5be6
        Step 10/14 : COPY THIRD_PARTY_LICENSES.txt /license/
        ---> 5ef683dbda22
        Step 11/14 : ARG JAR_FILE=target/*.jar
        ---> Running in 3b80032f8310
        Removing intermediate container 3b80032f8310
        ---> 8eca16289bd7
        Step 12/14 : COPY ${JAR_FILE} app.jar
        ---> dcb7e3ed0871
        Step 13/14 : ENTRYPOINT ["java","-jar","/app.jar"]
        ---> Running in 2191623bf524
        Removing intermediate container 2191623bf524
        ---> 11e59e19cfb4
        Step 14/14 : USER 1000
        ---> Running in 16446779b92b
        Removing intermediate container 16446779b92b
        ---> a701fa912f2e
        Successfully built a701fa912f2e
        Successfully tagged lhr.ocir.io/tenancynamespace/springboot-ankit:v1
        $
        
4.  Dadurch wird das Docker-Image erstellt, das Sie in Ihrem lokalen Repository einchecken können.
    
        $ docker images
        REPOSITORY                            TAG     IMAGE ID    CREATED      SIZE
        lhr.ocir.io/namespace/springboot-ankit v1    a701fa912f2 3 minutes ago 664MB
        ghcr.io/oracle/oraclelinux            7-slim 1d56b1a0fd8 3 weeks ago   138MB
        $ 
        
    
    Kopieren Sie den ersetzten vollständigen Bildnamen `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1` in Ihre Textdatei, da Sie ihn später benötigen.
    

## Aufgabe 3: Authentifizierungstoken für die Anmeldung bei Oracle Cloud Container Registry generieren

In diesem Schritt generieren wir ein _Authentifizierungstoken_, das wir zur Anmeldung bei der Oracle Cloud Container Registry verwenden.

1.  Wählen Sie in der oberen rechten Ecke das Benutzersymbol und dann _Mein Profil_ aus.
    
    ![Mein Profil](images/my-profile.png " ")
    
2.  Scrollen Sie nach unten, und wählen Sie _Authentifizierungstoken_ aus.
    
    ![Authentifizierungstoken](images/auth-token.png " ")
    
3.  Klicken Sie auf _Token generieren_.
    
    ![Token generieren](images/generate-token.png " ")
    
4.  Kopieren Sie _`springboot-your_first_name`_, fügen Sie es in das Feld _Beschreibung_ ein, und klicken Sie auf _Token generieren_.
    
    ![Token erstellen](images/token-create.png " ")
    
5.  Wählen Sie unter "Generiertes Token" die Option _Kopieren_ aus, und fügen Sie es in die Textdatei ein. Wir können es später nicht kopieren. Klicken Sie dann auf _Schließen_.
    
    ![Token kopieren](images/copy-token.png " ")
    

## Aufgabe 4: Springboot-Anwendungs-Docker-Image per Push an das Container Registry Repository übergeben

1.  In Aufgabe 1 dieser Übung haben Sie eine URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) geöffnet, den Endpunkt für Ihren Regionsnamen bestimmt und in eine Textdatei kopiert. In unserem Beispiel lautet der Regionsname UK South (London). Sie benötigen diese Informationen für diese Aufgabe. ![Endpunkt](images/end-points.png)
    
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in den Texteditor ein. Ersetzen Sie dann `ENDPOINT_OF_REGION_NAME` durch den Endpunkt Ihrer Region.
    
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
    
5.  Wählen Sie das Compartment aus, und klicken Sie auf **Erstellen**. ![Repository-Erstellen](images/repository-create.png)
    
6.  Wählen Sie das Compartment aus, und geben Sie _`springboot-your_first_name`_ als Repository-Namen ein. Wählen Sie dann "Zugriff" als **Öffentlich** aus, und klicken Sie auf **Repository erstellen**.
    
    ![Repository-Beschreibung](images/describe-repository.png)
    
7.  Um das Docker-Image in das Repository in der Oracle Cloud Container Registry zu übertragen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die Textdatei ein. Ersetzen Sie dann _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/springboot-your\_firstname:1.0_ durch den vollständigen Docker-Imagenamen, den Sie zuvor gespeichert haben.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1</copy>
        
    
    Das Ergebnis sollte wie folgt aussehen:
    
        $ docker push lhr.ocir.io/namespace/springboot-ankit:v1
        The push refers to repository [lhr.ocir.io/namespace/springboot-ankit]
        31118271414e: Pushed 
        e6144652ec48: Pushed 
        a5ac4d4576aa: Pushed 
        2f93ab3a0c42: Pushed 
        3ed60ad88e51: Pushed 
        f47db30f116a: Pushed 
        f50ba2e0b2f9: Pushed 
        v1: digest: sha256:96aacff31cb255ea815213aba837f16f40d73b14d67449d4744ed811c7a864c8 size: 1795
        $ 
        
8.  Nachdem der Befehl _docker push_ erfolgreich ausgeführt wurde, blenden Sie das Repository _`springboot-ankit:v1`_ ein, und Sie werden feststellen, dass ein neues Image in dieses Repository hochgeladen wurde.
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
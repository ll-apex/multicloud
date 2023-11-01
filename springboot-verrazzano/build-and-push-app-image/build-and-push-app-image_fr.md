# Transmission de l'image d'application Springboot à Oracle Cloud Container Registry

## Présentation

Dans cet atelier, vous allez créer une image Docker avec votre application Springboot et la propager vers un référentiel dans Oracle Cloud Container Registry.

Temps estimé : 10 minutes

### Objectifs

*   Créez et packagez votre application à l'aide de Docker.
*   Générez un jeton d'authentification pour vous connecter à Oracle Cloud Container Registry.
*   Propager l'image Docker de l'application Springboot vers le référentiel Oracle Cloud Container Registry.

### Prérequis

*   Docker
*   Compte Oracle Cloud

## Tâche 1 : télécharger le code source de l'application et le JDK requis

1.  Copiez et collez la commande suivante pour télécharger le code source de cet atelier.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/EeNNvCVLObhXjHIwu5M-J_zbSXJsbLop6wcsFFnDfneY3zqbAFfdKikZe-Q0GT3I/n/lrv4zdykjqrj/b/ankit-bucket/o/springboot-app.zip >~/springboot-app.zip</copy>
        
2.  Copiez et collez la commande suivante pour décompresser le code source et changer le répertoire actuel en dossier d'application.
    
        <copy>unzip ~/springboot-app.zip
        cd ~/springboot-app/</copy>
        
3.  Nous allons créer une image Docker pour l'application springboot, mais cette application utilise une version spécifique de JDK et nous ne voulons pas modifier les fichiers Docker qui créent la nouvelle image. Nous téléchargeons donc le kit JDK requis. Pour télécharger la version de JDK requise, copiez la commande ci-dessous et collez-la dans Cloud Shell.
    
        <copy>wget https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz</copy>
        
4.  Copiez et collez la commande suivante pour créer l'application springboot.
    
        <copy>mvn clean package</copy>
        

## Tâche 2 : création de l'image Docker de l'application Springboot

Nous commencerons par préparer l'image Docker que vous utiliserez pour le déploiement sur Verrazzano.

Nous sommes en train de créer une image Docker que vous téléchargerez vers Oracle Cloud Container Registry appartenant à votre compte OCI. Pour ce faire, vous devez créer un nom d'image qui reflète les coordonnées de votre registre.

Vous devez disposer des informations suivantes :

*   Espace de noms de location
    
*   Adresse de la région
    
    > Copiez ces informations dans un fichier texte pour pouvoir y faire référence tout au long de l'exercice.
    

1.  Pour rechercher l'espace de noms de la location, cliquez sur l'icône _Utilisateur_ -> _Location_ comme indiqué. Dans les **paramètres Object Storage**, vous trouverez l'espace de noms. Copiez-le et enregistrez-le dans votre fichier texte, car nous l'utiliserons également plus tard.
    
    ![Copier l'espace ténancynamique](images/copy-tenancynamespace.png " ")
    
2.  Localisez l'_adresse de votre région_.  
    Reportez-vous au tableau documenté à cette URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). Dans l'exemple présenté, l'adresse de la région est _Sud du Royaume-Uni (Londres)_ (en tant que nom de région) et son adresse est _lhr.ocir.io_. Localisez l'adresse de votre propre _nom de région_ et enregistrez-la dans le fichier texte.
    
    ![Adresses](images/end-points.png)
    
    > Vous disposez désormais de l'espace de noms de location et de l'adresse de votre région.
    
3.  Copiez la commande suivante et collez-la dans votre fichier texte. Remplacez ensuite _`ENDPOINT_OF_YOUR_REGION`_ par l'adresse du nom de région, _`NAMESPACE_OF_YOUR_TENANCY`_ par l'espace de noms de votre location et _`your_first_name`_ par le prénom.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-your_first_name:v1 .</copy>
        
    
    Lorsque la commande est prête, exécutez-la dans Cloud Shell à partir du répertoire `~/springboot-app/`. Le paramétrage produira le résultat suivant :
    
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
        
4.  Cette opération crée l'image Docker, que vous pouvez réinsérer dans le référentiel local.
    
        $ docker images
        REPOSITORY                            TAG     IMAGE ID    CREATED      SIZE
        lhr.ocir.io/namespace/springboot-ankit v1    a701fa912f2 3 minutes ago 664MB
        ghcr.io/oracle/oraclelinux            7-slim 1d56b1a0fd8 3 weeks ago   138MB
        $ 
        
    
    Copiez dans votre fichier texte le nom d'image complet remplacé `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1` car vous en aurez besoin ultérieurement.
    

## Tâche 3 : génération d'un jeton d'authentification pour la connexion à Oracle Cloud Container Registry

Cette étape consiste à générer un _jeton d'authentification_ que nous utiliserons pour nous connecter à Oracle Cloud Container Registry.

1.  Sélectionnez l'icône Utilisateur dans l'angle supérieur droit, puis _Mon profil_.
    
    ![Mon profil](images/my-profile.png " ")
    
2.  Faites défiler la page vers le bas et sélectionnez _Jetons d'authentification_.
    
    ![Jetons d'authentification](images/auth-token.png " ")
    
3.  Cliquez sur _Générer un jeton_.
    
    ![Générer un jeton](images/generate-token.png " ")
    
4.  Copiez _`springboot-your_first_name`_ et collez-le dans la zone _Description_, puis cliquez sur _Générer un jeton_.
    
    ![Création de jeton](images/token-create.png " ")
    
5.  Sélectionnez _Copier_ sous Jeton généré et collez-le dans le fichier texte. Nous ne pourrons pas le copier plus tard. Cliquez ensuite sur _Fermer_.
    
    ![Copier le jeton](images/copy-token.png " ")
    

## Tâche 4 : transmission de l'image Docker de l'application Springboot vers le référentiel Container Registry

1.  Dans la tâche 1 de cet exercice, vous avez ouvert l'URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab), déterminé l'adresse du nom de région et copié dans un fichier texte. Dans notre exemple, le nom de la région est UK South (Londres). Vous aurez besoin de ces informations pour cette tâche. ![Adresse](images/end-points.png)
    
2.  Copiez la commande suivante, collez-la dans l'éditeur de texte, puis remplacez `ENDPOINT_OF_REGION_NAME` par l'adresse de votre région.
    
    > Dans notre exemple, le nom de région est _Sud du Royaume-Uni (Londres)_ et l'adresse est _lhr.ocir.io_. Vous aurez besoin de vos informations spécifiques pour cette tâche.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  A l'étape précédente, vous avez également déterminé l'espace de noms de location. Entrez le nom utilisateur comme suit : `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Remplacez `NAMESPACE_OF_YOUR_TENANCY` par l'espace de noms de votre location.
    *   Remplacez `YOUR_ORACLE_CLOUD_USERNAME` par le nom utilisateur de votre compte Oracle Cloud, puis copiez le nom utilisateur remplacé à partir du fichier texte et collez-le dans _Cloud Shell_.
    *   Pour Password, copiez et collez le jeton d'authentification à partir de votre fichier texte (ou de l'emplacement où vous l'avez enregistré).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Revenez au registre du conteneur. Dans la console, ouvrez le menu de navigation et cliquez sur **Services de développeur**. Sous **Conteneurs et artefacts**, cliquez sur **Registre du conteneur**. ![Registre du conteneur](images/container-registry.png)
    
5.  Sélectionnez le compartiment, puis cliquez sur **Créer**. ![Création de référentiel](images/repository-create.png)
    
6.  Sélectionnez le compartiment et entrez _`springboot-your_first_name`_ comme nom de référentiel, puis choisissez Accès en tant que **public** et cliquez sur **Créer un référentiel**.
    
    ![Description de référentiel](images/describe-repository.png)
    
7.  Pour propager l'image Docker vers le référentiel dans Oracle Cloud Container Registry, copiez et collez la commande suivante dans votre fichier texte, puis remplacez _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/springboot-your\_firstname :1.0_ par le nom complet de l'image Docker que vous avez enregistré précédemment.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1</copy>
        
    
    Le résultat doit ressembler à :
    
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
        
8.  Une fois la commande _docker push_ exécutée, développez le référentiel _`springboot-ankit:v1`_ et vous remarquerez qu'une nouvelle image a été téléchargée vers ce référentiel.
    

Vous pouvez maintenant **passer à l'exercice suivant**.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
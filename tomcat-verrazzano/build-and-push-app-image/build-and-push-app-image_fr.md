# Transmission de l'image d'application Tomcat à Oracle Cloud Container Registry

## Présentation

Dans cet atelier, vous allez créer une image Docker avec votre application Tomcat et la propager vers un référentiel dans Oracle Cloud Container Registry.

Temps estimé : 10 minutes

### Objectifs

*   Créez et packagez votre application à l'aide de Docker.
*   Générez un jeton d'authentification pour vous connecter à Oracle Cloud Container Registry.
*   Poussez l'image Docker de l'application Tomcat vers le référentiel Oracle Cloud Container Registry.

### Prérequis

*   Docker
*   Compte Oracle Cloud

## Tâche 1 : télécharger le code source de l'application

1.  Copiez et collez la commande suivante pour télécharger le code source de cet atelier.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/5DhoxZ22MseWaoSjG3EoBRf6DF5JbQ1QyR7tHa2qem-7IHb7nNEymF1ZSm2eA1ix/n/lrv4zdykjqrj/b/ankit-bucket/o/tomcat-examples.zip >~/tomcat-examples.zip</copy>
        
2.  Copiez et collez la commande suivante pour décompresser le code source et changer le répertoire actuel en dossier d'application.
    
        <copy>unzip ~/tomcat-examples.zip
        cd ~/tomcat-examples/</copy>
        

## Tâche 2 : création de l'image Docker de l'application Tomcat

Nous commencerons par préparer l'image Docker que vous utiliserez pour le déploiement sur Verrazzano.

Nous sommes en train de créer une image Docker que vous téléchargerez vers Oracle Cloud Container Registry appartenant à votre compte OCI. Pour ce faire, vous devez créer un nom d'image qui reflète les coordonnées de votre registre.

Vous devez disposer des informations suivantes :

*   Espace de noms de location
    
*   Adresse de la région
    
    > Copiez ces informations dans un fichier texte pour pouvoir y faire référence tout au long de l'exercice.
    

1.  Pour rechercher l'espace de noms de la location, cliquez sur l'icône _Utilisateur_ -> _Location_ comme indiqué. Dans les **paramètres Object Storage**, vous trouverez l'espace de noms. Copiez-le et enregistrez-le dans votre fichier texte, car nous l'utiliserons également plus tard.
    
    ![Copier l'espace ténancynamique](images/copy-tenancynamespace.png " ")
    
2.  Ouvrez l'URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab), déterminez l'adresse du nom de région et copiez-la dans un fichier texte. Dans notre exemple, le nom de la région est UK South (Londres). Vous aurez besoin de ces informations pour cette tâche. ![Adresse](images/end-point.png)
    
3.  Copiez la commande suivante et collez-la dans votre éditeur de texte. Remplacez ensuite _`ENDPOINT_OF_YOUR_REGION`_ par l'adresse du nom de région, _`NAMESPACE_OF_YOUR_TENANCY`_ par l'espace de noms de votre location et _`your_first_name`_ par le prénom.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-your_first_name:v1 .</copy>
        
    
    Lorsque la commande est prête, exécutez-la dans Cloud Shell à partir du répertoire `~/tomcat-examples/`. Le paramétrage produira le résultat suivant :
    
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
        
4.  Cette opération crée l'image Docker, que vous pouvez réinsérer dans le référentiel local.
    
        $ docker images
        REPOSITORY                           TAG IMAGE ID      CREATED       SIZE
        lhr.ocir.io/name/tomcat-example-ankit v1 516065fe1bf5 2 minutes ago  147MB
        tomcat                       8.0-alpine  624fb61775c3 4 years ago    147MB
        
    
    Copiez dans votre éditeur de texte le nom d'image complet remplacé `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1` car vous en aurez besoin ultérieurement.
    

## Tâche 3 : génération d'un jeton d'authentification pour la connexion à Oracle Cloud Container Registry

Cette étape consiste à générer un _jeton d'authentification_ que nous utiliserons pour nous connecter à Oracle Cloud Container Registry.

1.  Sélectionnez l'icône Utilisateur dans l'angle supérieur droit, puis _Mon profil_.
    
    ![Mon profil](images/my-profile.png " ")
    
2.  Faites défiler la page vers le bas et sélectionnez _Jetons d'authentification_.
    
    ![Jetons d'authentification](images/auth-token.png " ")
    
3.  Cliquez sur _Générer un jeton_.
    
    ![Générer un jeton](images/generate-token.png " ")
    
4.  Copiez _`tomcat-example-your_first_name`_ et collez-le dans la zone _Description_, puis cliquez sur _Générer un jeton_.
    
    ![Création de jeton](images/token-create.png " ")
    
5.  Sélectionnez _Copier_ sous Jeton généré et collez-le dans l'éditeur de texte. Nous ne pourrons pas le copier plus tard. Cliquez ensuite sur _Fermer_.
    
    ![Copier le jeton](images/copy-token.png " ")
    

## Tâche 4 : propager l'image Docker de l'application tomcat vers le référentiel Container Registry

1.  Dans la tâche 2 de cet exercice, vous avez ouvert l'URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab), déterminé l'adresse du nom de région et copié dans un fichier texte. Dans notre exemple, le nom de la région est UK South (Londres). Vous aurez besoin de ces informations pour cette tâche. ![Adresse](images/end-point.png)
    
2.  Copiez la commande suivante, collez-la dans l'éditeur de texte, puis remplacez `ENDPOINT_OF_REGION_NAME` par l'adresse de votre région.
    
    > Dans notre exemple, le nom de région est _Sud du Royaume-Uni (Londres)_ et l'adresse est _lhr.ocir.io_. Vous aurez besoin de vos informations spécifiques pour cette tâche.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  A l'étape précédente, vous avez également déterminé l'espace de noms de location. Entrez le nom utilisateur comme suit : `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Remplacez `NAMESPACE_OF_YOUR_TENANCY` par l'espace de noms de votre location.
    *   Remplacez `YOUR_ORACLE_CLOUD_USERNAME` par le nom utilisateur de votre compte Oracle Cloud, puis copiez le nom utilisateur remplacé à partir de l'éditeur de texte et collez-le dans _Cloud Shell_.
    *   Pour Password, copiez et collez le jeton d'authentification à partir de votre éditeur de texte (ou de l'emplacement où vous l'avez enregistré).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Revenez au registre du conteneur. Dans la console, ouvrez le menu de navigation et cliquez sur **Services de développeur**. Sous **Conteneurs et artefacts**, cliquez sur **Registre du conteneur**. ![Registre du conteneur](images/container-registry.png)
    
5.  Sélectionnez le compartiment, puis cliquez sur **Créer un référentiel**. ![Création de référentiel](images/repository-create.png)
    
6.  Sélectionnez le compartiment et entrez _`tomcat-example-your_first_name`_ comme nom de référentiel, puis choisissez Accès en tant que **public** et cliquez sur **Créer**.
    
    ![Description de référentiel](images/describe-repository.png)
    
7.  Pour propager l'image Docker vers le référentiel dans Oracle Cloud Container Registry, copiez et collez la commande suivante dans votre éditeur de texte, puis remplacez `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`tomcat-example-your_first_name` :1.0 par le nom complet de l'image Docker que vous avez enregistré précédemment.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1</copy>
        
    
    Le résultat doit ressembler à :
    
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
        
8.  Une fois la commande _docker push_ exécutée, développez le référentiel _`tomcat-example-ankit:v1`_ et vous remarquerez qu'une nouvelle image a été téléchargée vers ce référentiel.
    

Vous pouvez maintenant **passer à l'exercice suivant**.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
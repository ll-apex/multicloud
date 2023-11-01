# Création et propagation d'une image d'application Helidon vers Oracle Cloud Container Registry

## Présentation

Dans cet atelier, vous allez créer une image Docker _native_ avec votre application Helidon et la propager vers un référentiel dans Oracle Cloud Container Registry.

Temps estimé : 15 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Création et propagation d'une image d'application Helidon vers Oracle Cloud Container Registry](videohub:1_mh1brw5t)

### Objectifs

*   Créez et packagez votre application à l'aide de Docker.
*   Générez un jeton d'authentification pour vous connecter à Oracle Cloud Container Registry.
*   Transférez l'image Docker de l'application Helidon vers le référentiel Oracle Cloud Container Registry.

### Prérequis

*   L'application Helidon que vous avez créée dans l'exercice précédent
*   Docker
*   Compte Oracle Cloud

## Tâche 1 : création de l'image Docker de l'application Helidon

Nous sommes en train de créer une image Docker que vous téléchargerez vers Oracle Cloud Container Registry appartenant à votre compte OCI. Pour ce faire, vous devez créer un nom d'image qui reflète les coordonnées de votre registre.

Vous devez disposer des informations suivantes :

*   Nom de région
*   Espace de noms de location
*   Adresse de la région
    
    > Copiez ces informations dans un fichier texte pour pouvoir y faire référence tout au long de l'exercice.
    

1.  Localisez votre _nom de région_.  
    Votre _nom de région_ se trouve dans l'angle supérieur droit de la console Oracle Cloud. Dans cet exemple, il s'agit de _Sud du Royaume-Uni (Londres)_. Le vôtre peut être différent.
    
    ![Registre du conteneur](images/region-name.png)
    
2.  Localisez l'_espace de noms de location_.  
    Dans la console, ouvrez le menu de navigation et cliquez sur **Services de développeur**. Sous **Conteneurs et artefacts**, cliquez sur **Registre du conteneur**.
    
    ![Espace de noms de location](images/container-registry.png)
    
    > L'espace de noms de location est répertorié dans le compartiment. Copiez-le et enregistrez-le dans un fichier texte. Vous utiliserez également ces informations dans l'exercice suivant. ![Espace de noms de location](images/name-space.png)
    
3.  Localisez l'_adresse de votre région_.  
    Reportez-vous au tableau documenté à cette URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). Dans l'exemple présenté, l'adresse de la région est _Sud du Royaume-Uni (Londres)_ (en tant que nom de région) et son adresse est _lhr.ocir.io_. Localisez l'adresse de votre propre _nom de région_ et enregistrez-la dans le fichier texte. Vous en aurez également besoin pour le prochain laboratoire.
    
    ![Adresses](images/end-points.png)
    
    > Vous disposez désormais de l'espace de noms de location et de l'adresse de votre région.
    
4.  Copiez la commande suivante et collez-la dans votre fichier texte. Remplacez ensuite _`ENDPOINT_OF_YOUR_REGION`_ par l'adresse du nom de région, _`NAMESPACE_OF_YOUR_TENANCY`_ par l'espace de noms de votre location et _`your_first_name`_ par le prénom.
    
    > Il s'agit d'un build complet dans le conteneur Docker. La première fois que vous l'exécutez, cela prendra un certain temps car il télécharge toutes les dépendances Maven et les met en cache dans une couche Docker. Les builds suivants seront beaucoup plus rapides tant que vous ne modifiez pas le fichier pom.xml. Si le pom est modifié, les dépendances seront à nouveau téléchargées.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0 -f Dockerfile.native .</copy>
        
    
    Lorsque la commande est prête, exécutez-la dans le terminal dans l'éditeur de code à partir du répertoire _`~/helidon-project/myproject/myproject`_. Le paramétrage produira le résultat suivant à la fin :
    
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
        
5.  Cette opération crée l'image Docker, que vous pouvez réinsérer dans le référentiel local.
    
        $ docker images
        REPOSITORY                     TAG IMAGE ID        CREATED           SIZE
        lhr.ocir.io/tn/myproject-ankit 1.0 aa8b6e7b04c0 About a minute ago   80.2MB
        
    
    Copiez dans votre éditeur de texte le nom d'image complet remplacé `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0` car vous en aurez besoin ultérieurement.
    
6.  Copiez et collez la commande suivante dans le terminal pour exécuter l'image docker dans Cloud Shell de l'éditeur de code.
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    ![Exécution Docker](images/docker-run.png)
    
7.  Ouvrez un nouveau terminal/une nouvelle console et exécutez les commandes suivantes pour vérifier l'application :
    
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
        

## Tâche 2 : génération d'un jeton d'authentification pour la connexion à Oracle Cloud Container Registry

Cette étape consiste à générer un _jeton d'authentification_ que nous utiliserons pour nous connecter à Oracle Cloud Container Registry.

1.  Sélectionnez l'icône Utilisateur dans l'angle supérieur droit, puis _Mon profil_.
    
    ![Mon profil](images/my-profile.png " ")
    
2.  Faites défiler la page vers le bas et sélectionnez _Jetons d'authentification_.
    
    ![Jetons d'authentification](images/auth-token.png " ")
    
3.  Cliquez sur _Générer un jeton_.
    
    ![Générer un jeton](images/generate-token.png " ")
    
4.  Copiez _`myproject-your_first_name`_ et collez-le dans la zone _Description_, puis cliquez sur _Générer un jeton_.
    
    ![Création de jeton](images/token-create.png " ")
    
5.  Sélectionnez _Copier_ sous Jeton généré et collez-le dans l'éditeur de texte. Nous ne pourrons pas le copier plus tard. Cliquez ensuite sur _Fermer_.
    
    ![Copier le jeton](images/copy-token.png " ")
    

## Tâche 3 : propager l'image Docker de l'application Helidon (myproject) vers le référentiel Container Registry

1.  Dans la tâche 1 de cet exercice, vous avez ouvert l'URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab), déterminé l'adresse du nom de région et copié dans un fichier texte. Dans notre exemple, le nom de la région est UK South (Londres). Vous aurez besoin de ces informations pour cette tâche. ![Adresse](images/end-points.png)
    
2.  Copiez la commande suivante, collez-la dans votre fichier texte, puis remplacez `ENDPOINT_OF_REGION_NAME` par l'adresse de votre région.
    
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
    
5.  Sélectionnez le compartiment, puis cliquez sur **Créer un référentiel**. ![Création de référentiel](images/repository-create.png)
    
6.  Sélectionnez le compartiment et entrez _`myproject-your_first_name`_ comme nom de référentiel, puis choisissez Accès en tant que **public** et cliquez sur **Créer un référentiel**.
    
    ![Description de référentiel](images/describe-repository.png)
    
7.  Une fois le référentiel _`myproject-your_first_name`_ créé, vous pouvez le vérifier dans la liste des référentiels et ses paramètres.
    
    ![Vérifier l'espace de noms](images/verify-namespace.png)
    
8.  Pour propager l'image Docker vers le référentiel dans Oracle Cloud Container Registry, copiez et collez la commande suivante dans votre fichier texte, puis remplacez `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/myproject-your\_first\_name :1.0 par le nom complet de l'image Docker que vous avez enregistré précédemment.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    Le résultat doit ressembler à :
    
        $ docker push lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 The push refers to repository [lhr.ocir.io/tenancynamespace/myproject-ankit]
        0bf9e88b7284: Pushed 
        0acc08535a89: Pushed 
        1.0: digest: sha256:3e8cc0ff23d256499dff8d907150b639925687aed0c41008cbe1794bc02433e2 size: 735
        $
        
9.  Une fois la commande _docker push_ exécutée, développez le référentiel _`myproject-your_first_name`_ et vous remarquerez qu'une nouvelle image a été téléchargée vers ce référentiel.
    
    ![Image téléchargée](images/verify-push.png)
    

## Accusés de réception

*   **Auteur** - Dmitry Aleksandrov
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, avril 2023
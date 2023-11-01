# Transmission de l'image d'application Helidon à Oracle Cloud Container Registry

## Présentation

Dans cet atelier, vous allez créer une image Docker avec votre application Helidon et la propager vers un référentiel dans Oracle Cloud Container Registry.

Temps estimé : 10 minutes

### Objectifs

*   Créez et packagez votre application à l'aide de Docker.
*   Générez un jeton d'authentification pour vous connecter à Oracle Cloud Container Registry.
*   Transférez l'image Docker de l'application Helidon vers le référentiel Oracle Cloud Container Registry.

### Prérequis

*   L'application Helidon que vous avez créée dans l'exercice précédent
*   Docker
*   Compte Oracle Cloud

## Tâche 1 : création de l'image Docker de l'application Helidon

Nous commencerons par préparer l'image Docker que vous utiliserez pour le déploiement sur Verrazzano.

Nous sommes en train de créer une image Docker que vous téléchargerez vers Oracle Cloud Container Registry appartenant à votre compte OCI. Pour ce faire, vous devez créer un nom d'image qui reflète les coordonnées de votre registre.

Vous devez disposer des informations suivantes :

*   Espace de noms de location
*   Adresse de la région

1.  Pour rechercher l'espace de noms de la location, cliquez sur l'icône _Utilisateur_ -> _Location_ comme indiqué. Dans les **paramètres Object Storage**, vous trouverez l'espace de noms. Copiez-le et enregistrez-le dans votre fichier texte, car nous l'utiliserons également plus tard.
    
    ![Copier l'espace ténancynamique](images/copy-tenancynamespace.png " ")
    
2.  Localisez l'_adresse de votre région_. Reportez-vous au tableau documenté à cette URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). Dans l'exemple présenté, l'adresse de la région est _Sud du Royaume-Uni (Londres)_ (en tant que nom de région) et son adresse est _lhr.ocir.io_. Localisez l'adresse de votre propre _nom de région_ et enregistrez-la dans le fichier texte. Vous en aurez également besoin pour le prochain laboratoire.
    
    ![Adresses](images/end-point.png " ")
    
    > Vous disposez désormais de l'espace de noms de location et de l'adresse de votre région.
    
3.  Copiez la commande suivante et collez-la dans votre éditeur de texte. Remplacez ensuite _`ENDPOINT_OF_YOUR_REGION`_ par l'adresse du nom de région, _`NAMESPACE_OF_YOUR_TENANCY`_ par l'espace de noms de votre location et _`your_first_name`_ par le prénom.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0 .</copy>
        
    
    Lorsque la commande est prête, exécutez-la dans Cloud Shell à partir du répertoire _`~/quickstart-mp/`_. Le paramétrage produira le résultat suivant :
    
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
        
4.  Cette opération crée l'image Docker, que vous pouvez réinsérer dans le référentiel local.
    
        $ docker images
        
        REPOSITORY                                                                           TAG                               IMAGE ID       CREATED         SIZE
        lhr.ocir.io/tenancynamespace/quickstart-mp-ankit                                               1.0                               587a079ad854   5 minutes ago   243MB
        
    
    Copiez dans votre fichier texte le nom d'image complet remplacé `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-ankit:1.0` car vous en aurez besoin ultérieurement.
    

## Tâche 2 : génération d'un jeton d'authentification pour la connexion à Oracle Cloud Container Registry

Cette étape consiste à générer un _jeton d'authentification_ que nous utiliserons pour nous connecter à Oracle Cloud Container Registry.

1.  Sélectionnez l'icône Utilisateur dans l'angle supérieur droit, puis _Mon profil_.
    
    ![Mon profil](images/my-profile.png)
    
2.  Faites défiler la page vers le bas et sélectionnez _Jetons d'authentification_.
    
    ![Jeton d'authentification](images/auth-token.png)
    
3.  Cliquez sur _Générer un jeton_.
    
    ![Générer les jetons](images/generate-token.png)
    
4.  Copiez _`quickstart-mp-your_first_name`_, collez-le dans la zone Description et cliquez sur _Générer un jeton_.
    
    ![Créer un jeton](images/create-token.png)
    
5.  Cliquez sur _Copier_ sous Jeton généré et collez-le dans l'éditeur de texte. Vous ne pouvez pas le copier plus tard, alors assurez-vous d'avoir une copie de ce jeton enregistrée. Cliquez ensuite sur _Fermer_.
    
    ![Copier le jeton](images/copy-token.png)
    

## Tâche 3 : propager l'image Docker de l'application Helidon (quickstart-mp) vers le référentiel Container Registry

1.  Dans la tâche 1 de cet exercice, vous avez ouvert l'URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab), déterminé l'adresse du nom de région et copié dans un éditeur de texte. Dans notre exemple, le nom de la région est UK South (Londres). Vous aurez besoin de ces informations pour cette tâche. ![Adresse](images/end-point.png)
    
2.  Copiez la commande suivante, collez-la dans l'éditeur de texte, puis remplacez `ENDPOINT_OF_REGION_NAME` par l'adresse de votre région.
    
    > Dans notre exemple, le nom de région est _Sud du Royaume-Uni (Londres)_ et l'adresse est _lhr.ocir.io_. Vous aurez besoin de vos informations spécifiques pour cette tâche.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  A l'étape précédente, vous avez également déterminé l'espace de noms de location. Entrez le nom utilisateur comme suit : `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Remplacez _`NAMESPACE_OF_YOUR_TENANCY`_ par l'espace de noms de votre location.
    *   Remplacez _`YOUR_ORACLE_CLOUD_USERNAME`_ par le nom utilisateur de votre compte Oracle Cloud, puis copiez le nom utilisateur remplacé à partir de l'éditeur de texte et collez-le dans _Cloud Shell_.
    *   Pour Password, copiez et collez le jeton d'authentification à partir de votre éditeur de texte (ou de l'emplacement où vous l'avez enregistré).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Revenez au registre du conteneur. Dans la console, ouvrez le menu de navigation et cliquez sur **Services de développeur**. Sous **Conteneurs et artefacts**, cliquez sur **Registre du conteneur**. ![Registre du conteneur](images/container-registry.png)
    
5.  Sélectionnez le compartiment, puis cliquez sur **Créer**. ![Création de référentiel](images/repository-create.png)
    
6.  Sélectionnez le compartiment et entrez _`quickstart-mp-your_first_name`_ comme nom de référentiel, puis choisissez Accès en tant que **public** et cliquez sur **Créer un référentiel**.
    
    ![Description de référentiel](images/describe-repository.png)
    
7.  Une fois le référentiel _`quickstart-mp-your_first_name`_ créé, vous pouvez le vérifier dans la liste des référentiels et ses paramètres.
    
    ![Vérifier l'espace de noms](images/verify-namespace.png)
    
8.  Pour propager l'image Docker vers le référentiel dans Oracle Cloud Container Registry, copiez et collez la commande suivante dans votre éditeur de texte, puis remplacez `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`quickstar-mp-your_first_name` :1.0 par le nom complet de l'image Docker que vous avez enregistré précédemment.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0</copy>
        
    
    Le résultat doit ressembler à :
    
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
        
9.  Une fois la commande _docker push_ exécutée, développez le référentiel _`quickstart-mp-your_first_name`_ et vous remarquerez qu'une nouvelle image a été téléchargée vers ce référentiel.
    
10.  Laissez la page du référentiel _Cloud Shell_ et Container Registry ouverte. Vous en aurez besoin pour les exercices suivants.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
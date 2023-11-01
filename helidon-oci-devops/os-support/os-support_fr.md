# Modification de l'application Helidon pour ajouter la prise en charge d'Object Storage

## Présentation

L'objectif de cet atelier est de démontrer **comment ajouter un accès à Object Storage** à partir de l'application Helidon. Pour ce faire, remplacez une variable utilisée pour stocker le mot d'accueil par un objet qui deviendra désormais le nouveau conteneur de mots d'accueil et qui sera stocké et **récupéré à partir d'un bucket Object Storage**. Comme l'objet est persistant, la valeur du dernier mot de bienvenue survivra au redémarrage de l'application. Sans cette modification et avec le mot de bienvenue en mémoire via la variable, le mot de bienvenue sera réinitialisé à une valeur par défaut lorsque l'application sera redémarrée.

Durée estimée : 15 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Prise en charge d'Object Storage](videohub:1_p5v2wehm)

### Objectifs

Dans cet exercice, vous allez :

*   Modifier l'application Helidon pour afficher son intégration aux services OCI comme Object Storage
*   Vérifier la réussite de l'intégration Object Storage

### Prérequis

*   Un compte cloud Oracle Free Tier (essai), payant ou LiveLabs

## Tâche 1 : modification de l'application Helidon pour l'intégration Object Storage

1.  Vérifiez que **oci.bucket.name** est correctement configuré dans **~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties**, qui aurait déjà dû être défini à l'étape 5 de l'exercice 2/Tâche 3.
    
2.  Dans l'**éditeur de code**, cliquez sur le nom de fichier **`pom.xml`** sous _~/oci-mp/server/_ pour l'ouvrir et ajouter la dépendance de **kit SDK OCI Object Storage** dans la clause **dependencies** comme indiqué ci-dessous.
    
        <copy><dependency>
                    <groupId>com.oracle.oci.sdk</groupId>
                    <artifactId>oci-java-sdk-objectstorage</artifactId>
        </dependency></copy>
        
    
    ![ajouter une dépendance](images/add-dependency.png)
    
    > Assurez-vous que l'indentation est correcte.
    
3.  Nous modifions le fichier _~/oci-mp/server/src/main/java/ocw/hol/mp/oci/server/GreetingProvider.java_ pour ajouter l'accès **Object Storage**. Dans l'intérêt du temps, nous avons le code source pour ce changement. Copiez et collez le code suivant pour mettre ce fichier à jour avec les modifications requises. Vous pouvez lire la _Remarque_ ci-dessous qui décrit les modifications apportées dans ce fichier.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/source/GreetingProvider.java server/src/main/java/ocw/hol/mp/oci/server/</copy>
        
    
    ![object storage ajouté](images/os-added.png)
    
    > **Veuillez lire :-**
    
    *   Dans la section d'argument du constructeur, le paramètre _ObjectStorage objectStorageClient_ a été ajouté. Comme il fait partie de l'annotation _@Injected_, le paramètre est automatiquement traité et défini par Helidon pour contenir le client qui peut être utilisé pour communiquer avec le service Object Storage sans avoir à ajouter plusieurs lignes de code de **kit SDK OCI** à cette fin.
    *   Dans la même section d'argument du constructeur, ajoutez **ConfigProperty** qui extrait la valeur d'une propriété **oci.bucket.name** dans la configuration. Cette valeur a été précédemment renseignée dans **microprofile-config.properties** lors de la configuration initiale de l'application lorsqu'un script utilitaire appelé **`update_config_values.sh`** a été exécuté à partir du répertoire de référentiel **`devops_helidon_to_instance_ocw_hol`**.
    *   A l'aide de la méthode **getNamespace() Object Storage SDK**, extrayez l'espace de noms Object Storage tel qu'il sera utilisé ultérieurement pour extraire ou stocker un objet :
    
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
        
    
    *   Juste en dessous de la classe GreetingProvider, enlève la déclaration de variable :
    
        <copy>private final AtomicReference<String> message = new AtomicReference<>();</copy>
        
    
    *   Remplacez-le par des variables globales locales comme ci-dessous. _LOGGER_ sera utilisé pour la journalisation tandis que _objectStorageClient_, _namespaceName_, _bucketName_ et _objectName_ seront utilisés sur les appels du _kit SDK Object Storage_.
    
        <copy>private static final Logger LOGGER = Logger.getLogger(GreetingProvider.class.getName());
        private ObjectStorage objectStorageClient;
        private String namespaceName;
        private String bucketName;
        private final String objectName = "hello.txt";</copy>
        
    
    *   Remplacez le contenu de _getMessage()_ par le code ci-dessous. Cette opération utilisera la méthode du kit SDK **getObject()** pour extraire le mot de bienvenue de l'objet **hello.txt** extrait du bucket.
    
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
        
    
    *   Remplacez le contenu de la méthode void **setMessage(String message)** par le code ci-dessous. La méthode du kit SDK **putObject()** permet de stocker l'objet **hello.txt** contenant le mot de bienvenue dans le bucket.
    
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
        
    
    *   L'import a été supprimé (**import java.util.concurrent.atomic.AtomicReference ;**) et remplacé par ces nouveaux imports afin de prendre en charge tout le nouveau code ajouté.
    
        <copy>import java.io.ByteArrayInputStream;
        import java.util.logging.Logger;
        
        import com.oracle.bmc.objectstorage.ObjectStorage;
        import com.oracle.bmc.objectstorage.requests.GetNamespaceRequest;
        import com.oracle.bmc.objectstorage.requests.GetObjectRequest;
        import com.oracle.bmc.objectstorage.requests.PutObjectRequest;
        import com.oracle.bmc.objectstorage.responses.GetNamespaceResponse;
        import com.oracle.bmc.objectstorage.responses.GetObjectResponse;</copy>
        

## Tâche 2 : propager la modification du code de l'application Helidon et déclencher le pipeline DevOps

1.  Copiez et collez la commande suivante dans le terminal **pour valider et appliquer la modification**.
    
        <copy>git add .
        git status
        git commit -m "Add support Object Storage integration"
        git push</copy>
        
    
    > Le pipeline sera déclenché par cette poussée git.
    
2.  Attendez que le **cycle de vie DevOps** soit terminé en surveillant les journaux de pipeline de build et de déploiement. ![exécution du déploiement](images/deployment-run.png)
    

## Tâche 3 : vérifier le succès de l'intégration d'Object Storage

Testez à l'aide de curl et vérifiez qu'un nouvel objet **hello.txt** a été ajouté au **bucket**. Vérifiez que la taille de l'objet est identique à celle du mot de bienvenue. Par exemple, si le mot de bienvenue est _Hello_, la taille doit être **5**. Si le mot d'accueil est _Hola_, la taille doit être **4**.

1.  Configurez le noeud de déploiement **`PUBLIC_IP`** en tant que variable d'environnement.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Copiez et collez la commande suivante pour placer une demande **GET**. Vous obtiendrez un résultat similaire à celui indiqué ci-dessous.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  Dans la **console cloud**, cliquez sur _menu Hamburger_ -> _Stockage_ -> _Buckets_ comme indiqué. ![menu de bucket](images/bucket-menu.png)
    
4.  Sélectionnez le compartiment approprié et cliquez sur **app-bucket-helidonocw-hol-string** pour ouvrir le **bucket**. ![sélectionner un compartiment](images/select-compartment.png)
    
5.  Vérifiez que le bucket contient désormais un objet **hello.txt** et qu'il a une taille de **5 octets** car le mot de bienvenue est **Hello**. Vous pouvez également télécharger l'objet et vérifier que le contenu est bien _Hello_. ![vérifier la taille](images/verify-size.png)
    
    > Laissez cette page ouverte, car nous actualiserons cette page, une fois que nous aurons modifié le mot de bienvenue à l'étape suivante.
    
6.  Copiez et collez la commande suivante pour remplacer le mot d'accueil **Hello** par **Hola**.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
7.  Vérifiez que l'objet **hello.txt** du bucket a désormais une taille de **4 octets** car le mot de bienvenue est remplacé par **Hola**. Vous pouvez également télécharger l'objet et vérifier que le contenu est remplacé par **Hola**. ![taille de cola](images/hola-size.png)
    
8.  Redémarrez l'application à l'aide de l'outil **restart.sh** pour démontrer que la valeur du mot de bienvenue survivra telle qu'elle est conservée dans **Object Storage**.
    
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
        
    
    ![redémarrer l'application](images/restart-application.png)
    
    > Entrez _yes_, lorsque vous êtes invité à **Are you sure you want to continue connect (yes/no) ?**.
    
9.  Appelez la demande Hello World par défaut et **observez que le mot de bienvenue est toujours Hola**.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
10.  Actualisez la page Bucket dans la **console cloud** et vous remarquerez que la taille est toujours de **4**, ce qui confirme que le mot de bienvenue est toujours **Hola**. ![Vérifier la persistance](images/verify-persistence.png)
    

**Félicitations !** Vous avez terminé l'atelier. Si vous le souhaitez, vous pouvez poursuivre l'**exercice 6**, qui **supprime toutes les ressources** créées lors de cet atelier.

## En savoir plus

*   [Intégration Helidon OCI](https://helidon.io/docs/v3/#/mp/integrations/oci)

## Accusés de réception

*   **Auteur** - Keith Lustria
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
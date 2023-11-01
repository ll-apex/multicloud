# Modification de l'application Bobby's Book et création d'une image de composant d'application

## Présentation

Dans cet exercice, nous allons modifier l'application bobbys-helidon-stock via _Cloud Shell_. Plus tard, nous allons créer une nouvelle image Docker pour bobbys-helidon-stock-application. Cette image bobbys-helidon-stock-application est un composant de l'application Bobby's Books.

Durée estimée : 10 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Modifiez l'application bobbys-helidon-stock.
*   Créez une image Docker pour bobbys-helidon-stock-application.

### Prérequis

Vous devez disposer d'un éditeur de texte, dans lequel vous pouvez coller les commandes et les URL et les modifier, conformément à votre environnement. Vous pouvez ensuite copier et coller les commandes modifiées pour les exécuter dans _Cloud Shell_.

## Tâche 1 : modifier bobbys-helidon-stock-application

1.  Sélectionnez l'onglet Livre de Bobby, cliquez sur _Livres_, puis cliquez sur l'image du livre _Le Hobbit_, comme indiqué ci-dessous :
    
    ![Cliquez sur Books.](images/clickbooks.png " ")
    
    Il affiche le nom du livre au format _Le Hobbit_, comme illustré sur l'image.
    
    ![Le Hobbit](images/thehobbit.png " ")
    
2.  Nous voulons convertir le nom du livre en majuscules (THE HOBBIT). Nous devons télécharger le code source de l'application Bobby's Books. Assurez-vous que vous êtes dans le dossier d'accueil. Copiez les commandes suivantes et collez-les dans _Cloud Shell_.
    
        <copy>cd ~
        git clone -b v0.16.0 https://github.com/verrazzano/examples.git</copy>
        
    
    ![Copier le référentiel](images/clonerepository.png " ")
    
3.  Pour afficher les fichiers dans l'application Livre de Bobby, copiez la commande suivante et collez-la dans _Cloud Shell_.
    
        <copy>ls -la ~/examples/bobs-books/</copy>
        
    
    La sortie doit être similaire à la suivante : `bash $ ls -la ~/examples/bobs-books/ total 16 drwxr-xr-x. 5 ankit_x_pa oci 100 Mar 21 12:14 . drwxr-xr-x. 7 ankit_x_pa oci 4096 Mar 21 12:14 .. drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobbys-books drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobs-bookstore-order-manager -rw-r--r--. 1 ankit_x_pa oci 942 Mar 21 12:14 README.md drwxr-xr-x. 4 ankit_x_pa oci 72 Mar 21 12:14 roberts-books $`
    
4.  Maintenant, nous allons apporter des modifications dans le JAVA\_FILE pertinent. Pour ouvrir le fichier, copiez la commande suivante et collez-la dans _Cloud Shell_.
    
        <copy>vi ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/src/main/java/org/books/bobby/BookResource.java</copy>
        
    
    ![Ouvrir fichier](images/openfile.png " ")
    
5.  Appuyez sur _i_ pour modifier le code. Pour ajouter une nouvelle ligne à la ligne numéro 84, copiez la ligne suivante et collez-la à la ligne numéro 84 comme indiqué :
    
        <copy>optional.get().setTitle(optional.get().getTitle().toUpperCase());</copy>
        
    
    ![Insérer une ligne](images/insertline.png " ")
    
6.  Appuyez sur _Echap_, puis sur _:wq_ pour enregistrer les modifications.
    
    ![Enregistrer les modifications](images/savechanges.png " ")
    

## Tâche 2 : créer une image Docker pour l'application bobbys-helidon-stock

1.  Etant donné que nous allons créer une nouvelle image Docker pour _bobbys-helidon-stock-application_, qui a des dépendances sur _bobbys-coherence-application_, nous exécutons une commande _Maven_ pour nettoyer l'archive d'application bobbys-coherence existante et compiler, créer, packager et installer une nouvelle archive d'application bobby-coherence dans un référentiel Maven local. Pour passer au dossier de répertoire _bobbys-coherence_ et compiler, créer et installer l'archive d'application _bobbys-coherence_ dans un référentiel Maven local, copiez la commande suivante et collez-la dans _Cloud Shell_.
    
        <copy>
        cd ~/examples/bobs-books/bobbys-books/bobbys-coherence/
        mvn clean install
        </copy>
        
    
    La sortie obtenue est similaire à la suivante (en abrégé pour afficher uniquement le début et la fin de la sortie) : `bash $ mvn clean install [INFO] Scanning for projects... [INFO] [INFO] ------------< io.verrazzano.example.books:bobbys-coherence >------------ [INFO] Building bobbys-coherence 1.0-SNAPSHOT [INFO] --------------------------------[ jar ]--------------------------------- [INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ bobbys-coherence --- [[INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/target/bobbys-coherence.jar to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.jar [INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/pom.xml to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.pom [INFO] ------------------------------------------------------------------------ [INFO] BUILD SUCCESS [INFO] ------------------------------------------------------------------------ [INFO] Total time: 8.887 s [INFO] Finished at: 2023-03-22T04:09:11Z [INFO] ------------------------------------------------------------------------ $`
    
2.  Etant donné que nous avons modifié _bobbys-helidon-stock-application_, nous devons compiler, créer et packager cette application. Pour passer au répertoire _bobbys-helidon-stock-application_ et packager _bobbys-helidon-stock-application_ dans un fichier JAR, copiez la commande suivante et collez-la dans _Cloud Shell_. Dans la deuxième image, vous pouvez voir la création du fichier `bobbys-helidon-stock-application.jar` à l'adresse `~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target`.
    
        <copy>cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/
        mvn clean package
        </copy>
        
    
    The resulting output is similar to the following (abbreviated to show only the start and end of output): \`\`\`bash $ cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/ $ mvn clean package \[INFO\] Scanning for projects... \[INFO\] ------------------------------------------------------------------------ \[INFO\] Detecting the operating system and CPU architecture \[INFO\] ------------------------------------------------------------------------
    
         [INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ bobbys-helidon-stock-application ---
         [INFO] Building jar: /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target/bobbys-helidon-stock-application.jar
         [INFO] ------------------------------------------------------------------------
         [INFO] BUILD SUCCESS
         [INFO] ------------------------------------------------------------------------
         [INFO] Total time:  10.106 s
         [INFO] Finished at: 2023-03-22T04:11:38Z
         [INFO] ------------------------------------------------------------------------
         $
         ```
        
3.  Nous allons créer une image Docker pour bobby-stock-helidon-application, mais cette application utilise une version spécifique de JDK et nous ne voulons pas modifier les fichiers Docker qui créent la nouvelle image. Nous téléchargeons donc le kit JDK requis. Pour télécharger la version de JDK requise, copiez la commande ci-dessous et collez-la dans _Cloud Shell_.
    
        <copy>wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2_linux-x64_bin.tar.gz</copy>
        
    
    La sortie doit être similaire à la suivante : \`\`bash $ wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz --2023-03-22 04:20:16-- https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz Résolution de download.java.net (download.java.net)... 2.23.220.73 Connexion à download.java.net (download.java.net)|2.23.220.73| :443... connexion. Demande HTTP envoyée, en attente de réponse... 200 OK Longueur : 198606200 (189M) \[application/x-gzip\] Enregistrement sur : 'openjdk-14.0.2\_linux-x64\_bin.tar.gz'
    
         100%[==============================================================================================================================>] 198,606,200  194MB/s   in 1.0s   
        
         2023-03-22 04:20:17 (194 MB/s) - ‘openjdk-14.0.2_linux-x64_bin.tar.gz’ saved [198606200/198606200]
         $
         ```
        
4.  Nous sommes en train de créer une image Docker que nous allons télécharger vers Oracle Cloud Container Registry dans l'atelier 6. Pour créer le nom de fichier, nous avons besoin des informations suivantes :
    
    *   Espace de noms de location
    *   Adresse de la région
5.  Pour rechercher l'espace de noms de la location, cliquez sur l'icône _Utilisateur_ -> _Location_ comme indiqué. Dans les **paramètres Object Storage**, vous trouverez l'espace de noms. Copiez-la et enregistrez-la quelque part dans votre fichier texte, car nous l'utiliserons également dans l'exercice 6.
    
    ![Copier l'espace ténancynamique](images/copy-tenancynamespace.png " ")
    
6.  Localisez l'_adresse de votre région_. Reportez-vous au tableau documenté à cette URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). Dans l'exemple présenté, l'adresse de la région est _Sud du Royaume-Uni (Londres)_ (en tant que nom de région) et son adresse est _lhr.ocir.io_. Localisez l'adresse de votre propre _nom de région_ et enregistrez-la dans le fichier texte. Vous en aurez également besoin pour le prochain laboratoire.
    
    ![Adresses](images/end-point.png " ")
    
    > Vous disposez désormais de l'espace de noms de location et de l'adresse de votre région.
    
7.  Vous disposez désormais de l'espace de noms et de l'adresse de location pour votre région. Copiez la commande suivante et collez-la dans votre éditeur de texte. Remplacez ensuite **`END_POINT_OF_YOUR_REGION`** par l'adresse du nom de région, **`NAMESPACE_OF_YOUR_TENANCY`** par l'espace de noms de votre location et **`your_first_name`** par votre prénom en minuscules.
    
        <copy>docker build --force-rm=true -f Dockerfile -t END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0 .</copy>
        
    
    ![Build Docker](images/docker-build.png " ")
    
    ![Image créée](images/image-created.png " ")
    
    > Par exemple, dans mon cas, la commande est `docker build --force-rm=true -f Dockerfile -t lhr.ocir.io/tenancynamespace/helidon-stock-application-ankit`.
    
    Cette opération crée l'image Docker que nous propagerons dans le référentiel Oracle Cloud Container Registry dans l'atelier 6. Vous devez copier le nom d'image complet remplacé **`END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0`** dans le texte editor.In Atelier 6. Lorsque vous devez créer le référentiel, vous devez lui attribuer le nom **`helidon-stock-application-your_first_name`**.
    
    Laissez _Cloud Shell_ ouvert ; nous en avons besoin pour l'exercice suivant.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
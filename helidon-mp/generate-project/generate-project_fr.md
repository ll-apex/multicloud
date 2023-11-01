# Générer un projet Helidon MP et exécuter le projet dans l'éditeur de code

## Présentation

Cet atelier explique comment créer une application Helidon MP.

Temps estimé : 15 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Générer un projet MP Helidon et exécuter le projet dans l'éditeur de code](videohub:1_22nv8v4q)

### A propos du produit/technologie

Helidon est conçu pour être simple à utiliser, avec des outils et des exemples pour vous faire avancer rapidement. Comme Helidon n'est qu'une collection de bibliothèques fonctionnant sur un noyau Netty rapide, il n'y a pas de surcharge ou de gonflement supplémentaire. Helidon prend en charge MicroProfile et fournit des API courantes telles que JAX-RS, CDI et JSON-P/B. Notre implémentation MicroProfile s'exécute sur notre WebServer réactif Helidon rapide. Le Reactive WebServer fournit un modèle de programmation moderne et fonctionnel et s'exécute sur Netty. Léger, flexible et réactif, Helidon WebServer fournit une base simple à utiliser et rapide pour vos microservices.

Grâce à la prise en charge des vérifications de l'état, des mesures, du suivi et de la tolérance aux pannes, Helidon a ce dont vous avez besoin pour écrire des applications prêtes pour le cloud qui s'intègrent à Prometheus, Jaeger/Zipkin et Kubernetes.

### A propos de Helidon Project Starter

Project Starter est une nouvelle interface utilisateur Web permettant de créer des projets Helidon. Il est hautement personnalisable et propose diverses options permettant aux utilisateurs de sélectionner les fonctionnalités Helidon qu'ils souhaitent ajouter au projet. Les utilisateurs finaux pourront générer des projets en fonction de leurs besoins spécifiques. Pour plus d'informations, cliquez sur [Helidon Starter](https://helidon.io/starter).

### A propos de l'éditeur de code

L'éditeur de code vous permet de modifier et de déployer du code pour divers services OCI directement à partir de la console OCI. Vous pouvez désormais mettre à jour les scripts et les workflows de service sans avoir à basculer entre la console et vos environnements de développement locaux. Cela facilite la création rapide de prototypes de solutions cloud, l'exploration de nouveaux services et l'exécution rapide de tâches de codage.

L'intégration directe de l'éditeur de code à Cloud Shell vous permet d'accéder à l'image native d'entreprise GraalVM et à JDK 17 (Java Development Kit) préinstallés dans Cloud Shell.

### A propos d'OCI Cloud Shell

[OCI Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm) est un terminal basé sur un navigateur accessible à partir de la console Oracle Cloud. Il fournit un accès à un shell Linux avec une interface de ligne de commande (CLI) OCI préauthentifiée et des outils de développement préinstallés, et est livré avec 5 Go de stockage.

A partir de la version 22.2.0, GraalVM Enterprise JDK 17 et Native Image sont préinstallés dans Cloud Shell.

> GraalVM Enterprise est disponible sur Oracle Cloud Infrastructure gratuitement.

### Objectifs

*   Créez un microservice pris en charge par MicroProfile appelé application Helidon Greeting.
*   Ouvrir l'application Helidon dans l'éditeur de code
*   Change the default JDK in the cloud shell
*   Configurer le Maven requis
*   Exécuter et exécuter l'application Helidon Greeting

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.

## Tâche 1 : générer un projet Helidon à l'aide du démarreur de projet

1.  Copiez l'URL ci-dessous et collez-la dans le navigateur pour ouvrir la page du projet Helidon.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  Sous Générer votre projet, sélectionnez _Helidon MP_ en tant que saveur Helidon, puis cliquez sur _Suivant_.
    
3.  Pour Type d'application, sélectionnez _Démarrage rapide_, puis cliquez sur _Suivant_.
    
4.  Pour Support média, sélectionnez _JSON-B_, puis cliquez sur _Suivant_.
    
5.  Pour Personnaliser le projet, sélectionnez les valeurs par défaut et cliquez sur _Téléchargements_. Cela apparaîtra dans une fenêtre, enregistrez ce fichier _myproject.zip_ à l'emplacement de votre choix. Dans le reste de cet atelier, le nom de myproject sera utilisé. Si vous choisissez un autre nom, modifiez-le respectivement.
    

## Tâche 2 : créer et exécuter le projet helidon localement

1.  Dans la console cloud, cliquez sur l'icône _Outils de développement_ comme indiqué, puis sur _Editeur de code_. ![Editeur de code](images/code-editor.png)
    
2.  Dans l'éditeur de code, cliquez sur _Terminal_ -> _New Terminal_. ![ouvrir un terminal](images/open-terminal.png)
    
3.  Copiez et collez la commande ci-dessous dans le terminal pour créer le dossier _myproject_, où nous téléchargerons le fichier myproject.zip par défaut.
    
        <copy>mkdir -p ~/helidon-project/myproject</copy>
        
4.  Dans l'éditeur de code, cliquez sur _Fichier_ -> _Ouvrir_. ![Ouvrir un dossier](images/open-folder.png)
    
5.  Sélectionnez le dossier _helidon-project_ et cliquez sur _Open_. ![Ouvrir Helidon](images/open-helidon.png)
    
6.  Dans l'éditeur de code, dans la fenêtre EXPLORER, vous pouvez voir votre _HELIDON-PROJECT_. Vous pouvez voir le dossier _myproject_ ici, cliquez dessus. Cliquez à présent sur _Fichier_ -> _Télécharger des fichiers_, puis indiquez l'emplacement où vous avez téléchargé le projet, sélectionnez le fichier ZIP et cliquez sur _Ouvrir_. ![myprojet](images/myproject.png) ![charger des fichiers](images/upload-files.png)
    
7.  Copiez et collez la commande suivante pour décompresser le fichier _myproject.zip_.
    
        <copy>cd ~/helidon-project/myproject
        unzip myproject.zip
        </copy>
        
8.  Développez le dossier _myproject_ pour afficher la structure du projet. ![Développer le projet](images/expand-project.png)
    
9.  Pour exécuter ce projet, nous utiliserons Maven 3.8+ et JDK 17+. Dans le cloud Oracle, vous disposez de différents JDK. Ici, nous allons sélectionner GraalVM JDK. Copiez et collez la commande suivante dans le terminal pour connaître votre JDK par défaut.
    
        <copy>csruntimectl java list</copy>
        
    
    ![liste JDK](images/list-jdk.png)
    
    > Le JDK avec \* _astérisque_ au début est votre JDK par défaut. Si vous disposez d'un autre JDK, graalvmeejdk, modifiez la version par défaut du JDK en exécutant la commande ci-dessous. Veuillez utiliser la version affichée de graalvmeejdk, car elle peut être différente de ce qui est affiché dans la commande.
    
        <copy>csruntimectl java set graalvmeejdk-17</copy>
        
10.  Pour configurer le maven requis, copiez et collez la commande suivante dans le terminal.
    
        <copy>cd ~/helidon-project/myproject/
        wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
        tar -xzvf apache-maven-3.8.8-bin.tar.gz
        PATH=~/helidon-project/myproject/apache-maven-3.8.8/bin:$PATH</copy>
        
    
    ![configurer maven](images/configure-maven.png)
    
11.  Pour vérifier que vous disposez de la bonne version de JDK et de Maven comme indiqué ci-dessous, exécutez la commande suivante dans le terminal.
    
        <copy>mvn -v</copy>
        
    
    ![Vérifier le prérequis](images/verify-prerequisite.png)
    
12.  Dans le dossier _myproject_, exécutez la commande suivante pour créer le projet.
    
        <copy>cd ~/helidon-project/myproject/myproject
        mvn clean package</copy>
        
    
    ![créer un projet](images/build-project.png)
    
    > La mention _BUILD SUCCESS_ doit apparaître à la fin de l'exécution de cette commande.
    
13.  Copiez et collez la commande suivante dans le terminal pour exécuter cette application. La sortie est similaire à celle illustrée dans la capture d'écran ci-dessous.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    ![exécuter un projet](images/run-project.png)
    

> Notez que l'heure de début est de 4822 millisecondes. Nous comparerons cette heure avec l'exécutable d'image natif ultérieurement.

14.  Ouvrez un nouveau terminal/une nouvelle console dans l'éditeur de code et exécutez les commandes suivantes pour vérifier l'application :
    
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
        
15.  _Arrêtez l'application **myproject** en saisissant `Ctrl + C` dans le terminal où la commande "java -jar target/myproject.jar" est exécutée_. C'EST TRÈS IMPORTANT, AUTREMENT VOUS FACEZ DES QUESTIONS DANS LE LAB PLUS TARD.
    

## Accusés de réception

*   **Auteur** - Dmitry Aleksandrov
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, avril 2023
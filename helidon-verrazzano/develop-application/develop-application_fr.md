# Développer une application Helidon

## Présentation

Cet atelier explique comment créer une application Helidon MP.

Temps estimé : 15 minutes

### A propos du produit/technologie

Helidon est conçu pour être simple à utiliser, avec des outils et des exemples pour vous faire avancer rapidement. Comme Helidon n'est qu'une collection de bibliothèques fonctionnant sur un noyau Netty rapide, il n'y a pas de surcharge ou de gonflement supplémentaire. Helidon prend en charge MicroProfile et fournit des API courantes telles que JAX-RS, CDI et JSON-P/B. Notre implémentation MicroProfile s'exécute sur notre WebServer réactif Helidon rapide. Le Reactive WebServer fournit un modèle de programmation moderne et fonctionnel et s'exécute sur Netty. Léger, flexible et réactif, Helidon WebServer fournit une base simple à utiliser et rapide pour vos microservices.

Grâce à la prise en charge des vérifications de l'état, des mesures, du suivi et de la tolérance aux pannes, Helidon a ce dont vous avez besoin pour écrire des applications prêtes pour le cloud qui s'intègrent à Prometheus, Jaeger/Zipkin et Kubernetes.

### A propos de la CLI Helidon

L'interface de ligne de commande Helidon vous permet de créer facilement un projet Helidon en sélectionnant un ensemble d'archétypes. Il prend également en charge une boucle de développement qui effectue une compilation continue et un redémarrage de l'application, de sorte que vous pouvez facilement itérer les modifications du code source.

La CLI est distribuée en tant qu'exécutable autonome (compilée à l'aide de GraalVM) pour faciliter l'installation. Il est actuellement disponible en téléchargement pour Linux, Mac et Windows. Il vous suffit de télécharger le fichier binaire, de l'installer à un emplacement accessible à partir de votre PATH et vous êtes prêt à partir.

### Objectifs

*   Installation de l'interface de ligne de commande Helidon
*   Créez un microservice pris en charge par MicroProfile appelé Helidon Greeting.
*   Exécuter et exécuter l'application Helidon Greeting
*   Afficher les données d'état et de mesures
*   Ajouter une nouvelle fonctionnalité à l'application

### Prérequis

*   Helidon nécessite Java 11+
*   Maven 3.6.x

> **Attention : N'utilisez pas la version 3.8.x en raison d'un problème connu avec le build de l'application !**

*   Java et `mvn` se trouvent dans votre chemin.
*   Les utilisateurs Windows auront également besoin de l'exécution redistribuable Visual C++.  
    Pour plus d'informations, reportez-vous à [Helidon sous Windows](https://helidon.io/docs/v2/#/about/04_windows).

## Tâche 1 : installation de l'interface de ligne de commande Helidon

1.  Installation de l'interface de ligne de commande Helidon
    
    Pour macOS :
    
        <copy>
        curl -O https://helidon.io/cli/latest/darwin/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Pour Linux :
    
        <copy>
        curl -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Sous Windows :
    
        <copy>
        PowerShell -Command Invoke-WebRequest -Uri "https://helidon.io/cli/latest/windows/helidon.exe" -OutFile "C:\Windows\system32\helidon.exe"
        </copy>
        

## Tâche 2 : créer une application d'accueil Helidon

1.  Dans la console, entrez :
    
        <copy>helidon init --version 2.4.0 </copy>
        
    
    > Pour éviter tout problème potentiel, définissez la version Helidon spécifique testée pour l'environnement de cet atelier.
    
2.  Pour cette démonstration, nous allons créer un microservice pris en charge par MicroProfile. Choisissez donc l'option **(2)** pour **Helidon MP Flavor** :
    
        Helidon flavor
        (1) SE 
        (2) MP 
        Enter selection (Default: 1): 2
        
3.  Pour tirer le meilleur parti des fonctionnalités, choisissez l'option **(2) démarrage rapide**, puis **Entrée** pour les réponses par défaut. Notez que vous pouvez avoir différents noms de package et de groupe de projets par défaut car il utilise le nom utilisateur du système d'exploitation. Notez le nom du package. Vous devez l'utiliser lors de la création d'une classe Java.
    
        Select archetype
        (1) bare | Minimal Helidon MP project suitable to start from scratch 
        (2) quickstart | Sample Helidon MP project that includes multiple REST operations 
        (3) database | Helidon MP application that uses JPA with an in-memory H2 database 
        Enter selection (Default: 1): 2
        Project name (Default: quickstart-mp): 
        Project groupId (Default: me.user-helidon): 
        Project artifactId (Default: quickstart-mp): 
        Project version (Default: 1.0-SNAPSHOT): 
        Java package name (Default: me.user_name.mp.quickstart): 
        Switch directory to /home/user/quickstart-mp to use CLI
        
        Start development loop? (Default: n):
        $
        
    
    > Pour la **boucle de développement**, acceptez la valeur par défaut (**n**) pour le moment. Vous commencerez la boucle de développement plus tard dans cet atelier.
    
    Vous disposez à présent d'un projet Microservice Maven entièrement fonctionnel :
    

        quickstart-mp
        ├── Dockerfile
        ├── Dockerfile.jlink
        ├── Dockerfile.native
        ├── README.md
        ├── app.yaml
        ├── pom.xml
        └── src
            ├── main
            │   ├── java
            │   │   └── me
            │   │       └── buzz
            │   │           └── mp
            │   │               └── quickstart
            │   │                   ├── GreetResource.java
            │   │                   ├── GreetingProvider.java
            │   │                   └── package-info.java
            │   └── resources
            │       ├── META-INF
            │       │   ├── beans.xml
            │       │   ├── microprofile-config.properties
            │       │   └── native-image
            │       │       └── reflect-config.json
            │       └── logging.properties
            └── test
                └── java
                    └── me
                        └── buzz
                            └── mp
                                └── quickstart
                                    └── MainTest.java
    
    

## Tâche 3 : exécuter l'application Helidon Greeting

1.  Dans la même console/terminal, accédez au répertoire quickstart-mp et exécutez les commandes suivantes :
    
        <copy> cd quickstart-mp
        </copy>
        
2.  Avec JDK11+
    
        <copy>
        mvn package
        java -jar target/quickstart-mp.jar
        </copy>
        

### Exercer l'application

1.  Ouvrez un nouveau terminal/une nouvelle console et exécutez les commandes suivantes pour vérifier l'application :
    
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
        

### Vérifier les données d'état et de mesures

1.  Dans le même terminal/la même console, exécutez les commandes suivantes pour vérifier l'état et les mesures :
    
        <copy>
        curl -s -X GET http://localhost:8080/health
        </copy>
        {"outcome":"UP",...
        . . .
        
    
        # Prometheus Format
        <copy>
        curl -s -X GET http://localhost:8080/metrics
        </copy>
        # TYPE base:gc_g1_young_generation_count gauge
        . . .
        
    
        # JSON Format
        <copy>
        curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
        </copy>
        {"base":...
        . . .
        
2.  Arrêtez l'application _quickstart-mp_ en saisissant `Ctrl + C` dans le terminal où la commande "java -jar target/quickstart-mp.jar" est exécutée.
    

## Tâche 4 : modifier l'application

1.  Ouvrez votre IDE favori et accédez au fichier **microprofile-config.properties**.
    
    ![Fichier de configuration](images/config.jpg)
    
2.  Dans la console/le terminal, accédez au dossier du projet et saisissez :
    
        <copy>helidon dev</copy>
        
    
    > Cela lancera la **boucle de développement** mentionnée dans la tâche précédente.
    
3.  Remplacez la propriété _app.greeting_ par "Hello Oracle" et enregistrez le fichier.
    
        <copy>app.greeting=Hello Oracle</copy>
        
    
    ![HelidonDev](images/properties.jpg)
    
    > Vous verrez que chaque fois que vous modifiez un fichier, l'**interface de ligne de commande Helidon** reconnaît une modification, recompile l'application et la réexécute. Comme Helidon est petit, tout se passe rapidement.
    
4.  Dans la console/le terminal, saisissez ce qui suit :
    
        <copy>curl -X GET http://localhost:8080/greet</copy>
        
    
    Le résultat doit être :
    
        {"message":"Hello Oracle World!"}
        
    
    > Veillez à arrêter la boucle de développement avec `CTRL+C`.
    
5.  Revenez à votre dossier de projet et ouvrez le fichier **GreetResource.java**.
    
    > Vous pouvez voir qu'il s'agit d'un code pur compatible MicroProfile :
    
    ![ModifyJava](images/greet-resource.jpg)
    
6.  Créez une adresse qui fournit de l'aide pour différents salutations dans différentes langues. Pour créer cette nouvelle fonctionnalité, créez une classe nommée **GreetHelpResource** avec le code suivant :
    
        <copy>
        package me.user_name.me.quickstart;
        import java.util.Arrays;
        import java.util.List;
        import java.util.logging.Logger;
        
        import javax.enterprise.context.ApplicationScoped;
        import javax.ws.rs.GET;
        import javax.ws.rs.Path;
        
        import org.eclipse.microprofile.metrics.annotation.Counted;
        
        @ApplicationScoped
        @Path("/help")
        public class GreetHelpResource {
        
            Logger LOGGER = Logger.getLogger(GreetHelpResource.class.getName());
        
            @GET
            @Path("/allGreetings")
            @Counted(name = "helpCalled", description = "How many time help was called")
            public String getAllGreetings(){
                LOGGER.info("Help requested!");
                return Arrays.toString(List.of("Hello","Привет","Hola","Hallo","Ciao","Nǐ hǎo", "Marhaba","Olá").toArray());
            }
        }
        </copy>
        
    
    > La classe dispose d'une seule méthode _getAllGreetings_ qui renvoie la liste des salutations dans différentes langues. Lors de la copie du code, veillez à ajouter le nom de package nécessaire au-dessus de la classe.
    
7.  Créez et exécutez l'application :
    
        <copy>
        mvn package -DskipTests
        java -jar target/quickstart-mp.jar
        </copy>
        
8.  Exécutez la commande suivante et notez les résultats :
    
        <copy>curl http://localhost:8080/help/allGreetings</copy>
        
    
    Le résultat attendu :
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
9.  Regardez les métriques et vous verrez qu'un nouveau compteur est apparu. Chaque fois que cette adresse est appelée, la valeur est incrémentée :
    
        curl http://localhost:8080/metrics
        
        ...
        # TYPE application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total counter
        # HELP application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total How many time help was called
        application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total 1
        ...
        
10.  Dans la console, vous verrez maintenant la ligne de journal INFO concernant cet appel :
    
        INFO me.buzz.mp.quickstart.GreetHelpResource Thread[helidon-4,5,server]: Help requested!
        
    
    Le nouveau endpoint a été ajouté.
    
    ![NewEndpoint](images/logs-output.jpg)
    
    > Travailler avec Helidon et ses outils est facile et rapide !
    
11.  Laissez votre terminal/console ouvert et continuez avec le laboratoire d'installation de Verrazzano.
    

## Accusés de réception

*   **Auteur** - Dmitry Aleksandrov
*   **Contributeurs** - Maciej Gruszka, Ankit Pandey
*   **Dernière mise à jour par/date** - Ankit Pandey, mars 2023
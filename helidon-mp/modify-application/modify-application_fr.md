# Modifier l'application Helidon MP dans l'éditeur de code

## Présentation

Dans cet exercice, vous allez ajouter une adresse personnalisée à la classe JAVA dans l'éditeur de code.

Temps estimé : 10 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Modifier l'application Helidon MP dans l'éditeur de code](videohub:1_sv1iug41)

### Objectifs

Dans cet exercice, vous allez :

*   Ajouter une adresse personnalisée dans la classe Java
*   Créer et exécuter l'application modifiée

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.

## Tâche 1 : ajouter une adresse personnalisée

1.  Dans l'éditeur de code, cliquez sur le fichier _GreetResource.java_ pour l'ouvrir. ![ouvrir le fichier](images/open-file.png)
    
2.  Comme vous pouvez le voir dans le code, il est entièrement basé sur MicroProfile. Cela signifie que toutes les fonctionnalités sont fournies avec des objets POJO et des annotations. Ces annotations sont Standard, donc portables entre différents fournisseurs. Cela signifie que vous pouvez facilement exécuter du code sur d'autres plates-formes prenant en charge la même version de MicroProfile. Pour plus d'informations, cliquez [ici](https://microprofile.io/).
    
3.  Créez un point de terminaison qui fournit des titres au hasard sur un groupe de cinq titres. Pour créer cette adresse, copiez et collez le code ci-dessous à la ligne numéro 80 comme indiqué ci-dessous.
    
        <copy>@Path("/title")
        @GET
        @Produces(MediaType.APPLICATION_JSON)
        public String getRandomTitle() {
            return new String[] {"Mr. ", "Mrs.", "Miss", "Dr.", "Dr. sc."} [(int)(Math.random()*5)];
        }</copy>
        
    
    ![ajouter du code](images/add-code.png)
    
4.  Pour enregistrer le contenu du fichier, dans l'éditeur de code, cliquez sur _Fichier_ -> _Enregistrer_.
    
    > AutoSave est activé par défaut dans l'éditeur de code.
    

## Tâche 2 : exécuter l'application

1.  Copiez et collez la commande suivante dans le terminal pour exécuter l'application.
    
        <copy>mvn clean package
        java -jar target/myproject.jar</copy>
        
2.  Ouvrez un nouveau terminal/une nouvelle console et exécutez les commandes suivantes pour vérifier l'application :
    
        <copy>curl -X GET http://localhost:8080/greet/title</copy>
        Mr.
        
    
    > Il fournit aléatoirement le titre sur 5 options, vous pouvez exécuter cette commande plusieurs fois.
    
3.  _Arrêtez l'application **myproject** en saisissant `Ctrl + C` dans le terminal où la commande "java -jar target/myproject.jar" est exécutée_. C'EST TRÈS IMPORTANT, AUTREMENT VOUS FACEZ DES QUESTIONS DANS LE LAB PLUS TARD.
    

## Accusés de réception

*   **Auteur** - Dmitry Aleksandrov
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, avril 2023
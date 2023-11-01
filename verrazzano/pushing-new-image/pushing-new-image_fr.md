# Propagation de l'image Docker pour l'application bobbys-helidon-stock vers Oracle Cloud Container Registry

## Présentation

Dans le laboratoire 5, nous avons modifié l'application bobbys-helidon-stock et créé une nouvelle image Docker. Dans cet atelier, nous propagerons cette image dans un référentiel à l'intérieur d'Oracle Cloud Container Registry.

Durée estimée : 10 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Générez un jeton d'authentification pour vous connecter à Oracle Cloud Container Registry.
*   Transférez l'image Docker de l'application bobbys-helidon-stock vers le référentiel Oracle Cloud Container Registry.

### Prérequis

Vous devez disposer d'un éditeur de texte, dans lequel vous pouvez coller les commandes et les URL et les modifier, conformément à votre environnement. Vous pouvez ensuite copier et coller la commande modifiée pour les exécuter dans _Cloud Shell_.

## Tâche 1 : génération d'un jeton d'authentification pour la connexion à Oracle Cloud Container Registry

Cette étape consiste à générer un _jeton d'authentification_ que nous utiliserons pour nous connecter à Oracle Cloud Container Registry.

1.  Sélectionnez l'icône Utilisateur dans l'angle supérieur droit, puis _Mon profil_.
    
    ![Paramètres utilisateur](images/user-settings.png " ")
    
2.  Faites défiler la page vers le bas et sélectionnez _Jetons d'authentification_.
    
    ![Jetons d'authentification](images/auth-token.png " ")
    
3.  Cliquez sur _Générer un jeton_.
    
    ![Générer un jeton](images/generate-token.png " ")
    
4.  Copiez _`helidon-stock-application-your_first_name`_ et collez-le dans la zone _Description_, puis cliquez sur _Générer un jeton_.
    
    ![Création de jeton](images/token-create.png " ")
    
5.  Sélectionnez _Copier_ sous Jeton généré et collez-le dans le fichier texte. Nous ne pourrons pas le copier plus tard.
    
    ![Copier le jeton](images/copy-token.png " ")
    

## Tâche 2 : propager l'image Docker de l'application bobbys-helidon-stock vers le référentiel Oracle Cloud Container Registry

1.  Dans la tâche 1 de cet exercice, vous avez ouvert l'URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab), déterminé l'adresse du nom de région et copié dans un éditeur de texte. Dans notre exemple, le nom de la région est UK South (Londres). Vous aurez besoin de ces informations pour cette tâche. ![Adresse](images/end-point.png)
    
2.  Copiez la commande suivante et collez-la dans l'éditeur de texte, puis remplacez **`END_POINT_OF_REGION_NAME`** par l'adresse de votre région.
    
        <copy> docker login `END_POINT_OF_REGION_NAME`</copy>
        

3\. Dans l'exercice précédent, vous avez déterminé l'espace de noms de location. Définissez le nom utilisateur comme suit : \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`/oracleidentitycloudservice/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*. Remplacez \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`\*\* par l'espace de noms de votre location et \*\*\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\* par le nom utilisateur de votre compte Oracle Cloud, puis copiez le nom utilisateur remplacé à partir du fichier texte et collez-le dans \*Cloud Shell\*. Pour Password, collez le jeton d'authentification à partir de votre éditeur de texte ou de l'emplacement où vous l'avez enregistré. !\[Login Registry\](images/login-registry.png ") 3\. Dans l'exercice précédent, vous avez déterminé l'espace de noms de location. Définissez le nom d'utilisateur comme suit : \`NAMESPACE\_OF\_YOUR\_TENANCY\`/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`. Vous devez utiliser l'espace de noms et le nom utilisateur de la location en minuscules lors de la connexion à docker. Remplacez \`NAMESPACE\_OF\_YOUR\_TENANCY\` par l'espace de noms de votre location et \`YOUR\_ORACLE\_CLOUD\_USERNAME\` par votre nom utilisateur de compte Oracle Cloud, puis copiez le nom utilisateur remplacé à partir de votre éditeur de texte et collez-le dans \*Cloud Shell\*. Pour Password, collez le jeton d'authentification à partir de votre éditeur de texte ou de l'emplacement où vous l'avez enregistré.

4.  Revenez à Container Registry, sélectionnez _Menu Hamburger -> Developer Services -> Container Registry_.
    
    ![Registre du conteneur](images/container-registry.png " ")
    
5.  Sélectionnez le compartiment, puis cliquez sur _Créer un référentiel_.
    
    ![Création de référentiel](images/repository-create.png " ")
    
6.  Sélectionnez le compartiment et entrez _`helidon-stock-application-your_first_name`_ comme nom de référentiel, puis choisissez Accès en tant que _public_ et cliquez sur _Créer_.
    
    ![Données du référentiel](images/repository-data.png " ")
    
    Une fois le référentiel _`helidon-stock-application-your_first_name`_ créé, vous pouvez vérifier l'espace de noms et il doit être identique à l'espace de noms de votre location.
    
    !\[Vérifier l'espace de noms\](images/verify-namespace.png")
    
7.  Dans le cadre de l'atelier 5, vous avez copié le nom complet de l'image Docker dans votre éditeur de texte. Pour propager l'image Docker vers le référentiel dans Oracle Cloud Container Registry, copiez et collez la commande suivante dans votre éditeur de texte, puis remplacez _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`helidon-stock-application-your_first_name` :1.0_ par le nom complet de l'image Docker que vous avez enregistré dans votre éditeur de texte.
    
        <copy>docker push `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/helidon-stock-application-your_first_name:1.0</copy>
        
    
    ![Propagation Docker](images/docker-push.png " ")
    
    Une fois la commande _docker push_ exécutée, développez le référentiel _`helidon-stock-application-your_first_name`_ et vous remarquerez qu'une nouvelle image a été téléchargée dans ce référentiel.
    
    Laissez la page du référentiel _Cloud Shell_ et Container Registry ouverte. Nous en aurons besoin pour les prochains exercices.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, mars 2023
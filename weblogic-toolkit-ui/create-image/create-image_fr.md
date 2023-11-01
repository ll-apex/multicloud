# Créer une image auxiliaire et l'envoyer dans Oracle Container Image Registry

## Présentation

**Image principale :** image contenant le logiciel Oracle Fusion Middleware. Il est utilisé comme base de tous les conteneurs qui exécutent les serveurs WebLogic pour le domaine.

**Image auxiliaire :** image qui fournit le logiciel d'outil de déploiement WebLogic et les fichiers de modèle. Lors de l'exécution, le contenu de l'image auxiliaire est fusionné avec le contenu de l'image principale. ![Structure d'image](images/image-structure.png)

Dans cet exercice, nous utilisons l'image 12.2.1.3.0-ol8 du serveur WebLogic en tant qu'image principale. En outre, nous créons une image auxiliaire et la transmettons au référentiel Oracle Container Image Registry à l'aide du jeton d'authentification généré.

Temps estimé : 10 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Création d'images pour OKE sur OCI](videohub:1_y5o56oe5)

### Objectifs

Dans cet exercice, vous allez :

*   Créez une image auxiliaire et propagez-la vers Oracle Cloud Container Image Registry.

## Tâche 1 : préparer l'image auxiliaire et pousser l'image auxiliaire

Cette tâche consiste à créer une image auxiliaire que nous propagerons vers Oracle Cloud Container Registry.

1.  Cliquez sur _Image_. Pour Primary Image, nous allons utiliser la commande _weblogic_ Image.So ci-dessous pour conserver les valeurs par défaut sous la section _Primary Image_, comme indiqué
    
        <copy>container-registry.oracle.com/middleware/weblogic:12.2.1.3-ol8</copy>
        
    
    ![Image principale](images/primary-image.png)
    
    > **Pour information uniquement :**  
    > l'image principale est celle utilisée pour exécuter le domaine. Une image principale peut être réutilisée pour des centaines de domaines. L'image principale contient les installations du système d'exploitation, du JDK et du logiciel FMW.
    
2.  Pour créer la balise d'image auxiliaire, nous avons besoin des informations suivantes :
    
    *   Adresse de la région
    *   Espace de noms de location
3.  Localisez l'_adresse de votre région_. Reportez-vous au tableau documenté à cette URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). Dans l'exemple présenté, l'adresse de la région est _Sud du Royaume-Uni (Londres)_ (en tant que nom de région) et son adresse est _lhr.ocir.io_. Localisez l'adresse de votre propre _nom de région_ et enregistrez-la dans le fichier texte. Vous en aurez également besoin pour le prochain laboratoire.
    
    ![Adresses](images/end-point.png " ")
    
    > Vous disposez désormais de l'espace de noms de location et de l'adresse de votre région.
    
4.  Au cours de l'exercice 3, vous avez déjà noté l'espace de noms de location dans votre fichier texte. Dans le cas contraire, pour rechercher l'espace de noms de la location, sélectionnez _Menu Hamburger_ -> _Services de développeur_ -> _Container Registry_, comme indiqué. Sélectionnez le référentiel que vous avez créé. Vous trouverez l'espace de noms comme indiqué. ![Espace de noms de location](images/tenancy-namespace.png)
    
5.  Vous disposez désormais de l'espace de noms et de l'adresse de location pour votre région. Copiez la commande suivante et collez-la dans votre fichier texte. Remplacez ensuite `END_POINT_OF_YOUR_REGION` par l'adresse du nom de région, `NAMESPACE_OF_YOUR_TENANCY` par l'espace de noms de votre location. Cliquez sur l'onglet _Image auxiliaire_ comme indiqué. ![Onglet Auxiliaire](images/auxiliary-tab.png)
    
        <copy>END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/test-model-your_first_name:v1</copy>
        

> Par exemple, dans mon cas, la balise d'image auxiliaire est `lhr.ocir.io/tenancynamespace/test-model-ankit:v1`.

6.  A l'étape 4, vous avez également déterminé l'espace de noms de location. Entrez le nom utilisateur Push du registre d'images auxiliaires comme suit : `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    

*   Remplacez `NAMESPACE_OF_YOUR_TENANCY` par l'espace de noms de votre location.
*   Remplacez `YOUR_ORACLE_CLOUD_USERNAME` par le nom utilisateur de votre compte Oracle Cloud, puis copiez le nom utilisateur remplacé à partir de votre fichier texte et collez-le dans le _nom utilisateur Push du registre d'images auxiliaires_.

> Par exemple, dans mon cas, le **nom utilisateur Push du registre d'images auxiliaires** est `tenancynamespace/lab.user@oracle.com`.

*   Pour le mot de passe, copiez et collez le jeton d'authentification à partir de votre fichier texte (ou de l'emplacement où vous l'avez enregistré) et collez-le dans le **nom utilisateur Push du registre d'images auxiliaires**. ![Détails de l'image auxiliaire](images/auxiliary-image-details.png)

7.  Cliquez sur _Créer une image auxiliaire_. ![Créer une image auxiliaire](images/create-auxiliary-image.png)
    
8.  Comme nous avons déjà préparé le modèle dans l'atelier 2, cliquez sur _Non_. ![Préparer le modèle](images/prepare-model.png)
    
9.  Sélectionnez le dossier _Téléchargements_ dans lequel enregistrer _WebLogic Deployer_ et cliquez sur _Sélectionner_ comme indiqué. ![Lieu WDT](images/wdt-location.png)
    
10.  Une fois les images auxiliaires créées, dans la fenêtre _Créer une image auxiliaire terminée_, cliquez sur _OK_. ![Auxiliaire créé](images/auxiliary-created.png)
    
    > **Pour information uniquement :**  
    > Une image auxiliaire est propre au domaine. L'image auxiliaire contient les données qui définissent le domaine.
    
11.  Cliquez sur _propager l'image auxiliaire_ pour propager l'image dans le référentiel dans Oracle Cloud Container Image Registry. ![Auxiliaire Push](images/push-auxiliary.png)
    
12.  Une fois l'image propagée, dans la fenêtre _Push Image Complete_, cliquez sur _Ok_. ![Poussée auxiliaire](images/auxiliary-pushed.png)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
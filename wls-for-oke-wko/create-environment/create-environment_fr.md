# Configuration de l'environnement des exercices

## Présentation

Dans cet exercice, vous allez créer un référentiel dans Oracle Cloud Container Image Registry. En outre, vous acceptez le contrat de licence pour les images de serveur WebLogic dans Oracle Container Registry.

Temps estimé : 15 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Créez un référentiel dans Oracle Cloud Container Image Registry.
*   Acceptez la licence pour les images de serveur WebLogic dans Oracle Container Registry.

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.
*   Vous devez avoir un compte Oracle.
*   Vous devez disposer d'un éditeur de texte.

## Tâche 1 : création d'un référentiel

Dans cette tâche, vous créez un référentiel public. Au cours de l'exercice 5, nous propagerons l'image auxiliaire dans ce référentiel.

1.  Dans Bureau à distance, pour ouvrir Firefox, cliquez sur **Activités**, saisissez **Incendie** dans la zone de recherche, puis cliquez sur **Firefox** comme indiqué. ![ouvrir firefox](images/open-firefox.png)
    
2.  Connectez-vous à la [console Oracle Cloud](https://cloud.oracle.com) à l'aide de vos informations d'identification.
    
3.  Dans la console, sélectionnez _Menu Hamburger_ -> _Services de développeur_ -> _Container Registry_ comme indiqué. ![Registre du conteneur](images/container-registry.png)
    
4.  Sélectionnez le compartiment dans lequel vous êtes autorisé à créer le référentiel. Cliquez sur _Créer un référentiel_. ![Créer un référentiel](images/create-repository.png)
    
5.  Entrez _`test-model-your_firstname`_ en tant que nom de référentiel et Access en tant que _public_, puis cliquez sur _Créer_. ![Détails du référentiel](images/repository-details.png)
    
6.  Une fois votre référentiel prêt. Notez l'espace de noms de location dans votre fichier texte. ![Remarque sur la location NameSpace](images/tenancy-namespace.png)
    

## Tâche 2 : acceptation de la licence pour les images de serveur WebLogic

Dans cette tâche, nous acceptons le contrat de licence pour les images de serveur WebLogic résidant dans Oracle Container Registry. Comme dans l'atelier 5, nous utiliserons l'image WebLogic Server 12.2.1.3.0 comme image principale. Pour accéder à WebLogic Server Images, nous acceptons le contrat de licence.

1.  Copiez et collez le lien pour Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) dans le navigateur et connectez-vous. Pour cela, vous avez besoin d'un compte Oracle. ![Connexion au registre de conteneurs](images/container-registry-sign-in.png)
    
2.  Entrez vos _informations d'identification de compte Oracle_ dans les champs Nom utilisateur et Mot de passe, puis cliquez sur _Connexion_. ![Registre du conteneur de connexion](images/login-container-registry.png)
    
3.  Sur la page d'accueil d'Oracle Container Registry, recherchez _weblogic_. ![Rechercher sur WebLogic](images/search-weblogic.png)
    
4.  Cliquez sur _weblogic_ comme indiqué et sélectionnez _Anglais_ comme langue, puis cliquez sur _Continuer_. ![Cliquez sur WebLogic.](images/click-weblogic.png) ![Sélectionner une langue](images/select-language.png)
    
5.  Cliquez sur _Accepter_ pour accepter le contrat de licence. ![Accepter la licence](images/accept-license.png)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, octobre 2023
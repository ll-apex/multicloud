# Configuration de l'environnement des exercices

## Présentation

Dans cet exercice, vous allez créer un cluster Kubernetes à 3 noeuds configuré avec toutes les ressources réseau nécessaires. Vous allez également créer un référentiel dans Oracle Cloud Container Image Registry. Ensuite, vous allez générer un jeton d'authentification. En outre, vous acceptez le contrat de licence pour les images de serveur WebLogic dans Oracle Container Registry.

Temps estimé : 15 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Configuration de l'environnement des exercices](videohub:1_zhvohpqq)

### Objectifs

Dans cet exercice, vous allez :

*   Créez un cluster Oracle Kubernetes.
*   Créez un référentiel dans Oracle Cloud Container Image Registry.
*   Générez un jeton d'authentification.
*   Acceptez la licence pour les images de serveur WebLogic dans Oracle Container Registry.

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.
*   Vous devez avoir un compte Oracle.
*   Vous devez disposer d'un éditeur de texte.

## Tâche 1 : création d'un cluster Oracle Kubernetes

La fonctionnalité de _création rapide_ utilise les paramètres par défaut pour créer un _cluster rapide_ avec des ressources réseau, selon les besoins. Cette approche est le moyen le plus rapide de créer un cluster. Si vous acceptez toutes les valeurs par défaut, vous pouvez créer un cluster en quelques clics. Les ressources réseau du cluster sont créées automatiquement, ainsi qu'un pool de noeuds et trois noeuds de processus actif.

1.  Dans la console, sélectionnez _Menu Hamburger -> Developer Services -> Clusters Kubernetes (OKE)_ comme indiqué.
    
    ![Menu](images/hamburger-menu.png " ")
    
2.  Sur la page Liste des clusters, sélectionnez le compartiment, puis cliquez sur _Créer un cluster_.
    
    > Vous devez sélectionner un compartiment dans lequel vous êtes autorisé à créer un cluster, ainsi qu'un référentiel dans Oracle Container Registry.
    
    ![Sélectionner un compartiment](images/select-compartment.png " ")
    
3.  Dans la boîte de dialogue Créer une solution de cluster, sélectionnez _Création rapide_ et cliquez sur _Soumettre_.
    
    ![Lancer le workflow](images/launch-workflow.png " ")
    
    La _création rapide_ crée un cluster avec les paramètres par défaut, ainsi que les ressources réseau du nouveau cluster.
    
    Indiquez les détails de configuration suivants sur la page Création de cluster (veuillez prêter attention à la valeur que vous placez dans le champ _Forme_) :
    
    *   **Nom :** nom du cluster. Conservez la valeur par défaut.
    *   **Compartiment :** nom du compartiment. Sélectionnez le compartiment dans lequel vous êtes autorisé à créer des ressources.
    *   **Version Kubernetes :** version de Kubernetes. Sélectionnez _1.26.2_ en tant que version Kubernetes.
    *   **Adresse d'API Kubernetes :** les noeuds maître du cluster seront-ils routables ou non ? Sélectionnez la valeur _Adresse publique_.
    *   **Type de noeud :** sélectionnez _Géré_ en tant que type de noeud.
    *   **Noeuds de processus actif Kubernetes :** les noeuds de processus actif de cluster seront-ils routables ou non ? Conservez la valeur par défaut _Travailleurs privés_.
    *   **Forme :** forme à utiliser pour chaque noeud du pool de noeuds. La forme détermine le nombre d'UC et la quantité de mémoire alloués à chaque noeud. La liste affiche uniquement les formes disponibles dans votre location qui sont prises en charge par OKE. Sélectionnez _VM.Standard.E4. Flex_ (généralement disponible dans le compte Oracle Free Tier). Sélectionnez 1 OCPU et 16 Go comme quantité de mémoire.
    *   **Image :** sélectionnez l'image par défaut avec la version _1.26.2_ de Kubernetes.
    *   **Nombre de noeuds :** nombre de noeuds de processus actif à créer. Conservez la valeur par défaut, _3_.
    
    ![Cluster rapide](images/quick-cluster1.png " ") ![Entrer des données](images/enter-data.png " ")
    
4.  Cliquez sur _Suivant_ afin de vérifier les détails saisis pour le nouveau cluster.
    
5.  Sur la page _Vérifier_, cochez la case _Créer un cluster de base_, puis cliquez sur _Créer un cluster_ pour créer les ressources réseau et le cluster.
    
    ![Vérification du cluster](images/review-cluster.png " ")
    
    > Les ressources réseau créées pour vous sont affichées. Attendez que la demande de création du pool de noeuds soit lancée, puis cliquez sur _Fermer_.
    
    ![Ressource réseau](images/network-resource.png " ")
    
    > Le nouveau cluster apparaît ensuite sur la page _Détails du cluster_. Lorsque les noeuds maîtres sont créés, le nouveau cluster obtient le statut _Actif_ (cela prend environ 7 minutes). Vous n'avez pas besoin d'attendre, passez à la tâche suivante.
    
    ![accès au cluster](images/cluster-access.png " ")
    

## Tâche 2 : création d'un référentiel

Dans cette tâche, vous créez un référentiel public. Au cours de l'exercice 5, nous propagerons l'image auxiliaire dans ce référentiel.

1.  Dans la console, sélectionnez _Menu Hamburger_ -> _Services de développeur_ -> _Container Registry_ comme indiqué. ![Registre du conteneur](images/container-registry.png)
    
2.  Sélectionnez le compartiment dans lequel vous êtes autorisé à créer le référentiel. Cliquez sur _Créer un référentiel_. ![Créer un référentiel](images/create-repository.png)
    
3.  Entrez _`test-model-your_firstname`_ en tant que nom de référentiel et Access en tant que _public_, puis cliquez sur _Créer_. ![Détails du référentiel](images/repository-details.png)
    
4.  Une fois votre référentiel prêt. Notez l'espace de noms de location dans votre fichier texte dans l'éditeur de texte. ![Remarque sur la location NameSpace](images/tenancy-namespace.png)
    

## Tâche 3 : générer un jeton d'authentification

Dans cette tâche, nous allons générer un _jeton d'authentification_. Au cours de l'exercice 5, nous utiliserons ce jeton d'authentification pour propager l'image auxiliaire dans le référentiel Oracle Cloud Container Registry.

1.  Sélectionnez l'icône Utilisateur dans l'angle supérieur droit, puis sélectionnez _MyProfile_.
    
    ![Mon profil](images/my-profile.png)
    
2.  Faites défiler la page vers le bas et sélectionnez _Jetons d'authentification_, puis cliquez sur _Générer un jeton_.
    
    ![jeton d'authentification](images/auth-token.png)
    
3.  Copiez _`test-model-your_first_name`_ et collez-le dans la zone _Description_, puis cliquez sur _Générer un jeton_.
    
    ![Créer un jeton](images/create-token.png)
    
4.  Sélectionnez _Copier_ sous Jeton généré et collez-le dans l'éditeur de texte. Nous ne pourrons pas le copier plus tard. Cliquez sur _Fermer_.
    
    ![Copier le jeton](images/copy-token.png)
    

## Tâche 4 : acceptation de la licence pour les images de serveur WebLogic

Dans cette tâche, nous acceptons le contrat de licence pour les images de serveur WebLogic résidant dans Oracle Container Registry. Comme dans l'atelier 3, nous utiliserons l'image WebLogic Server 12.2.1.3.0 comme image principale. Pour accéder à WebLogic Server Images, nous acceptons le contrat de licence.

1.  Cliquez sur le lien correspondant à Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) et connectez-vous. Pour cela, vous avez besoin d'un compte Oracle. ![Connexion au registre de conteneurs](images/container-registry-sign-in.png)
    
2.  Entrez vos _informations d'identification de compte Oracle_ dans les champs Nom utilisateur et Mot de passe, puis cliquez sur _Connexion_. ![Registre du conteneur de connexion](images/login-container-registry.png)
    
3.  Sur la page d'accueil d'Oracle Container Registry, recherchez _weblogic_. ![Rechercher sur WebLogic](images/search-weblogic.png)
    
4.  Cliquez sur _weblogic_ comme indiqué et sélectionnez _Anglais_ comme langue, puis cliquez sur _Continuer_. ![Cliquez sur WebLogic.](images/click-weblogic.png) ![Sélectionner une langue](images/select-language.png)
    
5.  Cliquez sur _Accepter_ pour accepter le contrat de licence. ![Accepter la licence](images/accept-license.png)
    

## En savoir plus

_A propos d'Oracle Cloud Infrastructure Container Engine for Kubernetes_

Oracle Cloud Infrastructure Container Engine for Kubernetes est un service entièrement géré, évolutif et hautement disponible que vous pouvez utiliser pour déployer vos applications de conteneur vers le cloud. Utilisez Container Engine for Kubernetes (parfois abrégé OKE) lorsque votre équipe de développement souhaite créer, déployer et gérer des applications cloud natives de manière fiable. Vous pouvez indiquer les ressources de calcul requises par vos applications et OKE les provisionne sur Oracle Cloud Infrastructure dans une location OCI existante.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
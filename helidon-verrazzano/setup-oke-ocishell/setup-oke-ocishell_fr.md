# Configurer une instance Oracle Kubernetes Engine sur Oracle Cloud Infrastructure

## Présentation

Dans cet exercice, vous allez créer un cluster Kubernetes à 3 noeuds configuré avec toutes les ressources réseau nécessaires. Les noeuds seront déployés dans différents domaines de pannes pour assurer une haute disponibilité.

Pour plus d'informations sur OKE et le déploiement de cluster personnalisé, reportez-vous à la documentation [Oracle Kubernetes Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm).

Temps estimé : 15 minutes

### A propos du produit/technologie

Oracle Cloud Infrastructure Container Engine for Kubernetes est un service entièrement géré, évolutif et hautement disponible que vous pouvez utiliser pour déployer vos applications de conteneur vers le cloud. Utilisez Container Engine for Kubernetes (parfois abrégé OKE) lorsque votre équipe de développement souhaite créer, déployer et gérer des applications cloud natives de manière fiable. Vous pouvez indiquer les ressources de calcul requises par vos applications et OKE les provisionne sur Oracle Cloud Infrastructure dans une location OCI existante.

### Objectifs

Vous allez utiliser la fonctionnalité de cluster _Création rapide_ pour créer une instance OKE (Oracle Kubernetes Engine) avec les ressources réseau requises, un pool de noeuds et trois noeuds de processus actif. L'approche _Création rapide_ est le moyen le plus rapide de créer un cluster. Si vous acceptez toutes les valeurs par défaut, vous pouvez créer un cluster en quelques clics.

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.

## Tâche 1 : créer un cluster OKE

La fonctionnalité de _création rapide_ utilise les paramètres par défaut pour créer un _cluster rapide_ avec des ressources réseau, selon les besoins. Cette approche est le moyen le plus rapide de créer un cluster. Si vous acceptez toutes les valeurs par défaut, vous pouvez créer un cluster en quelques clics. Les ressources réseau du cluster sont créées automatiquement, ainsi qu'un pool de noeuds et trois noeuds de processus actif.

1.  Dans la console, sélectionnez _Menu Hamburger -> Developer Services -> Clusters Kubernetes (OKE)_ comme indiqué.
    
    ![Menu](images/hamburger-menu.png " ")
    
2.  Sur la page Liste des clusters, sélectionnez le compartiment de votre choix, dans lequel vous êtes autorisé à créer un cluster, puis cliquez sur _Créer un cluster_.
    
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
    *   **Forme :** forme à utiliser pour chaque noeud du pool de noeuds. La forme détermine le nombre d'UC et la quantité de mémoire alloués à chaque noeud. La liste affiche uniquement les formes disponibles dans votre location qui sont prises en charge par OKE. Sélectionnez _VM.Standard.E4. Flex_ (généralement disponible dans le compte Oracle Free Tier). Sélectionnez 2 OCPU et 32 Go comme quantité de mémoire.
    *   **Image :** sélectionnez l'image par défaut avec la version _1.26.2_ de Kubernetes.
    *   **Nombre de noeuds :** nombre de noeuds de processus actif à créer. Conservez la valeur par défaut, _3_.
    
    > _SOYEZ TRÈS PRUDENT POUR NE PAS QUITTER LA FORME PAR DÉFAUT ; LA FORME PAR DÉFAUT EST TROP PETITE POUR S'ADAPTER À TOUS LES COMPOSANTS VERRAZZANO_
    
    ![Cluster rapide](images/quick-cluster.png " ") ![Numéro de noeud](images/node-number.png " ")
    
4.  Cliquez sur _Suivant_ afin de vérifier les détails saisis pour le nouveau cluster.
    
5.  Sur la page _Vérifier_, cochez la case _Créer un cluster de base_, puis cliquez sur _Créer un cluster_ pour créer les ressources réseau et le cluster.
    
    ![Vérification du cluster](images/review-cluster.png " ")
    
    > Les ressources réseau créées pour vous sont affichées. Attendez que la demande de création du pool de noeuds soit lancée, puis cliquez sur _Fermer_.
    
    ![Ressource réseau](images/network-resource.png " ")
    
    > Le nouveau cluster apparaît ensuite sur la page _Détails du cluster_. Lorsque les noeuds maîtres sont créés, le nouveau cluster obtient le statut _Actif_ (cela prend environ 7 minutes). Ensuite, vous pouvez continuer vos laboratoires.
    
    ![provisionnement de cluster](images/cluster-provision.png " ")
    
    ![accès au cluster](images/cluster-access.png " ")
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
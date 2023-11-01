# Configurer une instance Oracle Kubernetes Engine sur Oracle Cloud Infrastructure

## Présentation

Cet atelier vous explique les étapes de création d'un environnement Kubernetes géré sur Oracle Cloud Infrastructure.

Durée estimée : 15 minutes

### A propos du produit/technologie

Oracle Cloud Infrastructure Container Engine for Kubernetes est un service entièrement géré, évolutif et hautement disponible que vous pouvez utiliser pour déployer vos applications de conteneur vers le cloud. Utilisez Container Engine for Kubernetes (parfois abrégé OKE) lorsque votre équipe de développement souhaite créer, déployer et gérer des applications cloud natives de manière fiable. Vous pouvez indiquer les ressources de calcul requises par vos applications et OKE les provisionne sur Oracle Cloud Infrastructure dans une location OCI existante.

### Objectifs

Dans cet exercice, vous allez :

*   Créez une instance OKE (Oracle Kubernetes Engine).
*   Ouvrez OCI Cloud Shell et configurez `kubectl` pour interagir avec le cluster Kubernetes.

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.

Pour créer Container Engine for Kubernetes (OKE), procédez comme suit :

*   Créez les ressources réseau (VCN, sous-réseaux, listes de sécurité, etc.).
*   Créez un cluster.
*   Créez un élément `NodePool`.

Cet atelier explique comment la fonctionnalité de _démarrage rapide_ crée et configure toutes les ressources nécessaires pour un cluster Kubernetes à 3 noeuds. Tous les noeuds seront déployés dans différents domaines de disponibilité pour assurer une haute disponibilité.

Pour plus d'informations sur OKE et le déploiement de cluster personnalisé, reportez-vous à la documentation [Oracle Container Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm).

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
    

## Tâche 2 : configurer `kubectl` (CLI de cluster Kubernetes)

Oracle Cloud Infrastructure (OCI) Cloud Shell est un terminal basé sur un navigateur Web, accessible à partir de la console Oracle Cloud. Cloud Shell fournit un accès à un shell Linux, avec une CLI Oracle Cloud Infrastructure préauthentifiée et d'autres outils utiles (_Git, kubectl, helm, OCI CLI_) pour suivre les tutoriels Verrazzano. Cloud Shell est accessible à partir de la console. Cloud Shell apparaît dans la console Oracle Cloud en tant que cadre persistant de la console et reste actif lorsque vous accédez aux différentes pages de la console.

Vous utiliserez _Cloud Shell_ pour terminer cet atelier.

Nous allons utiliser `kubectl` pour gérer le cluster à distance à l'aide de Cloud Shell. Il a besoin d'un fichier `kubeconfig`. Cette opération sera générée à l'aide de l'interface de ligne de commande OCI pré-authentifiée. Vous n'avez donc aucune configuration à effectuer avant de pouvoir commencer à l'utiliser.

1.  Cliquez sur _Accéder au cluster_ sur la page de détails du cluster.
    
    > Si vous avez quitté cette page, ouvrez le menu de navigation et, sous _Services de développeur_, sélectionnez _Clusters Kubernetes (OKE)_. Sélectionnez votre cluster et accédez à la page de détails.
    
    ![Accéder au cluster](images/access-cluster.png " ")
    
    > Une boîte de dialogue apparaît à partir de laquelle vous pouvez ouvrir Cloud Shell et contient la commande OCI personnalisée que vous devez exécuter pour créer un fichier de configuration Kubernetes.
    
2.  Laissez l'_accès Cloud Shell_ par défaut et sélectionnez d'abord le lien _Copier_ pour copier la commande `oci ce...` dans Cloud Shell.
    
    ![Copier la configuration kubectl](images/copy-config.png " ")
    
3.  Cliquez maintenant sur _Lancer Cloud Shell_ pour ouvrir la console intégrée. Fermez ensuite la boîte de dialogue de configuration avant de coller la commande dans _Cloud Shell_.
    
    ![Lancez Cloud Shell.](images/launch-cloudshell.png " ")
    
4.  Copiez la commande à partir du presse-papiers (Ctrl+V ou cliquez avec le bouton droit de la souris et copiez-la) dans Cloud Shell et exécutez la commande.
    
    Par exemple, la commande se présente comme suit :
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![Configuration kubectl](images/kube-config.png " ")
    
5.  Vérifiez maintenant que `kubectl` fonctionne, par exemple à l'aide de la commande `get node`. Vous devrez peut-être exécuter cette commande plusieurs fois jusqu'à ce que la sortie soit similaire à la suivante.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.192   Ready    node    11m   v1.26.2
        10.0.10.219   Ready    node    11m   v1.26.2
        10.0.10.90    Ready    node    11m   v1.26.2
        
    
    > Si vous voyez les informations du noeud, la configuration a réussi.
    
6.  Vous pouvez réduire et restaurer la taille du terminal à tout moment à l'aide des contrôles situés dans l'angle supérieur droit de Cloud Shell.
    
    ![cloud shell ](images/cloudshell.png " ")
    

Laissez _Cloud Shell_ ouvert. Nous l'utiliserons pour d'autres exercices.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
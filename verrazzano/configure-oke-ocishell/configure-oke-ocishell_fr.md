# Configurer KUBECTL pour interagir avec Oracle Container Engine for Kubernetes (OKE) sur Oracle Cloud Infrastructure (OCI)

## Présentation

Cet atelier vous explique les étapes de création d'un fichier de configuration, qui permettent d'accéder à l'environnement Kubernetes sur Oracle Cloud Infrastructure.

### A propos du produit/technologie

Oracle Cloud Infrastructure Container Engine for Kubernetes est un service entièrement géré, évolutif et hautement disponible que vous pouvez utiliser pour déployer vos applications de conteneur vers le cloud. Utilisez Container Engine for Kubernetes (parfois abrégé OKE) lorsque votre équipe de développement souhaite créer, déployer et gérer des applications cloud natives de manière fiable. Vous pouvez indiquer les ressources de calcul requises par vos applications et OKE les provisionne sur Oracle Cloud Infrastructure dans une location OCI existante.

### Objectifs

Dans cet exercice, vous allez :

*   Ouvrez OCI Cloud Shell et configurez `kubectl` pour interagir avec le cluster Kubernetes.

### Prérequis

*   Cet atelier suppose que vous avez réservé cet atelier sur LiveLabs.

## Tâche 1 : configurer `kubectl` (CLI de cluster Kubernetes)

Oracle Cloud Infrastructure (OCI) Cloud Shell est un terminal basé sur un navigateur Web, accessible à partir de la console Oracle Cloud. Cloud Shell fournit un accès à un shell Linux, avec une CLI Oracle Cloud Infrastructure préauthentifiée et d'autres outils utiles (_Git, kubectl, helm, OCI CLI_) pour suivre les tutoriels Verrazzano. Cloud Shell est accessible à partir de la console. Cloud Shell apparaît dans la console Oracle Cloud en tant que cadre persistant de la console et reste actif lorsque vous accédez aux différentes pages de la console.

Vous utiliserez _Cloud Shell_ pour terminer cet atelier.

Nous allons utiliser `kubectl` pour gérer le cluster à distance à l'aide de Cloud Shell. Il a besoin d'un fichier `kubeconfig`. Cette opération sera générée à l'aide de l'interface de ligne de commande OCI pré-authentifiée. Vous n'avez donc aucune configuration à effectuer avant de pouvoir commencer à l'utiliser.

1.  Dans la console, sélectionnez _Menu Hamburger -> Developer Services -> Clusters Kubernetes (OKE)_ comme indiqué.
    
    ![Menu](../setup-oke-ocishell/images/hamburgermenu.png " ")
    
2.  Sélectionnez le compartiment qui vous est associé. Cliquez ensuite sur le cluster _cluster1_.
    
3.  Cliquez sur _Accéder au cluster_ sur la page de détails du cluster.
    
    ![Accéder au cluster](../setup-oke-ocishell/images/accesscluster.png " ")
    
    > Une boîte de dialogue apparaît à partir de laquelle vous pouvez ouvrir Cloud Shell et contient la commande OCI personnalisée que vous devez exécuter pour créer un fichier de configuration Kubernetes.
    
4.  Laissez l'_accès Cloud Shell_ par défaut et sélectionnez d'abord le lien _Copier_ pour copier la commande `oci ce...` dans Cloud Shell.
    
    ![Copier la configuration kubectl](../setup-oke-ocishell/images/copyconfig.png " ")
    
5.  Cliquez maintenant sur _Lancer Cloud Shell_ pour ouvrir la console intégrée. Fermez ensuite la boîte de dialogue de configuration avant de coller la commande dans _Cloud Shell_.
    
    ![Lancez Cloud Shell.](../setup-oke-ocishell/images/launchcloudshell.png " ")
    
6.  Copiez la commande à partir du presse-papiers (Ctrl+V ou cliquez avec le bouton droit de la souris et copiez-la) dans Cloud Shell et exécutez la commande.
    
    Par exemple, la commande se présente comme suit :
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![Configuration kubectl](../setup-oke-ocishell/images/kubeconfig.png " ")
    
7.  Vérifiez maintenant que `kubectl` fonctionne, par exemple à l'aide de la commande `get node`. Vous devrez peut-être exécuter cette commande plusieurs fois jusqu'à ce que la sortie soit similaire à la suivante.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.17    Ready    node    11m   v1.21.5
        10.0.10.184   Ready    node    11m   v1.21.5
        10.0.10.31    Ready    node    11m   v1.21.5
        
    
    > Si vous voyez les informations du noeud, la configuration a réussi.
    
8.  Vous pouvez réduire et restaurer la taille du terminal à tout moment à l'aide des contrôles situés dans l'angle supérieur droit de Cloud Shell.
    
    ![cloud shell ](../setup-oke-ocishell/images/cloudshell.png " ")
    

Laissez _Cloud Shell_ ouvert. Nous l'utiliserons pour d'autres exercices.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, février 2023
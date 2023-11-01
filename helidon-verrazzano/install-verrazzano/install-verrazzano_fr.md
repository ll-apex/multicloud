# Installer Verrazzano

## Présentation

Cet atelier vous guide tout au long des étapes d'installation de Verrazzano sur un cluster Kubernetes dans Oracle Cloud Infrastructure.

Temps estimé : 15 minutes

### A propos du produit/technologie

Verrazzano est une plate-forme de conteneur d'entreprise de bout en bout pour le déploiement d'applications cloud natives et traditionnelles dans des environnements multicloud et hybrides. Il est composé d'un ensemble de composants open source sélectionnés - beaucoup que vous pouvez déjà utiliser et faire confiance, et certains qui ont été écrits spécifiquement pour rassembler toutes les pièces qui font de Verrazzano une plate-forme cohérente et facile à utiliser.

Verrazzano inclut les fonctionnalités suivantes :

*   Gestion de la charge de travail hybride et multicluster
*   Gestion spéciale pour les applications WebLogic, Coherence et Helidon
*   Gestion d'infrastructure multicluster
*   Surveillance intégrée et pré-câblée des applications
*   Sécurité intégrée
*   Activation de DevOps et GitOps

Verrazzano prend en charge les profils d'installation suivants : développement (`dev`), production (`prod`) et cluster géré (`managed-cluster`).

L'image suivante décrit les profils d'installation de Verrazzano. ![Installer le profil](images/installprofile.png)

Pour modifier les profils dans l'une des commandes suivantes, définissez la variable d'environnement _VZ\_PROFILE_ sur le nom du profil à installer.

Pour obtenir une description complète des options de configuration de Verrazzano, reportez-vous à la [définition de ressource personnalisée Verrazzano](https://verrazzano.io/docs/reference/api/verrazzano/verrazzano/).

Dans cet atelier, nous allons installer le _profil de développement de Verrazzano_, qui présente les caractéristiques suivantes :

*   DNS générique (nip.io)
*   Certificats auto-signés
*   Pile d'observabilité partagée utilisée par les composants système et toutes les applications
*   Stockage éphémère pour la pile d'observabilité (si les pods sont redémarrés, vous perdez tous vos journaux et mesures)
*   Il a une installation légère.
*   C'est à des fins d'évaluation.
*   Topologie de cluster Opensearch à noeud unique.

Verrazzano installe un ensemble de composants open source sélectionnés. Le tableau suivant répertorie chaque composant, sa version et une brève description.

![Profil Verrazzano](images/verrazzano-profile.png " ") ![Profils Verrazzano](images/verrazzano-profiles.png " ")

Selon notre choix de DNS, nous pouvons utiliser nip.io (DNS générique) ou [Oracle OCI DNS](https://docs.cloud.oracle.com/en-us/iaas/Content/DNS/Concepts/dnszonemanagement.htm). Dans cet atelier, nous allons procéder à l'installation à l'aide de nip.io (DNS générique).

Un contrôleur d'entrée permet de fournir un accès aux conteneurs Docker au monde extérieur (en fournissant une adresse IP). L'entrée achemine l'adresse IP vers différents clusters.

### Objectifs

Dans cet exercice, vous allez :

*   Configurer `kubectl` pour utiliser le cluster Oracle Kubernetes Engine
*   Installez la CLI Verrazzano vz.
*   Installez le profil de développement (`dev`) de Verrazzano.

### Prérequis

Verrazzano requiert les éléments suivants :

*   Cluster Kubernetes et `kubectl` compatible.
*   Au moins 2 UC, 100 Go de stockage sur disque et 16 Go de RAM disponibles sur les noeuds de processus actif Kubernetes. Cela suffit pour installer le profil de développement de Verrazzano. Selon les besoins en ressources des applications que vous déployez, cela peut ou non suffire au déploiement de vos applications.
*   Au cours de l'atelier 1, vous avez créé un cluster Kubernetes sur Oracle Cloud Infrastructure. Vous utiliserez ce cluster Kubernetes, _cluster1_, pour installer le profil de développement de Verrazzano.

## Tâche 1 : configurer `kubectl` (CLI de cluster Kubernetes)

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
        10.0.10.101   Ready    node    11m   v1.26.2
        10.0.10.29    Ready    node    11m   v1.26.2
        10.0.10.37    Ready    node    11m   v1.26.2
        
    
    > Si vous voyez les informations du noeud, la configuration a réussi.
    

## Tâche 2 : installation de la CLI Verrazzano

1.  Téléchargez la dernière interface de ligne de commande vz.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz</copy>
        
    
    La sortie doit ressembler à ceci :
    
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
        

0 0 0 0 0 0 0 0 -- :-- :-- :-- :-- -- :-- :-- :-- :-- 0 100 39.7M 100 39.7M 0 0 23.6M 0 0:00:01 0:00:01 -- :-- :-- 32.7M \`\`\`

2.  Téléchargez le fichier de somme de contrôle.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        

La sortie doit ressembler à ceci :

    ```bash
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    

0 0 0 0 0 0 0 0 -- :-- :-- :-- :-- -- :-- :-- :-- :-- 0 100 102 100 102 0 0 218 0 -- :-- :-- :-- :-- :-- -- :-- :-- :-- :-- 218 \`\`

3.  Validez le fichier binaire par rapport au fichier de somme de contrôle.
    
        <copy>sha256sum -c verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        
    
    La sortie doit ressembler à ceci :
    
        verrazzano-1.6.2-linux-amd64.tar.gz: OK
        
4.  Déballage et déplacement vers le répertoire binaire vz,
    
        <copy>tar xvf verrazzano-1.6.2-linux-amd64.tar.gz
        cd ~/verrazzano-1.6.2/bin/</copy>
        
5.  Testez pour vous assurer que la version que vous avez installée est à jour.
    
        <copy>./vz version</copy>
        
    
    La sortie doit ressembler à ceci :
    
        Version: v1.6.2
        BuildDate: 2023-07-21T12:06:02Z
        GitCommit: 5cd8a2f8670000d2ef0b02404e3169699d87f26b
        

## Tâche 3 : Installation du profil de développement Verrazzano

Un profil d'installation est une configuration bien connue des paramètres de Verrazzano qui peut être référencée par nom, qui peut ensuite être personnalisée si nécessaire.

Verrazzano prend en charge les profils d'installation suivants : développement (`dev`), production (`prod`) et cluster géré (`managed-cluster`).

1.  Installation à l'aide de la méthode DNS nip.io. Copiez la commande suivante et collez-la dans _Cloud Shell_ pour installer Verrazzano.
    
        <copy>./vz install -f - <<EOF
        apiVersion: install.verrazzano.io/v1beta1
        kind: Verrazzano
        metadata:
          name: example-verrazzano
        spec:
          profile: dev
        EOF
        </copy>
        
    
    La sortie doit ressembler à ceci :
    
        Installing Verrazzano version v1.6.2
        Applying the file https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-platform-operator.yaml
        customresourcedefinition.apiextensions.k8s.io/verrazzanos.install.verrazzano.io created
        namespace/verrazzano-install created
        serviceaccount/verrazzano-platform-operator created
        clusterrole.rbac.authorization.k8s.io/verrazzano-managed-cluster created
        clusterrolebinding.rbac.authorization.k8s.io/verrazzano-platform-operator created
        service/verrazzano-platform-operator created
        service/verrazzano-platform-operator-webhook created
        deployment.apps/verrazzano-platform-operator created
        deployment.apps/verrazzano-platform-operator-webhook created
        mutatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-mysql-backup created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-operator-webhook created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-mysqlinstalloverrides created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-requirements-validator created
        Waiting for verrazzano-platform-operator to be ready before starting install - 23 seconds
        
    
    > Il faut environ 15 à 20 minutes pour terminer l'installation. Cette commande installe l'opérateur de plate-forme Verrazzano et applique la ressource personnalisée Verrazzano. Les journaux d'installation sont transmis à la fenêtre de commande jusqu'à la fin de l'installation ou jusqu'à ce que le délai d'expiration par défaut (30 m) soit atteint.
    
2.  Laissez _Cloud Shell_ ouvert et laissez l'installation s'exécuter.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
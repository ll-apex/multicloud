# Déployer l'opérateur WebLogic sur le cluster Oracle Kubernetes (OKE)

## Présentation

Dans cet exercice, nous authentifions l'interface de ligne de commande OCI à l'aide du navigateur, qui crée le fichier _.OCI/config_. Comme nous allons utiliser kubectl pour gérer le cluster à distance à l'aide de l'_accès local_. Il a besoin d'un fichier _kubeconfig_. Ce fichier kubeconfig sera généré à l'aide de l'interface de ligne de commande OCI. Nous vérifions ensuite la connectivité au cluster Kubernetes à partir de l'interface utilisateur WebLogic Kubernetes Toolkit. Enfin, nous installons l'opérateur Kubernetes WebLogic sur le cluster Kubernetes (OKE).

Temps estimé : 10 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Déploiement de l'opérateur WebLogic vers le cluster OKE](videohub:1_0itbllhe)

### Objectifs

Dans cet exercice, vous allez :

*   Configurez kubectl (CLI de cluster Kubernetes) pour vous connecter au cluster Kubernetes.
*   Vérifiez la connectivité de l'interface utilisateur WebLogic Kubernetes Toolkit au cluster Kubernetes.
*   Installez l'opérateur Kubernetes WebLogic sur le cluster Kubernetes.

## Tâche 1 : configurer kubectl (CLI de cluster Kubernetes) pour la connexion au cluster Kubernetes Oracle

Dans cette tâche, nous créons le fichier de configuration _.oci/config_ et _.kube/config_ dans le répertoire _/home/opc_. Ce fichier de configuration nous permet d'accéder au cluster Kubernetes Oracle (OKE) à partir de cette machine virtuelle.

1.  Cliquez sur _Activités_ et saisissez _Firefox_ dans la zone de recherche. Cliquez sur l'icône de _Firefox_. ![ouvrir firefox](images/open-firefox.png)
    
2.  Ouvrez l'URL [https://cloud.oracle.com](https://cloud.oracle.com). Entrez votre _nom de compte cloud_, puis vos informations d'identification pour le compte Oracle Cloud et cliquez sur _Connexion_.
    
3.  Dans la console, sélectionnez _Menu Hamburger_ -> _Services de développeur_ -> _Clusters Kubernetes (OKE)_ comme indiqué. ![Icône OKE](images/oke-icon.png)
    
4.  Cliquez sur le nom du cluster créé à l'exercice 3, puis sur _Accéder au cluster_. ![Accéder au cluster](images/access-cluster.png)
    
5.  Sélectionnez _Accès local_, puis cliquez sur _Copier_ comme indiqué. ![Accès local](images/local-access.png)
    
6.  Cliquez sur _Activités_ et sélectionnez le _Terminal_. ![Terminal](images/click-terminal.png)
    
7.  Collez la commande copiée dans le terminal. Pour _Do you want to create a new config file ?_, tapez _y_, puis appuyez sur _Entrée_. Pour _Voulez-vous créer votre fichier de configuration en vous connectant via un navigateur ?_, tapez _y_, puis appuyez sur _Entrée_. ![Configuration OCI](images/oci-config.png)
    
8.  Dans le navigateur Firefox, cliquez sur votre session active.
    
    > Le message _Autorisation terminée_ s'affiche, comme indiqué. ![Autorisation terminée](images/authorization-complete.png)
    
9.  Dans _Entrer une phrase de passe pour votre clé privée_, laissez-la vide et appuyez sur _Entrée_. ![Phrase de passe vide](images/empty-passphrase.png)
    
10.  Utilisez la flèche supérieure pour exécuter à nouveau la commande _oce ce ..._ et réexécutez-la plusieurs fois, jusqu'à ce que vous voyiez la _Nouvelle configuration écrite dans le fichier Kubeconfig /home/opc/.kube/config_. ![Créer KubeConfig](images/create-kubeconfig.png)
    

## Tâche 2 : vérification de la connectivité de l'interface utilisateur du kit d'outils Kubernetes WebLogic au cluster Kubernetes Oracle

Dans cette tâche, nous vérifions la connectivité à _Oracle Kubernetes Cluster(OKE)_ à partir de l'application `WebLogic Kubernetes Toolkit UI`.

1.  Revenez à l'interface utilisateur du kit d'outils Kubernetes WebLogic, cliquez sur _Activités_ et sélectionnez la fenêtre d'interface utilisateur du kit d'outils Kubernetes WebLogic.
    
2.  Cliquez sur _Kubernetes_ -> _Configuration client_, puis sur _Vérifier la connectivité_. ![Vérifier la connexion](images/verify-connectivity.png)
    
3.  Une fois que la fenêtre _Vérifier la réussite de la connectivité du client Kubernetes_ apparaît, cliquez sur _OK_. ![Connexion réussie](images/successfully-connected.png)
    

## Tâche 3 : installation de l'opérateur Kubernetes WebLogic sur le cluster Kubernetes Oracle

Cette section prend en charge l'installation de l'opérateur Kubernetes WebLogic (l'"opérateur") dans le cluster Kubernetes cible.

1.  Cliquez sur _WebLogic Operator_. Indiquez les détails de configuration suivants et cliquez sur _Opérateur d'installation_.
    
    **Espace de noms Kubernetes :** espace de noms Kubernetes sur lequel installer l'opérateur. Conservez la valeur par défaut.  
    **Compte de service Kubernetes** - Compte de service Kubernetes que l'opérateur doit utiliser lors des demandes d'API Kubernetes. Conservez la valeur par défaut.  
    **Nom de version Helm à utiliser pour l'installation d'opérateur** - Nom de version Helm à utiliser pour identifier cette installation. Conservez la valeur par défaut.  
    
    ![WebLogic Opérateur](images/weblogic-operator.png)
    
    > **Pour information uniquement :**  
    > ! Par défaut, le champ _Balise d'image à utiliser_ de l'opérateur est défini sur la balise d'image correspondant à la dernière version de l'opérateur dans GitHub Container Registry.  
    > L'opérateur doit connaître les domaines WebLogic du cluster Kubernetes qu'il gérera. Il effectue cette opération au niveau de l'espace de noms Kubernetes, de sorte que tout domaine WebLogic dans un espace de noms Kubernetes que l'opérateur est configuré pour gérer sera géré par l'instance d'opérateur en cours d'installation.  
    > Pour le champ _Stratégie de sélection d'espace de noms Kubernetes_, nous sélectionnons _Sélecteur de libellé_, ce qui signifie que tout espace de noms Kubernetes avec un libellé _weblogic-operator=enabled_ sera géré par cet opérateur.  
    > ![Image de l'opérateur](images/operator-image.png)
    
    > En activant l'option Activer la liaison de rôle de cluster, l'installation de l'opérateur crée un Kubernetes ClusterRole et un ClusterRoleBinding que l'opérateur utilisera pour tous les espaces de noms gérés.  
    > Par défaut, l'API REST de l'opérateur n'est pas exposée en dehors du cluster Kubernetes. Pour permettre l'affichage de l'API REST, vous pouvez activer _Exposer l'API REST en externe_. [Liaison de rôle](images/role-binding.png)  
    
    > Ce panneau vous permet de remplacer la configuration de journalisation Java de l'opérateur, ce qui peut être utile lors du débogage de problèmes avec l'opérateur.  
    > ![Journalisation Java](images/java-logging.png)  
    
    > Pour plus d'informations sur _WebLogic Kubernetes Operator Image_, _Kubernetes Namespace Selection Strategy_, _WebLogic Kubernetes Role Bindings_, _External REST API Access_, _Third Party Integrations_ et _Java Logging_, reportez-vous à la documentation [WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/).
    
2.  Une fois l'installation de l'opérateur Kubernetes _WebLogic terminée_, cliquez sur _OK_. ![Opérateur installé](images/operator-installed.png)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
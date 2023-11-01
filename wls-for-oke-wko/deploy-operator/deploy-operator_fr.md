# Déployer l'opérateur WebLogic sur le cluster Oracle Kubernetes (OKE)

## Présentation

Dans cet exercice, nous authentifions l'interface de ligne de commande OCI à l'aide du navigateur, qui crée le fichier _.OCI/config_. Comme nous allons utiliser kubectl pour gérer le cluster à distance à l'aide de l'_accès local_. Il a besoin d'un fichier _kubeconfig_. Ce fichier kubeconfig sera généré à l'aide de l'interface de ligne de commande OCI. Nous vérifions ensuite la connectivité au cluster Kubernetes à partir de l'interface utilisateur WebLogic Kubernetes Toolkit. Enfin, nous installons l'opérateur Kubernetes WebLogic sur le cluster Kubernetes (OKE).

Temps estimé : 10 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Configurez kubectl (CLI de cluster Kubernetes) pour vous connecter au cluster Kubernetes.
*   Vérifiez la connectivité de l'interface utilisateur WebLogic Kubernetes Toolkit au cluster Kubernetes.
*   Installez l'opérateur Kubernetes WebLogic sur le cluster Kubernetes.

## Tâche 1 : configurer kubectl (CLI de cluster Kubernetes) pour la connexion au cluster Kubernetes Oracle

Dans cette tâche, nous créons le fichier de configuration _.oci/config_ et _.kube/config_ dans le répertoire _/home/opc_. Ce fichier de configuration nous permet d'accéder au cluster Kubernetes Oracle (OKE) à partir de cette machine virtuelle.

1.  Dans Firefox, ouvrez la console cloud, sélectionnez **Menu Hamburger** -> **Services de développeur** -> **Clusters Kubernetes (OKE)** comme indiqué. ![Icône OKE](images/oke-icon.png)
    
2.  Cliquez sur le nom du cluster créé à l'exercice 3, puis sur **Accéder au cluster**. ![Accéder au cluster](images/access-cluster.png)
    
3.  Sélectionnez **Accès local**, puis cliquez sur **Copier** comme indiqué. ![Accès local](images/local-access.png)
    
4.  Cliquez sur **Activités** et sélectionnez le **Terminal**. ![Terminal](images/click-terminal.png)
    
5.  Collez la commande copiée dans le terminal. Pour **Do you want to create a new config file ?**, tapez **y**, puis appuyez sur **Entrée**. Pour **Voulez-vous créer votre fichier de configuration en vous connectant via un navigateur ?**, tapez **y**, puis appuyez sur **Entrée**. ![Configuration OCI](images/oci-config.png)
    
6.  Dans le navigateur Firefox, cliquez sur votre session active.
    
    > Le message _Autorisation terminée_ s'affiche, comme indiqué. ![Autorisation terminée](images/authorization-complete.png)
    
7.  Dans **Entrer une phrase de passe pour votre clé privée**, laissez-la vide et appuyez sur **Entrée**. ![Phrase de passe vide](images/empty-passphrase.png)
    
8.  Utilisez la flèche supérieure pour exécuter à nouveau la commande **oce ce ...** et réexécutez-la plusieurs fois, jusqu'à ce que vous voyiez la **Nouvelle configuration écrite dans le fichier Kubeconfig /home/opc/.kube/config**. ![Créer KubeConfig](images/create-kubeconfig.png)
    
9.  Copiez et collez la commande suivante pour modifier les droits d'accès au fichier **~/.kube/config**.
    
        <copy>chmod 600 ~/.kube/config</copy>
        

## Tâche 2 : afficher les ressources cloud d'Oracle WebLogic Server pour la pile OKE

1.  Dans la console OCI, cliquez sur le **menu de navigation** et sélectionnez **Services de développeur**. Sous le groupe Resource Manager, cliquez sur **Piles**.
    
2.  Sélectionnez le **compartiment** qui contient la pile.
    
3.  Cliquez sur le nom de la pile.
    
4.  Accédez à l'onglet **Informations sur l'application**. Vous pouvez visualiser les ressources créées par votre pile. Copiez l'**adresse IP publique de l'instance de bastion** et collez-la dans un fichier texte. ![copier le bastion](images/copy-bastion.png)
    

## Tâche 3 : activer la configuration proxy dans le terminal

Dans cette tâche, nous activons la **mise en tunnel SSH** du bureau distant vers les noeuds de cluster Oracle Kubernetes.

1.  Ouvrez le terminal, copiez et collez la commande suivante pour créer le fichier de clés privées dans le bureau distant.
    
        <copy>vi ~/.ssh/opc.key</copy>
        
2.  Appuyez sur **i** pour passer en mode d'insertion, collez le contenu de la clé privée dans ce fichier et appuyez sur Echap, puis saisissez **:wq** pour enregistrer ce fichier.
    
3.  Copiez et collez la commande suivante pour modifier les droits d'accès au fichier de clés privées.
    
        <copy>chmod 600 ~/.ssh/opc.key</copy>
        
4.  Copiez la commande suivante et collez-la dans le terminal, puis remplacez **bastion-public-ip** par l'adresse IP publique que vous avez copiée dans la dernière tâche.
    
        <copy>ssh -D 1088 -fCqN -i ~/.ssh/opc.key opc@bastion-public-ip</copy>
        
5.  Ouvrez le fichier **~/.kube/config**, puis copiez et collez les éléments suivants dans ce fichier comme indiqué ci-dessous.
    
        <copy>proxy-url: socks5://localhost:1088</copy>
        
    
    ![configuration de proxy](images/proxy-config.png)
    

## Tâche 4 : vérification de la connectivité de l'interface utilisateur du kit d'outils Kubernetes WebLogic au cluster Kubernetes Oracle

Dans cette tâche, nous vérifions la connectivité à _Oracle Kubernetes Cluster(OKE)_ à partir de l'application `WebLogic Kubernetes Toolkit UI`.

1.  Revenez à l'interface utilisateur du kit d'outils Kubernetes WebLogic, cliquez sur _Activités_ et sélectionnez la fenêtre d'interface utilisateur du kit d'outils Kubernetes WebLogic.
    
2.  Cliquez sur _Kubernetes_ -> _Configuration client_, puis sur _Vérifier la connectivité_. ![Vérifier la connexion](images/verify-connectivity.png)
    
3.  Une fois que la fenêtre _Vérifier la réussite de la connectivité du client Kubernetes_ apparaît, cliquez sur _OK_. ![Connexion réussie](images/successfully-connected.png)
    

## Tâche 5 : mise à jour de l'opérateur Kubernetes WebLogic vers le cluster Kubernetes Oracle

Cette section prend en charge l'installation de l'opérateur Kubernetes WebLogic (l'"opérateur") dans le cluster Kubernetes cible.

1.  Cliquez sur **WebLogic Operator**. Indiquez les détails de configuration suivants et cliquez sur **Mettre à jour l'opérateur**.
    
    **Espace de noms Kubernetes :** espace de noms Kubernetes sur lequel installer l'opérateur. Entrez la valeur **wko-operator-ns** comme indiqué dans la capture d'écran ci-dessous.
    
    **Compte de service Kubernetes :** compte de service Kubernetes que l'opérateur doit utiliser lors des demandes d'API Kubernetes. Entrez la valeur **wko-operator-sa** comme indiqué dans la capture d'écran ci-dessous.
    
    **Nom de version Helm à utiliser pour l'installation d'opérateur** - Nom de version Helm à utiliser pour identifier cette installation. Entrez la valeur **wko-weblogic-operator** comme indiqué dans la capture d'écran ci-dessous.
    
    ![WebLogic Opérateur](images/weblogic-operator.png)
    
    > **Pour information uniquement :**  
    > ! Par défaut, le champ _Balise d'image à utiliser_ de l'opérateur est défini sur la balise d'image correspondant à la dernière version de l'opérateur dans GitHub Container Registry.  
    > L'opérateur doit connaître les domaines WebLogic du cluster Kubernetes qu'il gérera. Il effectue cette opération au niveau de l'espace de noms Kubernetes, de sorte que tout domaine WebLogic dans un espace de noms Kubernetes que l'opérateur est configuré pour gérer sera géré par l'instance d'opérateur en cours d'installation.  
    > Pour le champ _Stratégie de sélection d'espace de noms Kubernetes_, nous sélectionnons _Sélecteur de libellé_, ce qui signifie que tout espace de noms Kubernetes avec un libellé _weblogic-operator=enabled_ sera géré par cet opérateur.  
    > ![Image de l'opérateur](images/operator-image.png)
    
    > En activant l'option Activer la liaison de rôle de cluster, l'installation de l'opérateur crée un Kubernetes ClusterRole et un ClusterRoleBinding que l'opérateur utilisera pour tous les espaces de noms gérés.  
    > Par défaut, l'API REST de l'opérateur n'est pas exposée en dehors du cluster Kubernetes. Pour permettre l'affichage de l'API REST, vous pouvez activer _Exposer l'API REST en externe_. ![Liaison de rôle](images/role-binding.png)  
    
    > Ce panneau vous permet de remplacer la configuration de journalisation Java de l'opérateur, ce qui peut être utile lors du débogage de problèmes avec l'opérateur.  
    > ![Journalisation Java](images/java-logging.png)  
    
    > Pour plus d'informations sur _WebLogic Kubernetes Operator Image_, _Kubernetes Namespace Selection Strategy_, _WebLogic Kubernetes Role Bindings_, _External REST API Access_, _Third Party Integrations_ et _Java Logging_, reportez-vous à la documentation [WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/).
    
2.  Une fois l'installation de l'opérateur Kubernetes _WebLogic terminée_, cliquez sur _OK_. ![Opérateur installé](images/operator-installed.png)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, octobre 2023
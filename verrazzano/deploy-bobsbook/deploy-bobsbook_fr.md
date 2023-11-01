# Déployer l'application échantillon Livres de Bobby

## Présentation

### À propos de Bobby's Books Application

![Demande de livres de Bobby's](images/bobbyoverview.png " ")

Les [livres de Bobby](https://verrazzano.io/docs/samples/bobs-books/) se composent de trois parties principales :

*   Application back-end de _traitement des commandes_, qui est une application Java EE avec des services REST et une interface utilisateur JSP très simple, qui stocke les données dans une base de données MySQL. Cette application est exécutée sur le serveur WebLogic.
*   Boutique Web frontale _"Robert's Books"_, qui est un vendeur de livres général. Il est implémenté en tant que microservice Helidon, qui extrait les données des livres de Coherence, utilise une banque de cache Coherence pour conserver les données pour le gestionnaire de commandes et dispose d'une interface utilisateur Web React.
*   Une boutique Web front-end _"Bobby's Books"_, qui est une librairie spécialisée pour enfants. Il est implémenté en tant que microservice Helidon, qui obtient les données de livre à partir d'une (différente) banque de cache Coherence, s'interface directement avec le gestionnaire de commandes et dispose d'une interface utilisateur Web JSF exécutée sur le serveur WebLogic.

Pour plus d'informations et le code source de cette application, reportez-vous aux [exemples Verrazzano](https://github.com/verrazzano/examples).

### Verrazzano et déploiement d'application

Verrazzano prend en charge la définition d'application à l'aide d'[Open Application Model (OAM)](https://oam.dev/). Les applications Verrrazzano sont composées de composants et de configurations d'applications.

Lorsque vous déployez des applications avec Verrazzano, la plate-forme configure des connexions, des stratégies réseau et des entrées dans le maillage de service, et connecte une pile de surveillance pour capturer les mesures, les journaux et les traces. Verrazzano utilise des composants OAM pour définir les unités fonctionnelles d'un système qui sont ensuite assemblées et configurées en définissant des configurations d'application associées.

### Composants Verrazzano

Un composant OAM Verrazzano est une [ressource personnalisée Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) décrivant la composition générale et les exigences d'environnement d'une application.

Le code suivant présente le composant de l'exemple d'application Bobby's Books utilisé dans cet exercice. Cette ressource décrit un composant implémenté par une seule image Docker contenant une application Helidon exposant une seule adresse.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: bobby-helidon
      namespace: bobs-books
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: bobbys-helidon-stock-application
          labels:
            app: bobbys-helidon-stock-application
        spec:
          deploymentTemplate:
            metadata:
              name: bobbys-helidon-stock-application
            podSpec:
              containers:
                - name: bobbys-helidon-stock-application
                  image: container-registry.oracle.com/verrazzano/example-bobbys-helidon-stock-application:0.1.12-1-20210624160519-017d358
                  imagePullPolicy: IfNotPresent
                  ports:
                    - containerPort: 8080
                      name: http
                  env:
                    - name: BACKEND_PORT
                      value: "8001"
                    - name: BACKEND_HOSTNAME
                      value: bobs-bookstore-cluster-cluster-1.bobs-books.svc.cluster.local
                    - name: COH_CLUSTER
                      value: bobbys-coherence
                    - name: COH_CACHE_CONFIG
                      value: coherence-cache-config.xml
                    - name: COH_POF_CONFIG
                      value: pof-config.xml
              imagePullSecrets:
                - name: bobs-books-repo-credentials
    

Brève description de chaque champ du composant :

*   **apiVersion** - Version de la définition de ressource personnalisée de composant
*   **kind** - Nom standard de la définition de ressource personnalisée de composant
*   **metadata.name** : nom utilisé pour créer la ressource personnalisée du composant
*   **metadata.namespace** : espace de noms utilisé pour créer la ressource personnalisée de ce composant
*   **spec.workload.kind :** VerrazzanoHelidonWorkload définit une charge globale sans conservation de statut de Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name :** nom utilisé pour créer la charge globale sans conservation de statut de Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** : conteneurs d'implémentation
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** : ports exposés par le conteneur

Vous trouverez la description complète du composant de l'application de livres de Bobbys dans le fichier [bobs-books-comp.yaml](https://github.com/verrazzano/verrazzano/blob/master/examples/bobs-books/bobs-books-comp.yaml).

### Configurations d'application Verrazzano

Une configuration d'application Verrazzano est une ressource personnalisée Kubernetes qui fournit des personnalisations propres à l'environnement. Le code suivant indique la configuration de l'application pour l'exemple Bob's Books utilisé dans cet exercice. Cette ressource indique le déploiement de l'application vers l'espace de noms bobs-books.

Des fonctionnalités d'exécution supplémentaires sont spécifiées à l'aide de caractéristiques ou de superpositions d'exécution qui augmentent la charge globale. Par exemple, la caractéristique entrante indique l'hôte et le chemin entrants, tandis que la caractéristique de mesures fournit le racleur Prometheus utilisé pour obtenir les mesures liées à l'application.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: bobs-books
      namespace: bobs-books
      annotations:
        version: v1.0.0
        description: "Bob's Books"
    spec:
      components:
        - componentName: robert-helidon
          traits:
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
        - componentName: robert-coh
        - componentName: bobby-coh
        - componentName: bobby-helidon
        - componentName: bobby-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobbys-front-end"
                          pathType: Prefix
        - componentName: bobs-orders-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobs-bookstore-order-manager/orders"
                          pathType: Prefix
        - componentName: bobs-orders-configmap
        - componentName: bobs-mysql-deployment
        - componentName: bobs-mysql-service
        - componentName: bobs-mysql-configmap
    

Brève description de chaque champ de la configuration de l'application :

*   **apiVersion** - Version de la définition de ressource personnalisée ApplicationConfiguration
*   **kind** - Nom standard de la définition de ressource personnalisée de configuration d'application
*   **metadata.name** : nom utilisé pour créer cette ressource de configuration d'application
*   **metadata.namespace** : espace de noms utilisé pour cette ressource personnalisée de configuration d'application
*   **spec.components :** référence aux composants de l'application utilisés pour indiquer la configuration d'exécution
*   **spec.components\[\].traits** - Caractéristiques spécifiées pour les composants de l'application

Pour explorer les traits, nous pouvons examiner les champs d'un trait d'entrée :

*   **apiVersion** - Version de la définition de ressource personnalisée de caractéristique OAM
*   **kind** : IngressTrait est le nom de la définition de ressource personnalisée de caractéristique d'entrée d'application OAM.
*   **spec.rules.paths** : chemins de contexte permettant d'accéder à l'application

Cet atelier vous guide tout au long du processus de déploiement de l'exemple d'application Bobby's Books.

Durée estimée : 10 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Acceptez la licence permettant de télécharger les images à partir des référentiels dans Oracle Container Registry.
*   Déployer l'application échantillon Bobby's Books.
*   Vérifiez que le déploiement de l'exemple d'application Bobby's Books a réussi.

### Prérequis

Pour exécuter l'atelier 3, vous devez disposer des éléments suivants :

*   Exécutez l'atelier 1, qui crée un cluster OKE sur Oracle Cloud Infrastructure.

\* Exécutez l'atelier 1, qui configure kubectl pour l'accès à un cluster OKE sur Oracle Cloud Infrastructure.

*   Exécutez Lab 2, qui installe Verrazzano sur le cluster Kubernetes.
*   Editeur de texte, dans lequel vous pouvez coller les commandes et les URL et les modifier, en fonction de votre environnement. Vous pouvez ensuite copier et coller les commandes modifiées pour les exécuter dans _Cloud Shell_.

## Tâche 1 : accepter le contrat de licence pour télécharger les images à partir des référentiels dans Oracle Container Registry

Pour le déploiement de l'exemple d'application _Bobby's Books_, nous utiliserons les exemples d'image. Etant donné que ces images contiennent des produits Oracle, vous devez accepter le contrat de licence avant d'utiliser ces images dans votre fichier bobs-books, `bobs-books-comp.yaml`.

1.  Cliquez sur le lien correspondant à Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) et connectez-vous. Pour cela, vous avez besoin d'un compte Oracle.
    
    ![Connexion](images/registrysignin.png " ")
    
2.  Entrez vos _informations d'identification de compte Oracle_ dans les champs Nom utilisateur et Mot de passe, puis cliquez sur _Connexion_. Nous utiliserons ultérieurement ces informations d'identification pour créer une clé secrète dans Kubernetes.
    
    ![Oracle SSO ](images/oraclesso.png " ")
    
3.  Sur la page d'accueil, sélectionnez _Verrazzano_.
    
    ![Page d'accueil Registre](images/registryhomepage.png " ")
    
4.  Par exemple, bobbys-coherence, example-bobbys-front-end, example-bobs-books-order-manager et example-roberts-coherence repository, sélectionnez _English_ comme langue, puis cliquez sur _Continuer_.
    
    ![Continuer](images/continue.png " ")
    
5.  Cliquez sur _Accepter_ pour accepter le contrat de licence.
    
    ![Accepter l'accord](images/acceptagreement.png " ")
    
6.  Vérifiez que vous avez accepté le contrat de licence pour les référentiels liés à Verrazzano comme indiqué dans l'image suivante.
    
    ![Vérifier l'accord de licence](images/verifyaggrement.png " ")
    
7.  Sur la page d'accueil d'Oracle Container Registry, recherchez _weblogic_
    
    ![Rechercher sur WebLogic](images/searchweblogic.png " ")
    
8.  Cliquez sur _weblogic_ comme indiqué et acceptez la licence comme vous l'avez fait pour Verrazzano imagaes. ![cliquez sur weblogic](images/clickweblogic.png) ![accepter weblogic](images/acceptagreement.png " ")
    

## Tâche 2 : déployer l'application Bobby's Books

Nous devons télécharger le code source avec les fichiers de configuration `bobs-books-app.yaml` et `bobs-books-comp.yaml`.

1.  Téléchargez le fichier yaml du composant OAM Verrazzano et les fichiers de configuration de l'application Verrazzano de l'exemple de livre de Bobby. Cliquez sur _Copier_ et collez la commande dans Cloud Shell comme indiqué :
    
        <copy>
        curl -LSs  https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-app.yaml >~/bobs-books-app.yaml
        curl -LSs https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-comp.yaml >~/bobs-books-comp.yaml
        cd ~
        </copy>
        
2.  Nous conserverons tous les artefacts Kubernetes dans l'espace de noms distinct. Créez un espace de noms pour l'exemple d'application Bob's Books. Les espaces de noms permettent d'organiser les clusters en sous-clusters virtuels. Nous pouvons avoir un nombre illimité d'espaces de noms dans un cluster, chacun séparé logiquement des autres, mais avec la possibilité de communiquer entre eux. Nous devons également informer Verrazzano que nous stockons dans cet espace de noms des artefacts Verrazzano. Nous devons donc ajouter une étiquette identifiant l'espace de noms bobs-books géré par Verrazzano. Les libellés sont destinés à être utilisés pour spécifier des attributs d'identification d'objets significatifs et pertinents pour les utilisateurs. Ici, pour l'espace de noms bobs-book, nous lui attachons une étiquette, qui marque cet espace de noms comme géré par Verrazzano. La commande _istio-injection=enabled_ active un "sidecar" Istio et, en tant que tel, permet d'établir un proxy Istio. Avec un proxy Istio, nous pouvons accéder à d'autres services Istio comme une passerelle Istio et autres. Pour ajouter le libellé à l'espace de noms bobs-books avec les attributs mentionnés précédemment, copiez la commande suivante et exécutez-la dans _Cloud Shell_.
    
        <copy>
        kubectl create namespace bobs-books
        kubectl label namespace bobs-books verrazzano-managed=true istio-injection=enabled
        </copy>
        
    
    ![Créer un espace de noms](images/createnamespace.png " ")
    
3.  Copiez la commande suivante pour télécharger le script. Ce script authentifie l'utilisateur pour Oracle Container Registry. Si l'authentification réussit, la clé secrète du registre docker est créée. Le registre Docker est un moyen de stocker et de gérer les versions des images, comme GitHub pour le code normal mais pour les conteneurs (que Kubernetes peut extraire). Ici, nous allons créer une clé secrète de registre docker pour permettre l'extraction de l'image d'exemple des livres de Bobby à partir d'Oracle Container Registry. Cliquez sur _Copier_ dans la commande suivante, collez-la dans l'éditeur de texte de votre choix et remplacez le nom utilisateur et le mot de passe par l'ID de courriel et le mot de passe respectivement utilisés dans la tâche 1 pour accepter le contrat de licence de téléchargement d'images à partir d'Oracle Container Registry. Ensuite, dans Cloud Shell, collez la commande modifiée comme indiqué :
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/deploy-bobsbook/create_secret.sh >~/create_secret.sh
        chmod 777 create_secret.sh
        ./create_secret.sh username 'password'    
        </copy>
        
    
    ![Créer une clé secrète](images/createsecret.png " ")
    
    > Entrez le mot de passe entre apostrophes.
    
4.  Nous devons créer plusieurs clés secrètes Kubernetes avec des informations d'identification. Dans l'application Bobby's Books, nous avons deux domaines WebLogic _bobby-front-end_ et _bobs-bookstore_. Les informations d'identification du domaine WebLogic sont conservées dans une clé secrète Kubernetes où le nom de la clé secrète est indiqué à l'aide de _webLogicCredentialsSecret_ dans la ressource de domaine WebLogic. En outre, la clé secrète d'informations d'identification de domaine doit être créée dans l'espace de noms où le domaine sera exécuté. Nous devons créer les clés secrètes appelées _bobbys-front-end-weblogic-credentials_ et _bobs-bookstore-weblogic-credentials_ utilisées par les domaines de serveur WebLogic, avec une valeur de nom utilisateur `weblogic` et un mot de passe généré de manière aléatoire dans l'espace de noms bobs-books. Notre application Bobby's Books utilise une base de données _mysql_. Nous créons donc une clé secrète nommée _mysql-credentials_, avec la valeur de nom utilisateur `weblogic`, un mot de passe généré de manière aléatoire et l'URL JDBC _jdbc:mysql ://mysql.bobs-books.svc.cluster.local:3306/books_ dans l'espace de noms bobs-books. Nous utiliserons ces valeurs dans la chaîne de connexion JDBC de l'objet WebLogic DataSource. Copiez et collez le bloc de commandes dans _Cloud Shell_.
    
        <copy>
        export WLS_USERNAME=weblogic
        export WLS_PASSWORD=$((< /dev/urandom tr -dc 'A-Za-z0-9"'\''/<=>?\^_`|~' | head -c10);(date +%S))
        echo $WLS_PASSWORD
        kubectl create secret generic bobbys-front-end-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic bobs-bookstore-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic mysql-credentials \
            --from-literal=username=$WLS_USERNAME \
            --from-literal=password=$WLS_PASSWORD \
            --from-literal=url=jdbc:mysql://mysql.bobs-books.svc.cluster.local:3306/books \
            -n bobs-books
        cd ~
        </copy>
        
    
    ![Créer une ressource](images/createresource.png " ")
    
5.  Nous disposons d'un cluster Kuberneter, _cluster1__verrazzano_, avec trois noeuds. Désormais, nous voulons déployer l'application en conteneur Bobby's Books sur _cluster1__verrazzano_. Pour cela, nous avons besoin d'une configuration de déploiement Kubernetes. Ce déploiement indique à Kubernetes de créer et de mettre à jour des instances pour l'application Bobby's Books. Ici, nous avons le fichier `bobs-books-comp.yaml`, qui indique à Kubernetes de déployer l'application Bobby's Books. Copiez et collez les deux commandes suivantes comme indiqué. Le fichier `bobs-books-comp.yaml` contient les définitions de divers composants OAM, où un composant OAM est une ressource personnalisée Kubernetes décrivant la composition générale et les exigences d'environnement d'une application. Pour en savoir plus sur le fichier `bobs-books-comp.yaml`, consultez les composants Verrazzano dans la section Introduction de cet atelier 3.
    
        <copy>kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh created
        component.core.oam.dev/robert-helidon created
        component.core.oam.dev/bobby-coh created
        component.core.oam.dev/bobby-helidon created
        component.core.oam.dev/bobby-wls created
        component.core.oam.dev/bobs-mysql-configmap created
        component.core.oam.dev/bobs-mysql-service created
        component.core.oam.dev/bobs-mysql-deployment created
        component.core.oam.dev/bobs-orders-configmap created
        component.core.oam.dev/bobs-orders-wls created
        $
        
6.  Le fichier `bobs-books-app.yaml` est un fichier de configuration d'application Verrazzano, qui fournit des personnalisations propres à l'environnement. Pour en savoir plus sur le fichier `bobs-books-app.yaml`, consultez Configuration de l'application Verrazzano dans la section Introduction de cet atelier 3.
    
        <copy>kubectl apply -f ~/bobs-books-app.yaml -n bobs-books</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $ kubectl apply -f ~/bobs-books-app.yaml -n bobs-books
        applicationconfiguration.core.oam.dev/bobs-books created
        $
        
7.  Attendez que tous les pods de l'application exemple Bobby's Books soient à l'état _En cours d'exécution_. Vous devrez peut-être répéter cette commande plusieurs fois avant de réussir. La création et la préparation des pods WebLogic Server et Coherence peuvent prendre un certain temps. Cette commande _kubectl_ attend que tous les pods aient l'état _En cours d'exécution_ dans l'espace de noms bobs-books. Cela prend environ 4-5 minutes. Si vous attendez plus de 5 minutes, vous pouvez réexécuter la commande.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s</copy>
        
    
    La sortie doit ressembler à ceci :
    
          $ kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s
          pod/bobbys-coherence-0 condition met
          pod/bobbys-front-end-adminserver condition met
          pod/bobbys-front-end-managed-server1 condition met
          pod/bobbys-helidon-stock-application-5f74cbcc8b-cw4x4 condition met
          pod/bobs-bookstore-adminserver condition met
          pod/bobs-bookstore-managed-server1 condition met
          pod/mysql-6bc8f9f785-n4qjh condition met
          pod/robert-helidon-65b8874988-7x5vj condition met
          pod/robert-helidon-65b8874988-vnntp condition met
          pod/roberts-coherence-0 condition met
          pod/roberts-coherence-1 condition met
          $
        

## Tâche 3 : vérifier le succès du déploiement de l'application Bobby's Book

Vérifiez que la configuration de l'application, les domaines, les ressources Coherence et la caractéristique entrante existent tous.

1.  Pour vérifier que l'application _Bobby's Books_ a été déployée dans l'espace de noms bobs-books.
    
        <copy>kubectl get ApplicationConfiguration -n bobs-books</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $ kubectl get ApplicationConfiguration -n bobs-books
          NAME         AGE
          bobs-books   72m
        $
        
2.  Pour vérifier que les deux domaines WebLogic ont été créés dans l'espace de noms bobs-books.
    
        <copy>kubectl get Domain -n bobs-books</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $ kubectl get Domain -n bobs-books
          NAME               AGE
          bobbys-front-end   73m
          bobs-orders-wls    73m
        $
        
3.  Pour vérifier que les deux clusters Coherence ont été créés dans l'espace de noms bobs-books.
    
        <copy>kubectl get Coherence -n bobs-books</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $ kubectl get Coherence -n bobs-books
        NAME              CLUSTER           ROLE           REPLICAS  READY PHASE
        bobbys-coherence  bobbys-coherence  bobbys-coherence  1       1    Ready
        roberts-coherence roberts-coherence roberts-coherence 2       2    Ready
        $
        
4.  Afin d'obtenir IngressTrait pour l'application Livre de Bobby, exécutez la commande suivante dans _Cloud Shell_.
    
        <copy>kubectl get IngressTrait -n bobs-books</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $ kubectl get IngressTrait -n bobs-books
          NAME                               AGE
          bobby-wls-trait-79b67d9d88         76m
          bobs-orders-wls-trait-57b4d8cb4b   76m
          robert-helidon-trait-54d7bcd54b    76m
        $
        
5.  Vérifiez que les pods de service ont été créés et passez à l'état _En cours d'exécution_. Notez que cela peut prendre quelques minutes et que certains services peuvent s'interrompre et redémarrer. Enfin, vous verrez que tous les pods associés à l'espace de noms bobs-books ont le statut _En cours d'exécution_. Copiez les détails des pods pour l'application _bobbys-helidon-stock-application_.
    
        <copy>kubectl get pods -n bobs-books</copy>
        
    
        $ kubectl get pods -n bobs-books
        NAME                                          READY STATUS    RESTARTS   AGE
        bobbys-coherence-0                             2/2  Running   0          7m51s
        bobbys-front-end-adminserver                   4/4  Running   0          5m28s
        bobbys-front-end-managed-server1               4/4  Running   0          4m30s
        bobbys-helidon-stock-application-5f74cbc-cw4x4 2/2  Running   0          7m54s
        bobs-bookstore-adminserver                     4/4  Running   0          4m31s
        bobs-bookstore-managed-server1                 4/4  Running   0          3m41s
        mysql-6bc8f9f785-n4qjh                         2/2  Running   0          5m52s
        robert-helidon-65b8874988-7x5vj                2/2  Running   0          7m53s
        robert-helidon-65b8874988-vnntp                2/2  Running   0          7m54s
        roberts-coherence-0                            2/2  Running   0          7m52s
        roberts-coherence-1                            2/2  Running   0          7m51s
        $
        
    
    > Notez le nom du pod pour **bobbys-helidon-stock-application**. Lorsque nous redéployons ce composant, vous remarquerez que ce pod passe au statut _Terminaison_ et que le nouveau pod démarre et passe à l'état _En cours d'exécution_ dans l'atelier 7.
    
    Laissez _Cloud Shell_ ouvert. Nous l'utiliserons également pour les prochains exercices.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
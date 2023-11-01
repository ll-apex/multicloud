# Déployer l'application Springboot sur Verrazzano

## Présentation

Cet atelier vous guide tout au long du processus de déploiement de l'image docker de l'application Springboot sur OKE.

Temps estimé : 10 minutes

### Verrazzano et déploiement d'application

Verrazzano prend en charge la définition d'application à l'aide d'[Open Application Model (OAM)](https://oam.dev/). Les applications Verrazzano sont composées de composants et de configurations d'applications.

Lorsque vous déployez des applications avec Verrazzano, la plate-forme configure des connexions, des stratégies réseau, des entrées dans le maillage de service et connecte une pile de surveillance pour capturer les mesures, les journaux et les traces. Verrazzano utilise des composants OAM pour définir les unités fonctionnelles d'un système qui sont ensuite assemblées et configurées en définissant des configurations d'application associées.

### Composants Verrazzano

Un composant OAM Verrazzano est une [ressource personnalisée Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) décrivant la composition générale et les exigences d'environnement d'une application.

Le code suivant présente un composant d'application Springboot simple pour l'application échantillon Tomcat utilisée dans cet exercice. Cette ressource décrit un composant implémenté par une seule image Docker contenant une application Springboot exposant une seule adresse.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: springboot-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: springboot-workload
          labels:
            app: springboot
            version: v1
        spec:
          containers:
          - name: springboot-container
            image: "EndpointOfYourRegion/TenancyNamespace/springboot-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: springboot
    

Brève description de chaque champ du composant :

*   **apiVersion** - Version de la définition de ressource personnalisée de composant
*   **kind** - Nom standard de la définition de ressource personnalisée de composant
*   **metadata.name** : nom utilisé pour créer la ressource personnalisée du composant
*   **spec.workload.kind :** ContainerizedWorkload définit une charge globale sans conservation de statut de Kubernetes
*   **spec.workload.spec.containers.ports.containerPort** : ports exposés par le conteneur

### Configurations d'application Verrazzano

Une configuration d'application Verrazzano est une ressource personnalisée Kubernetes qui fournit des personnalisations propres à l'environnement. Le code suivant indique la configuration de l'application pour l'exemple d'application Springboot utilisé dans cet exercice. Cette ressource indique le déploiement de l'application vers l'espace de noms Springboot.

Des fonctionnalités d'exécution supplémentaires sont spécifiées à l'aide de caractéristiques ou de superpositions d'exécution qui augmentent la charge globale. Par exemple, la caractéristique entrante indique l'hôte et le chemin entrants, tandis que la caractéristique de mesures fournit le racleur Prometheus utilisé pour obtenir les mesures liées à l'application.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: springboot-appconf
      annotations:
        version: v1.0.0
        description: "Spring Boot application"
    spec:
      components:
        - componentName: springboot-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                  scraper: verrazzano-system/vmi-system-prometheus-0
                  path: "/actuator/prometheus"
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: springboot-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
    

Brève description de chaque champ de la configuration de l'application :

*   **apiVersion** - Version de la définition de ressource personnalisée ApplicationConfiguration
*   **kind** - Nom standard de la définition de ressource personnalisée de configuration d'application
*   **metadata.name** : nom utilisé pour créer cette ressource de configuration d'application
*   **spec.components :** référence aux composants de l'application utilisés pour indiquer la configuration d'exécution
*   **spec.components\[\].traits** - Caractéristiques spécifiées pour les composants de l'application

Pour explorer les traits, nous pouvons examiner les champs d'un trait d'entrée :

*   **apiVersion** - Version de la définition de ressource personnalisée de caractéristique OAM
*   **kind** : IngressTrait est le nom de la définition de ressource personnalisée de caractéristique d'entrée d'application OAM.
*   **spec.rules.paths** : chemins de contexte permettant d'accéder à l'application

### Objectifs

Dans cet exercice, vous allez :

*   Déployez l'application springboot.
*   Vérifiez le déploiement de l'application Springboot.

### Prérequis

Pour exécuter cet exercice, vous devez disposer des éléments suivants :

*   Cluster Kubernetes (OKE) exécuté sur Oracle Cloud Infrastructure.
*   L'installation de Verrazzano a démarré sur un cluster Kubernetes (OKE).
*   Application _`springboot-your_firstname`_ packagée par conteneur disponible dans un registre de conteneurs.

## Tâche 1 : déployer l'application d'initialisation Spring

1.  Créez un espace de noms `springboot` pour l'application springboot. Nous conserverons tous les artefacts Kubernetes dans un espace de noms distinct.
    
        <copy>kubectl create namespace springboot</copy>
        
    
    > Les espaces de noms permettent d'organiser les clusters en sous-clusters virtuels. Nous pouvons avoir un nombre illimité d'espaces de noms dans un cluster, chacun séparé logiquement des autres, mais avec la possibilité de communiquer entre eux.
    
2.  Nous devons informer Verrazzano que nous stockons dans cet espace de noms les artefacts Verrazzano. Nous devons donc ajouter un libellé identifiant l'espace de noms `springboot` comme géré par Verrazzano. Les libellés sont destinés à être utilisés pour spécifier des attributs d'identification d'objets significatifs et pertinents pour les utilisateurs.
    
    Ici, pour l'espace de noms `springboot`, nous lui attachons un libellé, qui marque cet espace de noms comme étant géré par Verrazzano. La commande _istio-injection=enabled_ active un "sidecar" Istio et, en tant que tel, permet d'établir un proxy Istio. Avec un proxy Istio, nous pouvons accéder à d'autres services Istio comme une passerelle Istio. Pour ajouter le libellé à l'espace de noms `springboot` avec les attributs mentionnés précédemment, copiez la commande suivante et exécutez-la dans Cloud Shell :
    
        <copy>kubectl label namespace springboot verrazzano-managed=true istio-injection=enabled</copy>
        
3.  Dans _~/springboot-app_, nous avons le fichier de configuration pour l'application springboot. Dans le cadre de l'atelier 2, nous avons créé une nouvelle image Docker pour le composant d'application. Nous avons également propagé cette image Docker vers le référentiel Oracle Cloud Container Registry. Dans cet exercice, nous allons modifier le fichier _springboot-comp.yaml_ de sorte qu'il prenne la nouvelle image Docker du référentiel Oracle Cloud Container Registry. Pour modifier le fichier _springboot-comp.yaml_, copiez la commande suivante et collez-la dans _Cloud Shell_.
    
        <copy>vi ~/springboot-app/springboot-comp.yaml</copy>
        
4.  Dans le cadre de l'atelier 2, vous avez enregistré le nom complet de l'image Docker. Vous devez copier la ligne suivante et la coller dans votre fichier texte. Vous devez ensuite remplacer `docker image full name` par le nom de l'image Docker. Copiez ensuite la ligne modifiée et appuyez sur _i_ pour insérer le texte dans le fichier `*springboot-comp.yaml*`. Collez la sortie à la ligne **19** (assurez-vous de conserver l'indentation), puis appuyez sur _Echap_ et saisissez _:wq_ pour enregistrer le fichier.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Insérer une ligne](images/insert-line.png " ")
    
5.  Désormais, nous voulons déployer une application en conteneur Springboot sur _cluster1_. Pour cela, nous avons besoin d'une configuration de déploiement Kubernetes. Ce déploiement indique à Kubernetes de créer et de mettre à jour des instances pour l'application Springboot. Ici, nous avons le fichier `springboot-comp.yaml`, qui indique à Kubernetes.
    
    Pour déployer l'application Springboot, copiez et collez les deux commandes suivantes comme indiqué. Le fichier `springboot-comp.yaml` contient les définitions de divers composants OAM, où un composant OAM est une ressource personnalisée Kubernetes décrivant la composition générale et les exigences d'environnement d'une application.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-comp.yaml -n springboot</copy>
        
    
    Le fichier `springboot-app.yaml` est un fichier de configuration d'application Verrazzano, qui fournit des personnalisations propres à l'environnement.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-app.yaml -n springboot</copy>
        
6.  Attendez que les pods aient le statut _En cours d'exécution_. Utilisez cette commande _kubectl_ pour attendre que tous les pods soient à l'état _Running_ dans l'espace de noms _springboot_. Cela prend environ 1-2 minutes.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s</copy>
        
    
    Lorsque les pods sont prêts, vous pouvez voir une réponse similaire :
    
        $ kubectl wait --for=condition=Ready pods --all -n kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s --timeout=600s
        pod/springboot-workload-5c4ffc8954-mbgbb condition met
        pod/springboot-workload-bfb9d487-dwp7q condition met
        
    
    Vous pouvez également répertorier directement les pods pour vérifier leur statut :
    
        <copy>kubectl  get po -n springboot</copy>
        
    
        $ kubectl  get po -n springboot
        NAME                                 READY   STATUS    RESTARTS   AGE
        springboot-workload-bfb9d487-dwp7q   2/2     Running   0          68s
        $
        

## Tâche 2 : vérification du déploiement réussi de l'application Springboot

1.  Vérifiez l'adresse `/facts`. Pour déterminer l'URL construite à partir de la configuration de l'adresse IP et de l'application de l'équilibreur de charge/externe, exécutez la commande suivante :
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io springboot-springboot-appconf-gw -n springboot -o jsonpath={.spec.servers[0].hosts[0]})/facts</copy>
        
    
    L'URL appropriée sera imprimée sur l'adresse REST, par exemple :
    
        https://springboot-appconf.springboot.xx.xx.xx.xx.nip.io/facts
        
2.  Collez l'URL dans le navigateur et cliquez sur le bouton _Actualiser_ dans le navigateur. Chaque fois que vous cliquez sur le bouton Actualiser, cela vous donne un fait aléatoire sur Verrazzano. ![URL d'application](images/application-url.png) ![Autre fait](images/another-fact.png)
    

Vous pouvez maintenant **passer à l'exercice suivant**.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
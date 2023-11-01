# Déployer l'application Helidon sur Verrazzano

## Présentation

Cet atelier vous guide tout au long du processus de déploiement de l'application Helidon quickstart-mp.

Temps estimé : 10 minutes

### Verrazzano et déploiement d'application

Verrazzano prend en charge la définition d'application à l'aide d'[Open Application Model (OAM)](https://oam.dev/). Les applications Verrazzano sont composées de composants et de configurations d'applications.

Lorsque vous déployez des applications avec Verrazzano, la plate-forme configure des connexions, des stratégies réseau, des entrées dans le maillage de service et connecte une pile de surveillance pour capturer les mesures, les journaux et les traces. Verrazzano utilise des composants OAM pour définir les unités fonctionnelles d'un système qui sont ensuite assemblées et configurées en définissant des configurations d'application associées.

### Composants Verrazzano

Un composant OAM Verrazzano est une [ressource personnalisée Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) décrivant la composition générale et les exigences d'environnement d'une application.

Le code suivant présente un composant d'application Helidon simple pour l'application _quickstart-mp_ Helidon utilisée dans cet exercice. Cette ressource décrit un composant implémenté par une seule image Docker contenant une application Helidon exposant une seule adresse.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: hello-helidon-component
      namespace: hello-helidon
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: hello-helidon-workload
          labels:
            app: hello-helidon
        spec:
          deploymentTemplate:
            metadata:
              name: hello-helidon-deployment
            podSpec:
              containers:
                - name: hello-helidon-container
                  image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp:1.0"
                  ports:
                    - containerPort: 8080
                      name: http
    

Brève description de chaque champ du composant :

*   **apiVersion** - Version de la définition de ressource personnalisée de composant
*   **kind** - Nom standard de la définition de ressource personnalisée de composant
*   **metadata.name** : nom utilisé pour créer la ressource personnalisée du composant
*   **metadata.namespace** : espace de noms utilisé pour créer la ressource personnalisée de ce composant
*   **spec.workload.kind :** VerrazzanoHelidonWorkload définit une charge globale sans conservation de statut de Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name :** nom utilisé pour créer la charge globale sans conservation de statut de Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** : conteneurs d'implémentation
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** : ports exposés par le conteneur

### Configurations d'application Verrazzano

Une configuration d'application Verrazzano est une ressource personnalisée Kubernetes qui fournit des personnalisations propres à l'environnement. Le code suivant indique la configuration de l'application pour l'exemple Helidon _quickstart-mp_ utilisé dans cet exercice. Cette ressource indique le déploiement de l'application vers l'espace de noms hello-helidon.

Des fonctionnalités d'exécution supplémentaires sont spécifiées à l'aide de caractéristiques ou de superpositions d'exécution qui augmentent la charge globale. Par exemple, la caractéristique entrante indique l'hôte et le chemin entrants, tandis que la caractéristique de mesures fournit le racleur Prometheus utilisé pour obtenir les mesures liées à l'application.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: hello-helidon-appconf
      namespace: hello-helidon
      annotations:
        version: v1.0.0
        description: "Hello Helidon application"
    spec:
      components:
        - componentName: hello-helidon-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                    port: 8080
                    scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: hello-helidon-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/help/allGreetings"
                          pathType: Prefix
    

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

### Objectifs

Dans cet exercice, vous allez :

*   Vérifiez que l'installation de l'environnement Verrazzano a réussi.
*   Déployez l'application _quickstart-mp_ Helidon.
*   Vérifiez le déploiement de l'application Helidon _quickstart-mp_.

### Prérequis

Pour exécuter cet exercice, vous devez disposer des éléments suivants :

*   Cluster Kubernetes (OKE) exécuté sur Oracle Cloud Infrastructure.
*   L'installation de Verrazzano a démarré sur un cluster Kubernetes (OKE).
*   Application Helidon _quickstart-mp_ packagée par conteneur disponible dans un registre de conteneurs.

## Tâche 1 : vérification d'une installation Verrazzano réussie

Verrazzano installe plusieurs objets dans plusieurs espaces de noms. Les composants Verrazzano sont installés dans l'espace de noms _verrazzano-system_.

1.  Vérifiez que tous les pods associés aux objets multiples ont le statut _En cours d'exécution_. Les pods auront l'état _En cours d'exécution_ comme indiqué ci-dessous.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $   kubectl get pods -n verrazzano-system
        NAME                                     READY   STATUS    RESTARTS  AGE
        coherence-operator-b5dc669c6-rk2sm       1/1     Running     4       46m
        fluentd-54f5x                            2/2     Running     2       46m
        fluentd-h7mgh                            2/2     Running     1       46m
        fluentd-xcdfz                            2/2     Running     0       46m
        oam-kubernetes-runtime-5b48f944b-cx7b9   1/1     Running     0       46m
        verrazzano-application-operator-665c5c94 1/1     Running     0       46m
        verrazzano-application-operator-webhook  1/1     Running     0       46m
        verrazzano-authproxy-67776ff58b-8lkzs    3/3     Running     0       46m
        verrazzano-cluster-operator-67dc569555   1/1     Running     0       46m
        verrazzano-cluster-operator-webhook-57f  1/1     Running     0       46m
        verrazzano-console-7d95c98cb9-9ql2x      2/2     Running     0       46m
        verrazzano-monitoring-operator-59ff9576  2/2     Running     0       46m
        vmi-system-es-master-0                   2/2     Running     0       46m
        vmi-system-grafana-7fd956b585-2tbgl      3/3     Running     0       46m
        vmi-system-kiali-dd87546d6-ddxss         2/2     Running     0       46m
        vmi-system-osd-7687d6fccf-nm7kt          2/2     Running     0       46m
        weblogic-operator-54979449f4-njgrq       2/2     Running     0       46m
        weblogic-operator-webhook-f7ff8c8cf      1/1     Running     0       46m
        

## Tâche 2 : déployer l'application QuickStart-mp Helidon

1.  Téléchargez le fichier yaml du composant OAM Verrazzano et les fichiers de configuration de l'application Verrazzano dans l'environnement Cloud Shell :
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-app.yaml >~/hello-helidon-app.yaml
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-comp.yaml >~/hello-helidon-comp.yaml
        cd ~
        </copy>
        
2.  Modifiez le nom de l'image dans _hello-helidon-comp.yaml_. Vous pouvez utiliser l'éditeur `vi` :
    
        <copy>vi ~/hello-helidon-comp.yaml</copy>
        
3.  Utilisez `i` pour modifier le mode d'insertion et le nom de l'image afin de refléter le chemin du référentiel à la ligne 23 :
    
        image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0"
        
    
    Par exemple :
    
        image: "ocir.io/tenancynamespace/quickstart-mp-your_first_name:1.0"
        
4.  Utilisez `Esc` pour quitter le mode d'insertion et saisissez `:wq` pour enregistrer les modifications et fermer l'éditeur.
    
5.  Créez un espace de noms `hello-helidon` pour l'application QuickStart-mp Helidon. Nous conserverons tous les artefacts Kubernetes dans un espace de noms distinct.
    
        <copy>
        kubectl create namespace hello-helidon
        </copy>
        
    
    > Les espaces de noms permettent d'organiser les clusters en sous-clusters virtuels. Nous pouvons avoir un nombre illimité d'espaces de noms dans un cluster, chacun séparé logiquement des autres, mais avec la possibilité de communiquer entre eux.
    
6.  Nous devons informer Verrazzano que nous stockons dans cet espace de noms les artefacts Verrazzano. Nous devons donc ajouter un libellé identifiant l'espace de noms `hello-helidon` comme géré par Verrazzano. Les libellés sont destinés à être utilisés pour spécifier des attributs d'identification d'objets significatifs et pertinents pour les utilisateurs.
    
    Ici, pour l'espace de noms `hello-helidon`, nous lui attachons un libellé, qui marque cet espace de noms comme étant géré par Verrazzano. La commande _istio-injection=enabled_ active un "sidecar" Istio et, en tant que tel, permet d'établir un proxy Istio. Avec un proxy Istio, nous pouvons accéder à d'autres services Istio comme une passerelle Istio. Pour ajouter le libellé à l'espace de noms `hello-helidon` avec les attributs mentionnés précédemment, copiez la commande suivante et exécutez-la dans Cloud Shell :
    
        <copy>
        kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled
        </copy>
        
7.  Désormais, nous voulons déployer l'application en conteneur _quickstart-mp_ Helidon sur _cluster1_. Pour cela, nous avons besoin d'une configuration de déploiement Kubernetes. Ce déploiement indique à Kubernetes de créer et de mettre à jour des instances pour l'application Helidon _quickstart-mp_. Ici, nous avons le fichier `hello-helidon-comp.yaml`, qui indique à Kubernetes.
    
    Pour déployer l'application Helidon _quickstart-mp_, copiez et collez les deux commandes suivantes comme indiqué. Le fichier `hello-helidon-comp.yaml` contient les définitions de divers composants OAM, où un composant OAM est une ressource personnalisée Kubernetes décrivant la composition générale et les exigences d'environnement d'une application.
    
        <copy>kubectl apply -f ~/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    Le fichier `hello-helidon-app.yaml` est un fichier de configuration d'application Verrazzano, qui fournit des personnalisations propres à l'environnement.
    
        <copy>kubectl apply -f ~/hello-helidon-app.yaml -n hello-helidon</copy>
        
8.  Attendez que les pods aient le statut _En cours d'exécution_. Utilisez cette commande _kubectl_ pour attendre que tous les pods soient à l'état _Running_ dans l'espace de noms hello-helidon. Cela prend environ 1-2 minutes.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    Lorsque les pods sont prêts, vous pouvez voir une réponse similaire :
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    Vous pouvez également répertorier directement les pods pour vérifier leur statut :
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## Tâche 3 : vérifier le déploiement réussi de l'application QuickStart-mp Helidon

1.  Vérifiez l'adresse `help/allGreetings`. Pour déterminer l'URL construite à partir de la configuration de l'adresse IP et de l'application de l'équilibreur de charge/externe, exécutez la commande suivante :
    
        <copy>echo https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings</copy>
        
    
    L'URL appropriée sera imprimée sur l'adresse REST, par exemple :
    
        https://hello-helidon-appconf.hello-helidon.xx.xx.xx.xx.nip.io/help/allGreetings
        
2.  Utilisez ce lien pour effectuer un test à partir de votre navigateur. Cependant, en raison de certificats autosignés, vous devez accepter les risques et permettre au navigateur de poursuivre le traitement de la demande.
    
    Il est peut-être plus facile d'utiliser `curl` car la réponse n'est qu'une chaîne :
    
        <copy>curl -k https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings; echo</copy>
        
    
    Vous devriez voir le même résultat que vous avez reçu pendant le développement :
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
3.  Laissez _Cloud Shell_ ouvert. Nous l'utiliserons pour l'exercice suivant.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
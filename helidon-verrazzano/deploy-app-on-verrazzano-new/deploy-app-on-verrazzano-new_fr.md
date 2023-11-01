# Déployer l'application Helidon sur Verrazzano

## Présentation

Cet atelier vous guide tout au long du processus de déploiement de l'application Hello Helidon.

Temps estimé : 10 minutes

### Verrazzano et déploiement d'application

Verrazzano prend en charge la définition d'application à l'aide d'[Open Application Model (OAM)](https://oam.dev/). Les applications Verrazzano sont composées de composants et de configurations d'applications.

Lorsque vous déployez des applications avec Verrazzano, Verrazzano effectue une gestion intelligente des charges de travail. La plate-forme configure des connexions et des stratégies réseau, des entrées dans le maillage de service et connecte une pile de surveillance pour capturer les mesures, les journaux et les traces. Verrazzano utilise des composants OAM pour définir les unités fonctionnelles d'un système qui sont ensuite assemblées et configurées en définissant des configurations d'application associées.

![Verrazzano](images/Verrazzano_IntelligentWorkload.png)

### Objectifs

Dans cet exercice, vous allez :

*   Vérifiez que l'installation de l'environnement Verrazzano a réussi.
*   Déployez l'application Hello Helidon.
*   Vérifiez le déploiement de l'application Hello Helidon.

### Prérequis

Pour exécuter cet exercice, vous devez disposer des éléments suivants :

*   Cluster Kubernetes (OKE) exécuté sur Oracle Cloud Infrastructure.
*   L'installation de Verrazzano a démarré sur un cluster Kubernetes (OKE).

## Tâche 1 : vérification d'une installation Verrazzano réussie

Verrazzano installe plusieurs objets dans plusieurs espaces de noms. Les composants Verrazzano sont installés dans l'espace de noms _verrazzano-system_.

1.  Vérifiez que tous les pods associés aux objets multiples ont le statut _En cours d'exécution_.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $   kubectl get pods -n verrazzano-system
        NAME                                         READY   STATUS RESTARTS AGE
        coherence-operator-b5dc669c6-zm4dw           1/1     Running   1     153m
        fluentd-5s7hh                                2/2     Running   1     144m
        fluentd-bt4t4                                2/2     Running   1     144m
        fluentd-ghmkx                                2/2     Running   1     144m
        oam-kubernetes-runtime-5b48f944b-9bv2v       1/1     Running   0     154m
        verrazzano-application-operator-f976cf96f    1/1     Running   0     152m
        verrazzano-application-operator-webhook      1/1     Running   0     152m
        verrazzano-authproxy-58f8975c5d-jhx72        3/3     Running   0     151m
        verrazzano-cluster-operator-544d494f49       1/1     Running   0     144m
        verrazzano-cluster-operator-webhook-79       1/1     Running   0     144m
        verrazzano-console-6f59f7485d-xrdt8          2/2     Running   0     147m
        verrazzano-monitoring-operator-d88cdd66c     2/2     Running   0     151m
        vmi-system-es-master-0                       2/2     Running   0     151m
        vmi-system-grafana-78cd975dd9-vfjv5          3/3     Running   0     151m
        vmi-system-kiali-57d859dc4b-bnvjh            2/2     Running   0     151m
        vmi-system-osd-7687d6fccf-cjwqr              2/2     Running   0     147m
        weblogic-operator-54979449f4-t9q26           2/2     Running   0     152m
        weblogic-operator-webhook-f7ff8c8cf-nc4r     1/1     Running   0     152m
        

## Tâche 2 : déployer l'application Hello Helidon

1.  Créez un espace de noms `hello-helidon` pour l'application QuickStart-mp Helidon. Nous conserverons tous les artefacts Kubernetes dans un espace de noms distinct.
    
        <copy>kubectl create namespace hello-helidon</copy>
        
    
    > Les espaces de noms permettent d'organiser les clusters en sous-clusters virtuels. Nous pouvons avoir un nombre illimité d'espaces de noms dans un cluster, chacun séparé logiquement des autres, mais avec la possibilité de communiquer entre eux.
    
2.  Nous devons informer Verrazzano que nous stockons dans cet espace de noms les artefacts Verrazzano. Nous devons donc ajouter un libellé identifiant l'espace de noms `hello-helidon` comme géré par Verrazzano. Les libellés sont destinés à être utilisés pour spécifier des attributs d'identification d'objets significatifs et pertinents pour les utilisateurs.
    
    Ici, pour l'espace de noms `hello-helidon`, nous lui attachons un libellé, qui marque cet espace de noms comme étant géré par Verrazzano. La commande _istio-injection=enabled_ active un "sidecar" Istio et, en tant que tel, permet d'établir un proxy Istio. Avec un proxy Istio, nous pouvons accéder à d'autres services Istio comme une passerelle Istio. Pour ajouter le libellé à l'espace de noms `hello-helidon` avec les attributs mentionnés précédemment, copiez la commande suivante et exécutez-la dans Cloud Shell :
    
        <copy>kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled</copy>
        
3.  Désormais, nous voulons déployer l'application en conteneur Hello Helidon sur _cluster1_. Pour cela, nous avons besoin d'une configuration de déploiement Kubernetes. Ce déploiement indique à Kubernetes de créer et de mettre à jour des instances pour l'application Hello Helidon. Ici, nous avons le fichier `hello-helidon-comp.yaml`, qui indique à Kubernetes.
    
    Pour déployer l'application Hello Helidon, copiez et collez les deux commandes suivantes comme indiqué. Le fichier `hello-helidon-comp.yaml` contient les définitions de divers composants OAM, où un composant OAM est une ressource personnalisée Kubernetes décrivant la composition générale et les exigences d'environnement d'une application.
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    Le fichier `hello-helidon-app.yaml` est un fichier de configuration d'application Verrazzano, qui fournit des personnalisations propres à l'environnement.
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-app.yaml -n hello-helidon</copy>
        
    
    > L'action de déploiement de l'application avec verrazzano effectue la tâche de gestion intelligente de la charge de travail suivante.
    
    *   Création de comptes de service et de stratégie d'autorisation si nécessaire
    *   Déployer l'application
    *   Configurez la passerelle et les services virtuels selon vos besoins.
    *   Configurez les certificats, les clés secrètes et les stratégies réseau requis avec Istio.
    *   Coup d'envoi de la gestion automatisée de l'observabilité
        *   Mise au rebut des journaux dans OpenSearch
        *   Extraction de toutes les informations de surveillance dans Prometheus et Grafana
4.  Attendez que les pods aient le statut _En cours d'exécution_. Utilisez cette commande _kubectl_ pour attendre que tous les pods soient à l'état _Running_ dans l'espace de noms hello-helidon. Cela prend environ 1-2 minutes.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    Lorsque les pods sont prêts, vous pouvez voir une réponse similaire :
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    Vous pouvez également répertorier directement les pods pour vérifier leur statut :
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## Tâche 3 : vérifier le déploiement réussi de l'application Hello Helidon

1.  Vérifiez l'adresse `/greet`. Pour déterminer l'URL construite à partir de la configuration de l'adresse IP et de l'application de l'équilibreur de charge/externe, exécutez la commande suivante :
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet</copy>
        
    
    L'URL appropriée sera imprimée sur l'adresse REST, par exemple :
    
        https://hello-helidon.hello-helidon.xx.xx.xx.xx.nip.io/greet
        
2.  Utilisez ce lien pour effectuer un test à partir de votre navigateur. Cependant, en raison de certificats autosignés, vous devez accepter les risques et permettre au navigateur de poursuivre le traitement de la demande.
    
    Il est peut-être plus facile d'utiliser `curl` car la réponse n'est qu'une chaîne :
    
        <copy>curl -k https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet; echo</copy>
        
    
    Vous devriez voir le même résultat que vous avez reçu pendant le développement :
    
        {"message":"Hello World!"}
        
3.  Laissez _Cloud Shell_ ouvert. Nous l'utiliserons pour l'exercice suivant.
    

## En savoir plus sur Open Application Model

**Composants Verrazzano**

Un composant OAM Verrazzano est une [ressource personnalisée Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) décrivant la composition générale et les exigences d'environnement d'une application.

Le code suivant présente un composant d'application Helidon simple pour l'application Hello Helidon utilisée dans cet exercice. Cette ressource décrit un composant implémenté par une seule image Docker contenant une application Helidon exposant une seule adresse.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: hello-helidon-component
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: hello-helidon-workload
          labels:
            app: hello-helidon
            version: v1
        spec:
          deploymentTemplate:
            metadata:
              name: hello-helidon-deployment
            podSpec:
              containers:
                - name: hello-helidon-container
                  image: "ghcr.io/verrazzano/example-helidon-greet-app-v1:1.0.0-1-20230126194830-31cd41f"
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

**Configurations d'application Verrazzano**

Une configuration d'application Verrazzano est une ressource personnalisée Kubernetes qui fournit des personnalisations propres à l'environnement. Le code suivant indique la configuration de l'application pour l'exemple Helidon _quickstart-mp_ utilisé dans cet exercice. Cette ressource indique le déploiement de l'application vers l'espace de noms hello-helidon.

Des fonctionnalités d'exécution supplémentaires sont spécifiées à l'aide de caractéristiques ou de superpositions d'exécution qui augmentent la charge globale. Par exemple, la caractéristique entrante indique l'hôte et le chemin entrants, tandis que la caractéristique de mesures fournit le racleur Prometheus utilisé pour obtenir les mesures liées à l'application.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: hello-helidon
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
                    scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: hello-helidon-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/greet"
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

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
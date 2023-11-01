# Tomcat-Anwendung auf Verrazzano bereitstellen

## Einführung

In dieser Übung erfahren Sie, wie Sie das Docking-Image der tomcat-Beispielanwendung bereitstellen.

Geschätzte Zeit: 10 Minuten

### Verrazzano und Anwendungs-Deployment

Verrazzano unterstützt die Anwendungsdefinition mit [Open Application Model (OAM)](https://oam.dev/). Verrazzano-Anwendungen bestehen aus Komponenten und Anwendungskonfigurationen.

Wenn Sie Anwendungen mit Verrazzano bereitstellen, richtet die Plattform Verbindungen und Netzwerk-Policys ein, dringt in das Service-Mesh ein und verdrahtet einen Überwachungsstack, um die Metriken, Logs und Traces zu erfassen. Verrazzano verwendet OAM-Komponenten, um die Funktionseinheiten eines Systems zu definieren, die dann zusammengestellt und konfiguriert werden, indem zugehörige Anwendungskonfigurationen definiert werden.

### Verrazzano-Komponenten

Eine Verrazzano OAM-Komponente ist eine [benutzerdefinierte Kubernetes-Ressource](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/), die den allgemeinen Aufbau und die Umgebungsanforderungen einer Anwendung beschreibt.

Der folgende Code zeigt eine einfache Tomcat-Anwendungskomponente für Tomcat-Beispielanwendungen, die in dieser Übung verwendet werden. Diese Ressource beschreibt eine Komponente, die von einem einzelnen Docker-Image implementiert wird, das eine Tomcat-Anwendung enthält, die einen einzelnen Endpunkt bereitstellt.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: tomcat-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: tomcat-workload
          labels:
            app: tomcat
            version: v2
        spec:
          containers:
          - name: tomcat-container
            image: "ENDPOINT_OF_REGION/TENANCY_NAMESPACE/tomcat-example-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: ecnj
    

Eine kurze Beschreibung der einzelnen Felder der Komponente:

*   **apiVersion** - Version der benutzerdefinierten Komponentendefinition
*   **kind** - Standardname der benutzerdefinierten Komponentendefinition
*   **metadata.name**: Der Name, mit dem die benutzerdefinierte Ressource der Komponente erstellt wird
*   **spec.workload.kind**: ContainerizedWorkload definiert eine zustandslose Workload von Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports**: Vom Container bereitgestellte Ports

### Verrazzano-Anwendungskonfigurationen

Eine Verrazzano-Anwendungskonfiguration ist eine benutzerdefinierte Kubernetes-Ressource, die umgebungsspezifische Anpassungen bereitstellt. Der folgende Code zeigt die Anwendungskonfiguration für das Anwendungsbeispiel tomcat, das in dieser Übung verwendet wird. Diese Ressource gibt das Deployment der Anwendung im Namensraum tomcat-ns an.

Zusätzliche Laufzeit-Features werden mithilfe von Merkmalen oder Laufzeit-Overlays angegeben, welche die Workload erweitern. Beispiel: Das Ingress-Trait gibt den Ingress-Host und den Ingress-Pfad an, während das Metrik-Trait den Prometheus-Scraper bereitstellt, mit dem die anwendungsbezogenen Metriken abgerufen werden.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: tomcat-appconf
      annotations:
        version: v1.0.0
        description: "Verrazzano demo application"
    spec:
      components:
        - componentName: tomcat-component
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
                  name: tomcat-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
    

Eine kurze Beschreibung der einzelnen Felder in der Anwendungskonfiguration:

*   **apiVersion**: Version der benutzerdefinierten Ressourcendefinition ApplicationConfiguration
*   **kind** - Standardname der benutzerdefinierten Ressourcendefinition der Anwendungskonfiguration
*   **metadata.name**: Der Name, mit dem diese Anwendungskonfigurationsressource erstellt wird
*   **spec.components** - Referenz auf die Komponenten der Anwendung, die zur Angabe der Laufzeitkonfiguration verwendet werden
*   **spec.components\[\].traits** - Die Eigenschaften, die für die Komponenten der Anwendung angegeben sind

Um Merkmale zu untersuchen, können wir die Felder eines Ingress-Traits untersuchen:

*   **apiVersion** - Version der benutzerdefinierten Ressourcendefinition des OAM-Traits
*   **kind** - IngressTrait ist der Name der benutzerdefinierten Ressourcendefinition des Ingress-Traits der OAM-Anwendung.
*   **spec.rules.paths**: Die Kontextpfade für den Zugriff auf die Anwendung

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Stellen Sie die tomcat-Beispielanwendung bereit.
*   Prüfen Sie das Deployment der Tomcat-Beispielanwendung.

### Voraussetzungen

Um diese Übung ausführen zu können, benötigen Sie:

*   Kubernetes-(OKE-)Cluster, das auf Oracle Cloud Infrastructure ausgeführt wird.
*   Die Verrazzano-Installation wurde auf einem Kubernetes-(OKE-)Cluster gestartet.
*   In Container gepackte _`tomcat-example-your_first_name`_\-Anwendung in einer Container-Registry verfügbar.

## Aufgabe 1: Tomcat-Beispielanwendung bereitstellen

1.  Erstellen Sie einen `tomcat-ns`\-Namespace für die tomcat-Beispielanwendung. Alle Kubernetes-Artefakte werden in einem separaten Namespace gespeichert.
    
        <copy>kubectl create namespace tomcat-ns</copy>
        
    
    > Namespaces sind eine Möglichkeit, Cluster in virtuellen Subclustern zu organisieren. Wir können eine beliebige Anzahl von Namespaces innerhalb eines Clusters haben, die logisch von anderen getrennt sind, aber miteinander kommunizieren können.
    
2.  Wir müssen Verrazzano darauf aufmerksam machen, dass wir in diesem Namespace Verrazzano-Artefakte speichern. Daher müssen wir ein Label hinzufügen, das den `tomcat-ns`\-Namespace wie von Verrazzano verwaltet identifiziert. Labels dienen dazu, identifizierende Attribute von Objekten anzugeben, die für Benutzer relevant und relevant sind.
    
    Hier wird dem Namespace `tomcat-ns` ein Label zugeordnet, das diesen Namespace als von Verrazzano verwaltet markiert. Die Option _istio-injection=enabled_ aktiviert einen Istio-"sidecar" und hilft somit beim Aufbau eines Istio-Proxys. Mit einem Istio-Proxy können wir auf andere Istio-Dienste wie ein Istio-Gateway zugreifen. Um das Label mit den zuvor genannten Attributen dem Namespace `tomcat-ns` hinzuzufügen, kopieren Sie den folgenden Befehl, und führen Sie ihn in der Cloud Shell aus:
    
        <copy>kubectl label namespace tomcat-ns verrazzano-managed=true istio-injection=enabled</copy>
        
3.  In _~/tomcat-examples/_ finden Sie die Konfigurationsdatei für die tomcatsample-Anwendung. Im Rahmen von Übung 2 haben wir ein neues Docker-Image für die Anwendungskomponente erstellt. Außerdem haben wir dieses Docker-Image in das Oracle Cloud Container Registry-Repository übertragen. In dieser Übung ändern wir die Datei _tomcat-comp.yaml_ so, dass das neue Docker-Image aus dem Oracle Cloud Container Registry-Repository übernommen wird. Um die Datei _tomcat-comp.yaml_ zu ändern, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>vi ~/tomcat-examples/tomcat-comp.yaml</copy>
        
4.  Im Rahmen von Übung 3 haben Sie den vollständigen Namen Ihres Docker-Images gespeichert. Sie müssen die folgende Zeile kopieren und in Ihren Texteditor einfügen. Dann müssen Sie `docker image full name` durch Ihren Docker-Imagenamen ersetzen. Kopieren Sie dann die geänderte Zeile, und drücken Sie _i_, um den Text in die Datei `*tomcat-comp.yaml*` einzufügen. Fügen Sie die Ausgabe unter Zeilennummer **19** ein (stellen Sie sicher, dass Sie die Einrückung beibehalten), drücken Sie dann _Esc_, und geben Sie _:wq_ ein, um die Datei zu speichern.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Linie einfügen](images/insert-line.png " ")
    
5.  Jetzt möchten wir Tomcat Container-Anwendung auf _cluster1_ bereitstellen. Dazu benötigen wir eine Kubernetes-Deployment-Konfiguration. Dieses Deployment weist Kubernetes an, Instanzen für die tomcat-Anwendung zu erstellen und zu aktualisieren. Hier ist die Datei `tomcat-comp.yaml` enthalten, die Kubernetes anweist.
    
    Um die tomcat-Beispielanwendung bereitzustellen, kopieren Sie die folgenden beiden Befehle, und fügen Sie sie ein (siehe Abbildung). Die Datei `tomcat-comp.yaml` enthält Definitionen verschiedener OAM-Komponenten, wobei eine OAM-Komponente eine benutzerdefinierte Kubernetes-Ressource ist, die die allgemeine Zusammensetzung und die Umgebungsanforderungen einer Anwendung beschreibt.
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-comp.yaml -n tomcat-ns</copy>
        
    
    Die Datei `tomcat-app.yaml` ist eine Verrazzano-Anwendungskonfigurationsdatei, die umgebungsspezifische Anpassungen bereitstellt.
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-app.yaml -n tomcat-ns</copy>
        
6.  Warten Sie, bis sich die Pods im Status _Wird ausgeführt_ befinden. Mit diesem _kubectl_\-Befehl können Sie warten, bis alle Pods im Namespace _tomcat-ns_ den Status _Running_ aufweisen. Es dauert etwa 1-2 Minuten.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s</copy>
        
    
    Wenn die Pods bereit sind, wird eine ähnliche Antwort angezeigt:
    
        $ kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s
        pod/tomcat-workload-57d75d66d9-598lq condition met
        pod/tomcat-workload-57d75d66d9-b2m4l condition met
        
    
    Sie können die Pods auch direkt auflisten, um ihren Status zu überprüfen:
    
        <copy>kubectl  get po -n tomcat-ns</copy>
        
    
        $ kubectl  get po -n tomcat-ns
        NAME                               READY   STATUS    RESTARTS   AGE
        tomcat-workload-57d75d66d9-598lq   2/2     Running   0          57s
        tomcat-workload-57d75d66d9-b2m4l   2/2     Running   0          54s
        $
        

## Aufgabe 2: Erfolgreiches Deployment der Tomcat-Anwendung prüfen

1.  Prüfen Sie den Endpunkt `/sample-webapp/`. Um die URL zu bestimmen, die aus der IP- und Anwendungskonfiguration des externen Load Balancers erstellt wurde, führen Sie den folgenden Befehl aus:
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io tomcat-ns-tomcat-appconf-gw -n tomcat-ns -o jsonpath={.spec.servers[0].hosts[0]})/sample-webapp/</copy>
        
    
    Dadurch wird die richtige URL zu Ihrem REST-Endpunkt gedruckt. Beispiel:
    
        https://tomcat-appconf.tomcat-ns.xx.xx.xx.xx.nip.io/sample-webapp/
        
2.  Fügen Sie die URL in den Browser ein, und klicken Sie auf _Zum Aufrufen eines Servlets klicken_. ![Anwendungs-URL](images/application-url.png)
    
3.  _Host Name_ und _IP Address_ werden angezeigt. Öffnen Sie dieselbe URL in einem anderen _Incognito-Fenster_. Sie werden feststellen, dass diese Anforderung von einer anderen Serverinstanz bearbeitet wird. ![Hostname](images/host-name.png) ![Load Balancing anzeigen](images/show-loadbalancing.png)
    

> Es zeigt das Load Balancing zwischen zwei Replikaten.

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
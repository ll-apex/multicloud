# Springboot-Anwendung auf Verrazzano bereitstellen

## Einführung

In dieser Übung wird beschrieben, wie Sie das Docking-Image der Springboot-Anwendung in OKE bereitstellen.

Geschätzte Zeit: 10 Minuten

### Verrazzano und Anwendungs-Deployment

Verrazzano unterstützt die Anwendungsdefinition mit [Open Application Model (OAM)](https://oam.dev/). Verrazzano-Anwendungen bestehen aus Komponenten und Anwendungskonfigurationen.

Wenn Sie Anwendungen mit Verrazzano bereitstellen, richtet die Plattform Verbindungen und Netzwerk-Policys ein, dringt in das Service-Mesh ein und verdrahtet einen Überwachungsstack, um die Metriken, Logs und Traces zu erfassen. Verrazzano verwendet OAM-Komponenten, um die Funktionseinheiten eines Systems zu definieren, die dann zusammengestellt und konfiguriert werden, indem zugehörige Anwendungskonfigurationen definiert werden.

### Verrazzano-Komponenten

Eine Verrazzano OAM-Komponente ist eine [benutzerdefinierte Kubernetes-Ressource](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/), die den allgemeinen Aufbau und die Umgebungsanforderungen einer Anwendung beschreibt.

Der folgende Code zeigt eine einfache Springboot-Anwendungskomponente für die Tomcat-Beispielanwendung, die in dieser Übung verwendet wird. Diese Ressource beschreibt eine Komponente, die von einem einzelnen Docker-Image implementiert wird, das eine Springboot-Anwendung enthält, die einen einzelnen Endpunkt bereitstellt.

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
    

Eine kurze Beschreibung der einzelnen Felder der Komponente:

*   **apiVersion** - Version der benutzerdefinierten Komponentendefinition
*   **kind** - Standardname der benutzerdefinierten Komponentendefinition
*   **metadata.name**: Der Name, mit dem die benutzerdefinierte Ressource der Komponente erstellt wird
*   **spec.workload.kind**: ContainerizedWorkload definiert eine zustandslose Workload von Kubernetes
*   **spec.workload.spec.containers.ports.containerPort**: Vom Container bereitgestellte Ports

### Verrazzano-Anwendungskonfigurationen

Eine Verrazzano-Anwendungskonfiguration ist eine benutzerdefinierte Kubernetes-Ressource, die umgebungsspezifische Anpassungen bereitstellt. Der folgende Code zeigt die Anwendungskonfiguration für das in dieser Übung verwendete Springboot-Anwendungsbeispiel. Diese Ressource gibt das Deployment der Anwendung im Springboot-Namespace an.

Zusätzliche Laufzeit-Features werden mithilfe von Merkmalen oder Laufzeit-Overlays angegeben, welche die Workload erweitern. Beispiel: Das Ingress-Trait gibt den Ingress-Host und den Ingress-Pfad an, während das Metrik-Trait den Prometheus-Scraper bereitstellt, mit dem die anwendungsbezogenen Metriken abgerufen werden.

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

*   Springboot-Anwendung bereitstellen.
*   Prüfen Sie das Deployment der Springboot-Anwendung.

### Voraussetzungen

Um diese Übung ausführen zu können, benötigen Sie:

*   Kubernetes-(OKE-)Cluster, das auf Oracle Cloud Infrastructure ausgeführt wird.
*   Die Verrazzano-Installation wurde auf einem Kubernetes-(OKE-)Cluster gestartet.
*   In Container gepackte _`springboot-your_firstname`_\-Anwendung in einer Container-Registry verfügbar.

## Aufgabe 1: Spring Boot-Anwendung bereitstellen

1.  Erstellen Sie einen `springboot`\-Namespace für die Springboot-Anwendung. Alle Kubernetes-Artefakte werden in einem separaten Namespace gespeichert.
    
        <copy>kubectl create namespace springboot</copy>
        
    
    > Namespaces sind eine Möglichkeit, Cluster in virtuellen Subclustern zu organisieren. Wir können eine beliebige Anzahl von Namespaces innerhalb eines Clusters haben, die logisch von anderen getrennt sind, aber miteinander kommunizieren können.
    
2.  Wir müssen Verrazzano darauf aufmerksam machen, dass wir in diesem Namespace Verrazzano-Artefakte speichern. Daher müssen wir ein Label hinzufügen, das den `springboot`\-Namespace wie von Verrazzano verwaltet identifiziert. Labels dienen dazu, identifizierende Attribute von Objekten anzugeben, die für Benutzer relevant und relevant sind.
    
    Hier wird dem Namespace `springboot` ein Label zugeordnet, das diesen Namespace als von Verrazzano verwaltet markiert. Die Option _istio-injection=enabled_ aktiviert einen Istio-"sidecar" und hilft somit beim Aufbau eines Istio-Proxys. Mit einem Istio-Proxy können wir auf andere Istio-Dienste wie ein Istio-Gateway zugreifen. Um das Label mit den zuvor genannten Attributen dem Namespace `springboot` hinzuzufügen, kopieren Sie den folgenden Befehl, und führen Sie ihn in der Cloud Shell aus:
    
        <copy>kubectl label namespace springboot verrazzano-managed=true istio-injection=enabled</copy>
        
3.  In _~/springboot-app_ ist die Konfigurationsdatei für die Springboot-Anwendung vorhanden. Im Rahmen von Übung 2 haben wir ein neues Docker-Image für die Anwendungskomponente erstellt. Außerdem haben wir dieses Docker-Image in das Oracle Cloud Container Registry-Repository übertragen. In dieser Übung ändern wir die Datei _springboot-comp.yaml_ so, dass das neue Docker-Image aus dem Oracle Cloud Container Registry-Repository übernommen wird. Um die Datei _springboot-comp.yaml_ zu ändern, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>vi ~/springboot-app/springboot-comp.yaml</copy>
        
4.  Im Rahmen von Übung 2 haben Sie den vollständigen Namen Ihres Docker-Images gespeichert. Sie müssen die folgende Zeile kopieren und in Ihre Textdatei einfügen. Dann müssen Sie `docker image full name` durch Ihren Docker-Imagenamen ersetzen. Kopieren Sie dann die geänderte Zeile, und drücken Sie _i_, um den Text in die Datei `*springboot-comp.yaml*` einzufügen. Fügen Sie die Ausgabe unter Zeilennummer **19** ein (stellen Sie sicher, dass Sie die Einrückung beibehalten), drücken Sie dann _Esc_, und geben Sie _:wq_ ein, um die Datei zu speichern.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Linie einfügen](images/insert-line.png " ")
    
5.  Jetzt möchten wir containerisierte Springboot-Anwendung auf _cluster1_ bereitstellen. Dazu benötigen wir eine Kubernetes-Deployment-Konfiguration. Dieses Deployment weist Kubernetes an, Instanzen für die Springboot-Anwendung zu erstellen und zu aktualisieren. Hier ist die Datei `springboot-comp.yaml` enthalten, die Kubernetes anweist.
    
    Um die Springboot-Anwendung bereitzustellen, kopieren Sie die folgenden beiden Befehle, und fügen Sie sie ein (siehe Abbildung). Die Datei `springboot-comp.yaml` enthält Definitionen verschiedener OAM-Komponenten, wobei eine OAM-Komponente eine benutzerdefinierte Kubernetes-Ressource ist, die die allgemeine Zusammensetzung und die Umgebungsanforderungen einer Anwendung beschreibt.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-comp.yaml -n springboot</copy>
        
    
    Die Datei `springboot-app.yaml` ist eine Verrazzano-Anwendungskonfigurationsdatei, die umgebungsspezifische Anpassungen bereitstellt.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-app.yaml -n springboot</copy>
        
6.  Warten Sie, bis sich die Pods im Status _Wird ausgeführt_ befinden. Mit diesem _kubectl_\-Befehl können Sie warten, bis alle Pods im Namespace _springboot_ den Status _Running_ aufweisen. Es dauert etwa 1-2 Minuten.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s</copy>
        
    
    Wenn die Pods bereit sind, wird eine ähnliche Antwort angezeigt:
    
        $ kubectl wait --for=condition=Ready pods --all -n kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s --timeout=600s
        pod/springboot-workload-5c4ffc8954-mbgbb condition met
        pod/springboot-workload-bfb9d487-dwp7q condition met
        
    
    Sie können die Pods auch direkt auflisten, um ihren Status zu überprüfen:
    
        <copy>kubectl  get po -n springboot</copy>
        
    
        $ kubectl  get po -n springboot
        NAME                                 READY   STATUS    RESTARTS   AGE
        springboot-workload-bfb9d487-dwp7q   2/2     Running   0          68s
        $
        

## Aufgabe 2: Prüfen des erfolgreichen Deployments der Springboot-Anwendung

1.  Prüfen Sie den Endpunkt `/facts`. Um die URL zu bestimmen, die aus der IP- und Anwendungskonfiguration des externen Load Balancers erstellt wurde, führen Sie den folgenden Befehl aus:
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io springboot-springboot-appconf-gw -n springboot -o jsonpath={.spec.servers[0].hosts[0]})/facts</copy>
        
    
    Dadurch wird die richtige URL zu Ihrem REST-Endpunkt gedruckt. Beispiel:
    
        https://springboot-appconf.springboot.xx.xx.xx.xx.nip.io/facts
        
2.  Fügen Sie die URL in den Browser ein, und klicken Sie im Browser auf die Schaltfläche _Aktualisieren_. Jedes Mal, wenn Sie auf die Schaltfläche Aktualisieren klicken, erhalten Sie eine zufällige Tatsache über Verrazzano. ![Anwendungs-URL](images/application-url.png) ![Eine weitere Tatsache](images/another-fact.png)
    

Sie können jetzt **mit der nächsten Übung fortfahren**.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
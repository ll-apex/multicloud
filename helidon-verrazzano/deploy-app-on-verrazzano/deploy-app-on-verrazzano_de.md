# Helidon-Anwendung auf Verrazzano bereitstellen

## Einführung

In dieser Übung werden Sie durch das Deployment der Helidon Quickstart-mp-Anwendung geführt.

Geschätzte Zeit: 10 Minuten

### Verrazzano und Anwendungs-Deployment

Verrazzano unterstützt die Anwendungsdefinition mit [Open Application Model (OAM)](https://oam.dev/). Verrazzano-Anwendungen bestehen aus Komponenten und Anwendungskonfigurationen.

Wenn Sie Anwendungen mit Verrazzano bereitstellen, richtet die Plattform Verbindungen und Netzwerk-Policys ein, dringt in das Service-Mesh ein und verdrahtet einen Überwachungsstack, um die Metriken, Logs und Traces zu erfassen. Verrazzano verwendet OAM-Komponenten, um die Funktionseinheiten eines Systems zu definieren, die dann zusammengestellt und konfiguriert werden, indem zugehörige Anwendungskonfigurationen definiert werden.

### Verrazzano-Komponenten

Eine Verrazzano OAM-Komponente ist eine [benutzerdefinierte Kubernetes-Ressource](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/), die den allgemeinen Aufbau und die Umgebungsanforderungen einer Anwendung beschreibt.

Der folgende Code zeigt eine einfache Helidon-Anwendungskomponente für die in dieser Übung verwendete Helidon-Anwendung _quickstart-mp_. Diese Ressource beschreibt eine Komponente, die von einem einzelnen Docker-Image implementiert wird, das eine Helidon-Anwendung enthält, die einen einzelnen Endpunkt bereitstellt.

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
    

Eine kurze Beschreibung der einzelnen Felder der Komponente:

*   **apiVersion** - Version der benutzerdefinierten Komponentendefinition
*   **kind** - Standardname der benutzerdefinierten Komponentendefinition
*   **metadata.name**: Der Name, mit dem die benutzerdefinierte Ressource der Komponente erstellt wird
*   **metadata.namespace**: Der Namespace, mit dem die benutzerdefinierte Ressource dieser Komponente erstellt wird
*   **spec.workload.kind**: VerrazzanoHelidonWorkload definiert eine zustandslose Workload von Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name**: Der Name, mit dem die zustandslose Workload von Kubernetes erstellt wird
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - Die Implementierungscontainer
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports**: Vom Container bereitgestellte Ports

### Verrazzano-Anwendungskonfigurationen

Eine Verrazzano-Anwendungskonfiguration ist eine benutzerdefinierte Kubernetes-Ressource, die umgebungsspezifische Anpassungen bereitstellt. Der folgende Code zeigt die Anwendungskonfiguration für das in dieser Übung verwendete Helidon-Beispiel _quickstart-mp_. Diese Ressource gibt das Deployment der Anwendung im hello-helidon-Namespace an.

Zusätzliche Laufzeit-Features werden mithilfe von Merkmalen oder Laufzeit-Overlays angegeben, welche die Workload erweitern. Beispiel: Das Ingress-Trait gibt den Ingress-Host und den Ingress-Pfad an, während das Metrik-Trait den Prometheus-Scraper bereitstellt, mit dem die anwendungsbezogenen Metriken abgerufen werden.

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
    

Eine kurze Beschreibung der einzelnen Felder in der Anwendungskonfiguration:

*   **apiVersion**: Version der benutzerdefinierten Ressourcendefinition ApplicationConfiguration
*   **kind** - Standardname der benutzerdefinierten Ressourcendefinition der Anwendungskonfiguration
*   **metadata.name**: Der Name, mit dem diese Anwendungskonfigurationsressource erstellt wird
*   **metadata.namespace**: Der Namespace, der für diese benutzerdefinierte Ressource der Anwendungskonfiguration verwendet wird
*   **spec.components** - Referenz auf die Komponenten der Anwendung, die zur Angabe der Laufzeitkonfiguration verwendet werden
*   **spec.components\[\].traits** - Die Eigenschaften, die für die Komponenten der Anwendung angegeben sind

Um Merkmale zu untersuchen, können wir die Felder eines Ingress-Traits untersuchen:

*   **apiVersion** - Version der benutzerdefinierten Ressourcendefinition des OAM-Traits
*   **kind** - IngressTrait ist der Name der benutzerdefinierten Ressourcendefinition des Ingress-Traits der OAM-Anwendung.
*   **spec.rules.paths**: Die Kontextpfade für den Zugriff auf die Anwendung

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Prüfen Sie die erfolgreiche Installation der Verrazzano-Umgebung.
*   Stellen Sie die Helidon-Anwendung _quickstart-mp_ bereit.
*   Prüfen Sie das Deployment der Helidon-Anwendung _quickstart-mp_.

### Voraussetzungen

Um diese Übung ausführen zu können, benötigen Sie:

*   Kubernetes-(OKE-)Cluster, das auf Oracle Cloud Infrastructure ausgeführt wird.
*   Die Verrazzano-Installation wurde auf einem Kubernetes-(OKE-)Cluster gestartet.
*   In Container gepackte Helidon _quickstart-mp_\-Anwendung in einer Containerregistrierung verfügbar.

## Aufgabe 1: Überprüfung einer erfolgreichen Verrazzano-Installation

Verrazzano installiert mehrere Objekte in mehreren Namespaces. Verrazzano-Komponenten werden im Namespace _verrazzano-system_ installiert.

1.  Stellen Sie sicher, dass alle Pods, die den mehreren Objekten zugeordnet sind, den Status _Wird ausgeführt_ haben. Pods mit dem Status _Wird ausgeführt_, wie unten gezeigt.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
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
        

## Aufgabe 2: Helidon Quickstart-mp-Anwendung bereitstellen

1.  Laden Sie die YAML-Datei der Verrazzano OAM-Komponente und die Konfigurationsdateien der Verrazzano-Anwendung in der Cloud Shell-Umgebung herunter:
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-app.yaml >~/hello-helidon-app.yaml
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-comp.yaml >~/hello-helidon-comp.yaml
        cd ~
        </copy>
        
2.  Ändern Sie den Bildnamen in _hello-helidon-comp.yaml_. Sie können den `vi`\-Editor verwenden:
    
        <copy>vi ~/hello-helidon-comp.yaml</copy>
        
3.  Verwenden Sie `i`, um den Einfügemodus zu ändern und den Bildnamen entsprechend Ihrem Repository-Pfad in Zeile 23 zu ändern:
    
        image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0"
        
    
    Beispiel:
    
        image: "ocir.io/tenancynamespace/quickstart-mp-your_first_name:1.0"
        
4.  Verwenden Sie `Esc` im Einfügemodus "Beenden", und geben Sie `:wq` ein, um die Änderungen zu speichern und den Editor zu schließen.
    
5.  Erstellen Sie einen `hello-helidon`\-Namespace für die Helidon quickstart-mp-Anwendung. Alle Kubernetes-Artefakte werden in einem separaten Namespace gespeichert.
    
        <copy>
        kubectl create namespace hello-helidon
        </copy>
        
    
    > Namespaces sind eine Möglichkeit, Cluster in virtuellen Subclustern zu organisieren. Wir können eine beliebige Anzahl von Namespaces innerhalb eines Clusters haben, die logisch von anderen getrennt sind, aber miteinander kommunizieren können.
    
6.  Wir müssen Verrazzano darauf aufmerksam machen, dass wir in diesem Namespace Verrazzano-Artefakte speichern. Daher müssen wir ein Label hinzufügen, das den `hello-helidon`\-Namespace wie von Verrazzano verwaltet identifiziert. Labels dienen dazu, identifizierende Attribute von Objekten anzugeben, die für Benutzer relevant und relevant sind.
    
    Hier wird dem Namespace `hello-helidon` ein Label zugeordnet, das diesen Namespace als von Verrazzano verwaltet markiert. Die Option _istio-injection=enabled_ aktiviert einen Istio-"sidecar" und hilft somit beim Aufbau eines Istio-Proxys. Mit einem Istio-Proxy können wir auf andere Istio-Dienste wie ein Istio-Gateway zugreifen. Um das Label mit den zuvor genannten Attributen dem Namespace `hello-helidon` hinzuzufügen, kopieren Sie den folgenden Befehl, und führen Sie ihn in der Cloud Shell aus:
    
        <copy>
        kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled
        </copy>
        
7.  Jetzt möchten wir die containerisierte Helidon-Anwendung _quickstart-mp_ auf _cluster1_ bereitstellen. Dazu benötigen wir eine Kubernetes-Deployment-Konfiguration. Dieses Deployment weist Kubernetes an, Instanzen für die Helidon-Anwendung _quickstart-mp_ zu erstellen und zu aktualisieren. Hier ist die Datei `hello-helidon-comp.yaml` enthalten, die Kubernetes anweist.
    
    Um die Helidon-Anwendung _quickstart-mp_ bereitzustellen, kopieren Sie die folgenden beiden Befehle, und fügen Sie sie ein (siehe Abbildung). Die Datei `hello-helidon-comp.yaml` enthält Definitionen verschiedener OAM-Komponenten, wobei eine OAM-Komponente eine benutzerdefinierte Kubernetes-Ressource ist, die die allgemeine Zusammensetzung und die Umgebungsanforderungen einer Anwendung beschreibt.
    
        <copy>kubectl apply -f ~/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    Die Datei `hello-helidon-app.yaml` ist eine Verrazzano-Anwendungskonfigurationsdatei, die umgebungsspezifische Anpassungen bereitstellt.
    
        <copy>kubectl apply -f ~/hello-helidon-app.yaml -n hello-helidon</copy>
        
8.  Warten Sie, bis sich die Pods im Status _Wird ausgeführt_ befinden. Mit diesem _kubectl_\-Befehl können Sie warten, bis sich alle Pods im hello-helidon-Namespace im Status _Wird ausgeführt_ befinden. Es dauert etwa 1-2 Minuten.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    Wenn die Pods bereit sind, wird eine ähnliche Antwort angezeigt:
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    Sie können die Pods auch direkt auflisten, um ihren Status zu überprüfen:
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## Aufgabe 3: Erfolgreiches Deployment der Helidon quickstart-mp-Anwendung prüfen

1.  Prüfen Sie den Endpunkt `help/allGreetings`. Um die URL zu bestimmen, die aus der IP- und Anwendungskonfiguration des externen Load Balancers erstellt wurde, führen Sie den folgenden Befehl aus:
    
        <copy>echo https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings</copy>
        
    
    Dadurch wird die richtige URL zu Ihrem REST-Endpunkt gedruckt. Beispiel:
    
        https://hello-helidon-appconf.hello-helidon.xx.xx.xx.xx.nip.io/help/allGreetings
        
2.  Verwenden Sie diesen Link, um in Ihrem Browser zu testen. Aufgrund von selbstsignierten Zertifikaten müssen Sie jedoch Risiken akzeptieren und dem Browser erlauben, die Anforderungsverarbeitung fortzusetzen.
    
    Möglicherweise ist die Verwendung von `curl` einfacher, weil die Antwort nur eine Zeichenfolge ist:
    
        <copy>curl -k https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings; echo</copy>
        
    
    Sie sollten dasselbe Ergebnis sehen, das Sie während der Entwicklung erhalten haben:
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
3.  Lassen Sie _Cloud Shell_ geöffnet. Sie werden es für die nächste Übung verwenden.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
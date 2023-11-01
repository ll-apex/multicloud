# Bobby's Books-Beispielanwendung bereitstellen

## Einführung

### Über Bobby's Books Anwendung

![Bobby's Bücher Anwendung](images/bobbyoverview.png " ")

[Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/) besteht aus drei Hauptteilen:

*   Eine Backend-Anwendung zur _Auftragsverarbeitung_, eine Java EE-Anwendung mit REST-Services und eine sehr einfache JSP-UI, die Daten in einer MySQL-Datenbank speichert. Diese Anwendung wird auf WebLogic Server ausgeführt.
*   Ein Frontend-Online-Shop _"Roberts Bücher"_, der ein allgemeiner Buchverkäufer ist. Dies wird als Helidon-Microservice implementiert, der Buchdaten aus Coherence abruft, einen Coherence-Cachespeicher verwendet, um Daten für den Auftragsmanager zu persistieren, und über eine React-Web-UI verfügt.
*   Ein Frontend-Online-Shop _"Bobby's Books"_, bei dem es sich um eine Kinderbuchhandlung handelt. Dies wird als Helidon-Microservice implementiert, der Buchdaten aus einem (anderen) Coherence-Cachespeicher abruft, direkt mit dem Auftragsmanager interagiert und eine JSF-Web-UI auf WebLogic Server ausführt.

Weitere Informationen und den Quellcode dieser Anwendung finden Sie unter [Verrazzano-Beispiele](https://github.com/verrazzano/examples).

### Verrazzano und Anwendungs-Deployment

Verrazzano unterstützt die Anwendungsdefinition mit [Open Application Model (OAM)](https://oam.dev/). Verrrazzano-Anwendungen bestehen aus Komponenten und Anwendungskonfigurationen.

Wenn Sie Anwendungen mit Verrazzano bereitstellen, richtet die Plattform Verbindungen, Netzwerk-Policys und Ingress-Vorgänge im Service-Mesh ein und verdrahtet einen Überwachungsstack, um die Metriken, Logs und Traces zu erfassen. Verrazzano verwendet OAM-Komponenten, um die Funktionseinheiten eines Systems zu definieren, die dann zusammengestellt und konfiguriert werden, indem zugehörige Anwendungskonfigurationen definiert werden.

### Verrazzano-Komponenten

Eine Verrazzano OAM-Komponente ist eine [benutzerdefinierte Kubernetes-Ressource](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/), die den allgemeinen Aufbau und die Umgebungsanforderungen einer Anwendung beschreibt.

Der folgende Code zeigt die eine Komponente für die Beispielanwendung Bobby's Books, die in dieser Übung verwendet wird. Diese Ressource beschreibt eine Komponente, die von einem einzelnen Docker-Image implementiert wird, das eine Helidon-Anwendung enthält, die einen einzelnen Endpunkt bereitstellt.

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
    

Eine kurze Beschreibung der einzelnen Felder der Komponente:

*   **apiVersion** - Version der benutzerdefinierten Komponentendefinition
*   **kind** - Standardname der benutzerdefinierten Komponentendefinition
*   **metadata.name**: Der Name, mit dem die benutzerdefinierte Ressource der Komponente erstellt wird
*   **metadata.namespace**: Der Namespace, mit dem die benutzerdefinierte Ressource dieser Komponente erstellt wird
*   **spec.workload.kind**: VerrazzanoHelidonWorkload definiert eine zustandslose Workload von Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name**: Der Name, mit dem die zustandslose Workload von Kubernetes erstellt wird
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - Die Implementierungscontainer
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports**: Vom Container bereitgestellte Ports

Die vollständige Komponentenbeschreibung für die Bobbys-Bücher-Anwendung finden Sie in der Datei [bobs-books-comp.yaml](https://github.com/verrazzano/verrazzano/blob/master/examples/bobs-books/bobs-books-comp.yaml).

### Verrazzano-Anwendungskonfigurationen

Eine Verrazzano-Anwendungskonfiguration ist eine benutzerdefinierte Kubernetes-Ressource, die umgebungsspezifische Anpassungen bereitstellt. Der folgende Code zeigt die Anwendungskonfiguration für das Beispiel "Bob's Books", das in dieser Übung verwendet wird. Diese Ressource gibt das Deployment der Anwendung im Namespace "bobs-books" an.

Zusätzliche Laufzeit-Features werden mithilfe von Merkmalen oder Laufzeit-Overlays angegeben, welche die Workload erweitern. Beispiel: Das Ingress-Trait gibt den Ingress-Host und den Ingress-Pfad an, während das Metrik-Trait den Prometheus-Scraper bereitstellt, mit dem die anwendungsbezogenen Metriken abgerufen werden.

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

In dieser Übung erfahren Sie, wie Sie die Beispielanwendung "Bobby's Books" bereitstellen.

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Akzeptieren Sie die Lizenz zum Herunterladen der Images aus den Repositorys in der Oracle Container Registry.
*   Beispielanwendung "Bobby's Books" bereitstellen.
*   Prüfen Sie, ob die Bobby's Books-Beispielanwendung erfolgreich bereitgestellt wurde.

### Voraussetzungen

Um Übung 3 ausführen zu können, benötigen Sie:

*   Führen Sie Übung 1 aus, in der ein OKE-Cluster auf Oracle Cloud Infrastructure erstellt wird.

\* Führen Sie Übung 1 aus, in der kubectl für den Zugriff auf ein OKE-Cluster in Oracle Cloud Infrastructure konfiguriert wird.

*   Führen Sie Übung 2 aus, in der Verrazzano auf dem Kubernetes-Cluster installiert wird.
*   Ein Texteditor, in den Sie die Befehle und URLs einfügen und entsprechend Ihrer Umgebung ändern können. Anschließend können Sie die geänderten Befehle zum Ausführen in die _Cloud Shell_ kopieren und einfügen.

## Aufgabe 1: Lizenzvereinbarung zum Herunterladen der Images aus den Repositorys in der Oracle Container Registry akzeptieren

Für das Deployment der Beispielanwendung _Bobby's Books_ verwenden wir die Beispielbilder. Da diese Images Oracle-Produkte enthalten, müssen Sie die Lizenzvereinbarung akzeptieren, bevor Sie diese Images in der Bobs-Books-Datei `bobs-books-comp.yaml` verwenden können.

1.  Klicken Sie auf den Link für die Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/), und melden Sie sich an. Dazu benötigen Sie einen Oracle Account.
    
    ![Anmelden](images/registrysignin.png " ")
    
2.  Geben Sie Ihre _Oracle-Accountzugangsdaten_ in die Felder "Benutzername" und "Kennwort" ein, und klicken Sie auf _Anmelden_. Später verwenden wir diese Zugangsdaten, um ein Secret in Kubernetes zu erstellen.
    
    ![Oracle SSO](images/oraclesso.png " ")
    
3.  Wählen Sie auf der Homepage _Verrazzano_ aus.
    
    ![Registrierungshomepage](images/registryhomepage.png " ")
    
4.  Beispiel: bobbys-coherence, example-bobbys-front-end, example-bobs-books-order-manager und example-roberts-coherence repository. Wählen Sie _Englisch_ als Sprache aus, und klicken Sie auf _Weiter_.
    
    ![Fortfahren](images/continue.png " ")
    
5.  Klicken Sie auf _Akzeptieren_, um den Lizenzvertrag zu akzeptieren.
    
    ![Vereinbarung akzeptieren](images/acceptagreement.png " ")
    
6.  Stellen Sie sicher, dass Sie die Lizenzvereinbarung für die Repositorys für Verrazzano akzeptiert haben, wie in der folgenden Abbildung dargestellt.
    
    ![Lizenzvertrag prüfen](images/verifyaggrement.png " ")
    
7.  Suchen Sie auf der Homepage von Oracle Container Registry nach _weblogic_
    
    ![WebLogic durchsuchen](images/searchweblogic.png " ")
    
8.  Klicken Sie wie gezeigt auf _weblogic_, und akzeptieren Sie die Lizenz wie für Verrazzano Imagaes. ![Klicken Sie auf "Weblogik".](images/clickweblogic.png) ![WebLogic akzeptieren](images/acceptagreement.png " ")
    

## Aufgabe 2: Bobby's Books-Anwendung bereitstellen

Sie müssen den Quellcode herunterladen, in dem die Konfigurationsdateien `bobs-books-app.yaml` und `bobs-books-comp.yaml` vorhanden sind.

1.  Laden Sie die yaml-Datei der Verrazzano OAM-Komponente und die Verrazzano Application Configuration-Dateien des Bobby's Book-Beispiels herunter. Klicken Sie auf _Kopieren_, und fügen Sie den Befehl wie dargestellt in die Cloud Shell ein:
    
        <copy>
        curl -LSs  https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-app.yaml >~/bobs-books-app.yaml
        curl -LSs https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-comp.yaml >~/bobs-books-comp.yaml
        cd ~
        </copy>
        
2.  Alle Kubernetes-Artefakte werden im separaten Namespace aufbewahrt. Erstellen Sie einen Namespace für die Beispielanwendung "Bob's Books". Namespaces sind eine Möglichkeit, Cluster in virtuellen Subclustern zu organisieren. Wir können eine beliebige Anzahl von Namespaces innerhalb eines Clusters haben, die logisch von anderen getrennt sind, aber miteinander kommunizieren können. Außerdem müssen wir Verrazzano darauf hinweisen, dass wir in diesem Namespace Verrazzano-Artefakte speichern. Daher müssen wir ein Label hinzufügen, das den von Verrazzano verwalteten Namensraum für Bobs-Books identifiziert. Labels dienen dazu, identifizierende Attribute von Objekten anzugeben, die für Benutzer relevant und relevant sind. Hier fügen wir für den bobs-book-Namespace ein Label hinzu, das diesen Namespace als von Verrazzano verwaltet markiert. Die Option _istio-injection=enabled_ aktiviert einen Istio-"sidecar" und hilft somit beim Aufbau eines Istio-Proxys. Mit einem Istio-Proxy können wir auf andere Istio-Dienste wie ein Istio-Gateway usw. zugreifen. Um das Label dem Namespace "bobs-books" mit den zuvor genannten Attributen hinzuzufügen, kopieren Sie den folgenden Befehl, und führen Sie ihn in der _Cloud Shell_ aus
    
        <copy>
        kubectl create namespace bobs-books
        kubectl label namespace bobs-books verrazzano-managed=true istio-injection=enabled
        </copy>
        
    
    ![Namespace erstellen](images/createnamespace.png " ")
    
3.  Kopieren Sie den folgenden Befehl, um das Skript herunterzuladen. Dieses Skript authentifiziert den Benutzer für Oracle Container Registry. Wenn die Authentifizierung erfolgreich ist, wird das Docker Registry Secret erstellt. Die Docker-Registry ist eine Möglichkeit, Images wie GitHub für normalen Code, aber für Container (die Kubernetes abrufen kann) zu speichern und zu versionieren. Hier wird ein Docker-Registry Secret erstellt, mit dem das Beispielimage "Bobby's Books" aus der Oracle Container Registry abgerufen werden kann. Klicken Sie für den folgenden Befehl auf _Kopieren_, und fügen Sie ihn in einen beliebigen Texteditor ein. Ersetzen Sie Benutzernamen und Kennwort durch die E-Mail-ID und das Kennwort, die bzw. das Sie in Aufgabe 1 verwendet haben, um den Lizenzvertrag zum Herunterladen von Bildern aus der Oracle Container Registry zu akzeptieren. Fügen Sie dann in der Cloud Shell den geänderten Befehl wie dargestellt ein:
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/deploy-bobsbook/create_secret.sh >~/create_secret.sh
        chmod 777 create_secret.sh
        ./create_secret.sh username 'password'    
        </copy>
        
    
    ![Secret erstellen](images/createsecret.png " ")
    
    > Geben Sie das Kennwort in einfachen Anführungszeichen ein.
    
4.  Wir müssen mehrere Kubernetes-Secrets mit Zugangsdaten erstellen. In der Bobby's Books-Anwendung haben wir zwei WebLogic-Domains _bobby-front-end_ und _bobs-bookstore_. Die Zugangsdaten für die Domain WebLogic werden in einem Kubernetes-Secret gespeichert, in dem der Name des Secrets mit _webLogicCredentialsSecret_ in der Domainressource WebLogic angegeben wird. Außerdem muss das Secret mit den Domainzugangsdaten in dem Namespace erstellt werden, in dem die Domain ausgeführt wird. Wir müssen die von WebLogic Server-Domains verwendeten Secrets _bobbys-front-end-weblogic-credentials_ und _bobs-bookstore-weblogic-credentials_ mit dem Benutzernamenwert `weblogic` und einem Kennwort erstellen, das zufällig im bobs-books-Namespace generiert wird. Unsere Bobby's Books-Anwendung verwendet eine _mysql_\-Datenbank. Daher erstellen wir ein neues Secret namens _mysql-credentials_ mit dem Benutzernamenwert `weblogic`, einem zufällig generierten Kennwort und der JDBC-URL _JDBC:mysql://mysql.bobs-books.svc.cluster.local:3306/books_ im Namespace "bobs-books". Diese Werte werden in der JDBC-Verbindungszeichenfolge im Objekt WebLogic DataSource verwendet. Kopieren Sie den Befehlsblock, und fügen Sie ihn in die _Cloud Shell_ ein.
    
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
        
    
    ![Ressource erstellen](images/createresource.png " ")
    
5.  Wir haben ein Kuberneter-Cluster, _cluster1__verrazzano_, mit drei Knoten. Jetzt möchten wir die containerisierte Bobby's Books-Anwendung auf _cluster1__verrazzano_ bereitstellen. Dazu benötigen wir eine Kubernetes-Deployment-Konfiguration. Dieses Deployment weist Kubernetes an, Instanzen für die Bobby's Books-Anwendung zu erstellen und zu aktualisieren. Hier befindet sich die Datei `bobs-books-comp.yaml`, die Kubernetes anweist, die Bobby's Books-Anwendung bereitzustellen. Kopieren Sie die folgenden beiden Befehle, und fügen Sie sie ein (siehe Abbildung). Die Datei `bobs-books-comp.yaml` enthält Definitionen verschiedener OAM-Komponenten, wobei eine OAM-Komponente eine benutzerdefinierte Kubernetes-Ressource ist, die die allgemeine Zusammensetzung und die Umgebungsanforderungen einer Anwendung beschreibt. Weitere Informationen zur Datei `bobs-books-comp.yaml` finden Sie in den Verrazzano-Komponenten im Abschnitt "Einführung" in dieser Übung 3.
    
        <copy>kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
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
        
6.  Die Datei `bobs-books-app.yaml` ist eine Verrazzano-Anwendungskonfigurationsdatei, die umgebungsspezifische Anpassungen bereitstellt. Weitere Informationen zur Datei `bobs-books-app.yaml` finden Sie in der Verrazzano-Anwendungskonfiguration im Abschnitt "Einführung" in dieser Übung 3.
    
        <copy>kubectl apply -f ~/bobs-books-app.yaml -n bobs-books</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        $ kubectl apply -f ~/bobs-books-app.yaml -n bobs-books
        applicationconfiguration.core.oam.dev/bobs-books created
        $
        
7.  Warten Sie, bis alle Pods in der Beispielanwendung "Bobby's Books" den Status _Wird ausgeführt_ aufweisen. Möglicherweise müssen Sie diesen Befehl mehrmals wiederholen, bevor er erfolgreich ist. Die Erstellung und Bereitstellung der WebLogic Server- und Coherence-Pods kann einige Zeit in Anspruch nehmen. Dieser _kubectl_\-Befehl wartet, bis alle Pods im Namespace "bobs-books" den Status _Wird ausgeführt_ aufweisen. Es dauert etwa 4-5 Minuten. Wenn Sie mehr als 5 Minuten warten, können Sie den Befehl erneut ausführen.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
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
        

## Aufgabe 3: Überprüfen Sie die erfolgreiche Bereitstellung der Bobby's Book-Anwendung

Stellen Sie sicher, dass die Anwendungskonfiguration, Domains, Coherence-Ressourcen und Ingress-Eigenschaften vorhanden sind.

1.  Um zu prüfen, ob die Anwendung _Bobby's Books_ erfolgreich im Namespace "bobs-books" bereitgestellt wurde.
    
        <copy>kubectl get ApplicationConfiguration -n bobs-books</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        $ kubectl get ApplicationConfiguration -n bobs-books
          NAME         AGE
          bobs-books   72m
        $
        
2.  Um zu prüfen, ob beide WebLogic-Domains im bobs-books-Namespace erfolgreich erstellt wurden.
    
        <copy>kubectl get Domain -n bobs-books</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        $ kubectl get Domain -n bobs-books
          NAME               AGE
          bobbys-front-end   73m
          bobs-orders-wls    73m
        $
        
3.  Um zu prüfen, ob beide Coherence-Cluster im bobs-books-Namespace erfolgreich erstellt wurden.
    
        <copy>kubectl get Coherence -n bobs-books</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        $ kubectl get Coherence -n bobs-books
        NAME              CLUSTER           ROLE           REPLICAS  READY PHASE
        bobbys-coherence  bobbys-coherence  bobbys-coherence  1       1    Ready
        roberts-coherence roberts-coherence roberts-coherence 2       2    Ready
        $
        
4.  Um die IngressTrait für die Buchanwendung von Bobby abzurufen, führen Sie den folgenden Befehl in der _Cloud Shell_ aus.
    
        <copy>kubectl get IngressTrait -n bobs-books</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        $ kubectl get IngressTrait -n bobs-books
          NAME                               AGE
          bobby-wls-trait-79b67d9d88         76m
          bobs-orders-wls-trait-57b4d8cb4b   76m
          robert-helidon-trait-54d7bcd54b    76m
        $
        
5.  Prüfen Sie, ob die Servicepods erfolgreich erstellt wurden, und wechseln Sie in den Status _Wird ausgeführt_. Beachten Sie, dass dies einige Minuten dauern kann und einige der Services möglicherweise beendet und neu gestartet werden. Schließlich sehen Sie, dass sich alle Pods, die mit dem Namespace "bobs-books" verknüpft sind, im Status _Wird ausgeführt_ befinden. Kopieren Sie die Poddetails für die _bobbys-helidon-stock-application_.
    
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
        
    
    > Notieren Sie sich den Podnamen für **bobbys-helidon-stock-application**. Wenn Sie diese Komponente erneut bereitstellen, wird dieser Pod in den Status _Wird beendet_ versetzt. In Übung 7 wird der neue Pod gestartet und im Status _Wird ausgeführt_ angezeigt.
    
    Lassen Sie _Cloud Shell_ geöffnet. Sie wird auch für die nächsten Übungen verwendet.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
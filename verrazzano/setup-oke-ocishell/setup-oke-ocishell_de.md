# Oracle Kubernetes Engine-Instanz auf Oracle Cloud Infrastructure einrichten

## Einführung

In dieser Übung werden Sie durch die Schritte zum Erstellen einer verwalteten Kubernetes-Umgebung auf Oracle Cloud Infrastructure geführt.

Geschätzte Zeit: 15 Minuten

### Über Produkt/Technologie

Oracle Cloud Infrastructure Container Engine for Kubernetes ist ein vollständig verwalteter, skalierbarer und hochverfügbarer Service, mit dem Sie Ihre Containeranwendungen in der Cloud bereitstellen können. Verwenden Sie Container Engine for Kubernetes (manchmal abgekürzt OKE), wenn Ihr Entwicklungsteam cloudnative Anwendungen zuverlässig erstellen, bereitstellen und verwalten möchte. Sie geben die Compute-Ressourcen an, die Ihre Anwendungen benötigen, und OKE stellt sie in Oracle Cloud Infrastructure in einem vorhandenen OCI-Mandanten bereit.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Erstellen Sie eine OKE-(Oracle Kubernetes Engine-)Instanz.
*   Öffnen Sie OCI Cloud Shell, und konfigurieren Sie `kubectl` für die Interaktion mit dem Kubernetes-Cluster.

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.

Gehen Sie folgendermaßen vor, um Container Engine for Kubernetes (OKE) zu erstellen:

*   Erstellen Sie die Netzwerkressourcen (VCN, Subnetze, Sicherheitslisten usw.).
*   Erstellen Sie ein Cluster.
*   Erstellen Sie eine `NodePool`.

In dieser Übung erfahren Sie, wie das Feature _Schnellstart_ alle erforderlichen Ressourcen für ein Kubernetes-Cluster mit 3 Knoten erstellt und konfiguriert. Alle Knoten werden in verschiedenen Availability-Domains bereitgestellt, um High Availability sicherzustellen.

Weitere Informationen zum OKE- und benutzerdefinierten Cluster-Deployment finden Sie in der [Oracle Container Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm)\-Dokumentation.

## Aufgabe 1: OKE-Cluster erstellen

Das Feature _Schnellerstellung_ verwendet die Standardeinstellungen, um nach Bedarf ein _schnelles Cluster_ mit neuen Netzwerkressourcen zu erstellen. Dieser Ansatz ist der schnellste Weg, um ein neues Cluster zu erstellen. Wenn Sie alle Standardwerte akzeptieren, können Sie mit nur wenigen Klicks ein neues Cluster erstellen. Neue Netzwerkressourcen für das Cluster werden automatisch zusammen mit einem Knotenpool und drei Worker-Knoten erstellt.

1.  Wählen Sie in der Konsole _Hamburger Menü -> Developer Services -> Kubernetes Clusters (OKE)_, wie gezeigt.
    
    ![Hamburger-Menü](images/hamburger-menu.png " ")
    
2.  Wählen Sie auf der Seite "Clusterliste" das Compartment Ihrer Wahl aus, in dem Sie ein Cluster erstellen können, und klicken Sie auf _Cluster erstellen_.
    
    > Sie müssen ein Compartment auswählen, in dem Sie ein Cluster erstellen dürfen, sowie ein Repository in der Oracle Container Registry.
    
    ![Compartment auswählen](images/select-compartment.png " ")
    
3.  Wählen Sie im Dialogfeld "Clusterlösung erstellen" die Option _Schnellerstellung_ aus, und klicken Sie auf _Weiterleiten_.
    
    ![Workflow starten](images/launch-workflow.png " ")
    
    Mit _Erstellen_ wird ein neues Cluster mit den Standardeinstellungen zusammen mit neuen Netzwerkressourcen für das neue Cluster erstellt.
    
    Geben Sie die folgenden Konfigurationsdetails auf der Seite "Cluster erstellen" an (bitte achten Sie auf den Wert, den Sie im Feld _Ausprägung_ eingeben):
    
    *   **Name**: Der Name des Clusters. Übernehmen Sie den Standardwert.
    *   **Compartment**: Der Name des Compartments. Wählen Sie das Compartment aus, in dem Sie Ressourcen erstellen dürfen.
    *   **Kubernetes-Version**: Die Version von Kubernetes. Wählen Sie _1.26.2_ als Kubernetes-Version aus.
    *   **Kubernetes-API-Endpunkt**: Werden die Clustermasterknoten routbar oder nicht. Wählen Sie den Wert _Öffentlicher Endpunkt_ aus.
    *   **Knotyp**: Wählen Sie _Verwaltet_ als Knotentyp aus.
    *   **Kubernetes-Worker-Knoten**: Werden die Cluster-Worker-Knoten routbar oder nicht. Übernehmen Sie den Standardwert für _Private Mitarbeiter_.
    *   **Ausprägung**: Die Ausprägung, die für jeden Knoten im Knotenpool verwendet werden soll. Die Ausprägung bestimmt die Anzahl der CPUs und die Speichermenge, die jedem Knoten zugewiesen ist. In der Liste werden nur die in Ihrem Mandanten verfügbaren Ausprägungen angezeigt, die von OKE unterstützt werden. Wählen Sie _VM.Standard.E4. Flex_ (in der Regel im Oracle Free Tier-Account verfügbar). Wählen Sie die 2 OCPUs und 32 GB als Arbeitsspeichermenge aus.
    *   **Image**: Wählen Sie das Standardimage mit Kubernetes-Version _1.26.2_ aus.
    *   **Anzahl Knoten**: Die Anzahl der zu erstellenden Worker-Knoten. Übernehmen Sie den Standardwert _3_.
    
    > _BITTE SEIEN SIE SEHR VORSICHTIG, UM DIE STANDARDFORM NICHT ZU VERLASSEN; DIE STANDARDFORM IST ZU KLEIN, UM ALLE VERRAZZANO-KOMPONENTEN ZU PASSEN_
    
    ![Schnellcluster](images/quick-cluster.png " ") ![Knotennummer](images/node-number.png " ")
    
4.  Klicken Sie auf _Weiter_, um die Details zu prüfen, die Sie für das neue Cluster eingegeben haben.
    
5.  Aktivieren Sie auf der Seite _Prüfen_ das Kontrollkästchen _Basiscluster erstellen_, und klicken Sie dann auf _Cluster erstellen_, um die neuen Netzwerkressourcen und das neue Cluster zu erstellen.
    
    ![Cluster prüfen](images/review-cluster.png " ")
    
    > Die für Sie erstellten Netzwerkressourcen werden angezeigt. Warten Sie, bis die Anforderung zum Erstellen des Knotenpools initiiert wurde, und klicken Sie dann auf _Schließen_.
    
    ![Netzwerkressource](images/network-resource.png " ")
    
    > Anschließend wird das neue Cluster auf der Seite _Clusterdetails_ angezeigt. Wenn die Masterknoten erstellt werden, erhält das neue Cluster den Status _Aktiv_ (es dauert etwa 7 Minuten). Dann können Sie Ihre Übungen fortsetzen.
    
    ![Cluster-Provisioning](images/cluster-provision.png " ")
    
    ![Clusterzugriff](images/cluster-access.png " ")
    

## Aufgabe 2: `kubectl` konfigurieren (Kubernetes-Cluster-CLI)

Oracle Cloud Infrastructure (OCI) Cloud Shell ist ein browserbasiertes Terminal, auf das von der Oracle Cloud-Konsole aus zugegriffen werden kann. Die Cloud Shell bietet Zugriff auf eine Linux-Shell mit einer vorab authentifizierten Oracle Cloud Infrastructure-CLI und anderen nützlichen Tools (_Git, kubectl, helm, OCI-CLI_), um die Verrazzano-Tutorials abzuschließen. Auf die Cloud Shell kann von der Konsole aus zugegriffen werden. Die Cloud Shell wird in der Oracle Cloud-Konsole als persistenter Frame der Konsole angezeigt und bleibt aktiv, wenn Sie zu verschiedenen Seiten der Konsole navigieren.

Sie verwenden die _Cloud Shell_, um diesen Workshop abzuschließen.

Wir verwenden `kubectl`, um das Cluster remote mit der Cloud Shell zu verwalten. Sie benötigt eine `kubeconfig`\-Datei. Diese wird mit der OCI-CLI generiert, die vorab authentifiziert ist. Daher muss kein Setup ausgeführt werden, bevor Sie sie verwenden können.

1.  Klicken Sie auf der Detailseite des Clusters auf _Auf Cluster zugreifen_.
    
    > Wenn Sie diese Seite verlassen haben, öffnen Sie das Navigationsmenü, und wählen Sie unter _Entwicklerservices_ die Option _Kubernetes-Cluster (OKE)_ aus. Wählen Sie Ihr Cluster aus, und gehen Sie zur Detailseite.
    
    ![Zugriff auf Cluster](images/access-cluster.png " ")
    
    > Es wird ein Dialogfeld angezeigt, in dem Sie die Cloud Shell öffnen können. Es enthält den benutzerdefinierten OCI-Befehl, den Sie ausführen müssen, um eine Kubernetes-Konfigurationsdatei zu erstellen.
    
2.  Behalten Sie den _Cloud Shell-Standardzugriff_ bei, und wählen Sie zuerst den Link _Kopieren_ aus, um den Befehl `oci ce...` in die Cloud Shell zu kopieren.
    
    ![kubectl-Konfiguration kopieren](images/copy-config.png " ")
    
3.  Klicken Sie jetzt auf _Cloud Shell starten_, um die integrierte Konsole zu öffnen. Schließen Sie dann das Konfigurationsdialogfeld, bevor Sie den Befehl in die _Cloud Shell_ einfügen.
    
    ![Cloud Shell starten](images/launch-cloudshell.png " ")
    
4.  Kopieren Sie den Befehl aus der Zwischenablage (Ctrl+V oder klicken Sie mit der rechten Maustaste, und kopieren Sie ihn in die Cloud Shell, und führen Sie den Befehl aus.
    
    Beispiel: Der Befehl sieht wie folgt aus:
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl-Konfiguration](images/kube-config.png " ")
    
5.  Prüfen Sie jetzt, ob `kubectl` funktioniert, z.B. mit dem Befehl `get node`. Sie müssen diesen Befehl möglicherweise mehrmals ausführen, bis die Ausgabe ähnlich der folgenden angezeigt wird.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.192   Ready    node    11m   v1.26.2
        10.0.10.219   Ready    node    11m   v1.26.2
        10.0.10.90    Ready    node    11m   v1.26.2
        
    
    > Wenn die Knoteninformationen angezeigt werden, war die Konfiguration erfolgreich.
    
6.  Sie können die Terminalgröße jederzeit mit den Steuerelementen in der oberen rechten Ecke von Cloud Shell minimieren und wiederherstellen.
    
    ![cloud shell](images/cloudshell.png " ")
    

Lassen Sie diese _Cloud Shell_ geöffnet. Sie werden sie für weitere Übungen verwenden.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
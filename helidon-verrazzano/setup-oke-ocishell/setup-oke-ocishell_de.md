# Oracle Kubernetes Engine-Instanz auf Oracle Cloud Infrastructure einrichten

## Einführung

In dieser Übung erstellen Sie ein Kubernetes-Cluster mit 3 Knoten, das mit allen erforderlichen Netzwerkressourcen konfiguriert ist. Die Knoten werden in verschiedenen Faultdomains bereitgestellt, um High Availability sicherzustellen.

Weitere Informationen zum OKE- und benutzerdefinierten Cluster-Deployment finden Sie in der [Oracle Kubernetes Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm)\-Dokumentation.

Geschätzte Zeit: 15 Minuten

### Über Produkt/Technologie

Oracle Cloud Infrastructure Container Engine for Kubernetes ist ein vollständig verwalteter, skalierbarer und hochverfügbarer Service, mit dem Sie Ihre Containeranwendungen in der Cloud bereitstellen können. Verwenden Sie Container Engine for Kubernetes (manchmal abgekürzt OKE), wenn Ihr Entwicklungsteam cloudnative Anwendungen zuverlässig erstellen, bereitstellen und verwalten möchte. Sie geben die Compute-Ressourcen an, die Ihre Anwendungen benötigen, und OKE stellt sie in Oracle Cloud Infrastructure in einem vorhandenen OCI-Mandanten bereit.

### Ziele

Mit dem Clusterfeature _Schnellerstellung_ können Sie eine OKE-(Oracle Kubernetes Engine-)Instanz mit den erforderlichen Netzwerkressourcen, einem Knotenpool und drei Worker-Knoten erstellen. Mit der _Schnellerstellung_ können Sie am schnellsten ein neues Cluster erstellen. Wenn Sie alle Standardwerte akzeptieren, können Sie mit nur wenigen Klicks ein neues Cluster erstellen.

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.

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
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
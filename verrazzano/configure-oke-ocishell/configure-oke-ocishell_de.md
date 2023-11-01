# KUBECTL für die Interaktion mit Oracle Container Engine for Kubernetes (OKE) auf Oracle Cloud Infrastructure (OCI) konfigurieren

## Einführung

In dieser Übung werden die Schritte zum Erstellen einer Konfigurationsdatei erläutert, die den Zugriff auf die Kubernetes-Umgebung in Oracle Cloud Infrastructure ermöglicht.

### Über Produkt/Technologie

Oracle Cloud Infrastructure Container Engine for Kubernetes ist ein vollständig verwalteter, skalierbarer und hochverfügbarer Service, mit dem Sie Ihre Containeranwendungen in der Cloud bereitstellen können. Verwenden Sie Container Engine for Kubernetes (manchmal abgekürzt OKE), wenn Ihr Entwicklungsteam cloudnative Anwendungen zuverlässig erstellen, bereitstellen und verwalten möchte. Sie geben die Compute-Ressourcen an, die Ihre Anwendungen benötigen, und OKE stellt sie in Oracle Cloud Infrastructure in einem vorhandenen OCI-Mandanten bereit.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Öffnen Sie OCI Cloud Shell, und konfigurieren Sie `kubectl` für die Interaktion mit dem Kubernetes-Cluster.

### Voraussetzungen

*   Dieser Workshop setzt voraus, dass Sie diesen Workshop auf LiveLabs buchen.

## Aufgabe 1: `kubectl` konfigurieren (Kubernetes-Cluster-CLI)

Oracle Cloud Infrastructure (OCI) Cloud Shell ist ein browserbasiertes Terminal, auf das von der Oracle Cloud-Konsole aus zugegriffen werden kann. Die Cloud Shell bietet Zugriff auf eine Linux-Shell mit einer vorab authentifizierten Oracle Cloud Infrastructure-CLI und anderen nützlichen Tools (_Git, kubectl, helm, OCI-CLI_), um die Verrazzano-Tutorials abzuschließen. Auf die Cloud Shell kann von der Konsole aus zugegriffen werden. Die Cloud Shell wird in der Oracle Cloud-Konsole als persistenter Frame der Konsole angezeigt und bleibt aktiv, wenn Sie zu verschiedenen Seiten der Konsole navigieren.

Sie verwenden die _Cloud Shell_, um diesen Workshop abzuschließen.

Wir verwenden `kubectl`, um das Cluster remote mit der Cloud Shell zu verwalten. Sie benötigt eine `kubeconfig`\-Datei. Diese wird mit der OCI-CLI generiert, die vorab authentifiziert ist. Daher muss kein Setup ausgeführt werden, bevor Sie sie verwenden können.

1.  Wählen Sie in der Konsole _Hamburger Menü -> Developer Services -> Kubernetes Clusters (OKE)_, wie gezeigt.
    
    ![Hamburger-Menü](../setup-oke-ocishell/images/hamburgermenu.png " ")
    
2.  Wählen Sie das Compartment aus, das Ihnen zugewiesen ist. Klicken Sie dann auf das Cluster _cluster1_.
    
3.  Klicken Sie auf der Detailseite des Clusters auf _Auf Cluster zugreifen_.
    
    ![Zugriff auf Cluster](../setup-oke-ocishell/images/accesscluster.png " ")
    
    > Es wird ein Dialogfeld angezeigt, in dem Sie die Cloud Shell öffnen können. Es enthält den benutzerdefinierten OCI-Befehl, den Sie ausführen müssen, um eine Kubernetes-Konfigurationsdatei zu erstellen.
    
4.  Behalten Sie den _Cloud Shell-Standardzugriff_ bei, und wählen Sie zuerst den Link _Kopieren_ aus, um den Befehl `oci ce...` in die Cloud Shell zu kopieren.
    
    ![kubectl-Konfiguration kopieren](../setup-oke-ocishell/images/copyconfig.png " ")
    
5.  Klicken Sie jetzt auf _Cloud Shell starten_, um die integrierte Konsole zu öffnen. Schließen Sie dann das Konfigurationsdialogfeld, bevor Sie den Befehl in die _Cloud Shell_ einfügen.
    
    ![Cloud Shell starten](../setup-oke-ocishell/images/launchcloudshell.png " ")
    
6.  Kopieren Sie den Befehl aus der Zwischenablage (Ctrl+V oder klicken Sie mit der rechten Maustaste, und kopieren Sie ihn in die Cloud Shell, und führen Sie den Befehl aus.
    
    Beispiel: Der Befehl sieht wie folgt aus:
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl-Konfiguration](../setup-oke-ocishell/images/kubeconfig.png " ")
    
7.  Prüfen Sie jetzt, ob `kubectl` funktioniert, z.B. mit dem Befehl `get node`. Sie müssen diesen Befehl möglicherweise mehrmals ausführen, bis die Ausgabe ähnlich der folgenden angezeigt wird.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.17    Ready    node    11m   v1.21.5
        10.0.10.184   Ready    node    11m   v1.21.5
        10.0.10.31    Ready    node    11m   v1.21.5
        
    
    > Wenn die Knoteninformationen angezeigt werden, war die Konfiguration erfolgreich.
    
8.  Sie können die Terminalgröße jederzeit mit den Steuerelementen in der oberen rechten Ecke von Cloud Shell minimieren und wiederherstellen.
    
    ![cloud shell](../setup-oke-ocishell/images/cloudshell.png " ")
    

Lassen Sie diese _Cloud Shell_ geöffnet. Sie werden sie für weitere Übungen verwenden.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Februar 2023
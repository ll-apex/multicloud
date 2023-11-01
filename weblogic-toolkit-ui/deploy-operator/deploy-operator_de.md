# Operator WebLogic für Oracle Kubernetes-Cluster (OKE) bereitstellen

## Einführung

In dieser Übung authentifizieren wir die OCI-CLI mit dem Browser. Dadurch wird die Datei _.OCI/config_ erstellt. Wie Sie kubectl verwenden, um das Cluster remote mit dem _lokalen Zugriff_ zu verwalten. Er benötigt eine _kubeconfig_\-Datei. Diese kubeconfig-Datei wird mit der OCI-CLI generiert. Anschließend prüfen wir die Konnektivität zum Kubernetes-Cluster über die WebLogic Kubernetes Toolkit-UI. Schließlich installieren wir den Kubernetes-Operator WebLogic in Kubernetes-Cluster (OKE).

Geschätzte Zeit: 10 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Operator WebLogic für OKE-Cluster bereitstellen](videohub:1_0itbllhe)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Konfigurieren Sie kubectl (Kubernetes-Cluster-CLI) für die Verbindung mit dem Kubernetes-Cluster.
*   Prüfen Sie die Konnektivität der WebLogic Kubernetes Toolkit-UI zu Kubernetes-Cluster.
*   Installieren Sie den Kubernetes-Operator WebLogic in einem Kubernetes-Cluster.

## Aufgabe 1: kubectl (Kubernetes-Cluster-CLI) für die Verbindung mit dem Oracle Kubernetes-Cluster konfigurieren

In dieser Aufgabe wird die Konfigurationsdatei _.oci/config_ und _.kube/config_ im Verzeichnis _/home/opc_ erstellt. Mit dieser Konfigurationsdatei können wir über diese virtuelle Maschine auf Oracle Kubernetes Cluster (OKE) zugreifen.

1.  Klicken Sie auf _Aktivitäten_, und geben Sie _Firefox_ in das Suchfeld ein. Klicken Sie auf das Symbol für _Firefox_. ![open firefox](images/open-firefox.png)
    
2.  Öffnen Sie die URL [https://cloud.oracle.com](https://cloud.oracle.com). Geben Sie Ihren _Cloud-Accountnamen_ und dann Ihre Zugangsdaten für den Oracle Cloud-Account ein, und klicken Sie auf _Anmelden_.
    
3.  Wählen Sie in der Konsole das _Hamburger-Menü_ -> _Developer Services_ -> _Kubernetes-Cluster (OKE)_ wie dargestellt aus. ![OKE-Symbol](images/oke-icon.png)
    
4.  Klicken Sie auf den Clusternamen, den Sie in Übung 3 erstellt haben, und klicken Sie dann auf _Auf Cluster zugreifen_. ![Zugriff auf Cluster](images/access-cluster.png)
    
5.  Wählen Sie _Lokaler Zugriff_ aus, und klicken Sie wie dargestellt auf _Kopieren_. ![Lokaler Zugriff](images/local-access.png)
    
6.  Klicken Sie auf _Aktivitäten_, und wählen Sie das _Terminal_ aus. ![Ende](images/click-terminal.png)
    
7.  Fügen Sie den kopierten Befehl in das Terminal ein. _Möchten Sie eine neue Konfigurationsdatei erstellen?_ Geben Sie _y_ ein, und drücken Sie dann die _Eingabetaste_. _Möchten Sie die Konfigurationsdatei durch Anmeldung über einen Browser erstellen?_ Geben Sie _y_ ein, und drücken Sie dann die _Eingabetaste_. ![OCI-Konfiguration](images/oci-config.png)
    
8.  Klicken Sie im Firefox-Browser auf Ihre aktive Sitzung.
    
    > Die _Autorisierung abgeschlossen_ wird wie dargestellt angezeigt. ![Autorisierung abgeschlossen](images/authorization-complete.png)
    
9.  Geben Sie unter _Passphrase für den Private Key eingeben_ keine Passphrase ein, und drücken Sie die _Eingabetaste_. ![Leere Passphrase](images/empty-passphrase.png)
    
10.  Führen Sie den Befehl _oce ce ..._ mit der oberen Pfeiltaste erneut aus, und führen Sie ihn mehrmals erneut aus, bis die _Neue Konfiguration in die Kubeconfig-Datei /home/opc/.kube/config_ geschrieben wird. ![Erstellen Sie KubeConfig.](images/create-kubeconfig.png)
    

## Aufgabe 2: Konnektivität der Kubernetes Toolkit-UI von WebLogic mit Oracle Kubernetes-Cluster prüfen

In dieser Aufgabe prüfen wir die Konnektivität zu _Oracle Kubernetes Cluster (OKE)_ aus der Anwendung `WebLogic Kubernetes Toolkit UI`.

1.  Gehen Sie zurück zur WebLogic-UI des Kubernetes-Toolkits, klicken Sie auf _Aktivitäten_, und wählen Sie das UI-Fenster WebLogic des Kubernetes-Toolkits aus.
    
2.  Klicken Sie auf _Kubernetes_ -> _Clientkonfiguration_ und dann auf _Konnektivität prüfen_. ![Verbindung prüfen](images/verify-connectivity.png)
    
3.  Wenn das Fenster _Kubernetes-Clientkonnektivität prüfen_ angezeigt wird, klicken Sie auf _OK_. ![Verbindung erfolgreich hergestellt](images/successfully-connected.png)
    

## Aufgabe 3: Kubernetes-Operator WebLogic in Oracle Kubernetes-Cluster installieren

Dieser Abschnitt bietet Unterstützung für die Installation des Kubernetes-Operators WebLogic (der "Operator") im Ziel-Kubernetes-Cluster.

1.  Klicken Sie auf _WebLogic Operator_. Geben Sie die folgenden Konfigurationsdetails an, und klicken Sie auf _Installationsoperator_.
    
    **Kubernetes-Namespace**: Der Kubernetes-Namespace, in dem der Operator installiert werden soll. Übernehmen Sie den Standardwert.  
    **Kubernetes-Serviceaccount** - Der Kubernetes-Serviceaccount, den der Operator beim Erstellen von Kubernetes-API-Anforderungen verwenden soll. Übernehmen Sie den Standardwert.  
    **Helm-Releasename für die Operatorinstallation** - Der Helm-Releasename zur Identifizierung dieser Installation. Übernehmen Sie den Standardwert.  
    
    ![WebLogic-Operatotr](images/weblogic-operator.png)
    
    > **Nur zu Informationszwecken:**  
    > ! Standardmäßig ist das Feld _Zu verwendendes Imagetag_ des Operators auf das Imagetag gesetzt, das der neuesten Operatorreleaseversion in der GitHub Container Registry entspricht.  
    > Der Operator muss wissen, welche WebLogic-Domains im Kubernetes-Cluster er verwaltet. Dies geschieht auf Kubernetes-Namespace-Ebene. Daher wird jede WebLogic-Domain in einem Kubernetes-Namespace, für die der Operator konfiguriert ist, von der zu installierenden Operatorinstanz verwaltet.  
    > Im Feld _Kubernetes-Namespace-Auswahlstrategie_ wählen wir _Labelselektor_ aus. Das bedeutet, dass jeder Kubernetes-Namespace mit dem Label _weblogic-operator=enabled_ von diesem Operator verwaltet wird.  
    > ![Operatorbild](images/operator-image.png)
    
    > Durch Aktivieren von "Cluster-Rollen-Binding aktivieren" erstellt der Operator eine Kubernetes-Datei ClusterRole und ClusterRoleBinding, die der Operator für alle verwalteten Namespaces verwendet.  
    > Standardmäßig wird die REST-API des Operators nicht außerhalb des Kubernetes-Clusters bereitgestellt. Damit die REST-API verfügbar gemacht werden kann, können Sie _REST-API extern bereitstellen_ aktivieren. [Rollen-Binding](images/role-binding.png)  
    
    > In diesem Bereich können Sie die Java-Loggingkonfiguration des Operators außer Kraft setzen. Dies kann nützlich sein, wenn Sie Probleme mit dem Operator debuggen.  
    > ![Java-Protokollierung](images/java-logging.png)  
    
    > Weitere Informationen zu _WebLogic Kubernetes-Operatorimage_, _Kubernetes-Namespace-Auswahlstrategie_, _WebLogic Kubernetes-Rollen-Bindings_, _Externer REST-API-Zugriff_, _Integrationen von Drittanbietern_ und _Java Logging_ finden Sie in der Dokumentation zu [WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/).
    
2.  Wenn _WebLogic Kubernetes Operator Installation Complete_ angezeigt wird, klicken Sie auf _OK_. ![Operator installiert](images/operator-installed.png)
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
# Operator WebLogic für Oracle Kubernetes-Cluster (OKE) bereitstellen

## Einführung

In dieser Übung authentifizieren wir die OCI-CLI mit dem Browser. Dadurch wird die Datei _.OCI/config_ erstellt. Wie Sie kubectl verwenden, um das Cluster remote mit dem _lokalen Zugriff_ zu verwalten. Er benötigt eine _kubeconfig_\-Datei. Diese kubeconfig-Datei wird mit der OCI-CLI generiert. Anschließend prüfen wir die Konnektivität zum Kubernetes-Cluster über die WebLogic Kubernetes Toolkit-UI. Schließlich installieren wir den Kubernetes-Operator WebLogic in Kubernetes-Cluster (OKE).

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Konfigurieren Sie kubectl (Kubernetes-Cluster-CLI) für die Verbindung mit dem Kubernetes-Cluster.
*   Prüfen Sie die Konnektivität der WebLogic Kubernetes Toolkit-UI zu Kubernetes-Cluster.
*   Installieren Sie den Kubernetes-Operator WebLogic in einem Kubernetes-Cluster.

## Aufgabe 1: kubectl (Kubernetes-Cluster-CLI) für die Verbindung mit dem Oracle Kubernetes-Cluster konfigurieren

In dieser Aufgabe wird die Konfigurationsdatei _.oci/config_ und _.kube/config_ im Verzeichnis _/home/opc_ erstellt. Mit dieser Konfigurationsdatei können wir über diese virtuelle Maschine auf Oracle Kubernetes Cluster (OKE) zugreifen.

1.  Öffnen Sie in Firefox die Cloud-Konsole, und wählen Sie **Hamburger-Menü** -> **Entwicklerservices** -> **Kubernetes-Cluster (OKE)** wie dargestellt. ![OKE-Symbol](images/oke-icon.png)
    
2.  Klicken Sie auf den Clusternamen, den Sie in Übung 3 erstellt haben, und klicken Sie dann auf **Auf Cluster zugreifen**. ![Zugriff auf Cluster](images/access-cluster.png)
    
3.  Wählen Sie **Lokaler Zugriff** aus, und klicken Sie wie dargestellt auf **Kopieren**. ![Lokaler Zugriff](images/local-access.png)
    
4.  Klicken Sie auf **Aktivitäten**, und wählen Sie das **Terminal** aus. ![Ende](images/click-terminal.png)
    
5.  Fügen Sie den kopierten Befehl in das Terminal ein. **Möchten Sie eine neue Konfigurationsdatei erstellen?** Geben Sie **y** ein, und drücken Sie dann die **Eingabetaste**. **Möchten Sie die Konfigurationsdatei durch Anmeldung über einen Browser erstellen?** Geben Sie **y** ein, und drücken Sie dann die **Eingabetaste**. ![OCI-Konfiguration](images/oci-config.png)
    
6.  Klicken Sie im Firefox-Browser auf Ihre aktive Sitzung.
    
    > Die _Autorisierung abgeschlossen_ wird wie dargestellt angezeigt. ![Autorisierung abgeschlossen](images/authorization-complete.png)
    
7.  Geben Sie unter **Passphrase für den Private Key eingeben** keine Passphrase ein, und drücken Sie die **Eingabetaste**. ![Leere Passphrase](images/empty-passphrase.png)
    
8.  Führen Sie den Befehl **oce ce ...** mit der oberen Pfeiltaste erneut aus, und führen Sie ihn mehrmals erneut aus, bis die **Neue Konfiguration in die Kubeconfig-Datei /home/opc/.kube/config** geschrieben wird. ![Erstellen Sie KubeConfig.](images/create-kubeconfig.png)
    
9.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Dateiberechtigungen **~/.kube/config** zu ändern.
    
        <copy>chmod 600 ~/.kube/config</copy>
        

## Aufgabe 2: Cloud-Ressourcen von Oracle WebLogic Server für OKE-Stack anzeigen

1.  Klicken Sie in der OCI-Konsole auf das **Navigationsmenü**, und wählen Sie **Entwicklerservices** aus. Klicken Sie unter der Gruppe "Ressourcenmanager" auf **Stacks**.
    
2.  Wählen Sie das **Compartment** aus, das Ihren Stack enthält.
    
3.  Klicken Sie auf den Namen des Stacks.
    
4.  Navigieren Sie zur Registerkarte **Anwendungsinformationen**. Sie können die vom Stack erstellten Ressourcen anzeigen. Kopieren Sie die **öffentliche Bastioninstanz-IP**, und fügen Sie sie in die Textdatei ein. ![Bastion kopieren](images/copy-bastion.png)
    

## Aufgabe 3: Proxy-Konfiguration im Terminal aktivieren

In dieser Aufgabe aktivieren wir das **SSH-Tunneling** vom Remote-Desktop zu Oracle Kubernetes-Clusterknoten.

1.  Öffnen Sie das Terminal, kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Private-Key-Datei auf dem Remote-Desktop zu erstellen.
    
        <copy>vi ~/.ssh/opc.key</copy>
        
2.  Drücken Sie **i**, um in den Einfügemodus zu wechseln, fügen Sie den Inhalt des Private Keys in diese Datei ein, und drücken Sie die Escape-Taste. Geben Sie dann **:wq** ein, um diese Datei zu speichern.
    
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Berechtigungen für die Private-Key-Datei zu ändern.
    
        <copy>chmod 600 ~/.ssh/opc.key</copy>
        
4.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein. Ersetzen Sie dann **bastion-public-ip** durch die öffentliche IP, die Sie in der letzten Aufgabe kopiert haben.
    
        <copy>ssh -D 1088 -fCqN -i ~/.ssh/opc.key opc@bastion-public-ip</copy>
        
5.  Öffnen Sie die Datei **~/.kube/config**, kopieren Sie die folgende Datei, und fügen Sie sie wie unten gezeigt in diese Datei ein.
    
        <copy>proxy-url: socks5://localhost:1088</copy>
        
    
    ![Proxykonfiguration](images/proxy-config.png)
    

## Aufgabe 4: Konnektivität der Kubernetes Toolkit-UI von WebLogic mit Oracle Kubernetes-Cluster prüfen

In dieser Aufgabe prüfen wir die Konnektivität zu _Oracle Kubernetes Cluster (OKE)_ aus der Anwendung `WebLogic Kubernetes Toolkit UI`.

1.  Gehen Sie zurück zur WebLogic-UI des Kubernetes-Toolkits, klicken Sie auf _Aktivitäten_, und wählen Sie das UI-Fenster WebLogic des Kubernetes-Toolkits aus.
    
2.  Klicken Sie auf _Kubernetes_ -> _Clientkonfiguration_ und dann auf _Konnektivität prüfen_. ![Verbindung prüfen](images/verify-connectivity.png)
    
3.  Wenn das Fenster _Kubernetes-Clientkonnektivität prüfen_ angezeigt wird, klicken Sie auf _OK_. ![Verbindung erfolgreich hergestellt](images/successfully-connected.png)
    

## Aufgabe 5: Kubernetes-Operator WebLogic in Oracle Kubernetes-Cluster aktualisieren

Dieser Abschnitt bietet Unterstützung für die Installation des Kubernetes-Operators WebLogic (der "Operator") im Ziel-Kubernetes-Cluster.

1.  Klicken Sie auf **WebLogic Operator**. Geben Sie die folgenden Konfigurationsdetails an, und klicken Sie auf **Operator aktualisieren**.
    
    **Kubernetes-Namespace**: Der Kubernetes-Namespace, in dem der Operator installiert werden soll. Geben Sie das Vale **wko-operator-ns** ein (siehe Screenshot unten).
    
    **Kubernetes-Serviceaccount** - Der Kubernetes-Serviceaccount, den der Operator beim Erstellen von Kubernetes-API-Anforderungen verwenden soll. Geben Sie den Vale **wko-operator-sa** ein (siehe Screenshot unten).
    
    **Helm-Release-Name für Operatorinstallation** - Der Helm-Release-Name zur Identifizierung dieser Installation. Geben Sie den Vale **wko-weblogic-operator** ein (siehe Screenshot unten).
    
    ![WebLogic-Operatotr](images/weblogic-operator.png)
    
    > **Nur zu Informationszwecken:**  
    > ! Standardmäßig ist das Feld _Zu verwendendes Imagetag_ des Operators auf das Imagetag gesetzt, das der neuesten Operatorreleaseversion in der GitHub Container Registry entspricht.  
    > Der Operator muss wissen, welche WebLogic-Domains im Kubernetes-Cluster er verwaltet. Dies geschieht auf Kubernetes-Namespace-Ebene. Daher wird jede WebLogic-Domain in einem Kubernetes-Namespace, für die der Operator konfiguriert ist, von der zu installierenden Operatorinstanz verwaltet.  
    > Im Feld _Kubernetes-Namespace-Auswahlstrategie_ wählen wir _Labelselektor_ aus. Das bedeutet, dass jeder Kubernetes-Namespace mit dem Label _weblogic-operator=enabled_ von diesem Operator verwaltet wird.  
    > ![Operatorbild](images/operator-image.png)
    
    > Durch Aktivieren von "Cluster-Rollen-Binding aktivieren" erstellt der Operator eine Kubernetes-Datei ClusterRole und ClusterRoleBinding, die der Operator für alle verwalteten Namespaces verwendet.  
    > Standardmäßig wird die REST-API des Operators nicht außerhalb des Kubernetes-Clusters bereitgestellt. Damit die REST-API verfügbar gemacht werden kann, können Sie _REST-API extern bereitstellen_ aktivieren. ![Rollen-Binding](images/role-binding.png)  
    
    > In diesem Bereich können Sie die Java-Loggingkonfiguration des Operators außer Kraft setzen. Dies kann nützlich sein, wenn Sie Probleme mit dem Operator debuggen.  
    > ![Java-Protokollierung](images/java-logging.png)  
    
    > Weitere Informationen zu _WebLogic Kubernetes-Operatorimage_, _Kubernetes-Namespace-Auswahlstrategie_, _WebLogic Kubernetes-Rollen-Bindings_, _Externer REST-API-Zugriff_, _Integrationen von Drittanbietern_ und _Java Logging_ finden Sie in der Dokumentation zu [WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/).
    
2.  Wenn _WebLogic Kubernetes Operator Installation Complete_ angezeigt wird, klicken Sie auf _OK_. ![Operator installiert](images/operator-installed.png)
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Oktober 2023
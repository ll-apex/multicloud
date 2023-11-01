# Domain WebLogic für Oracle Kubernetes-Cluster (OKE) bereitstellen

## Einführung

In dieser Übung stellen wir die WebLogic-Domain im Kubernetes-Cluster bereit. Im Abschnitt "Primäres Bild" geben Sie die Zugangsdaten für den oracle-Account an. Im Abschnitt "Auxiliary-Image" geben Sie Zugangsdaten für den oracle-Cloud-Account an. Hier wird auch das Replikat für das Cluster angegeben.

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Stellen Sie die WebLogic-Domain für das Kubernetes-Cluster bereit.

## Aufgabe 1: Domain WebLogic im Oracle Kubernetes-Cluster bereitstellen

In dieser Aufgabe stellen wir die benutzerdefinierte Kubernetes-Ressource für die Domain WebLogic im Kubernetes-Cluster bereit.

1.  Scrollen Sie nach unten, geben Sie im Abschnitt "Primäres Image" Folgendes ein: Geben Sie _domainsecret_ als _Image Pull Secret-Name_ ein, und verwenden Sie Oracle-Accountbenutzernamen und -kennwort unter _Image Registry Pull-Benutzername_ und _Image Registry Pull-Kennwort_. Geben Sie Ihre Oracle-E-Mail-ID unter _E-Mail-Adresse für Pull in Image Registry_ ein. Dies sind dieselben Zugangsdaten, mit denen Sie die Lizenz für _Weblogic_\-Images in Oracle Container Registry akzeptiert haben. ![Details zum primären Bild](images/primary-image-details.png)
    
    > **Nur zu Ihrer Information:**  
    > Das Image wird aus der Oracle Container Registry abgerufen. Daher geben wir die Zugangsdaten an, mit denen wir die Lizenzvereinbarung für WebLogic Server-Images akzeptiert haben.
    
2.  Blättern Sie nach unten, und deaktivieren Sie im Abschnitt "Auxiliary-Bild" das Kontrollkästchen **Auxiliary Image Pull-Zugangsdaten angeben**.
    
3.  Klicken Sie im Abschnitt _Cluster_ wie dargestellt auf das Symbol _Bearbeiten_. ![Clusterskalierung](images/cluster-resize.png)
    
4.  Geben Sie _2_ als _Replikate_ ein, und klicken Sie auf _OK_. Die Replikatgröße bestimmt die Anzahl der Managed Server im Status _Wird ausgeführt_ nach erfolgreichem Deployment der WebLogic-Domain in dem Kubernetes-Cluster. ![Clusterreplikate](images/cluster-replicas.png)
    
5.  Doppelklicken Sie im Abschnitt "Datenquellen", um _Kennwörter_ für zwei Datenquellen zu bearbeiten. Sie können _tiger_ in beiden Datenquellen als Kennwort angeben. Klicken Sie anschließend auf _Domain bereitstellen_. ![Datasoure-Kennwort](images/datasource-password.png)
    
    > Dadurch wird die Domaintestdomain WebLogic im Kubernetes-Namespace _test-domain-ns_ bereitgestellt.
    
6.  Wenn das Fenster _WebLogic Domain-Deployment an Kubernetes abgeschlossen_ angezeigt wird, klicken Sie auf _OK_. ![Deployment abgeschlossen](images/deployment-complete.png)
    
7.  Kehren Sie zum Terminal zurück, klicken Sie auf _Aktivitäten_, und wählen Sie das Fenster _Terminal_. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein. Die ähnliche Ausgabe sollte angezeigt werden, wobei der Pod für den Introspektor zuerst für den Admin-Server und spätere Pods für den Managed Server im Status _Wird ausgeführt_ ausgeführt wird.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Podstatus](images/pod-status.png)
    
8.  Sie können den Domainstatus auch über die _WebLogic Kubernetes Toolkit-UI_ abrufen. Gehen Sie zurück zur _WebLogic Kubernetes Toolkit-UI_, und klicken Sie auf _Domainstatus abrufen_. ![Domainstatus](images/domain-status.png)
    
9.  Scrollen Sie im Fenster "Domainstatus" nach unten, um den Status aller Serverpods anzuzeigen, und klicken Sie dann auf _OK_. ![Serverstatus](images/server-status.png)
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Oktober 2023
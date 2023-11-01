# WebLogic-Cluster skalieren (optional)

## Einführung

In dieser Übung skalieren wir ein WebLogic-Cluster. Hier ändern wir den Wert in _3_ und stellen die Domain erneut bereit.

Voraussichtliche Zeit: 5 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   WebLogic-Cluster skalieren.

## Aufgabe 1: WebLogic-Cluster mit der WebLogic Kubernetes Toolkit-UI skalieren

In dieser Aufgabe müssen Sie nur den Wert für _Replikat_ von 2 auf 3 ändern und die Domain erneut bereitstellen.

1.  Gehen Sie zurück zur WebLogic-UI des Kubernetes-Toolkits, und klicken Sie auf _WebLogic-Domain_. Gehen Sie zum Abschnitt _Cluster_, und klicken Sie auf das Symbol _Bearbeiten_.  
    ![Clusterskalierung](images/cluster-resize.png)
    
2.  Ändern Sie die Replikate von _2_ in _3_, und klicken Sie auf _OK_. ![Replikate ändern](images/change-replicas.png)
    
3.  Um die Domain erneut bereitzustellen, klicken Sie auf _Domain bereitstellen_. ![Domain erneut bereitstellen](images/redeploy-domain.png)
    
4.  Wenn das Fenster _WebLogic Domain-Deployment zu Kubernetes abgeschlossen_ angezeigt wird, klicken Sie auf _OK_. ![Deployment abgeschlossen](images/deployment-complete.png)
    
5.  Gehen Sie zurück zum Fenster _Terminal_, klicken Sie auf _Aktivitäten_, und wählen Sie das Fenster _Terminal_. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Skalierung anzeigen](images/view-scaling.png)
    
    > Sie können sehen, dass das erneute Deployment der Domain den Introspektorjob startet, der den Prozess zum Erstellen des Pods für test-domain-managed-server3 startet, und irgendwann wird dieser Pod in den Status _Wird ausgeführt_ versetzt.
    
6.  Kehren Sie zum Browser zurück, in dem die Anwendungsseite geöffnet ist. Klicken Sie auf "Aktualisieren", um das Load Balancing zwischen drei Managed Servern anzuzeigen. ![neuer Server](images/new-server.png)
    
7.  Gehen Sie zurück zu WebLogic Remote Console, und klicken Sie auf _Monitoring Tree_ -> _Environment_ -> _Servers_. Sie werden auch Managed-server3 hier bemerken. ![Remotekonsole](images/remote-console.png)
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Oktober 2023
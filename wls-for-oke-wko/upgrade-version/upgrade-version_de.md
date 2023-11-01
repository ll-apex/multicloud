# Upgrade in WebLogic Server-Version durchführen (optional)

## Einführung

In dieser Übung ändern wir das primäre Image. Wir verwenden WebLogic Server-Image mit dem Tag _12.2.1.4-slim-ol8_. Anschließend wird die Domain mit der Kubernetes Toolkit-UI WebLogic erneut bereitgestellt. Schließlich wird geprüft, ob neu verwaltete Serverpods die aktualisierten WebLogic Server-Images über die Remotekonsole WebLogic verwenden.

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Verwenden Sie das WebLogic Server-Image (12.2.1.4) als primäres Image.
*   Stellen Sie die WebLogic-Domain erneut bereit.

### Voraussetzungen

*   Zugriff auf noVNC Remote Desktop, erstellt in Übung 1.

## Aufgabe 1: Details des neuen WebLogic Server-Images als primäres Image eingeben

In dieser Aufgabe aktualisieren wir das primäre Image so, dass das upgegradete WebLogic Server 12.2.1.4.0-Image verwendet wird.

1.  Gehen Sie zurück zur WebLogic-UI des Kubernetes-Toolkits, und klicken Sie auf _WebLogic-Domain_. Das WebLogic Server-Tag wurde in _12.2.1.4-slim-ol8_ geändert. ![Primäres Imagetag aktualisieren](images/update-primary-image-tag.png)

## Aufgabe 2: Bereitgestellte Anwendung durch einen rollierenden Neustart der Serverpods aktualisieren

In dieser Aufgabe wird die Domain WebLogic erneut bereitgestellt. Später verwenden wir die Remotekonsole WebLogic, um zu prüfen, ob Serverpods das aktualisierte WebLogic Server 12.2.1.4.0-Image verwenden.

1.  Klicken Sie auf _Domain bereitstellen_. Dadurch wird die Domain erneut bereitgestellt. ![Domain erneut bereitstellen](images/redeploy-domain.png)
    
    > **Nur zu Ihrer Information:**  
    > Da wir unser primäres Image geändert haben, werden wir einen rollenden Neustart der Server nach dem anderen bemerken. Wenn Sie auf _Domain bereitstellen_ klicken, wird ein _Introspektorjob_ gestartet, der die ausgeführten Admin-Serverpods beendet und einen neuen Pod für den Admin-Server erstellt, der das WebLogic Server 12.2.1.4.0-Image verwendet. Introspector führt denselben Prozess mit beiden Managed Servern aus.
    
2.  Wenn das Fenster _WebLogic Domain-Deployment an Kubernetes abgeschlossen_ angezeigt wird, klicken Sie auf _OK_. ![Deployment abgeschlossen](images/deployment-complete.png)
    
3.  Gehen Sie zurück zu _Terminal_, kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein. Sie werden feststellen, dass die Server einzeln neu gestartet werden. Zuerst werden Admin-Serverpods beendet und im Status _Wird ausgeführt_ angezeigt.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Pods anzeigen](images/view-pods.png)
    
4.  Um zu prüfen, ob Admin-Server- und Managed Server-Pods das aktualisierte WebLogic Server-Image verwenden, klicken Sie auf das Symbol "Überwachungsbaum", und wählen Sie _Umgebung_ -> _Server_ -> _Admin-Server_. Sie können sehen, es verwendet 12.2.1.4.0. ![WLS-Version](images/wls-version.png)
    

Congratulation !!!

Dies ist das Ende des Workshops.

Wir hoffen, Sie haben diesen Workshop nützlich gefunden.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Oktober 2023
# Anwendungs-Deployment testen

## Einführung

### Informationen zur WebLogic-Remotekonsole

Die WebLogic Remotekonsole ist eine einfache Open-Source-Konsole, mit der Sie Ihre WebLogic Server-Domain verwalten können, die überall ausgeführt wird, z.B. auf einer physischen oder virtuellen Maschine, in einem Container, Kubernetes oder in der Oracle Cloud. Die Remotekonsole WebLogic muss nicht mit der WebLogic Server-Domain untergebracht werden.

Sie können die Remotekonsole WebLogic überall installieren und ausführen und mit den REST-APIs WebLogic eine Verbindung zu Ihrer Domain herstellen. Sie starten einfach die Desktopanwendung und stellen eine Verbindung zum Administrationsserver Ihrer Domain her. Alternativ können Sie den Konsolenserver starten, die Konsole in einem Browser starten und dann eine Verbindung zum Administrationsserver herstellen.

Die Remotekonsole WebLogic wird in WebLogic Server 12.2.1.3, 12.2.1.4 und 14.1.1.0 vollständig unterstützt.

**Wichtige Features der WebLogic-Remotekonsole**

*   WebLogic Server-Instanzen und -Cluster konfigurieren
*   WDT-Metadatenmodelle erstellen oder ändern
*   WebLogic Server-Services, wie Datenbankverbindung (JDBC), und Messaging (JMS), konfigurieren
*   Anwendungen bereitstellen und Deployment aufheben
*   Server und Anwendungen starten und anhalten
*   Performance von Servern und Anwendungen überwachen

In dieser Übung greifen wir auf das Anwendungs-_opdemo_ zu und prüfen die erfolgreiche Migration einer On-Premise-Offlinedomain. Außerdem wird das Load Balancing zwischen Managed Server-Pods geprüft. Später verwenden wir WebLogic Remote Console, um das erfolgreiche Deployment von Ressourcen der Testdomain in der Kubernetes-Umgebung zu überprüfen.

Geschätzte Zeit: 10 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Anwendungs-Deployment testen](videohub:1_1khcsrbq)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Greifen Sie über den Browser auf die Anwendung zu.
*   Erkunden Sie die Domain WebLogic mit der Remotekonsole WebLogic.

## Aufgabe 1: Über den Browser auf die Anwendung zugreifen

In dieser Aufgabe greifen wir auf die _opdemo_\-Anwendung zu. Klicken Sie auf das Aktualisierungssymbol, um mehrere Anforderungen an die Anwendung zu stellen und das Load Balancing zwischen zwei Managed Server-Pods zu prüfen.

1.  Kopieren Sie die folgende URL, und ersetzen Sie _XX.XX.XX.XX_ durch Ihre IP, die Sie in der letzten Übung notiert haben. Die folgende Ausgabe wird angezeigt.
    
        <copy>http://XX.XX.XX.XX/opdemo/?dsname=testDatasource</copy>
        
    
    ![Anwendung öffnen](images/open-application.png)
    
2.  Wenn Sie auf das Symbol "Aktualisieren" klicken, wird das Load Balancing zwischen zwei Managed Server-Pods angezeigt. ![Load Balancing anzeigen](images/show-load-balancing.png)
    

## Aufgabe 2: Domain WebLogic auf Kubernetes-Cluster mit der Remotekonsole WebLogic erkunden

In dieser Aufgabe wird die WebLogic-Remotekonsole erläutert. Wir erstellen eine Verbindung zu _Admin-Server_ in der Remotekonsole, und prüfen die Ressourcen in der Domain WebLogic. Dadurch wird die erfolgreiche Migration einer On-Premise-Domain in das Oracle Kubernetes-Cluster geprüft.

1.  Um die WebLogic-Remotekonsole zu öffnen, klicken Sie auf _Aktivitäten_, geben Sie _WebLogic_ in das Suchfeld ein, und klicken Sie auf das Symbol _WebLogic Remote Console_.
    
2.  Klicken Sie unter _Kiosk_ auf `Three dots`, wählen Sie _Admin-Serververbindungsprovider hinzufügen_ aus, und klicken Sie auf _Auswählen_. ![Admin-Serververbindung](images/adminserver-connection.png)
    
3.  Geben Sie die folgenden Daten ein, und klicken Sie auf _OK_.  
    Verbindungsprovidername: AdminServer  
    Benutzername: weblogic  
    Kennwort: welcome1  
    URL: `Copy_IP_From_TextFile`  
    ![Verbindungsdetails](images/connection-details.png)
    
4.  Klicken Sie auf das Symbol _Baum bearbeiten_, und wählen Sie _Services_ -> _Datenquellen_ aus. Sie können dasselbe Datensouce beobachten, das wir in der On-Premise-Domain gesehen haben. ![Datenquellen prüfen](images/verify-datasources.png)
    
5.  Zeigt an, welche Server in Ihrer Domain ausgeführt werden. Klicken Sie wie gezeigt auf das Symbol **Überwachungsbaum**, und wählen Sie **Umgebung** -> **Server**. Sie können sehen, dass **Admin Server** und 2 Managed Server-Pods ausgeführt werden. ![Ausgeführte Server](images/running-server-status.png)
    
6.  Klicken Sie auf **admin-server**. Die WebLogic-Version lautet **12.2.1.3.0**. ![Serverstatus](images/wls-version.png)
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
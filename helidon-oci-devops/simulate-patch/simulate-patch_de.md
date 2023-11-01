# Patchszenario simulieren

## Einführung

In dieser Übung wird versucht, ein Patching-Szenario zu simulieren. Nehmen Sie einen Fall an, in dem die anfängliche Umgebung _Open JDK_ zum Testen verwendet und schließlich zur Verwendung von _Oracle JDK_ verschoben wird, wenn sie für die Produktion bereit ist. Die vorherige Build- und Deployment-Pipeline legt bereits _Open JDK 20_ als zu verwendende Java-Aroma fest. In dieser Übung wird sie durch _Oracle JDK 20_ ersetzt.

Geschätzte Zeit: 10 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Patchszenario simulieren](videohub:1_4pecvhmf)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   JDK-Aroma ändern
*   Neues JDK-Aroma in der Deployment-Pipeline prüfen

### Voraussetzungen

*   Free Tier-(Testversion), kostenpflichtiger oder LiveLabs Cloud-Account für Oracle

## Aufgabe 1: JDK-Installationsprogramm ändern

1.  Wie in **Übung 2** zu sehen, wurde im zuvor abgeschlossenen **Deployment-Pipelinelog** beobachtet, dass es am Ende des Logs **JDK öffnen** verwendete. ![jdk öffnen](images/open-jdk.png)
    
2.  Klicken Sie im **Codeeditor** auf den Dateinamen **`build_spec.yaml`**, um ihn zu öffnen. Kommentieren Sie die Umgebungsvariable **`JDK20_TAR_GZ_INSTALLER`** aus, die den URL-Pfad auf ein **Open JDK**\-Installationsprogramm unter Zeilennummer 15 festlegt, und entfernen Sie die Kommentarzeichen **`JDK20_TAR_GZ_INSTALLER`**, das den URL-Pfad auf ein **Oracle JDK**\-Installationsprogramm unter Zeilennummer 16 festlegt, wie unten gezeigt. ![jdk ändern](images/modify-jdk.png)
    

## Aufgabe 2: Pushen Sie die Änderung, und lösen Sie die Pipeline DevOps aus

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, **um die Änderung festzuschreiben und zu übertragen**.
    
        <copy>git add .
        git status
        git commit -m "Replace OpenJDK 20 with OracleJDK 20"
        git push</copy>
        
    
    > Die Pipeline wird durch diesen Git-Push ausgelöst.
    

## Aufgabe 3: Neuen JDK-Flavor in der Deployment-Pipeline prüfen

1.  Öffnen Sie die [Cloud-Konsole](https://cloud.oracle.com/) auf einer neuen Registerkarte, und klicken Sie auf _Hamburger-Menü_ -> _Entwicklerservices_ -> _Projekte_ unter **DevOps**. ![devops-Projekt](images/devops-project.png)
    
2.  Wählen Sie das Compartment aus, das Sie in **Übung 1** erstellt haben, und klicken Sie auf _devops-project-helidon-ocw-hol-string_, um das **DevOps-Projekt** zu öffnen. ![Compartment auswählen](images/select-compartment.png)
    
3.  Unter **Letzte Build-Historie** werden die **Ausführungen** und der Status **Akzeptiert/In Bearbeitung** angezeigt. Klicken Sie auf die letzten Ausführungen wie unten gezeigt. ![Erstellungshistorie](images/build-history.png)
    
4.  Sobald die Build-Pipeline **alle drei Phasen abgeschlossen hat**, wird die Ausgabe wie unten dargestellt angezeigt. ![Build-Ausführung zuerst](images/build-run-first.png)
    
5.  Klicken Sie im Build-Ausführungsfortschritt in der dritten Phase auf **Drei Punkte** und dann wie unten gezeigt auf **Deployment anzeigen**. Dadurch wird die Deployment-Pipeline geöffnet. ![Deployment anzeigen](images/view-deployment.png)
    
6.  Hier wird **Deployment-Fortschritt** angezeigt. Nachdem Sie die Deployment-Pipeline abgeschlossen haben, wird die Ausgabe wie unten dargestellt angezeigt. ![Deployment-Lauf](images/deployment-run.png)
    
    > Dadurch wird die Helidon-Anwendung erfolgreich für Compute-Instanzen in OCI bereitgestellt.
    
7.  Um die Logs der Deployment-Pipeline anzuzeigen, klicken Sie auf **Drei Punkte** in der Nähe der Deployment-Phase, und klicken Sie wie unten gezeigt auf **Details anzeigen**. ![Logs anzeigen](images/view-logs.png)
    
8.  Scrollen Sie in den Logs nach unten, und prüfen Sie das JDK-Aroma. Es sollte **Oracle JDK** sein, wie unten gezeigt. ![oracle jdk](images/oracle-jdk.png)
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Keith Lustria
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
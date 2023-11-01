# WKT-UI-Projekt ändern und Modelldatei erstellen

## Einführung

In dieser Übung untersuchen wir die On-Premise-Domain WebLogic. Wir navigieren durch die Administrationskonsole, um die bereitgestellte Anwendung, Datenquellen und Server in _test-domain_ anzuzeigen. Außerdem öffnen wir das vorab erstellte _`base_project.wktproj`_, das bereits vordefinierte Werte für den Abschnitt _Projekteinstellungen_ enthält. Anschließend erstellen wir die Modelldatei, indem wir eine Offline-On-Premise-Domain untersuchen. Schließlich validieren wir das Modell und bereiten das Modell vor, das auf Oracle Kubernetes Cluster (OKE) bereitgestellt werden soll.

Geschätzte Zeit: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Modell für OKE auf OCI erstellen](videohub:1_qdch3qqg)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Erkunden Sie die _Testdomain_ der On-Premise-Domain WebLogic.
*   Öffnen Sie das WKT-Basisprojekt.
*   Introspektion einer Offline-On-Premise-Domain.
*   Validieren und bereiten Sie das Modell vor.

### Voraussetzungen

Um diese Übung ausführen zu können, benötigen Sie:

*   Zugriff auf noVNC Remote Desktop, erstellt in Übung 2.

## Aufgabe 1: On-Premise-Domain erkunden

In dieser Aufgabe navigieren wir mit der WebLogic-Administrationskonsole durch die Ressourcen in der On-Premise-_Testdomain_.

1.  Klicken Sie auf der linken Seite auf _Pfeilsymbol_. ![Zwischenablage](images/clipboard.png)

> **Wichtig**: Das _Clipboard_ wird angezeigt. Zum Kopieren und Einfügen zwischen dem Hostrechner und dem Remotedesktop wird das _Clipboard_ verwendet. Beispiel: Wenn Sie vom Hostrechner kopieren und ihn in den Remote-Desktop einfügen möchten, müssen Sie ihn zuerst in die Zwischenablage einfügen und dann in den Remote-Desktop einfügen. Klicken Sie erneut auf _Pfeilsymbol_, um die Option _Einstellungen_ auszublenden.

2.  Klicken Sie auf die Registerkarte _Oracle WebLogic Server_, und geben Sie _weblogic/Welcome1%_ als `Username/Password` ein. Klicken Sie dann auf _Anmelden_. Sie sehen, wir haben WebLogic Server Version _12.2.1.3.0_.  
    ![Anmelde-Admin-Konsole](images/login-admin-console.png)
    
3.  Um verfügbare Server anzuzeigen, blenden Sie _Umgebung_ ein, und klicken Sie auf _Server_. Sie können sehen, dass ein dynamisches Cluster mit 5 Managed Servern vorhanden ist. ![Server anzeigen](images/view-servers.png)
    
4.  Um die Datenquellen anzuzeigen, blenden Sie _Services_ ein, und klicken Sie auf _Datenquellen_. ![Datenquellen anzeigen](images/view-datasources.png)
    
5.  Um die bereitgestellte Anwendung anzuzeigen, klicken Sie auf _Deployment_. Sie können sehen, dass _opdemo_ als bereitgestellte Anwendung verwendet wird. ![Deployments anzeigen](images/view-deployments.png)
    

## Aufgabe 2: Basisprojekt WKT UI öffnen

Der Einfachheit halber haben wir _`base_project.wktproj`_ erstellt, das den Speicherort von Dockingstation, Java, Oracle Home und Primary Image Tag vorgibt. In dieser Aufgabe wird das Projekt _`base_project.wktproj`_ geöffnet.

1.  Klicken Sie auf _Aktivitäten_, und geben Sie **WebLogic** in das Suchfeld ein. Klicken Sie auf das Symbol für _WebLogic Kubernetes Toolkit-UI_. ![WKTUI öffnen](images/open-wktui.png)
    
2.  Um das Projekt _base\_project.wktproj_ zu öffnen, klicken Sie auf _Datei_ -> _Projekt öffnen_. ![Projekt öffnen](images/open-project.png)
    
3.  Klicken Sie auf der linken Seite auf _Downloads_, wählen Sie _base\_project.wktproj_ aus, und klicken Sie auf _Projekt öffnen_. ![Projektort](images/project-location.png)
    
    > **For your information only:**  
    > As _Credential Story Policy_, we select **Store in Native OS Credentials Store**. It means the credentials (like for WebLogic Server and datasources) are only stored on the local machine.  
    > For _Where would you like the target Oracle Fusion Middleware domain to live?_, we select **Created in the container from the model in the image**. In this case, the set of model-related files are added to the image. So when the WebLogic Kubernetes Operator domain object is deployed, its inspector process runs and creates the WebLogic Server domain inside a running container on-the-fly.  
    > ![Projekteigenschaften](images/project-settings.png) As _Kubernetes Environment Target Type_, we select **WebLogic Kubernetes Operator**. This means, you want this domain to be deployed in Kubernetes managed by the WebLogic Kubernetes Operator. This settings also determine what sections and their associated actions within the application, to display.  
    > we also specify the location for _JAVA HOME_ and _ORACLE\_HOME_. WebLogic Kubernetes Toolkit UI uses this directory when invoking the WebLogic Deployer Tooling and WebLogic Image Tool.  
    > To build new images, inspect images and interact with image repositories, the WKT UI application uses an image build tool, which defaults to docker.  
    > ![Kubernetes-Cluster-Typ](images/kubernetes-cluster-type.png)
    
4.  Geben Sie unter _Kennwort_ **welcome1** ein, und klicken Sie auf _Entsperren_. ![entsperren](images/unlock.png)
    

## Aufgabe 3: Introspektion einer Offline-On-Premise-Domain

In dieser Aufgabe führen wir die Introspektion einer On-Premise-Domain durch, die eine Modelldatei erstellt, die aus der Domainkonfiguration besteht.

1.  Klicken Sie in der WebLogic Kubernetes Toolkit-UI auf _Modell_. ![Modell](images/click-model.png)
    
2.  Klicken Sie auf _Datei_ -> _Modell hinzufügen_ -> _Modell ermitteln (offline)_. ![Modell erkennen](images/discover-model.png)
    
3.  Klicken Sie auf das _Symbol_ "Ordner öffnen", um das _Domain-Home_ zu öffnen. ![Domain-Hom öffnen](images/open-domain-home.png)
    
4.  Navigieren Sie im Home-Ordner zum Verzeichnis _`/home/opc/Oracle/Middleware/Oracle_Home/user_projects/domains/`_, wählen Sie den Ordner _test-domain_ aus, und klicken Sie auf _Auswählen_. Klicken Sie auf _OK_. ![Speicherort navigieren](images/navigate-location.png) ![Speicherort angeben](images/specify-location.png)
    
    > Wenn Sie in der Konsole nachsehen, wird WebLogic Deployer Tool aufgerufen, um die Domainkonfiguration im Offlinemodus zu prüfen.
    
5.  Sie sehen das Fenster wie unten gezeigt, am Ende haben Sie das Modell für Sie bereit. ![Modell anzeigen](images/view-model.png)
    
    > Das Ergebnis dieser WDT-Introspektion sind model(eine Metadatendarstellung Ihrer Domainkonfiguration), Platzhalter, in dem Sie die Werte (wie Passwort für Datenquelle) und Anwendung im Anwendungsarchiv angeben können.
    

## Aufgabe 4: Modell validieren und vorbereiten

In dieser Aufgabe validieren wir das Modell und bereiten das Modell vor, das auf Oracle Kubernetes Cluster (OKE) bereitgestellt werden soll.

1.  Klicken Sie auf _Codeansicht_, und um das Modell zu validieren, klicken Sie auf _Modell validieren_. ![Modell validieren](images/validate-model.png)
    
    > **Nur zu Informationszwecken:**  
    > "Modell validieren" ruft das WDT-Tool [Modell validieren](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/validate/) auf. Dieses Tool validiert, ob das Modell und die zugehörigen Artefakte wohlgeformt sind, und bietet Hilfe zu den gültigen Attributen und Unterordnern für einen bestimmten Modellspeicherort.
    
2.  Wenn das Fenster _Modell validieren abgeschlossen_ angezeigt wird, klicken Sie auf _OK_. ![Validierung abgeschlossen](images/validate-complete.png)
    
3.  Um das Modell vorzubereiten, das auf dem Kubernetes-Cluster bereitgestellt werden soll, klicken Sie auf _Modell vorbereiten_ ![Modell vorbereiten](images/prepare-model.png)
    
    > **For your information only:**  
    > Prepare model invokes the WDT [Prepare Model Tool](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/prepare/) to modify the model to work in a Kubernetes cluster with WebLogic Kubernetes Operator or Verrazzano installed.  
    > Prepare Model does the following:
    
    *   Entfernt Modellabschnitte und Felder, die nicht mit der Zielumgebung kompatibel sind.
    *   Ersetzt Endpunktwerte durch Modelltoken, die Variablen referenzieren.
    *   Ersetzt Zugangsdatenwerte durch Modelltoken, die entweder ein Feld in einem Kubernetes-Secret oder eine Variable referenzieren.
    *   Stellt Standardwerte für Felder bereit, die in den Variablen, Variablenüberschreibungen und Secret-Editoren der Anwendung angezeigt werden.
    *   Extrahiert Topologieinformationen in die Anwendung, mit der die Ressourcendatei für das Deployment der Domain generiert wird.
4.  Wenn das Fenster _Prepare Model Complete_ angezeigt wird, klicken Sie auf _OK_. ![Vorbereitung abgeschlossen](images/prepare-complete.png)
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
# Helidon MP-Anwendung generieren

## Einführung

In dieser Übung werden Sie durch die Schritte zum Erstellen einer **Helidon-MP**\-Anwendung mit der **Helidon-CLI** geführt. Außerdem ändern Sie die Helidon-Anwendung, um ihre Integration mit **OCI Logging and Monitoring**\-Services anzuzeigen.

Geschätzte Zeit: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Helidon MP-Anwendung generieren](videohub:1_i4j33ed4)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Helidon-CLI herunterladen
*   Helidon MP-Anwendung mit der Helidon-CLI generieren
*   Integration mit OCI Logging und Metrics-Service ausführen
*   Anwendungscode in das OCI-Code-Repository übertragen
*   Lösen Sie die Pipeline DevOps aus.

### Voraussetzungen

*   Free Tier-(Testversion), kostenpflichtiger oder LiveLabs Cloud-Account für Oracle
*   Vertrautheit mit Git-Befehlen

## Aufgabe 1: Helidon-CLI herunterladen

1.  Öffnen Sie ein neues Terminal im **OCI-Codeeditor**, und fügen Sie den folgenden Befehl ein, um zum Home-Ordner zu navigieren.
    
        <copy>cd ~</copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die **Helidon-CLI** herunterzuladen und die **Berechtigungen** zu ändern, damit sie ausführbar ist.
    
        <copy>curl -L -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon</copy>
        

## Aufgabe 2: Helidon Microprofile-Anwendung mit der Helidon-CLI generieren

1.  Führen Sie die CLI aus, um ein **Helidon Microprofile-Anwendungsprojekt** zu generieren.
    
        <copy>./helidon init</copy>
        
2.  Wenn er zur Eingabe der _Helidon-Version_ auffordert, geben Sie _4_ ein, um alle Versionen anzuzeigen, und geben Sie _35_ ein, um die _4.0.0-ALPHA6_\-Version auszuwählen, wie unten gezeigt.
    
        Helidon versions
        (1) 3.2.2 
        (2) 2.6.2 
        (3) 4.0.0-M1 
        (4) Show all versions 
        Enter selection (default: 1): 4
        -----------------------------
        -----------------------------
        (33) 2.0.1 
        (34) 2.0.0 
        (35) 4.0.0-ALPHA6 
        (36) 4.0.0-M1 
        Enter selection (default: 1): 35 
        
3.  Wenn Sie zur Eingabe von _Aroma auswählen_ aufgefordert werden, kopieren Sie den folgenden Wert, und fügen Sie ihn in das Terminal ein.
    
            | Helidon Flavor
        
            Select a Flavor
            (1) se   | Helidon SE
            (2) mp   | Helidon MP
            (3) nima | Helidon Níma
            Enter selection (default: 1): <copy>2</copy>
        
4.  Wenn Sie zur Eingabe von _Anwendungstyp auswählen_ aufgefordert werden, kopieren Sie den folgenden Wert, und fügen Sie ihn in das Terminal ein.
    
            Select an Application Type
        (1) quickstart | Quickstart
        (2) database   | Database
        (3) custom     | Custom
        (4) oci        | OCI
        Enter selection (default:1):<copy>4</copy>
        
5.  Wenn Sie zur Eingabe von _Projekt groupId_, _Projekt artifactId_ und _Projektversion_ aufgefordert werden, **übernehmen Sie nur die Standardwerte**.
    
6.  Wenn Sie zur Eingabe des _Java-Packagenamens_ aufgefordert werden, kopieren Sie den folgenden Wert, und fügen Sie ihn in das Terminal ein.
    
        Java package name (default: me.username.mp.oci): <copy>ocw.hol.mp.oci</copy>
        
7.  Wenn die Promotion für _Entwicklungsschleife starten? (Standard: n):_ erfolgt, drücken Sie die _Eingabetaste_, um den Standardwert zu wählen.
    
    > Sobald die Ausführung abgeschlossen ist, wird ein **oci-mp**\-Projekt generiert.
    

## Aufgabe 3: Helidon-Anwendung für Logging und Metrik-Explorer ändern

1.  Um das Projekt _oci-mp_ in **Codeeditor** zu öffnen, klicken Sie auf _Datei_ -> _Öffnen_. ![Projekt öffnen](images/open-project.png)
    
2.  Klicken Sie auf den _Pfeil nach oben_, um zum übergeordneten Ordner zu navigieren, wählen Sie den Ordner _oci-mp_ aus, und klicken Sie auf _Öffnen_. ![OCI MP öffnen](images/open-ocimp.png)
    
    > Dadurch wird die Anwendung _oci-mp_ im Explorer geöffnet.
    
3.  Öffnen Sie ein neues Terminal, kopieren Sie den folgenden Befehl, und fügen Sie ihn in den Ordner **`devops_helidon_to_instance_ocw_hol`** ein, um die Build- und Deployment-Pipeline-Spezifikationen zu kopieren.
    
        <copy>cd ~/oci-mp/
        cp ~/devops_helidon_to_instance_ocw_hol/pipeline_specs/* .</copy>
        
4.  Fügen Sie **.gitignore** hinzu, damit Dateien und Verzeichnisse, die nicht Teil des Repositorys sein müssen, von git ignoriert werden.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/.gitignore .</copy>
        
5.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um das Utilityskript aus dem Hauptordner _`devops_helidon_to_instance_ocw_hol`_ auszuführen, um die Parameter **config** zu aktualisieren. Wenn Sie dazu aufgefordert werden, **das Root-Verzeichnis des Helidon MP-Projekts einzugeben**, drücken Sie die Eingabetaste, um den **Standardwert** zu wählen.
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/update_config_values.sh</copy>
        
    
    Die Ausgabe ähnelt der folgenden: ![Konfiguration aktualisieren](images/update-config.png)
    
    > **Bitte lesen Sie:-**
    
    *   Beim Aufrufen dieses Skripts wird Folgendes ausgeführt:
    *   Aktualisierungen in der Konfigurationsdatei _~/OCI-mp/server/src/main/resources/application.yaml_, um ein Helidon-Feature einzurichten, das von Helidon generierte Metriken an den OCI-Überwachungsservice sendet.
        *   **compartmentId**: Compartment-OCID, die für diese Demo verwendet wird
        *   **Namespace** - Dies kann eine beliebige Zeichenfolge sein. Für diese Demo wird diese jedoch auf helidon\_metrics gesetzt.
    *   Aktualisiert in der Konfigurationsdatei _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_, um Konfigurationsparameter einzurichten, die vom Helidon-Anwendungscode zur Integration mit OCI Logging and Metrics-Service verwendet werden.
        *   **oci.monitoring.compartmentId**: Compartment-OCID, die für diese Demo verwendet wird
        *   **oci.monitoring.namespace** - Hierbei kann es sich um eine beliebige Zeichenfolge handeln. Für diese Demo wird diese jedoch auf helidon\_application gesetzt.
        *   **oci.logging.id**: Anwendungslog-ID, die von den Terraform-Skripten bereitgestellt wurde.
    *   Aktualisieren Sie in der Konfigurationsdatei _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_, um die Eigenschaft _oci.bucket.name_ so einzurichten, dass sie den Objektspeicher-Bucket-Namen enthält, der von den Terraform-Skripten bereitgestellt wurde, die in einer späteren Übung zur Demonstration der Object Storage-Unterstützung von einer Helidon-Anwendung verwendet werden.
6.  Öffnen Sie die Datei _~/oci-mp/server/src/main/resources/application.yaml_ im Codeeditor, um zu prüfen, ob der Wert von **compartmentId** und **Namespace** aktualisiert wird. ![Anwendungs-YAML](images/application-yaml.png)
    
7.  Öffnen Sie die Datei _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ im Codeeditor, um zu prüfen, ob die Werte von **oci.monitoring.compartmentId**, **oci.monitoring.namespace**, **oci.logging.id** und **oci.bucket.name** aktualisiert werden. _oci.bucket.name_ wird später in Übung 5 verwendet.
    

## Aufgabe 4: Authentifizierungstoken generieren, um den Code in das OCI-Code-Repository zu übertragen

In diesem Schritt generieren wir ein _Authentifizierungstoken_, mit dem wir den Helidon-Anwendungscode an das _OCI-Code-Repository_ pushen.

1.  Wählen Sie in der oberen rechten Ecke das _Benutzersymbol_ und dann _Mein Profil_ aus. ![Benutzersymbol](images/user-icon.png)
    
2.  Scrollen Sie nach unten, und wählen Sie _Authentifizierungstoken_ aus. ![Authentifizierungstoken](images/auth-tokens.png)
    
3.  Klicken Sie auf _Token generieren_. ![Token generieren](images/generate-token.png)
    
4.  Kopieren Sie _oci-mp_, fügen Sie es in das Feld "Beschreibung" ein, und klicken Sie auf _Token generieren_. ![Tokenbeschreibung](images/token-description.png)
    
5.  Wählen Sie unter "Generiertes Token" die Option **Kopieren** aus, und fügen Sie es mit einem Editor Ihrer Wahl in eine Textdatei ein bzw. speichern Sie es. Denken Sie daran, dass das Token später nicht mehr abgerufen werden kann. Daher ist es wichtig, eine Kopie davon jetzt aufzubewahren. Klicken Sie anschließend auf **Schließen**. ![Token kopieren](images/copy-token.png)
    

## Aufgabe 5: Helidon-Anwendungsprojekt mit dem OCI-Code-Repository im Projekt DevOps synchronisieren

1.  Öffnen Sie ein neues Terminal, kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um in das Verzeichnis _oci-mp_ zu navigieren.
    
        <copy>cd ~/oci-mp</copy>
        
2.  Initialisieren Sie das Projektverzeichnis _oci-mp_, um ein **Git-Repository** zu werden.
    
        <copy>git init</copy>
        
3.  Setzen Sie die Verzweigung auf **main**, damit sie mit der entsprechenden Remoteverzweigung übereinstimmt.
    
        <copy>git checkout -b main</copy>
        
4.  Legen Sie das **Remote-Repository** fest. Verwenden Sie die HTTPS-URL des OCI Code Repositorys, die aus der letzten terraform-Ausgabe angezeigt wird, oder verwenden Sie das Tool get.sh aus `devops_helidon_to_instance_ocw_hol`, um diesen Wert abzurufen.
    
        <copy>git remote add origin $(~/devops_helidon_to_instance_ocw_hol/main/get.sh code_repo_https_url)
        git remote -v</copy>
        
    
    > Mit **git remote -v** wird geprüft, ob der Ursprung festgelegt wurde.
    
5.  Konfigurieren Sie Git so, dass der **credential helper**\-Speicher verwendet wird, sodass Benutzername und Kennwort des OCI-Repositorys nur einmal für Git-Befehle eingegeben werden, für die sie erforderlich sind. Legen Sie außerdem **user.name** und **user.email** fest, die für den Git-Commit erforderlich sind.
    
        <copy>git config credential.helper store
        git config --global user.email "my.name@example.com"
        git config --global user.name "FIRST_NAME LAST_NAME"</copy>
        
6.  Synchronisieren Sie das Git-Log des OCI-Repositorys mit dem lokalen Repository, indem Sie **Git Pull** verwenden.
    
        <copy>git pull origin main</copy>
        
7.  Dies fordert Sie zur Eingabe eines Benutzernamens und Passworts auf. Verwenden Sie **Mandantenname**/**Benutzername** für den Benutzernamen und das OCI-Benutzer-**Authentifizierungstoken**, das in **Aufgabe 4** für das Kennwort generiert wurde. ![git-Benutzername](images/git-username.png) ![Git-Kennwort](images/git-password.png)
    
8.  Die Ausgabe sieht in etwa wie folgt aus. Wenn nicht, überprüfen Sie bitte, ob Sie jeden git-Befehl korrekt ausgeführt haben. ![git Pull-Synchronisierung](images/git-pull-sync.png)
    

## Aufgabe 6: Helidon-Anwendungscode per Push übergeben und Pipeline DevOps auslösen

1.  **Stage** aller Dateien für den Git-Commit.
    
        <copy>git add .
        git status</copy>
        
    
    > git status gibt alle Dateien im Repository aus.
    
2.  Führen Sie den ersten **Commit** aus.
    
        <copy>git commit -m "Helidon oci-mp first commit"</copy>
        
3.  **Pushen** Sie die Änderungen an das Remote-Repository.
    
        <copy>git push -u origin main</copy>
        
    
    > Dadurch wird DevOps ausgelöst, um die Build-Pipeline zu starten.
    
4.  Öffnen Sie die [Cloud-Konsole](https://cloud.oracle.com/) auf einer neuen Registerkarte, und klicken Sie auf _Hamburger-Menü_ -> _Entwicklerservices_ -> _Projekte_ unter **DevOps**. ![devops-Projekt](images/devops-project.png)
    
5.  Wählen Sie das Compartment aus, das Sie in **Übung 1** erstellt haben, und klicken Sie auf _devops-project-helidon-ocw-hol-string_, um das **DevOps-Projekt** zu öffnen. ![Compartment auswählen](images/select-compartment.png)
    
    > Aktualisieren Sie den Browser, wenn das neue Compartment nicht angezeigt wird.
    
6.  Unter _Letzte Build-Historie_ werden die _Ausführungen_ und der Status _Akzeptiert/In Bearbeitung_ angezeigt. Klicken Sie auf die letzten Ausführungen wie unten gezeigt. ![Erstellungshistorie](images/build-history.png)
    
7.  Sobald die Build-Pipeline alle drei Phasen abgeschlossen hat, wird die Ausgabe wie unten dargestellt angezeigt. Sie können auf den Pfeil direkt vor den Phasen klicken, um anzuzeigen, welche Aktion sie ausführen. Diese Aktion wird in der Datei _`build_spec.yaml`_ im Ordner _oci-mp_ entfernt. ![Build-Ausführung zuerst](images/build-run-first.png)
    
8.  Klicken Sie im Build-Ausführungsfortschritt in der dritten Phase auf **Drei Punkte** und dann wie unten gezeigt auf **Deployment anzeigen**. Dadurch wird die **Deployment-Pipeline** geöffnet. ![Deployment anzeigen](images/view-deployment.png)
    
9.  Hier wird _Deployment-Fortschritt_ angezeigt. Nachdem Sie die Deployment-Pipeline abgeschlossen haben, wird die Ausgabe wie unten dargestellt angezeigt. Sie können auf den Pfeil direkt vor der Phase klicken, um anzuzeigen, welche Aktion sie ausführen. Diese Aktion hat "defind" in der Datei _`deployment_spec.yaml`_ im Ordner _oci-mp_. ![Deployment-Lauf](images/deployment-run.png)
    
    > Dadurch wird die Helidon-Anwendung erfolgreich in **Compute-Instanzen** in OCI bereitgestellt.
    
10.  Um die Logs der Deployment-Pipeline anzuzeigen, klicken Sie auf **Drei Punkte** in der Nähe der Deployment-Phase, und klicken Sie wie unten gezeigt auf **Details anzeigen**. ![Logs anzeigen](images/view-logs.png)
    
11.  Scrollen Sie in den Logs nach unten, und prüfen Sie, ob der JDK-Flavor **JDK öffnen** ist. Beachten Sie auch in den Logs, dass die Helidon-Anwendung das neue Feature **Virtuelle Threads** in Java wie unten gezeigt nutzt. ![jdk öffnen](images/open-jdk.png)
    
    > Im Rahmen von **Übung 4** wird **Open JDK** durch **Oracle JDK** ersetzt.
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Weitere Informationen

*   [Helidon-CLI](https://helidon.io/docs/v3/#/about/cli)
*   [Kurzanleitung für Helidon MP](https://helidon.io/docs/v3/#/mp/guides/quickstart)
*   [Helidon MP-Konfigurationsquellen](https://helidon.io/docs/v3/#/mp/config/advanced-configuration)
*   [Helidon MP-Konfigurationsquellen](https://helidon.io/docs/v3/#/mp/guides/config)

## Danksagungen

*   **Autor** - Keith Lustria
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
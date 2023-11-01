# Bereitstellung der Infrastruktur

## Einführung

In dieser Übung erstellen Sie ein **Compartment**, **dynamische Gruppen**, eine **Benutzergruppe und Policys**. Anschließend erstellen Sie ein **DevOps-Projekt** und die zugehörigen Ressourcen mit dem Terraform-Service in **OCI Code Editor**.

Geschätzte Zeit: 10 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Infrastruktur bereitstellen](videohub:1_tnhjvsxr)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Öffnen Sie den Codeeditor, um das Terraform-Skript herunterzuladen.
*   Stellen Sie das Compartment, dynamische Gruppen, Benutzergruppen und Policys bereit.
*   Stellen Sie die für das Projekt DevOps erforderlichen Ressourcen bereit

### Voraussetzungen

*   Free Tier-(Testversion), kostenpflichtiger oder LiveLabs Cloud-Account für Oracle
*   Vertrautheit mit Compartments, dynamischen Gruppen, Benutzergruppen und Policys

## Aufgabe 1: Codeeditor öffnen und Quellcode herunterladen

1.  Klicken Sie in der Cloud-Konsole wie gezeigt auf das Symbol _Entwicklertools_ und dann auf _Codeeditor_. ![Codeeditor öffnen](images/open-codeeditor.png)
    
2.  Klicken Sie auf _Terminal_ > _New Terminal_, um das Terminal zu öffnen. Während des Workshops werden Sie aufgefordert, ein neues Terminal zu eröffnen. Auf diese Weise können Sie ein neues Terminal im Codeeditor öffnen. ![Terminal öffnen](images/open-terminal.png)
    
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um den Quellcode herunterzuladen. Dieser Quellcode enthält die Terraform-Skripte, mit denen die OCI-Ressourcen erstellt werden, die für diesen Workshop erforderlich sind.
    
        <copy>curl -O https://objectstorage.uk-london-1.oraclecloud.com/p/IxV3cO7Uf5VnbniLEmx8zOBhr6liix40QWNTnTy0TTBcGdrLaRNSt2IJYxBPHqdw/n/lrv4zdykjqrj/b/ankit-bucket/o/devops_helidon_to_instance_ocw_hol.zip
        unzip ~/devops_helidon_to_instance_ocw_hol.zip</copy>
        

![Quellcode herunterladen](images/download-sourcecode.png)

4.  Um den Quellcode _`devops_helidon_to_instance_ocw_hol`_ im **Codeeditor** zu öffnen, klicken Sie auf _Datei_ > _Öffnen_. ![Quelltext öffnen](images/open-sourcecode.png)
    
5.  Wählen Sie _`devops_helidon_to_instance_ocw_hol`_ im Home-Verzeichnis aus, und klicken Sie auf _Öffnen_. ![Offene Devops](images/open-devops.png)
    
6.  Klicken Sie im Ordner _`devops_helidon_to_instance_ocw_hol`_ auf den Dateinamen _terraform.tfvars_ (siehe Abbildung). Sie sehen, dass die Variablen **`tenancy_ocid`**, **region**, **`compartment_ocid`**, **`user_ocid`** vorhanden sind, die wir für Ihren Test-Cloud-Account anpassen werden. ![Offene Fernseher](images/open-tfvars.png)
    

> Wenn Sie das Projekt nicht sehen. Möglicherweise müssen Sie im Codeeditor auf das Symbol _Explorer_ klicken. ![Exlorer](images/explorer.png)

7.  Öffnen Sie in Ihrem Browser eine **neue Registerkarte** für die [Cloud-Konsole](https://cloud.oracle.com/). Wir verwenden diese Registerkarte, um den Wert der oben genannten Variablen zu erhalten.
    
8.  Um **`tenancy_ocid`** abzurufen, klicken Sie auf _Benutzersymbol_ und dann wie dargestellt auf _Mandant_. ![OCID des Mandanten abrufen](images/get-tenancyocid.png)
    
9.  Klicken Sie auf _Kopieren_, um die **OCID** für den Mandanten zu kopieren und in die Datei _terraform.tfvars_ als Wert von _`tenancy_ocid`_ einzufügen. ![Mandanten-OCID kopieren](images/copy-tenancyocid.png)
    
10.  Um **`user_ocid`** abzurufen, klicken Sie auf _Benutzersymbol_ und dann wie dargestellt auf _Mein Profil_. ![Mein Profil](images/my-profile.png)
    
11.  Klicken Sie auf _Kopieren_, um die **OCID** für den Benutzer zu kopieren und in die Datei _terraform.tfvars_ als Wert von _`user_ocid`_ einzufügen. ![Benutzer-OCID kopieren](images/copy-userocid.png)
    
12.  Um den Regionsnamen zu suchen, klicken Sie wie unten gezeigt auf **Regionen verwalten**. Kopieren Sie dann die **Regions-ID** Ihrer Hauptregion, und fügen Sie sie als Wert von _region_ in die Datei _terraform.tfvars_ ein. ![Region verwalten](images/manage-region.png) ![Regionsname](images/region-name.png)
    
13.  Schließlich sollte die _terraform.tfvars_ wie folgt aussehen. Lassen Sie den Wert _`compartment_ocid`_ unverändert. Der Wert wird ersetzt, sobald das Compartment als Teil von Aufgabe 2 erstellt wird. ![Init tfvars](images/init-tfvars.png)
    

## Aufgabe 2: Compartment, dynamische Gruppen, Benutzergruppen und Policys erstellen

Ziel dieser Aufgabe ist es, die Umgebung für das Setup DevOps vorzubereiten, indem ein Compartment, eine dynamische Gruppe, eine Benutzergruppe und Policys erstellt werden. Dieser Abschnitt erfordert einen Benutzer mit Administratorberechtigungen. Wenn Sie es nicht haben, fordern Sie einen anderen Benutzer mit dieser Berechtigung an, dies für Sie auszuführen.

1.  Öffnen Sie ein Terminal, kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um zum Ordner _init_ zu navigieren.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/init/</copy>
        
2.  Im Codeeditor können Sie verschiedene Dateien im Ordner _init_ anzeigen. Dies sind die Terraform-Skripte, mit denen das Compartment, dynamische Gruppen, Benutzergruppen und Policys erstellt werden. ![Initialisierungsdateien](images/init-files.png)
    
3.  Kopieren Sie die folgenden Befehle, und fügen Sie sie ein, um das Compartment, die dynamischen Gruppen, die Benutzergruppe und die Policys bereitzustellen.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    Sie sehen die Ausgabe ähnlich wie unten. Beachten Sie die **Ausgabe**, um zu erfahren, was das terraform-Skript erstellt. Außerdem können Sie den Code referenzieren, um dessen Implementierung anzuzeigen. ![init erstellt](images/init-created.png)
    
    > Wenn Fehler auftreten, stellen Sie sicher, dass Sie die Werte in der Datei **terraform.tfvars** richtig festgelegt haben.
    

## Aufgabe 3: DevOps-Projekt und die zugehörigen Ressourcen erstellen

1.  Kopieren Sie im Terminal den folgenden Befehl, und fügen Sie ihn ein, um zum _main_\-Ordner zu navigieren.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main/</copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Datei **compartment\_ocid** eines neu erstellten Compartments in _terraform.tfvars_ im Ordner _`devops_helidon_to_instance_ocw_hol`_ zu aktualisieren.
    
        <copy>../utils/update_compartment.sh</copy>
        
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um alle DevOps-Ressourcen bereitzustellen.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    > **Lesen Sie:** Dadurch werden die folgenden Ressourcen bereitgestellt, die für DevOps erforderlich sind:
    
    *   **OCI-DevOps-Service**
        *   **OCI DevOps-Projekt**, das alle für dieses Projekt erforderlichen DevOps-Komponenten enthält.
        *   **OCI-Code-Repository**, das das Quellcodeprojekt der Anwendung hostet.
        *   **DevOps-Build-Pipeline** mit den folgenden Phasen:
            *   **Build verwalten** - Führt Schritte zum Herunterladen von JDK20, Maven und Erstellen der Helidon-Anwendung aus
            *   **Artefakte bereitstellen** - Lädt die erstellte Helidon-App und das Deployment in das Artefakt-Repository hoch
            *   **Deployment auslösen** - Löst die Deployment-Pipeline aus
        *   **DevOps Deployment-Pipeline**, die in der Zielumgebung Folgendes ausführt:
            *   JDK20 herunterladen
            *   OCI-CLI installieren und zum Herunterladen des Anwendungsgegenstands verwenden
            *   Anwendung ausführen
        *   **DevOps-Instanzgruppenumgebung**, mit der die Deployment-Pipeline die erstellte OCI-Compute-Instanz als Deployment-Ziel identifiziert.
        *   **DevOps-Trigger**, der den Pipelinelebenszyklus von Anfang bis Ende aufruft, wenn ein Pushereignis im OCI-Code-Repository auftritt.
    *   **OCI Artifact Registry**
        *   **OCI-Artefakt-Repository**, in dem die erstellten Helidon-App-Binärdateien und das Deployment-Manifest als versionierte Artefakte gehostet werden.
    *   **OCI-Plattform**
        *   **OCI-Compute-Instanz**, die Port 8080 von der Firewall öffnet. Hier wird die Anwendung schließlich bereitgestellt.
    *   **Virtuelles OCI-Cloud-Netzwerk (VCN) mit Sicherheitsliste** mit einem Ingress, der Port 8080 öffnet. Auf Port 8080 wird von dort aus auf die Helidon-Anwendung zugegriffen. Das OCI-VCN wird von der OCI-Compute-Instanz für seine Netzwerkanforderungen verwendet.
4.  Das folgende Diagramm zeigt, wie das Setup von DevOps funktioniert: ![devops-Diagramm](images/devops-diagram.png)
    
5.  Sie erhalten eine ähnliche Ausgabe wie unten gezeigt. ![tf-Ausgabe](images/tf-output.png)
    

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Keith Lustria
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
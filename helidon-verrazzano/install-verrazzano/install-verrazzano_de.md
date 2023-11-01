# Verrazzano installieren

## Einführung

In dieser Übung werden die Schritte zur Installation von Verrazzano auf einem Kubernetes-Cluster in Oracle Cloud Infrastructure beschrieben.

Geschätzte Zeit: 15 Minuten

### Über Produkt/Technologie

Verrazzano ist eine End-to-End-Containerplattform für Unternehmen für die Bereitstellung cloudnativer und traditioneller Anwendungen in Multi-Cloud- und Hybridumgebungen. Es besteht aus einem kuratierten Satz von Open-Source-Komponenten - viele, die Sie bereits verwenden und vertrauen können, und einige, die speziell geschrieben wurden, um alle Teile zusammenzuführen, die Verrazzano zu einer zusammenhängenden und benutzerfreundlichen Plattform machen.

Verrazzano umfasst die folgenden Funktionen:

*   Hybrid- und Multicluster-Workload-Management
*   Spezielle Handhabung für WebLogic-, Coherence- und Helidon-Anwendungen
*   Multicluster-Infrastrukturverwaltung
*   Integrierte und vorverdrahtete Anwendungsüberwachung
*   Integrierte Sicherheit
*   Aktivierung von DevOps und GitOps

Verrazzano unterstützt die folgenden Installationsprofile: Entwicklung (`dev`), Produktion (`prod`) und verwaltetes Cluster (`managed-cluster`).

Die folgende Abbildung beschreibt die Verrazzano-Installationsprofile. ![Profil installieren](images/installprofile.png)

Um Profile in einem der folgenden Befehle zu ändern, setzen Sie die Umgebungsvariable _VZ\_PROFILE_ auf den Namen des Profils, das Sie installieren möchten.

Eine vollständige Beschreibung der Verrazzano-Konfigurationsoptionen finden Sie unter [Benutzerdefinierte Verrazzano-Ressourcendefinition](https://verrazzano.io/docs/reference/api/verrazzano/verrazzano/).

In dieser Übung installieren wir das _Entwicklungsprofil von Verrazzano_ mit den folgenden Eigenschaften:

*   Platzhalter (nip.io) DNS
*   Selbstsignierte Zertifikate
*   Gemeinsamer Beobachtbarkeitsstack, der von den Systemkomponenten und allen Anwendungen verwendet wird
*   Flüchtiger Speicher für den Beobachtungsstack (wenn die Pods neu gestartet werden, gehen alle Logs und Metriken verloren)
*   Es hat eine leichte Installation.
*   Es dient Bewertungszwecken.
*   Einknoten-Openearch-Clustertopologie.

Verrazzano installiert eine kuratierte Gruppe von Open-Source-Komponenten. In der folgenden Tabelle werden jede Komponente, ihre Version und eine kurze Beschreibung aufgeführt.

![Verrazzano-Profil](images/verrazzano-profile.png " ") ![Verrazzano-Profile](images/verrazzano-profiles.png " ")

Gemäß unserer DNS-Auswahl können wir nip.io (DNS mit Platzhalterzeichen) oder [Oracle OCI DNS](https://docs.cloud.oracle.com/en-us/iaas/Content/DNS/Concepts/dnszonemanagement.htm) verwenden. In dieser Übung installieren wir mit nip.io (DNS mit Platzhalterzeichen).

Ein Ingress-Controller ermöglicht den Zugriff auf Docker-Container nach außen (durch Angabe einer IP-Adresse). Der Ingress leitet die IP-Adresse an verschiedene Cluster weiter.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Richten Sie `kubectl` ein, um das Oracle Kubernetes Engine-Cluster zu verwenden
*   Installieren Sie die Verrazzano vz CLI.
*   Installieren Sie das Entwicklungsprofil (`dev`) von Verrazzano.

### Voraussetzungen

Verrazzano erfordert Folgendes:

*   Ein Kubernetes-Cluster und ein kompatibles `kubectl`.
*   Mindestens 2 CPUs, 100 GB Festplattenspeicher und 16 GB RAM sind auf den Kubernetes-Worker-Knoten verfügbar. Dies reicht aus, um das Entwicklungsprofil von Verrazzano zu installieren. Je nach Ressourcenbedarf der Anwendungen, die Sie bereitstellen, reicht dies möglicherweise für das Deployment Ihrer Anwendungen aus.
*   In Übung 1 haben Sie ein Kubernetes-Cluster auf Oracle Cloud Infrastructure erstellt. Dieses Kubernetes-Cluster, _cluster1_, wird zur Installation des Entwicklungsprofils von Verrazzano verwendet.

## Aufgabe 1: `kubectl` konfigurieren (Kubernetes-Cluster-CLI)

Oracle Cloud Infrastructure (OCI) Cloud Shell ist ein browserbasiertes Terminal, auf das von der Oracle Cloud-Konsole aus zugegriffen werden kann. Die Cloud Shell bietet Zugriff auf eine Linux-Shell mit einer vorab authentifizierten Oracle Cloud Infrastructure-CLI und anderen nützlichen Tools (_Git, kubectl, helm, OCI-CLI_), um die Verrazzano-Tutorials abzuschließen. Auf die Cloud Shell kann von der Konsole aus zugegriffen werden. Die Cloud Shell wird in der Oracle Cloud-Konsole als persistenter Frame der Konsole angezeigt und bleibt aktiv, wenn Sie zu verschiedenen Seiten der Konsole navigieren.

Sie verwenden die _Cloud Shell_, um diesen Workshop abzuschließen.

Wir verwenden `kubectl`, um das Cluster remote mit der Cloud Shell zu verwalten. Sie benötigt eine `kubeconfig`\-Datei. Diese wird mit der OCI-CLI generiert, die vorab authentifiziert ist. Daher muss kein Setup ausgeführt werden, bevor Sie sie verwenden können.

1.  Klicken Sie auf der Detailseite des Clusters auf _Auf Cluster zugreifen_.
    
    > Wenn Sie diese Seite verlassen haben, öffnen Sie das Navigationsmenü, und wählen Sie unter _Entwicklerservices_ die Option _Kubernetes-Cluster (OKE)_ aus. Wählen Sie Ihr Cluster aus, und gehen Sie zur Detailseite.
    
    ![Zugriff auf Cluster](images/access-cluster.png " ")
    
    > Es wird ein Dialogfeld angezeigt, in dem Sie die Cloud Shell öffnen können. Es enthält den benutzerdefinierten OCI-Befehl, den Sie ausführen müssen, um eine Kubernetes-Konfigurationsdatei zu erstellen.
    
2.  Behalten Sie den _Cloud Shell-Standardzugriff_ bei, und wählen Sie zuerst den Link _Kopieren_ aus, um den Befehl `oci ce...` in die Cloud Shell zu kopieren.
    
    ![kubectl-Konfiguration kopieren](images/copy-config.png " ")
    
3.  Klicken Sie jetzt auf _Cloud Shell starten_, um die integrierte Konsole zu öffnen. Schließen Sie dann das Konfigurationsdialogfeld, bevor Sie den Befehl in die _Cloud Shell_ einfügen.
    
    ![Cloud Shell starten](images/launch-cloudshell.png " ")
    
4.  Kopieren Sie den Befehl aus der Zwischenablage (Ctrl+V oder klicken Sie mit der rechten Maustaste, und kopieren Sie ihn in die Cloud Shell, und führen Sie den Befehl aus.
    
    Beispiel: Der Befehl sieht wie folgt aus:
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl-Konfiguration](images/kube-config.png " ")
    
5.  Prüfen Sie jetzt, ob `kubectl` funktioniert, z.B. mit dem Befehl `get node`. Sie müssen diesen Befehl möglicherweise mehrmals ausführen, bis die Ausgabe ähnlich der folgenden angezeigt wird.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.101   Ready    node    11m   v1.26.2
        10.0.10.29    Ready    node    11m   v1.26.2
        10.0.10.37    Ready    node    11m   v1.26.2
        
    
    > Wenn die Knoteninformationen angezeigt werden, war die Konfiguration erfolgreich.
    

## Aufgabe 2: Verrazzano-CLI installieren

1.  Laden Sie die neueste vz-CLI herunter.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
        

0 0 0 0 0 0 0 0 --:-- --:--:-- --:--:-- 0 100 39.7M 100 39.7M 0 0 23.6M 0 0:00:01 0:00:01 --:--:-- 32.7M \`\`

2.  Laden Sie die Prüfsummendatei herunter.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        

Die Ausgabe sollte der folgenden ähneln:

    ```bash
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    

0 0 0 0 0 0 0 0 --:--:--:-- --:--:--:-- 0 100 102 100 102 0 0 218 0 --:--:-- --:--:--:--:--:--:-- 218 \`\`

3.  Validieren Sie die Binärdatei anhand der Prüfsummendatei.
    
        <copy>sha256sum -c verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        verrazzano-1.6.2-linux-amd64.tar.gz: OK
        
4.  Auspacken und in das vz-Binärverzeichnis verschieben,
    
        <copy>tar xvf verrazzano-1.6.2-linux-amd64.tar.gz
        cd ~/verrazzano-1.6.2/bin/</copy>
        
5.  Testen Sie, ob die installierte Version auf dem neuesten Stand ist.
    
        <copy>./vz version</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        Version: v1.6.2
        BuildDate: 2023-07-21T12:06:02Z
        GitCommit: 5cd8a2f8670000d2ef0b02404e3169699d87f26b
        

## Aufgabe 3: Installation des Verrazzano-Entwicklungsprofils

Ein Installationsprofil ist eine bekannte Konfiguration von Verrazzano-Einstellungen, die nach Namen referenziert werden kann und dann nach Bedarf angepasst werden kann.

Verrazzano unterstützt die folgenden Installationsprofile: Entwicklung (`dev`), Produktion (`prod`) und verwaltetes Cluster (`managed-cluster`).

1.  Installieren Sie es mit der DNS-Methode nip.io. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein, um Verrazzano zu installieren.
    
        <copy>./vz install -f - <<EOF
        apiVersion: install.verrazzano.io/v1beta1
        kind: Verrazzano
        metadata:
          name: example-verrazzano
        spec:
          profile: dev
        EOF
        </copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        Installing Verrazzano version v1.6.2
        Applying the file https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-platform-operator.yaml
        customresourcedefinition.apiextensions.k8s.io/verrazzanos.install.verrazzano.io created
        namespace/verrazzano-install created
        serviceaccount/verrazzano-platform-operator created
        clusterrole.rbac.authorization.k8s.io/verrazzano-managed-cluster created
        clusterrolebinding.rbac.authorization.k8s.io/verrazzano-platform-operator created
        service/verrazzano-platform-operator created
        service/verrazzano-platform-operator-webhook created
        deployment.apps/verrazzano-platform-operator created
        deployment.apps/verrazzano-platform-operator-webhook created
        mutatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-mysql-backup created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-operator-webhook created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-mysqlinstalloverrides created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-requirements-validator created
        Waiting for verrazzano-platform-operator to be ready before starting install - 23 seconds
        
    
    > Die Installation dauert etwa 15 bis 20 Minuten. Mit diesem Befehl wird der Verrazzano-Plattformoperator installiert und die benutzerdefinierte Verrazzano-Ressource angewendet. Installationsprotokolle werden bis zum Abschluss der Installation oder bis zum Erreichen des Standardtimeouts (30 m) in das Befehlsfenster gestreamt.
    
2.  Lassen Sie _Cloud Shell_ geöffnet, und führen Sie die Installation aus.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
# Verrazzano installieren

## Einführung

In dieser Übung werden die Schritte zur Installation von Verrazzano auf einem Kubernetes-Cluster in Oracle Cloud Infrastructure beschrieben.

Geschätzte Zeit: 20 Minuten

### Über Produkt/Technologie

Verrazzano ist eine End-to-End-Containerplattform für Unternehmen für die Bereitstellung cloudnativer und traditioneller Anwendungen in Multicloud- und Hybridumgebungen. Es besteht aus einer kuratierten Reihe von Open-Source-Komponenten - viele, die Sie bereits verwenden und vertrauen können, und einige, die speziell geschrieben wurden, um alle Teile zusammenzuführen, die Verrazzano zu einer zusammenhängenden und einfach zu bedienenden Plattform machen.

Verrazzano umfasst die folgenden Funktionen:

*   Hybrid- und Multicluster-Workload-Management
*   Spezielle Handhabung für WebLogic-, Coherence- und Helidon-Anwendungen
*   Multicluster-Infrastrukturverwaltung
*   Integrierte und vorverdrahtete Anwendungsüberwachung
*   Integrierte Sicherheit
*   Aktivierung von DevOps und GitOps

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Installieren Sie das Verrazzano-Befehlszeilentool.
*   Installieren Sie das Entwicklungsprofil (`dev`) von Verrazzano.
*   Prüfen Sie die erfolgreiche Verrazzano-Installation.

### Voraussetzungen

Verrazzano erfordert Folgendes:

*   Ein Kubernetes-Cluster und ein kompatibles `kubectl`.
*   Mindestens 2 CPUs, 100 GB Festplattenspeicher und 16 GB RAM sind auf den Kubernetes-Worker-Knoten verfügbar. Dies reicht aus, um das Entwicklungsprofil von Verrazzano zu installieren. Je nach Ressourcenbedarf der Anwendungen, die Sie bereitstellen, reicht dies möglicherweise für das Deployment Ihrer Anwendungen aus.

In Übung 1 haben Sie ein Kubernetes-Cluster auf Oracle Cloud Infrastructure erstellt. Sie verwenden dieses Kubernetes-Cluster, \*cluster1\*, zur Installation des Entwicklungsprofils von Verrazzano. In Übung 1 haben Sie eine Konfigurationsdatei für den Zugriff auf das Kubernetes-Cluster in Oracle Cloud Infrastructure erstellt. Sie verwenden dieses Kubernetes-Cluster, \*cluster1\*, zur Installation des Entwicklungsprofils von Verrazzano.

## Aufgabe 1: vz-CLI installieren

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
        

## Aufgabe 2: Installation des Verrazzano-Entwicklungsprofils

Ein Installationsprofil ist eine bekannte Konfiguration von Verrazzano-Einstellungen, die nach Namen referenziert werden kann und dann nach Bedarf angepasst werden kann.

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
        \> Die Installation dauert etwa 15 bis 20 Minuten. Mit diesem Befehl wird der Verrazzano-Plattformoperator installiert und die benutzerdefinierte Verrazzano-Ressource angewendet. \> Die Installation dauert etwa 8 bis 10 Minuten. Mit diesem Befehl wird der Verrazzano-Plattformoperator installiert und die benutzerdefinierte Verrazzano-Ressource angewendet.
2.  Warten Sie, bis die Installation abgeschlossen ist. Installationsprotokolle werden bis zum Abschluss der Installation oder bis zum Erreichen des Standardtimeouts (30 m) in das Befehlsfenster gestreamt.
    

## Aufgabe 3: Überprüfung einer erfolgreichen Verrazzano-Installation

Verrazzano installiert mehrere Objekte in mehreren Namespaces. Verrazzano-Komponenten werden im Namespace _verrazzano-system_ installiert.

1.  Stellen Sie sicher, dass alle Pods, die den mehreren Objekten zugeordnet sind, den Status _Wird ausgeführt_ haben.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        $   kubectl get pods -n verrazzano-system
        NAME                                    READY   STATUS  RESTARTS AGE
        coherence-operator-b5dc669c6-rk2sm      1/1     Running   1       23m
        fluentd-54f5x                           2/2     Running   1       11m
        fluentd-h7mgh                           2/2     Running   1       11m
        fluentd-xcdfz                           2/2     Running   0       11m
        oam-kubernetes-runtime-5b48f944b-cx7b9  1/1     Running   0       24m
        verrazzano-application-operator-665c5   1/1     Running   0       22m
        verrazzano-application-operator-webhook 1/1     Running   0       22m
        verrazzano-authproxy-67776ff58b-8l      3/3     Running   0       21m
        verrazzano-cluster-operator-67dc5695    1/1     Running   0       14m
        verrazzano-cluster-operator-webhook     1/1     Running   0       14m
        verrazzano-console-7d95c98cb9-9ql2      2/2     Running   0       20m
        verrazzano-monitoring-operator-59f      2/2     Running   0       21m
        vmi-system-es-master-0                  2/2     Running   0       19m
        vmi-system-grafana-7fd956b585-2tbgl     3/3     Running   0       19m
        vmi-system-kiali-dd87546d6-ddxss        2/2     Running   0       22m
        vmi-system-osd-7687d6fccf-nm7kt         2/2     Running   0       14m
        weblogic-operator-54979449f4-njgrq      2/2     Running   0       22m
        weblogic-operator-webhook-f7ff8c8cf     1/1     Running   0       22m
        
    
    Lassen Sie die _Cloud Shell_ geöffnet. Sie benötigen sie für Übung 4.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
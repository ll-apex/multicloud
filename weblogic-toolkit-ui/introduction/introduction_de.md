# Einführung

## Über diesen Workshop

Dieser Workshop zeigt eine End-to-End-Migration einer On-Premise-WebLogic Server-Domain in die Container und macht sie mit Oracle Container Engine for Kubernetes (OKE) in OCI ausführbar. Wir zeigen die grafische Oberfläche der WebLogic Kubernetes Toolkit UI sowie WebLogic Deployer Tool und Weblogic Kubernetes Operator. Wir zeigen, wie der Migrationsprozess mit einem DevOps-orientierten Tooling vereinfacht und beschleunigt werden kann.

![Laborfluss](images/lab-flow.png)

Voraussichtliche Zeit: 120 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Vollständiger Workshop](videohub:1_q1mmkimy)

### Über Produkt/Technologie

Das WebLogic Kubernetes Toolkit (WKT) ist eine Sammlung von Open-Source-Tools, mit denen Sie WebLogic-basierte Anwendungen für die Ausführung in Linux-Containern in einem Kubernetes-Cluster bereitstellen können. WKT umfasst die folgenden Tools:  

*   [WebLogic Deploy Tooling (WDT)](https://github.com/oracle/weblogic-deploy-tooling) - Eine Gruppe von Einzweck-Lebenszyklustools, die von einer einzelnen Metadatenmodelldarstellung einer WebLogic-Domain ausgeführt werden.
*   [WebLogic Image Tool (WIT)](https://github.com/oracle/weblogic-image-tool) - Ein Tool zum Erstellen von Linux-Containerimages zum Ausführen von WebLogic-Domains.
*   [WebLogic Kubernetes Operator (WKO)](https://github.com/oracle/weblogic-kubernetes-operator) - Ein Kubernetes-Operator, mit dem WebLogic-Domains nativ in einem Kubernetes-Cluster ausgeführt werden können.

Die WKT-UI bietet eine grafische Benutzeroberfläche, die WKT-Tools, Docker, Helm und den Kubernetes-Client (kubectl) umschließt und Sie durch den Prozess beim Erstellen und Ändern eines Modells führt der Domain WebLogic, Erstellen eines Linux-Containerimages zur Ausführung der Domain und Einrichten und Bereitstellen der Software und Konfiguration, die für das Deployment und den Zugriff auf die Domain in Ihrem Kubernetes-Cluster erforderlich ist.

### Ziele

*   Erstellung einer virtuellen Maschine vom Marketplace-Image
*   Oracle Kubernetes Engine-Instanz auf Oracle Cloud Infrastructure einrichten
*   WKT UI-Projekt ändern und Modelldatei erstellen
*   Auxiliary Image erstellen und in Oracle Container Image Registry übertragen
*   Operator WebLogic für Oracle Kubernetes-Cluster (OKE) bereitstellen
*   Domain WebLogic für Oracle Kubernetes-Cluster (OKE) bereitstellen
*   Ingress-Controller für Oracle Kubernetes-Cluster (OKE) bereitstellen
*   Load Balancing zwischen Managed Server-Pods anzeigen und WebLogic-Remote-Konsole erkunden
*   WebLogic-Cluster skalieren
*   Bereitgestellte Anwendung durch einen rollierenden Neustart des neuen Images aktualisieren

## Weitere Informationen

*   [WebLogic Kubernetes-Toolkit-UI](https://oracle.github.io/weblogic-toolkit-ui/)

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
# Einführung

## Über diesen Workshop

In diesem Workshop lernen Sie, wie Sie eine **Helidon-Microserviceanwendung** von Anfang bis Ende mit dem **OCI DevOps-Service** erstellen, verwalten und upgraden. Wir zeigen Ihnen, wie Sie den gesamten Lebenszyklusmanagementprozess mithilfe von JDK20 mit Virtual Threads beschleunigen und optimieren können.

Voraussichtliche Zeit: 5 Minuten

### Ziele

In diesem Workshop werden Sie:

*   Compartment, dynamische Gruppe und Policys erstellen
*   Erstellen Sie ein DevOps-Projekt und zugehörige Ressourcen mit Terraform
*   Erstellen und Bereitstellen der Helidon MP-Anwendung für Compute-Instanzen mit OCI DevOps
*   OCI Monitoring SDK-Integration anzeigen
*   OCI Logging SDK-Integration validieren, um Nachrichten an den Logging-Service zu übertragen
*   Patching-Szenario simulieren
*   Objektspeicherzugriff aus der Helidon MP-Anwendung hinzufügen

### Voraussetzungen

*   Free Tier-(Testversion), kostenpflichtiger oder LiveLabs Cloud-Account für Oracle
*   [Vertrautheit mit der OCI-Konsole](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
*   [Networking - Überblick](https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm)
*   [Vertrautheit mit Compartments](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/concepts.htm)
*   [OCI DevOps-Services](https://docs.oracle.com/en-us/iaas/Content/devops/using/home.htm)

## Oracle DevOps

Der Oracle Cloud Infrastructure DevOps-Service bietet Entwicklern eine End-to-End-CI/CD-Plattform. OCI DevOps-Services decken im Großen und Ganzen alle wichtigen Anforderungen für einen Softwarelebenszyklus ab. wie

*   **OCI-Deployment-Pipelines** - Automatisieren Sie Releases mit deklarativen Pipeline-Releasestrategien für OCI-Plattformen wie VM und Baremetals, Oracle Container Engine for Kubernetes (OKE) und OCI Functions
*   **OCI-Artefakt-Repositorys** - Ein Ort zum Speichern versionierter Artefakte, einschließlich unveränderlicher Artefakte.
*   **OCI-Code-Repositorys** - OCI hat einen skalierbaren Code-Repository-Service bereitgestellt.
*   **OCI Build-Pipelines** - Ein serverloser, skalierbarer Service zur Automatisierung von Build-, Test-, Artefakt- und Deployment-Aufrufen.

![Devops-Architektur](images/oci-devops.png)

## Rollenspielarchitektur

Sie erstellen und implementieren die unten genannte Architektur mit OCI-Services und -Features.

![Devops-Diagramm](images/devops-diagram.png)

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Weitere Informationen

*   [Referenzarchitektur: Moderne Anwendungsbereitstellungsstrategien mit Oracle Cloud Infrastructure DevOps verstehen](https://docs.oracle.com/en/solutions/mod-app-deploy-strategies-oci/index.html)
*   [https://helidon.io/](https://helidon.io/)
*   [https://medium.com/helidon](https://medium.com/helidon)
*   [https://github.com/helidon-io/helidon](https://github.com/helidon-io/helidon)
*   [https://www.youtube.com/Helidon\_Project](https://www.youtube.com/Helidon_Project)
*   [https://helidon.slack.com](https://helidon.slack.com)

## Danksagungen

*   **Autor** - Keith Lustria
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Mai 2023
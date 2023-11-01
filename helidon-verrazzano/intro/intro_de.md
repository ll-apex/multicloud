# Einführung

## Über diesen Workshop

Diese Übung bietet den Teilnehmern eine praktische Einführung in Helidon und Verrazzano. Sie lernen, wie Sie Services erstellen und zusammenstellen und auf einer Unternehmenscontainerplattform bereitstellen.

Dies ist eine BYOL-Sitzung (Bring Your Laptop), also bringen Sie Ihren Windows-, OSX- oder Linux-Laptop mit. Bevor Sie beginnen, müssen Sie JDK 11+, Apache Maven (3.6) und Docker installieren.

In dieser Übung installieren Sie Helidon-CLI-Tools und entwickeln die HTTP-Microserviceanwendung. Für Verrazzano richten Sie Oracle Kubernetes Engine (OKE) auf Oracle Cloud Infrastructure mit dem Oracle Cloud Free Tier-Account ein. Der Free Tier-Account reicht aus, um Microservice-Anwendungen auf Unternehmensebene auszuführen und zu betreiben.

Das Ziel dieses Workshops ist, dass Sie die Grundlagen der Verwendung von Helidon und Verrazzano lernen und verstehen, wie sie Ihnen bei Ihren Projekten helfen können. Wenn Sie mehr über die Funktionen dieser Projekte erfahren möchten, nutzen Sie Ihren Oracle Free Tier-Cloud-Account und Oracle Cloud Infrastructure.

Dieser Workshop ist so selbsterklärend wie möglich, kann aber gerne um Klärung oder Unterstützung auf dem Weg bitten.

> Das Provisioning der Oracle Kubernetes Engine (OKE) und die Installation von Verrazzano kann einige Minuten dauern. Um Zeit zu sparen, werden Sie aufgefordert, Ihr Entwicklungs- und Umgebungssetup parallel auszuführen. Befolgen Sie die Anweisungen, und wechseln Sie zwischen den Aufgaben, wenn dies erforderlich und erforderlich ist.

Geschätzte Zeit: 90 Minuten

### Ziele

*   Richten Sie Ihren Oracle Cloud Free Tier-Account ein (sofern noch nicht geschehen).
*   Richten Sie eine Oracle Kubernetes Engine-Instanz auf Oracle Cloud Infrastructure ein.
*   Installieren Sie die Verrazzano-Plattform.
*   Stellen Sie die _Helidon MP_\-Anwendung bereit.
*   Überwachen Sie die _Helidon MP_\-Anwendung mit Verrazzano-Tools, einschließlich OpenSearch, Grafana und Prometheus.

### Voraussetzungen

In dieser Übung wird davon ausgegangen, dass auf Ihrem Rechner Folgendes installiert ist:

*   JDK ab 11
*   Apache Maven (3.6)
*   Docker

## Über Helidon

Helidon ist ein Open-Source-Microservices-Framework von Oracle, das eine Sammlung von Java-Librarys bereitstellt, mit denen leichte und schnelle Microservices-basierte Anwendungen erstellt werden können. Das Framework unterstützt zwei Programmiermodelle zum Schreiben von Microservices: Helidon SE und Helidon MP.

Während Helidon SE als Mikroframework konzipiert ist, das das reaktive Programmiermodell unterstützt, ist Helidon MP eine Implementierung der Spezifikation MicroProfile. Da MicroProfile seine Wurzeln in Java EE hat, verfolgen die MicroProfile-APIs einen vertrauten, deklarativen Ansatz mit starker Verwendung von Anmerkungen. Dies macht es zu einer guten Wahl für Java EE-Entwickler.

Die MicroProfile-Funktionen zielen auf die Implementierung von Microservices ab. Sie finden APIs zum Definieren von REST-Clients, zum Überwachen der Anwendung, zum Lesen technischer und funktionaler Statistiken und zum Konfigurieren der Anwendung. Helidon hat außerdem dem Kernsatz der Microprofile-APIs zusätzliche APIs hinzugefügt, die Ihnen alle Funktionen bieten, die Sie zum Schreiben moderner cloudnativer Anwendungen benötigen.

> Der [MicroProfile](https://microprofile.io/)\-Standard baut auf Jakarta EE auf. Wie Jakarta EE ist MicroProfile Open Source und wird von der Eclipse Foundation entwickelt. Die Implementierung mit MicroProfile erfolgt in den Bibliotheken oder Anwendungsservern, die den Standard implementieren, genau wie Jakarta EE.

## Über Verrazzano

Verrazzano ist eine End-to-End-Containerplattform für Unternehmen für die Bereitstellung cloudnativer und traditioneller Anwendungen in Multi-Cloud- und Hybridumgebungen. Es besteht aus einem kuratierten Satz von Open-Source-Komponenten - viele, die Sie bereits verwenden und vertrauen können, und einige, die speziell geschrieben wurden, um alle Teile zusammenzuführen, die Verrazzano zu einer zusammenhängenden und benutzerfreundlichen Plattform machen.

Verrazzano umfasst die folgenden Funktionen:

*   Hybrid- und Multicluster-Workload-Management
*   Spezielle Handhabung für WebLogic-, Coherence- und Helidon-Anwendungen
*   Multicluster-Infrastrukturverwaltung
*   Integrierte und vorverdrahtete Anwendungsüberwachung
*   Integrierte Sicherheit
*   Aktivierung von DevOps und GitOps

![Verrazzano](images/verrazzano.png)

## Weitere Informationen

*   [https://helidon.io](https://helidon.io)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
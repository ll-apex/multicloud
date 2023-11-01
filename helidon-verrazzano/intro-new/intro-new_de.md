# Einführung

## Über diesen Workshop

Diese Übung bietet den Teilnehmern eine praktische Einführung in Oracle Kubernetes Engine, Verrazzano und Helidon. Sie lernen, wie Sie eine Beispiel-Hello-Helidon-Anwendung mit Verrazzano auf OKE bereitstellen.

In dieser Übung richten Sie Oracle Kubernetes Engine (OKE) auf Oracle Cloud Infrastructure (OCI) mit dem Oracle Cloud Free Tier-Account ein. Der Free Tier-Account reicht aus, um Microservice-Anwendungen auf Unternehmensebene auszuführen und zu betreiben.

Das Ziel dieses Workshops ist, dass Sie die Grundlagen der Verwendung von OKE und Verrazzano lernen und verstehen, wie sie Ihnen bei Ihrem Unternehmen helfen können. Wenn Sie mehr über die Funktionen dieser Projekte erfahren möchten, nutzen Sie Ihren Oracle Free Tier-Cloud-Account und Oracle Cloud Infrastructure.

Dieser Workshop ist so selbsterklärend wie möglich, kann aber gerne um Klärung oder Unterstützung auf dem Weg bitten.

Geschätzte Zeit: 60 Minuten

### Ziele

*   Richten Sie Ihren Oracle Cloud Free Tier-Account ein (sofern noch nicht geschehen).
*   Richten Sie eine Oracle Kubernetes Engine-Instanz auf Oracle Cloud Infrastructure ein.
*   Installieren Sie die Verrazzano-Plattform.
*   Stellen Sie die Anwendung _Simple Hello Helidon_ bereit.
*   Überwachen Sie die _Simple Hello Helidon_\-Anwendung mit Verrazzano-Tools, einschließlich Rancher, Opensearch, Grafana und Prometheus.

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.

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
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, März 2023
# Einführung

## Über diesen Workshop

Die Bereitstellung einer Tomcat-Anwendung auf einem Kubernetes-Cluster kann ein komplexer Prozess sein, bei dem mehrere Container verwaltet, Netzwerke konfiguriert und High Availability und Skalierbarkeit sichergestellt werden. Um diesen Prozess zu vereinfachen, setzen viele Unternehmen auf Containerisierungstechnologien wie Docker und Kubernetes.

Docker ermöglicht die Erstellung portabler Containerimages, die auf jeder Plattform bereitgestellt werden können. Kubernetes bietet eine leistungsstarke Orchestrierungs-Engine, die das Deployment und die Skalierung von Containern verwalten kann.

Oracle Verrazzano ist eine native Cloud-Plattform, die ein einheitliches Managementerlebnis für das Deployment und den Betrieb containerisierter Anwendungen auf Kubernetes-Clustern bietet. Sie vereinfacht die Bereitstellung komplexer Anwendungen, indem sie ein konsistentes Set von Tools und APIs bereitstellt, mit denen Anwendungen über mehrere Cluster hinweg verwaltet und überwacht werden können.

In diesem Workshop werden wir den Prozess des Deployments eines Tomcat-Anwendungs-Docker-Images in einem Oracle Kubernetes-Cluster mit Verrazzano untersuchen. Wir werden die Schritte zum Erstellen eines Docker-Images, Konfigurieren von Verrazzano, Bereitstellen der Anwendung und Überwachen ihrer Performance behandeln.

Am Ende dieses Workshops haben Sie ein klares Verständnis dafür, wie Sie containerisierte Anwendungen mit Verrazzano auf einem Oracle Kubernetes-Cluster bereitstellen.

Dieser Workshop ist so selbsterklärend wie möglich, kann aber gerne um Klärung oder Unterstützung auf dem Weg bitten.

Geschätzte Zeit: 90 Minuten

### Ziele

*   Richten Sie Ihren Oracle Cloud Free Tier-Account ein (sofern noch nicht geschehen).
*   Richten Sie eine Oracle Kubernetes Engine-Instanz auf Oracle Cloud Infrastructure ein.
*   Installieren Sie das Verrazzano-Produktionsprofil.
*   Erstellen Sie ein Beispiel-Tomcat-Anwendungsdocker-Image.
*   Stellen Sie tomcat-Anwendung mit Verrazzano in OKE bereit.
*   Entdecken Sie die Grafana-, Prometheus- und OpenSearch Dashboard-Konsole.

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.

## Weitere Informationen

**Über Tomcat**

Apache Tomcat ist ein Open-Source-Webserver und Servlet-Container, der häufig zur Bereitstellung von Java-basierten Webanwendungen verwendet wird. Tomcat ist leicht, effizient und benutzerfreundlich und bietet zahlreiche Funktionen für die Verwaltung und Bereitstellung von Webanwendungen.

**Über Verrazzano**

Verrazzano ist eine End-to-End-Containerplattform für Unternehmen für die Bereitstellung cloudnativer und traditioneller Anwendungen in Multi-Cloud- und Hybridumgebungen. Es besteht aus einem kuratierten Satz von Open-Source-Komponenten - viele, die Sie bereits verwenden und vertrauen können, und einige, die speziell geschrieben wurden, um alle Teile zusammenzuführen, die Verrazzano zu einer zusammenhängenden und benutzerfreundlichen Plattform machen.

Verrazzano umfasst die folgenden Funktionen:

*   Hybrid- und Multicluster-Workload-Management
*   Spezielle Handhabung für WebLogic-, Coherence- und Helidon-Anwendungen
*   Multicluster-Infrastrukturverwaltung
*   Integrierte und vorverdrahtete Anwendungsüberwachung
*   Integrierte Sicherheit
*   Aktivierung von DevOps und GitOps

![Verrazzano](images/verrazzano.png)

*   [https://tomcat.apache.org/](https://tomcat.apache.org/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
# Einführung

## Über diesen Workshop

In diesem Workshop wird erläutert, wie Spring Boot-Anwendungen mit Verrazzano in einem Oracle Kubernetes Engine-(OKE-)Cluster bereitgestellt werden.

Im heutigen digitalen Zeitalter suchen Unternehmen nach Möglichkeiten, Anwendungen schneller und effizienter zu entwickeln und bereitzustellen. Kubernetes ist zum De-facto-Standard für die Containerorchestrierung geworden, sodass Unternehmen containerisierte Anwendungen bereitstellen, verwalten und skalieren können. Oracle Kubernetes Engine (OKE) stellt einen verwalteten Kubernetes-Service bereit, der das Deployment und Management von containerisierten Anwendungen auf Oracle Cloud Infrastructure (OCI) vereinfacht.

Spring Boot ist ein beliebtes Java-basiertes Framework zum Erstellen von Webanwendungen und Microservices. Es bietet eine optimierte Entwicklungserfahrung mit einem konventionellen Konfigurationsansatz, was es zu einer ausgezeichneten Wahl für das Erstellen und Bereitstellen containerisierter Anwendungen auf Kubernetes macht.

Verrazzano ist eine native Open-Source-Plattform für Kubernetes, die eine vollständige End-to-End-Lösung für die Bereitstellung und Verwaltung cloudnativer Anwendungen bietet. Sie vereinfacht die Bereitstellung komplexer, mehrkomponentiger Anwendungen, indem sie eine einheitliche Ansicht der Anwendungstopologie sowie eine Reihe integrierter Tools für Deployment, Überwachung und Verwaltung bereitstellt.

Dieser Workshop ist so selbsterklärend wie möglich, kann aber gerne um Klärung oder Unterstützung auf dem Weg bitten.

Geschätzte Zeit: 90 Minuten

### Ziele

*   Richten Sie Ihren Oracle Cloud Free Tier-Account ein (sofern noch nicht geschehen).
*   OKE-Cluster auf OCI einrichten
*   Verrazzano auf dem Cluster installieren und konfigurieren
*   Containerimage für eine Spring Boot-Anwendung erstellen
*   Anwendung mit Verrazzano im OKE-Cluster bereitstellen
*   Überwachung und Verwaltung der Anwendung mit den integrierten Tools von Verrazzano

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.

## Weitere Informationen

**Über Springboot**

Spring Boot ist ein beliebtes Java-basiertes Framework zum Erstellen von Webanwendungen und Microservices. Es bietet eine optimierte Entwicklungserfahrung mit einem konventionellen Konfigurationsansatz, was es zu einer ausgezeichneten Wahl für das Erstellen und Bereitstellen containerisierter Anwendungen auf Kubernetes macht.

Spring Boot bietet eine Vielzahl von Funktionen, mit denen Sie Anwendungen schnell erstellen und bereitstellen können, wie eingebettete Server, automatische Konfiguration und eine Vielzahl von Bibliotheken und Tools zum Erstellen von Webanwendungen, Messaging-Systemen und datengesteuerten Anwendungen. Es bietet auch eine reiche Reihe von Tools zum Testen und Debuggen, so dass es einfacher ist, robusten und zuverlässigen Code zu schreiben.

Einer der Hauptvorteile von Spring Boot ist die einfache Integration mit anderen Technologien und Frameworks, einschließlich Datenbanksystemen, Nachrichtenwarteschlangen und Caching-Systemen. Auf diese Weise können Entwickler komplexe, verteilte Systeme erstellen, die große Workloads bewältigen und die Anforderungen moderner, cloudbasierter Anwendungen erfüllen können.

Mit seinem Fokus auf Einfachheit und Benutzerfreundlichkeit ist Spring Boot zu einer beliebten Wahl bei Entwicklern und Organisationen geworden, die moderne, cloudnative Anwendungen erstellen möchten. Es hat eine lebendige Gemeinschaft von Mitwirkenden und Benutzern und entwickelt sich ständig weiter, um die sich ändernden Anforderungen der Branche zu erfüllen.

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

*   [https://spring.io/](https://spring.io/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, April 2023
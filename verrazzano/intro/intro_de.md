# Einführung

## Über diesen Workshop

In dieser Übung wird gezeigt, wie Sie die Verrazzano-Plattform auf einem einzelnen Kubernetes-Cluster installieren und eine Beispielanwendung mit Verrazzano-Konzepten bereitstellen.

Die Beispielanwendung [Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/) besteht aus drei Hauptteilen:

*   Eine Backend-Anwendung zur Auftragsverarbeitung, eine Java EE-Anwendung mit REST-Services und eine sehr einfache JSP-UI, die Daten in einer MySQL-Datenbank speichert. Diese Anwendung wird auf WebLogic Server ausgeführt.
*   Ein Frontend-Online-Shop "Roberts Bücher", der ein allgemeiner Buchhändler ist. Dies wird als Helidon-Microservice implementiert, der Buchdaten aus Coherence abruft, einen Coherence-Cachespeicher verwendet, um Daten für den Auftragsmanager zu persistieren, und über eine React-Web-UI verfügt.
*   Ein Front-End-Online-Shop "Bobby's Books", ein Kinderbuchladen für Kinder. Dies wird als Helidon-Microservice implementiert, der Buchdaten aus einem (anderen) Coherence-Cachespeicher abruft, direkt mit dem Auftragsmanager interagiert und eine JSF-Web-UI auf WebLogic Server ausführt.

### Über Produkt/Technologie

Verrazzano ist eine End-to-End-Containerplattform für Unternehmen für die Bereitstellung cloudnativer und traditioneller Anwendungen in Multicloud- und Hybridumgebungen. Es besteht aus einer kuratierten Reihe von Open-Source-Komponenten - viele, die Sie bereits verwenden und vertrauen können, und einige, die speziell geschrieben wurden, um alle Teile zusammenzuführen, die Verrazzano zu einer zusammenhängenden und einfach zu bedienenden Plattform machen.

Verrazzano umfasst die folgenden Funktionen:

*   Hybrid- und Multicluster-Workload-Management
*   Spezielle Handhabung für WebLogic-, Coherence- und Helidon-Anwendungen
*   Multicluster-Infrastrukturverwaltung
*   Integrierte und vorverdrahtete Anwendungsüberwachung
*   Integrierte Sicherheit
*   Aktivierung von DevOps und GitOps

![Verrazzano](images/verrazzano.png)

### Ziele

1\. Richten Sie eine Oracle Kubernetes Engine-Instanz auf Oracle Cloud Infrastructure ein. 1\. Konfigurieren Sie "kubectl", um mit der Oracle Kubernetes Engine-Instanz in Oracle Cloud Infrastructure zu interagieren. 2\. Installieren und lernen Sie die Verrazzano-Plattform kennen. 3. Stellen Sie die Beispielanwendung \*Bobby's Books\* bereit. 4. Ändern und stellen Sie die Helidon-basierte Komponente \*Stock\* erneut bereit.

## Weitere Informationen

*   [Verrazzano](https://verrazzano.io/)

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
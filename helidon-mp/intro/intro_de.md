# Einführung

## Über diesen Workshop

In dieser Übung entwickeln Sie mit Helidon Project Starter die HTTP-Microservice-Anwendung. Es ist sehr anpassbar und bietet verschiedene Optionen, mit denen Benutzer Helidon-Funktionen auswählen können, die sie dem Projekt hinzufügen möchten.

Anschließend erkunden Sie den OCI Code Editor und öffnen die darin enthaltene Helidon-Anwendung. Außerdem erfahren Sie, wie Sie die GraalVM Enterprise Edition in Oracle Cloud Infrastructure (OCI) Cloud Shell verwenden.

Sie erstellen ein natives GraalVM-Image für eine Helidon-MP-Anwendung mit maven. Anschließend ändern Sie die Anwendung, indem Sie einen benutzerdefinierten Endpunkt in der vorhandenen Java-Klasse hinzufügen. Später erstellen Sie ein natives Docking-Image dieser Anwendung und übertragen es in Oracle Cloud Container Image Registry. Außerdem erstellen Sie eine Compute-Instanz, rufen das Docking-Image hier ab und führen den Docking-Container für diese Anwendung aus.

Dieser Workshop ist so selbsterklärend wie möglich, kann aber gerne um Klärung oder Unterstützung auf dem Weg bitten.

Geschätzte Zeit: 90 Minuten

### Ziele

*   Helidon MP-Anwendung generieren
*   OCI Code Editor kennenlernen
*   Natives Image für die Helidon-MP-Anwendung erstellen
*   Natives Docking-Image GraalVM für Helidon-Anwendung erstellen
*   VM erstellen
*   Docking-Container für Helidon MP-Anwendung ausführen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account

## Über Helidon

Helidon ist ein von Oracle eingeführtes Open-Source-Microservices-Framework, das eine Sammlung von Java-Librarys bereitstellt, die für die Erstellung leichter und schneller Microservices-basierter Anwendungen entwickelt wurden. Das Framework unterstützt zwei Programmiermodelle zum Schreiben von Microservices: Helidon SE und Helidon MP.

Während Helidon SE als Mikroframework konzipiert ist, das das reaktive Programmiermodell unterstützt, ist Helidon MP eine Implementierung der Spezifikation MicroProfile. Da MicroProfile seine Wurzeln in Java EE hat, verfolgen die MicroProfile-APIs einen vertrauten, deklarativen Ansatz mit starker Verwendung von Anmerkungen. Dies macht es zu einer guten Wahl für Java EE-Entwickler.

Die MicroProfile-Funktionen zielen auf die Implementierung von Microservices ab. Sie finden APIs zum Definieren von REST-Clients, zum Überwachen der Anwendung, zum Lesen technischer und funktionaler Statistiken und zum Konfigurieren der Anwendung. Helidon hat außerdem dem Kernsatz der Microprofile-APIs zusätzliche APIs hinzugefügt, die Ihnen alle Funktionen bieten, die Sie zum Schreiben moderner cloudnativer Anwendungen benötigen.

> Der [MicroProfile](https://microprofile.io/)\-Standard baut auf Jakarta EE auf. Wie Jakarta EE ist MicroProfile Open Source und wird von der Eclipse Foundation entwickelt. Die Implementierung mit MicroProfile erfolgt in den Bibliotheken oder Anwendungsservern, die den Standard implementieren, genau wie Jakarta EE.

## Weitere Informationen

*   [https://helidon.io](https://helidon.io)

## Danksagungen

*   **Autor** - Dmitry Aleksandrov
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, April 2023
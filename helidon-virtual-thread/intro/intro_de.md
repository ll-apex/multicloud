# Einführung

## Über diesen Workshop

In dieser Übung verwenden Sie den Oracle Code Editor, um Microservices mit der in Helidon 4 verfügbaren Helidon Nima WebServer zu erstellen, auszuführen und zu ändern. Nima wird von Grund auf geschrieben, um die virtuellen Threads von Java 19 zu nutzen und bietet ein einfaches blockierendes Programmiermodell.

Sie werden auch sehen, wie es sich mit einer komplexeren reaktiven Programmierung vergleicht.

In dieser Übung entwickeln Sie mit Helidon Project Starter eine Helidon Microprofile-Anwendung. Es ist sehr anpassbar und bietet verschiedene Optionen, mit denen Benutzer Helidon-Funktionen auswählen können, die sie ihrem Projekt hinzufügen möchten. Anschließend migrieren Sie die Anwendung mit virtuellen Threads zu Helidon 4, die auf der neuen Helidon Nima WebServer ausgeführt wird.

Dieser Workshop ist so selbsterklärend wie möglich, kann aber gerne um Klärung oder Unterstützung auf dem Weg bitten.

Geschätzte Zeit: 90 Minuten

### Ziele

*   OCI Code Editor kennenlernen
*   Nima-Anwendung erstellen und ausführen
*   Reaktive Anwendung erstellen und ausführen
*   Analyse der Einfachheit der Nima-Anwendung
*   Stack Trace für Nima- und Reactive-Anwendung vergleichen
*   Helidon MP-Anwendung generieren
*   Helidon 3-Anwendung zu Helidon 4 migrieren

**Über Helidon Nima**

Helidon ist ein von Oracle eingeführtes Open-Source-Microservices-Framework, das eine Sammlung von Java-Librarys bereitstellt, die für die Erstellung einer leichten und schnellen Microservices-basierten Anwendung entwickelt wurden.

In früheren Versionen von Helidon konnten Entwickler aus zwei Programmiermodellen wählen. [Helidon MP](https://helidon.io/docs/v3/#/mp/introduction) eine Implementierung des Eclipse-Standards MicroProfile mit APIs, die Jakarta EE-Entwicklern vertraut sind. Und [Helidon SE](https://helidon.io/docs/v3/#/se/introduction) ist ein schlankes, reaktives Framework.

In Helidon 4 stellt Oracle Níma vor, einen Webserver, der von Grund auf für die Verwendung virtueller JEP 425-Threads geschrieben wurde. Nima bietet ein benutzerfreundliches Programmiermodell mit hervorragender Leistung. Bei virtuellen Threads sind Threads keine knappe Ressource mehr, die sorgfältig gepoolt und verwaltet werden muss. Stattdessen sind sie eine reichliche Ressource, die nach Bedarf erstellt werden kann, um nahezu unbegrenzte gleichzeitige Anforderungen zu verarbeiten. Da jede Anforderung in ihrem dedizierten Thread ausgeführt wird, können Blockierungsvorgänge wie das Aufrufen einer Datenbank oder eines anderen Service ausgeführt werden. Und es kann dies auf einfache synchrone Weise tun, ohne Angst zu haben, einen Plattform-Thread zu blockieren und andere Anfragen zu verhungern. Sie müssen nicht mehr auf komplizierten asynchronen Code zurückgreifen, um einen hochgradig gleichzeitigen Service mit geringem Overhead zu implementieren.

Der Webserver Helidon Níma will Netty im Helidon-Ökosystem ersetzen. Es kann auch von anderen Frameworks als eingebettete Webserverkomponente verwendet werden.

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account

## Weitere Informationen

*   [https://helidon.io](https://helidon.io)

## Danksagungen

*   **Autor** - Joe DiPol
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Feb 2023
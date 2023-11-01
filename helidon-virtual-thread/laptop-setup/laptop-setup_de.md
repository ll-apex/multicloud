# Natives Image der Helidon MP-Anwendung erstellen

## Einführung

In dieser Übung lernen Sie, wie Sie die Umgebung auf Ihrem lokalen Linux-Rechner einrichten.

Geschätzte Zeit: 10 Minuten

### Ziele

*   Konfigurieren Sie das erforderliche JDK und Maven.
*   Laden Sie den Quellcode der Anwendung herunter.

### Voraussetzungen

*   Sie benötigen eine IDE, um das Projekt zu ändern, zu erstellen und auszuführen.

## Aufgabe 1: Laden Sie die erforderliche Maven- und JDK-Version herunter

1.  Öffnen Sie in Ihrer IDE ein Terminal/eine Konsole.
    
2.  Kopieren Sie die folgenden Befehle, und fügen Sie sie in das Terminal ein. Es lädt die erforderliche Version von JDK und Maven herunter und legt die PATH-Variable so fest, dass sie das erforderliche Maven und JDK verwendet.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.tar.gz
        tar -xzvf jdk-19_linux-x64_bin.tar.gz</copy>
        

## Aufgabe 2: Helidon-Quellcode herunterladen

1.  Kopieren Sie die folgenden Befehle, und fügen Sie sie in das Terminal ein, um den Quellcode der Helidon-Anwendung herunterzuladen.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Datei _helidon-levelup-2023-main.zip_ zu entpacken.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

Sie können jetzt _mit der nächsten Übung fortfahren_.

## Danksagungen

*   **Autor** - Joe DiPol
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Feb 2023
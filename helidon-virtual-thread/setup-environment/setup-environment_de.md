# Einrichten der Umgebung

## Einführung

In dieser Übung werden Sie durch die Schritte zum Einrichten der erforderlichen Umgebung für den Workshop geführt.

[Durchlauf durch Lab1](videohub:1_far2bboa)

Geschätzte Zeit: 10 Minuten

### Info über Codeeditor

Mit dem Codeeditor können Sie Code für verschiedene OCI-Services direkt über die OCI-Konsole bearbeiten und bereitstellen. Sie können jetzt Serviceworkflows und Skripte aktualisieren, ohne zwischen der Konsole und Ihren lokalen Entwicklungsumgebungen wechseln zu müssen. So können Sie schnell Prototypen von Cloud-Lösungen erstellen, neue Services erkunden und schnelle Codierungsaufgaben ausführen.

Durch die direkte Integration des Codeeditors mit Cloud Shell können Sie auf das in Cloud Shell vorinstallierte GraalVM Enterprise Native Image und JDK 17 (Java Development Kit) zugreifen.

### Ziele

*   Codeeditor einrichten
*   Laden Sie die erforderliche Maven- und JDK-Version herunter
*   Helidon-Quellcode herunterladen

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.
*   Sie müssen Firefox als Browser verwenden, um Codeeditor zu öffnen.

## Aufgabe 1: Codeeditor einrichten

1.  Klicken Sie in der Cloud-Konsole wie dargestellt auf das Symbol "Codeeditor". ![Codeeditor](images/code-editor.png)
    
2.  Klicken Sie im Code Editor auf Terminal -> Neues Terminal. ![Terminal öffnen](images/open-terminal.png)
    

## Aufgabe 2: Laden Sie die erforderliche Maven- und JDK-Version herunter

1.  Kopieren Sie die folgenden Befehle, und fügen Sie sie in das Terminal ein. Es lädt die erforderliche Version von JDK und Maven herunter.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/archive/jdk-19.0.2_linux-x64_bin.tar.gz
        tar -xzvf jdk-19.0.2_linux-x64_bin.tar.gz</copy>
        

## Aufgabe 3: Helidon-Quellcode herunterladen

1.  Kopieren Sie die folgenden Befehle, und fügen Sie sie in das Terminal ein, um den Quellcode der Helidon-Anwendung herunterzuladen.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Datei _helidon-levelup-2023-main.zip_ zu entpacken.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

Sie können jetzt _mit Übung 2 fortfahren_.

## Danksagungen

*   **Autor** - Joe DiPol
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Feb 2023
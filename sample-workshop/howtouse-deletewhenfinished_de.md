# Workshop mit einem einzigen Laborset

## Anweisungen - Löschen Sie diese Datei, wenn Sie fertig sind

1.  Öffnen Sie die Musterworkshopvorlage in Atom oder Visual Studio Code
2.  Wir haben 5 Ordner erstellt. Ein Workshop wird aus mehreren Labs erstellt.
3.  Entfernen Sie die folgenden Kommentare: _Ziele für diese Übung auflisten_
4.  Stellen Sie sicher, dass Sie Ordner und Dateinamen in Kleinbuchstaben und Bindestriche für Leerzeichen verwenden (setup-adb NOT Setup\_ADB)
5.  Ihre Bildnamen sollten beschreibende Namen haben. Nicht nur adb1, adb2, adb3. Für die Barrierefreiheit benötigen wir die Bildbeschreibungen, um zu erklären, wie das Bild aussieht. Denken Sie an Kleinbuchstaben und Bindestriche.
6.  Laden Sie unser QS-Dokument von WMS herunter. Wir finden, dass Werkstätten schneller in Produktion gehen, wenn Sie wissen, was für den Umstieg auf die Produktion erforderlich ist, und Sie das Skelett verwenden.

PS: Sie benötigen keine Readme.md. Readme's sind nur auf den obersten Bibliotheksebenen vorhanden. Wir leiten den gesamten Traffic an LiveLabs, da wir die Nutzung auf GitHub nicht verfolgen können. Erstellen Sie keine direkten Links zu GitHub, Ihr Workshop ist vielleicht sehr beliebt, aber wir können ihn nicht verfolgen, damit niemand es wissen wird.

## Absoluter Pfad für Oracle Cloud-Menü Navigation

**Übung 1: Instanz bereitstellen -> Schritt 0: Diese standardisierten Bilder für die Oracle Cloud-Navigation verwenden (allgemein für Provisioning)** - Wir haben eine Liste allgemeiner Screenshots für die Navigation im Oracle Cloud-Menü enthalten. Bitte lesen Sie diesen Abschnitt und verwenden Sie gegebenenfalls die entsprechenden absoluten Pfadbilder. Dadurch wird Ihr Workshop bei Aktualisierungen der Oracle Cloud-Benutzeroberfläche zukunftssicher.

## Ordnerstruktur

In diesem Beispiel sollen mehrere "Kinder"-Workshops aus einem längeren "Eltern"-Workshop erstellt werden. Die Kinder bestehen aus Teilen des Elternteils.

Beispielworkshop/ -- Einzelübungen

        provision/
        setup/
        dataload/
        query/
        introduction/
          introduction.md       -- description of the everything workshop, note that it is a "lab" since there is only one
    
    workshops/
       freetier/                -- freetier version of the workshop
        index.html
        manifest.json
       livelabs/                -- livelabs version of the workshop
        index.html
        manifest.json
    

### FreeTier und LiveLabs

*   "FreeTier" - enthält kostenlose Testversionen, kostenpflichtige Konten und für einige Workshops Accounts vom Typ "Immer kostenlos" (braune Schaltfläche)
*   "LiveLabs" - Dies sind Workshops, in denen von Oracle bereitgestellte Mandanten verwendet werden (grüne Schaltfläche)
*   "Desktop" - Dies ist ein neues Deployment, bei dem der Workshop in einer NoVNC-Umgebung gekapselt ist, die in einer Compute-Instanz ausgeführt wird

### Über den Workshop

Der Workshop umfasst alle 6 einzelnen Labore in einer Sequenz.

Die Ordnerstruktur enthält eine Einführung "lab", die den Workshop als einen kompletten Satz von 6 Übungen beschreibt. Hinweis: Sie müssen möglicherweise nicht für jede Eltern- und Kinderversion der Workshops eine andere Einführung haben. Dies ist nur zur Veranschaulichung gedacht.

Sehen Sie sich den Ordner Product-name-workshop/freetier an und sehen Sie sich die manifest.json-Datei an, um die Struktur zu sehen.

> **Hinweis:** Die Verwendung von "Lab n:" in den Titeln ist optional

Das Prerequisite "lab" ist die erste Übung in einem gemeinsamen Ordner im oracle-livelabs/common repo. Da diese Übung bereits vorhanden ist, können Sie stattdessen eine RAW/absolute URL verwenden:

    "filename": "https://oracle-livelabs.github.io/common/labs/cloud-login/cloud-login-livelabs2.md"        },
    

Die Datei manifest.json muss den Speicherort jeder Übung im Verhältnis zum Speicherort in der Hierarchie kennen. In dieser Struktur befinden sich Übungen auf zwei Ebenen, z.B.:

    "filename": "../../provision/provision.md"
    

### Beispiel:

Dieser [APEX-Workshop](https://oracle-livelabs.github.io/apex/spreadsheet/workshops/freetier/) ist ein gutes Beispiel für einen Workshop mit einer einzigen Gruppe von Übungen: [https://github.com/oracle-livelabs/APEX/tree/main/spreadsheet](https://github.com/oracle-livelabs/apex/tree/main/spreadsheet).
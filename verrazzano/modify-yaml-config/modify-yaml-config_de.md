# YAML-Datei der Bobby's Books-Konfiguration ändern

## Einführung

In Lab 6 haben wir das aktualisierte Docker-Image für _bobby-helidon-stock-application_ bereitgestellt. Jetzt möchten wir, dass Verrazzano die aktualisierte Anwendung und die aktualisierten Komponenten erneut bereitstellt, ohne dass sich dies auf die Services auswirkt. Dazu müssen wir die YAML-Datei so konfigurieren, dass Verrazzano das neue Bild aufnimmt und einen Pod dafür startet. Nachdem sich der Pod im Status _Wird ausgeführt_ befindet, wird der mit der vorherigen Anwendung und den Komponenten verknüpfte Pod beendet.

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Ändern Sie die Datei `bobs-books-comp.yaml`.
*   Wenden Sie die Änderungen mit `kubectl` an.

### Voraussetzungen

Sie benötigen einen Texteditor, in dem Sie die Befehle und URLs einfügen und entsprechend Ihrer Umgebung ändern können. Anschließend können Sie die geänderten Befehle zum Ausführen in die _Cloud Shell_ kopieren und einfügen.

## Aufgabe 1: Ändern der Datei bobs-books-comp.yaml

1.  Wir haben eine Anwendungskonfigurationsdatei, _bobs-books-comp.yaml_. In Lab 2 haben wir die Anwendung yaml Dateien heruntergeladen. Um zu dem Home-Verzeichnis zu wechseln, das die yaml-Datei enthält, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>cd ~</copy>
        
2.  In diesem Verzeichnis haben wir die Konfigurationsdatei für die bobs-books Anwendung. Im Rahmen von Lab 5 haben wir bobbys-helidon-stock-application modifiziert und ein neues Docker-Image für diese Komponente erstellt. In Übung 6 haben wir dieses Docker-Image in das Oracle Cloud Container Registry-Repository übertragen. In dieser Übung ändern wir die Datei _bobs-books-comp.yaml_ so, dass das neue aktualisierte Docker-Image aus dem Oracle Cloud Container Registry-Repository übernommen wird. Um die Datei _bobs-books-comp.yaml_ zu ändern, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>vi bobs-books-comp.yaml</copy>
        
    
    ![Datei öffnen](images/openfile.png " ")
    
3.  Im Rahmen von Übung 5 haben Sie den vollständigen Namen Ihres Docker-Images gespeichert. Sie müssen die folgende Zeile kopieren und in Ihren Texteditor einfügen. Dann müssen Sie `docker image full name` durch Ihren Docker-Imagenamen ersetzen. Kopieren Sie dann die geänderte Zeile, und drücken Sie _i_, um den Text in die Datei `*bobs-books-comp.yaml*` einzufügen. Fügen Sie die Ausgabe in Zeile 148 ein (stellen Sie sicher, dass Sie die Einrückung beibehalten), und kommentieren Sie die auslaufende Zeilennummer 147 mit _#_ aus, wie im folgenden Bild gezeigt. Drücken Sie dann _Esc_, und geben Sie _:wq_ ein, um die Datei zu speichern.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Linie einfügen](images/insert-line.png " ")
    

## Aufgabe 2: Änderungen mit `kubectl` anwenden

1.  Um die Änderungen anzuwenden, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein. Wenn Sie die Änderung anwenden, wird ein neuer Pod für die Bereitstellung von Anforderungen für eine neue Komponente initialisiert, während der mit der alten Komponente verknüpfte Pod weiterhin Anforderungen verarbeitet. Später, nachdem der neue Pod den Status _Wird ausgeführt_ erreicht hat, wird der alte Pod _Beendet_. Schließlich hat nur der neue Pod den Status _Wird ausgeführt_.
    
        <copy>kubectl apply -f bobs-books-comp.yaml -n bobs-books</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh unchanged
        component.core.oam.dev/robert-helidon unchanged
        component.core.oam.dev/bobby-coh unchanged
        component.core.oam.dev/bobby-helidon configured
        component.core.oam.dev/bobby-wls unchanged
        component.core.oam.dev/bobs-mysql-configmap unchanged
        component.core.oam.dev/bobs-mysql-service unchanged
        component.core.oam.dev/bobs-mysql-deployment unchanged
        component.core.oam.dev/bobs-orders-configmap unchanged
        component.core.oam.dev/bobs-orders-wls unchanged
        $
        
    
    Sie können die Ausgabe anzeigen. Nur _component.core.oam.dev/bobby-helidon_ wird konfiguriert, und andere Komponenten werden nicht geändert.
    
2.  Um anzuzeigen, wie der neue Pod initialisiert wird und der alte Pod den Status _Wird beendet_ erhält, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        $ kubectl get pods -n bobs-books
        NAME                                         READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                           2/2    Running      0         130m
        bobbys-front-end-adminserver                 4/4    Running      0         127m
        bobbys-front-end-managed-server1             4/4    Running      0         126m
        bobbys-helidon-stock-application-64fb55-zzp  0/2    PodInitializing  0     10s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Running      0         130m
        bobs-bookstore-adminserver                   4/4    Running      0         127m
        bobs-bookstore-managed-server1               4/4    Running      0         126m
        mysql-65d864bf8c-xf64p                       2/2    Running      0         130m
        robert-helidon-bfdfb58b8-58qfs               2/2    Running      0         130m
        robert-helidon-bfdfb58b8-lkw8m               2/2    Running      0         130m
        roberts-coherence-0                          2/2    Running      0         130m
        roberts-coherence-1                          2/2    Running      0         130m
        bobbys-helidon-stock-application-64fb55-zzp  1/2    Running      0         28s
        bobbys-helidon-stock-application-64fb55-zzp  2/2    Running      0         34s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        $
        
    
    Wenn Sie sehen, dass sich alle Pods im Status _Wird ausgeführt_ befinden, drücken Sie _STRG + C_, um diesen Befehl abzubrechen.
    
    Lassen Sie die _Cloud Shell_ geöffnet, da sie auch für unsere letzte Übung benötigt wird.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
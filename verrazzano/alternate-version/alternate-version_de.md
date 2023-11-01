# ALTERNATIVE VERSION mit vereinfachter Lösung

## Einführung

In dieser Übung verwenden wir das modifizierte bobbys-helidon-stock-application Bild. Dieses Image wird in unserem öffentlichen Repository in der Oracle Cloud Container Registry gespeichert. Wir verwenden auch die modifizierte bobs-books-comp-mod.yaml-Datei, die auf das modifizierte bobbys-helidon-stock-application-Bild verweist.

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Laden Sie die geänderte Datei bobs-books-comp-mod.yaml herunter.
*   Änderungen mit kubectl anwenden

### Voraussetzungen

*   Führen Sie Übung 1 aus, in der ein OKE-Cluster auf Oracle Cloud Infrastructure erstellt wird.
*   Führen Sie Übung 2 aus, in der Verrazzano auf dem Kubernetes-Cluster installiert wird.
*   Führen Sie Übung 3 aus, die Bobs-Books-Anwendung bereitstellt.
*   Sie benötigen einen Texteditor, in dem Sie die Befehle und URLs einfügen und entsprechend Ihrer Umgebung ändern können. Anschließend können Sie die geänderten Befehle zum Ausführen in die _Cloud Shell_ kopieren und einfügen.

## Aufgabe 1: Laden Sie die geänderte Datei bobs-books-comp-mod.yaml herunter

1.  Führen Sie den folgenden Befehl aus, um die geänderte Datei bobs-books-comp-mod.yaml herunterzuladen.
    
        <copy>curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/alternate-version/bobs-books-comp-mod.yaml >~/bobs-books-comp-mod.yaml</copy>
        
    
    ![Datei herunterladen](images/downloadfile.png " ")
    

## Aufgabe 2: Änderungen mit kubectl anwenden

1.  Um die Änderungen anzuwenden, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein. Wenn Sie die Änderung anwenden, wird ein neuer Pod für die Bereitstellung von Anforderungen für eine neue Komponente initialisiert, während der mit der alten Komponente verknüpfte Pod weiterhin Anforderungen verarbeitet. Später, nachdem der neue Pod den Status _Wird ausgeführt_ erreicht hat, wird der alte Pod _Beendet_. Schließlich hat nur der neue Pod den Status _Wird ausgeführt_.
    
        <copy>kubectl apply -f ~/bobs-books-comp-mod.yaml -n bobs-books</copy>
        
    
    ![Änderungen anwenden](images/applychanges.png " ")
    
    Sie können die Ausgabe anzeigen. Nur _component.core.oam.dev/bobby-helidon_ wird konfiguriert, und andere Komponenten werden nicht geändert.
    
2.  Um anzuzeigen, wie der neue Pod initialisiert wird und der alte Pod den Status _Wird beendet_ erhält, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    Eine ähnliche Ausgabe wie im folgenden Beispiel wird angezeigt:
    
        vera_zano@cloudshell:~ (us-ashburn-1)$ kubectl get pods -n bobs-books
        NAME                                               READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                                 2/2    Running  0         130m
        bobbys-front-end-adminserver                       4/4    Running  0         127m
        bobbys-front-end-managed-server1                   4/4    Running  0         126m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  0/2    PodInitializing  0         10s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Running  0         130m
        bobs-bookstore-adminserver                         4/4    Running  0         127m
        bobs-bookstore-managed-server1                     4/4    Running  0         126m
        mysql-65d864bf8c-xf64p                             2/2    Running  0         130m
        robert-helidon-bfdfb58b8-58qfs                     2/2    Running  0         130m
        robert-helidon-bfdfb58b8-lkw8m                     2/2    Running  0         130m
        roberts-coherence-0                                2/2    Running  0         130m
        roberts-coherence-1                                2/2    Running  0         130m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  1/2    Running  0         28s
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  2/2    Running  0         34s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        vera_zano@cloudshell:~ (us-ashburn-1)$
        
    
    Wenn Sie sehen, dass sich alle Pods im Status _Wird ausgeführt_ befinden, drücken Sie _STRG + C_, um diesen Befehl abzubrechen.
    

Lassen Sie die _Cloud Shell_ geöffnet, da sie auch für unsere letzte Übung benötigt wird.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Februar 2023
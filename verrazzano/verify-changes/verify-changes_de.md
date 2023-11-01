# Änderungen durch Anwendungszugriff und kuratierten Stack überprüfen

## Einführung

In Übung 7 haben wir Änderungen in der Anwendung bobbys-helidon-stock-application angewendet, und der Pod dafür befindet sich im Status _Wird ausgeführt_. In dieser Übung prüfen wir die Änderungen in der Anwendung, der Verrazzano-Konsole und der Grafana-Konsole.

Geschätzte Zeit: 05 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Prüfen Sie die Änderungen in der Bobby's Books-Anwendung.
*   Prüfen Sie die Änderungen in der Grafana-Konsole.

### Voraussetzungen

*   Sie benötigen einen Texteditor, in dem Sie die Befehle und URLs einfügen und entsprechend Ihrer Umgebung ändern können. Anschließend können Sie die geänderten Befehle zum Ausführen in die _Cloud Shell_ kopieren und einfügen.

## Aufgabe 1: Änderungen in Bobby's Books-Anwendung prüfen

1.  Öffnen Sie die Registerkarte Bücher von Bobby und wählen Sie Aktualisieren. Wenn Sie diese Registerkarte geschlossen haben, kopieren Sie den folgenden Befehl, und fügen Sie ihn in den Texteditor ein. Ersetzen Sie XX.XX.XX.XX durch EXTERNAL\_IP für die Anwendung. Sie werden feststellen, dass der Buchname in Großbuchstaben geschrieben ist.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![Bobby's Book](images/bobbysbooks.png " ")
    

## Aufgabe 2: Änderungen in der Grafana-Konsole prüfen

1.  Kehren Sie zur Verrazzano-Homepage zurück.
    
    ![Verrazzano Startseite](images/verrazzao-home.png " ")
    
2.  Wählen Sie den Link für Grafana aus, um die _Grafana-Konsole_ zu öffnen.
    
    ![Grafana-Link](images/grafana-link.png " ")
    
3.  Wählen Sie _Home_, geben Sie _Helidon_ ein, und wählen Sie _Helidon-Überwachungs-Dashboard_ aus.
    
    ![Helidon durchsuchen](images/search-helidon.png " ")
    
4.  Wählen Sie in ServiceID die Option _bobs-books\_default\_bobs-books\_bobby-helidon_ aus, und wählen Sie in der Instanz die neu erstellte Instanz aus. In diesem Fall erhalten Sie Informationen zur geänderten _bobby-helidon-stock-application_.
    
    ![Neue Komponente](images/new-component.png " ")
    
    Glückwunsch! Sie haben die Übungen erfolgreich abgeschlossen.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
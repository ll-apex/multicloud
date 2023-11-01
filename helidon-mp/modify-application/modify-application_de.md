# Helidon MP-Anwendung im Codeeditor ändern

## Einführung

In dieser Übung fügen Sie der JAVA-Klasse im Codeeditor einen benutzerdefinierten Endpunkt hinzu.

Geschätzte Zeit: 10 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Helidon MP-Anwendung im Codeeditor ändern](videohub:1_sv1iug41)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Benutzerdefinierten Endpunkt in der Java-Klasse hinzufügen
*   Geänderte Anwendung erstellen und ausführen

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.

## Aufgabe 1: Benutzerdefinierten Endpunkt hinzufügen

1.  Klicken Sie im Codeeditor auf die Datei _GreetResource.java_, um sie zu öffnen. ![Datei öffnen](images/open-file.png)
    
2.  Wie Sie vielleicht im Code sehen, ist er vollständig MicroProfile-basiert. Dies bedeutet, dass die gesamte Funktionalität mit POJOs und Anmerkungen erreicht wird. Diese Annotationen sind Standard, daher über verschiedene Anbieter hinweg tragbar. Dies bedeutet, dass Sie einfach Code ausführen können, der auf anderen Plattformen ausgeführt wird, die dieselbe Version von MicroProfile unterstützen. Weitere Informationen finden Sie [hier](https://microprofile.io/).
    
3.  Erstellen Sie einen neuen Endpunkt, der Titel zufällig aus einer Gruppe von fünf Titeln bereitstellt. Um diesen Endpunkt zu erstellen, kopieren Sie den folgenden Code in Zeile 80, wie unten gezeigt.
    
        <copy>@Path("/title")
        @GET
        @Produces(MediaType.APPLICATION_JSON)
        public String getRandomTitle() {
            return new String[] {"Mr. ", "Mrs.", "Miss", "Dr.", "Dr. sc."} [(int)(Math.random()*5)];
        }</copy>
        
    
    ![Code hinzufügen](images/add-code.png)
    
4.  Um den Inhalt der Datei zu speichern, klicken Sie im Codeeditor auf _Datei_ -> _Speichern_.
    
    > AutoSave ist im Codeeditor standardmäßig aktiviert.
    

## Aufgabe 2: Anwendung ausführen

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um die Anwendung auszuführen.
    
        <copy>mvn clean package
        java -jar target/myproject.jar</copy>
        
2.  Öffnen Sie einen neuen Terminal/eine neue Konsole, und führen Sie die folgenden Befehle aus, um die Anwendung zu prüfen.
    
        <copy>curl -X GET http://localhost:8080/greet/title</copy>
        Mr.
        
    
    > Es stellt zufällig den Titel von 5 Optionen bereit, Sie können diesen Befehl mehrmals ausführen.
    
3.  _Stoppen Sie die Anwendung **myproject**, indem Sie `Ctrl + C` IN das Terminal eingeben, IN dem der Befehl "java -jar target/myproject.jar" ausgeführt wird_. ES IST SEHR WICHTIG, SONSTIG WERDEN SIE IM LABELRECHT GESICHTEN.
    

## Danksagungen

*   **Autor** - Dmitry Aleksandrov
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, April 2023
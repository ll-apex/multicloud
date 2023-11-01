# Logs und Metrik-Explorer untersuchen

## Einführung

In dieser Übung prüfen Sie das erfolgreiche Deployment der Helidon-Anwendung. Außerdem untersuchen Sie den Log- und Metrik-Explorer-Service.

Voraussichtliche Zeit: 5 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Logs und Metrik-Explorer kennenlernen](videohub:1_7a0qaaif)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Prüfen Sie das erfolgreiche Deployment der Helidon-Anwendung.
*   Entdecken Sie OCI Metric Explorer
*   Entdecken Sie den OCI Logging-Service
*   Wirksamkeit und Bereitschaft der Helidon-Anwendung validieren

### Voraussetzungen

*   Free Tier-(Testversion), kostenpflichtiger oder LiveLabs Cloud-Account für Oracle
*   Vertrautheit mit der OCI-Konsole

## Aufgabe 1: Auf die Helidon-Anwendung zugreifen

In dieser Aufgabe greifen wir mit curl auf die Anwendung zu, um GET- und PUT-HTTP-Anforderungen auszuführen.

1.  Gehen Sie zurück zum **Codeeditor**, kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um den Deployment-Knoten **`PUBLIC_IP`** als Umgebungsvariable einzurichten.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um eine **GET-Anforderung** zu erstellen. Sie erhalten eine ähnliche Ausgabe wie unten gezeigt.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  Hallo an einen Namen, also an **Joe**. Sie erhalten eine ähnliche Ausgabe wie unten gezeigt.
    
        <copy>curl http://$PUBLIC_IP:8080/greet/Joe</copy>
        {"message":"Hello Joe!","date":[2023,5,23]}
        
4.  Ersetzen Sie Hello durch ein anderes Begrüßungswort, d.h. **Hola**. Sie erhalten eine ähnliche Ausgabe wie unten gezeigt.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,23]}
        

## Aufgabe 2: OCI Metrics Explorer erkunden

In dieser Aufgabe validieren Sie, ob die **Helidon-Metriken** an den **OCI Monitoring Service** übertragen werden.

1.  Klicken Sie in der Cloud-Konsole unter **Monitoring** auf _Hamburger-Menü_ -> _Observability & Management_ -> _Metrics Explorer_, wie unten gezeigt. ![Metrik-Explorer](images/metrics-explorer.png)
    
2.  Scrollen Sie nach unten, um zum Abschnitt **Abfrage** zu gehen, wählen Sie das richtige Compartment aus, und wählen Sie dann **`helidon_metrics`** als Metrik-Namespace und **`request.count_counter`** als Metriknamen aus, um die Abfrage zu erstellen. Klicken Sie dann wie unten gezeigt auf _Diagramm aktualisieren_. ![Abfrage erstellen](images/create-query.png)
    
3.  Scrollen Sie nach oben, und Sie sehen eine ähnliche Ausgabe wie unten gezeigt. Dadurch erhalten Sie Daten im Diagrammformat für den Anforderungszähler. ![Abfrageeditor](images/query-editor.png)
    
4.  Um die Daten in der Datentabelle anzuzeigen, schalten Sie die Schaltfläche für **Datentabelle anzeigen** ein, wie unten gezeigt. Wenn Sie die Schaltfläche umschalten, wird _Datentabelle anzeigen_ durch **Diagramm anzeigen** ersetzt. ![Datentabelle](images/data-table.png)
    

## Aufgabe 3: OCI Logging-Service erkunden

In dieser Aufgabe validieren wir die Integration des **OCI-Logging-SDK**, um Nachrichten an den Logging-Service zu übertragen, indem wir das Log **app-log-helidon-ocw-hol** in der Loggruppe **app-log-group-helidon-ocw-hol** untersuchen.

1.  Klicken Sie in der Cloud-Konsole auf _Hamburger-Menü_ -> _Observability and Management_ -> _Logs_. ![Logmenü](images/logs-menu.png)
    
2.  Wählen Sie das richtige Compartment aus, und wählen Sie _`app-log-group-helidon-ocw-hol`_ als **Loggruppe** aus. Klicken Sie dann wie unten gezeigt auf **app-log-helidon-ocw-hol**. ![Compartment auswählen](images/select-compartment.png)
    
3.  Wählen Sie unter **Nach Zeit filtern** in der Dropdown-Liste die Option **Heute** aus, und die Anwendungslogs werden angezeigt. ![Logs anzeigen](images/view-logs.png)
    

## Aufgabe 4: Health Check der Helidon-Anwendung zur Validierung von Lebendigkeit und Bereitschaft

Die Helidon-Anwendung **oci-mp** fügt eine **Health Check**\-Funktion hinzu, um **Leistung** und/oder **Bereitschaft** zu validieren. Sie können die Klassendateien _GreetLivenessCheck_ und _GreetReadinessCheck_ im Projekt prüfen, um zu sehen, wie sie ausgeführt werden. Dies ist besonders nützlich, wenn Sie die App als Microservice in einer Orchestratorumgebung wie Kubernetes ausführen, um festzustellen, ob der Microservice neu gestartet werden muss, wenn er nicht fehlerfrei ist. Spezifisch für diese Übung wird die Bereitschaftsprüfung in der Deployment-Pipeline-Spezifikation DevOps verwendet, nachdem die Anwendung gestartet wurde _um zu bestimmen, ob die Helidon-Anwendung erfolgreich gestartet wurde_. Sehen Sie sich den Code unter _Zeile 79_ in **deployment\_spec.yaml** an, um ihn in Aktion zu sehen.

1.  Gehen Sie zurück zum **Codeeditor**, kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um den Deployment-Knoten **`PUBLIC_IP`** als Umgebungsvariable einzurichten.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die **Lebenskraft** zu prüfen. Sie erhalten eine ähnliche Ausgabe wie unten gezeigt.
    
        <copy>curl http://$PUBLIC_IP:8080/health/live</copy>
        {"status":"UP","checks":[{"name":"CustomLivenessCheck","status":"UP","data":{"time":1684391639448}}]}
        
3.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die **Bereitschaft** zu prüfen. Sie erhalten eine ähnliche Ausgabe wie unten gezeigt.
    
        <copy>curl http://$PUBLIC_IP:8080/health/ready</copy>
        {"status":"UP","checks":[{"name":"CustomReadinessCheck","status":"UP","data":{"time":1684391438298}}]}
        

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Weitere Informationen

*   [OCI-Anwendung mit Helidon](https://medium.com/helidon/oci-application-with-helidon-caa78cacaee5)
*   [Helidon MP-Metriken](https://helidon.io/docs/v3/#/mp/metrics/metrics)
*   [Dokumentation zu Helidon MP Metrics](https://helidon.io/docs/v3/#/mp/guides/metrics)
*   [Helidon MP-Zustand](https://helidon.io/docs/v3/#/mp/health)
*   [Helidon MP-Health Check-Anleitung](https://helidon.io/docs/v3/#/mp/guides/health)

## Danksagungen

*   **Autor** - Keith Lustria
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
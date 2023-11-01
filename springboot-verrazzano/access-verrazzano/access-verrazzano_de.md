# Mit Verrazzano auf die Anwendung zugreifen und sie überwachen

## Einführung

Sie haben die Springboot-Anwendung bereitgestellt. In dieser Übung greifen wir mit mehreren von Verrazzano bereitgestellten Managementtools auf die Anwendung zu und prüfen sie.

Geschätzte Zeit: 15 Minuten

**Grafana-Sprache**

Grafana ist eine Open-Source-Lösung für die Ausführung von Datenanalysen, das Abrufen von Metriken, die Sinn für die riesige Datenmenge machen, und das Überwachen unserer Apps mit Hilfe von schönen anpassbaren Dashboards. Das Tool hilft, Daten über einen bestimmten Zeitraum zu studieren, zu analysieren und zu überwachen, technisch als Zeitreihenanalyse bezeichnet. Nützlich, um das Nutzerverhalten, das Anwendungsverhalten, die Häufigkeit von Fehlern, die in der Produktion oder in einer Pre-Prod-Umgebung auftauchen, die Art von Fehlern, die auftauchen, und die kontextbezogenen Szenarien durch die Bereitstellung relativer Daten zu verfolgen.

[https://grafana.com/grafana/](https://grafana.com/grafana/)

**OpenSearch Dashboards**

OpenSearch Dashboards ist ein Visualisierungs-Dashboard für den in einem OpenSearch-Cluster indexierten Inhalt. Verrazzano erstellt ein Dashboard-Deployment von OpenSearch, um eine Benutzeroberfläche zum Abfragen und Visualisieren der in OpenSearch erfassten Logdaten bereitzustellen.

Um auf die Dashboard-Konsole OpenSearch zuzugreifen, lesen Sie [Auf Verrazzano zugreifen](https://verrazzano.io/latest/docs/access/).

Um die Datensätze eines OpenSearch-Index über Dashboards OpenSearch anzuzeigen, erstellen Sie ein Indexmuster, um nach Datensätzen unter dem gewünschten Index zu filtern.

In Aufgabe 3 werden die Dashboards OpenSearch untersucht.

[https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)

**Prometheus**

Prometheus ist ein Überwachungs- und Warn-Toolkit. Prometheus sammelt und speichert seine Metriken als Zeitreihendaten, d.h. Metrikinformationen werden mit dem Zeitstempel gespeichert, zu dem sie aufgezeichnet wurden, sowie optionalen Schlüssel/Wert-Paaren, die als Labels bezeichnet werden.

[https://prometheus.io/](https://prometheus.io/)

**Rancher**

Rancher ist eine Plattform, mit der Verrazzano Container auf mehreren Kubernetes-Clustern in der Produktion ausführen kann. Es ermöglicht eine Hybridlösung, d.h. eine zentrale Verwaltung von On-Premise-Clustern und gehosteten Cloud-Services.

[https://rancher.com/](https://rancher.com/)

**Kiali**

Kiali ist die Benutzeroberfläche zu Istio, und es ist super einfach zu bedienen. Von der Verrazzano-Konsole aus können Sie direkt auf Kiali zugreifen. Von dort aus können Sie den Verkehrsfluss und alle Hot Spots oder Problembereiche sehen. Sie können dann einen Drilldown durchführen, um die Details anzuzeigen. Kiali verwendet Messdaten von Envoy, die in Prometheus gespeichert sind, um das grafische Modell zu erstellen

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Entdecken Sie die Rancher-Konsole.
*   Entdecken Sie die Prometheus-Konsole.
*   Entdecken Sie die Grafana-Konsole.
*   Untersuchen Sie die Dashboards OpenSearch.
*   Entdecken Sie die Kiali-Konsole.
*   Entdecken Sie die Keycloak Konsole.

### Voraussetzungen

*   Kubernetes-(OKE-)Cluster, das auf Oracle Cloud Infrastructure ausgeführt wird.
*   Verrazzano auf einem Kubernetes-(OKE-)Cluster installiert.
*   Springboot-Beispielanwendung bereitgestellt.

## Aufgabe 1: Rancher-Konsole erkunden

Verrazzano installiert mehrere Konsolen. Die Endpunkte für eine Installation werden im Feld `Status` der installierten benutzerdefinierten Verrazzano-Ressource gespeichert.

1.  Sie können die Endpunkte für diese Konsolen mit dem folgenden Befehl abrufen:
    
        <copy>kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
        $ kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .
        {
        "consoleUrl": "https://verrazzano.default.xx.xx.xx.xx.nip.io",
        "grafanaUrl": "https://grafana.vmi.system.default.xx.xx.xx.xx.nip.io",
        "keyCloakUrl": "https://keycloak.default.xx.xx.xx.xx.nip.io",
        "kialiUrl": "https://kiali.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchDashboardsUrl": "https://osd.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchUrl": "https://opensearch.vmi.system.default.xx.xx.xx.xx.nip.io",
        "prometheusUrl": "https://prometheus.vmi.system.default.xx.xx.xx.xx.nip.io",
        "rancherUrl": "https://rancher.default.xx.xx.xx.xx.nip.io"
        }
        $
        
2.  Verwenden Sie `https://rancher.default.YOUR_UNIQUE_IP.nip.io`, um die Rancher-Konsole zu öffnen.
    
3.  Das Verrazzano-_dev_\-Profil verwendet selbstsignierte Zertifikate. Sie müssen daher auf **Erweitert** klicken, um das Risiko zu akzeptieren und die Warnung zu überspringen.
    
    ![Erweitert](images/rancher-advanced.png)
    
4.  Klicken Sie auf **Fortfahren Sie zum Rancher-Standardwert XX.XX.XX.nip.io(unsafe)**. Wenn Sie diese Option zum Fortfahren nicht erhalten, geben Sie einfach _thisisunsafe_ ohne Platz irgendwo in diesem Chrome-Browserfenster ein. Wenn Sie das Chrome-Browserfenster eingeben, wird es nicht angezeigt. Sobald Sie _thisisunsafe_ eingegeben haben, wird die nächste Seite sofort angezeigt. Weitere Informationen finden Sie [hier](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Fortfahren](images/rancher-proceed.png)
    
5.  Klicken Sie auf _Mit Keycloak anmelden_. ![Rancher-Anmeldung](images/keycloak-login.png)
    
6.  Da es zur Authentifizierung zur Keycloak-Konsolen-URL umleitet, klicken Sie auf **Erweitert**.
    
    ![Keycloak-Authentifizierung](images/keycloak-advanced.png)
    
7.  Klicken Sie auf **Fortfahren Sie zum Keycloak-Standard XX.XX.XX.nip.io(unsafe)**. Wenn Sie diese Option zum Fortfahren nicht erhalten, geben Sie einfach _thisisunsafe_ ohne Platz irgendwo in diesem Chrome-Browserfenster ein. Wenn Sie das Chrome-Browserfenster eingeben, wird es nicht angezeigt. Sobald Sie _thisisunsafe_ eingegeben haben, wird die nächste Seite sofort angezeigt. Weitere Informationen finden Sie [hier](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Fortfahren](images/keycloak-proceed.png)
    
8.  Jetzt benötigen wir den Benutzernamen und das Passwort für die Verrazzano-Konsole. _Benutzername_ ist _verrazzano_. Um das Kennwort zu ermitteln, gehen Sie zurück zu _Cloud Shell_, und fügen Sie den folgenden Befehl ein, um das Kennwort für die _Rancher-Konsole_ zu ermitteln.
    
        <copy>kubectl get secret --namespace verrazzano-system verrazzano -o jsonpath={.data.password} | base64 --decode; echo</copy>
        
9.  Kopieren Sie das Kennwort, und kehren Sie zum Browser zurück, in dem die _Rancher-Konsole_ geöffnet ist. Fügen Sie das Kennwort in das Feld _Kennwort_ ein, und geben Sie _verrazzano_ als _Benutzername_ ein. Klicken Sie dann auf **Anmelden**.
    
    ![SignIn](images/verrazzano-sign-in.png)
    
10.  Auf der Homepage der Rancherkonsole können Sie die Verrazzano Links anzeigen. Sie können auf eine dieser Konsolen über den Rancher console.Click _Hamburger-Menü_ -> _Verrazzano_ zugreifen.
    
    ![Rancher Home](images/rancher-home.png)
    
11.  Klicken Sie auf _Anwendungen_. In diesem Abschnitt werden alle Anwendungen mit ihrem Namespace angezeigt und von Verrazzano verwaltet. Klicken Sie auf die Anwendung _springboot-appconf_ im Namespace _springboot_. ![Sprinngboot-Anwendung](images/springboot-application.png)
    
12.  Sie können die Pods anzeigen, die mit der Anwendung verknüpft sind. Der Podname enthält eine automatisch generierte eindeutige Zeichenfolge zur Identifizierung dieses bestimmten Replikats. Um die Logs der Pods anzuzeigen, klicken Sie auf _Drei Punkte_ -> _Logs anzeigen_. ![Springboot-Komponenten](images/view-pod-logs.png)
    
13.  Sie können die Anwendungslogs für die Anwendung anzeigen. Wenn das Anwendungslog nicht angezeigt wird, klicken Sie auf die **Einstellungen** (blaue Schaltfläche mit dem Zahnradsymbol), und ändern Sie den Zeitfilter, um alle Logeinträge ab dem Containerstart anzuzeigen. Um die mit der Anwendung verknüpfte Komponente anzuzeigen, klicken Sie auf _Komponenten_. ![Anwendungslogs](images/view-pod-log.png)
    
14.  Diese Anwendung hat nur eine component.To-Ansicht der zugehörigen Ressourcen. Klicken Sie auf _Zugehörige Ressourcen_. ![Springboot-Ressource](images/springboot-resource.png) ![Springboot-Ressourcen](images/springboot-resources.png)
    
15.  Klicken Sie auf _Hamburgar-Menü_ -> _lokal_, um den _Cluster Explorer_ zu öffnen. Mit dem _Cluster Explorer_ können Sie alle benutzerdefinierten Ressourcen und CRDs in einem Kubernetes-Cluster über die Rancher-UI anzeigen und bearbeiten. ![Verrazzano-Cluster](images/verrazzano-cluster.png)
    
16.  Das Dashboard bietet einen Überblick über das Cluster und die bereitgestellten Anwendungen. Die Anzahl der Ressourcen gehört zu den _Benutzernamensräumen_, bei denen es sich praktisch um fast alle Ressourcen einschließlich des Systems handelt. Sie können oben im Dashboard nach Namespace filtern. Dies ist jedoch jetzt nicht erforderlich. Klicken Sie im linken Menü auf das Element **Knoten**, um einen Überblick über die aktuelle Last der Knoten zu erhalten.
    
    ![Cluster-Explorer](images/cluster-dashboard.png)
    
17.  Das gesamte Deployment hat keine Auswirkungen auf das OKE-Cluster. Klicken Sie jetzt im linken Menü auf das Element **Workload** -> **Deployment**, um die bereitgestellte Anwendung zu prüfen.
    
    ![Knoten](images/cluster-nodes.png)
    
18.  Es werden mehrere Deployments angezeigt.
    
    ![Deployments](images/deployment.png)
    

## Aufgabe 2: Grafana-Konsole kennenlernen

1.  Klicken Sie auf _Hamburgar menu_ -> _Home_. ![Startseite](images/ranchar-menu.png)
    
2.  Auf der Homepage wird der Link zum Öffnen der _Grafana-Konsole_ angezeigt. Klicken Sie wie gezeigt auf den Link für die **Grafana-Konsole**:
    
    ![Grafana-Homepage](images/grafana-link.png)
    
3.  Klicken Sie auf **Erweitert**.
    
    ![Erweitert](images/grafana-advanced.png)
    
4.  Klicken Sie auf **Weiter zu grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)**. Wenn Sie diese Option zum Fortfahren nicht erhalten, geben Sie einfach _thisisunsafe_ ohne Platz irgendwo in diesem Chrome-Browserfenster ein. Wenn Sie das Chrome-Browserfenster eingeben, wird es nicht angezeigt. Sobald Sie _thisisunsafe_ eingegeben haben, wird die nächste Seite sofort angezeigt. Weitere Informationen finden Sie [hier](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Fortfahren](images/grafana-proceed.png)
    
5.  Klicken Sie im Grafana-Home auf _Home_, und wählen Sie _JVM(Micrometer)_ wie dargestellt aus. ![Grafana-Home-Link](images/grafana-home-link.png) ![JVM-Kennzahlen](images/jvm-metrics.png)
    
6.  Sie können die Metriken wie Anwendungsbetriebszeit, belegter Heap und JVM-Speicherdaten wie unten dargestellt anzeigen. ![JVM 1](images/jvm-1.png) ![JVM 2](images/jvm-2.png)
    
7.  Klicken Sie einfach auf "JVM", und öffnen Sie die Dashboards _Anwendungsstatus_ wie unten gezeigt. ![Zuordnungsstatus](images/application-status.png)
    
8.  Sie können den Anwendungsstatus in Bezug auf "Verwendete CPU", "Verwendeter Speicher", "Verwendete Festplatte" und "HTTP-Anforderung/Sekunde" und andere Informationen wie unten dargestellt anzeigen. ![Anwendungsdaten](images/application-data.png)
    

## Aufgabe 3: OpenSearch-Dashboards untersuchen

1.  Kehren Sie zur Verrazzano-Homepage zurück, und klicken Sie auf die Konsole **OpenSearch Dashboards**.
    
    ![Kibana-Link](images/opensearch-link.png)
    
2.  Klicken Sie bei Bedarf auf _Weiter zu ... Standard XX.XX.XX.nip.io(unsicher)_. Bei der ersten Anzeige von _OpenSearch-Dashboards_ wird die Willkommensseite angezeigt. Es bietet integrierte Beispieldaten, um OpenSearch auszuprobieren. Sie können jedoch die Option **Auf eigene Faust explorieren** auswählen, da Verrazzano die erforderliche Konfiguration abgeschlossen hat und die Anwendungsdaten bereits verfügbar sind.
    
    ![Kibana-Willkommensseite](images/opensearch-proceed.png)
    
3.  Klicken Sie auf der OpenSearch-Homepage auf **Home** -> **Discover**.
    
    ![Klicken auf Kibana-Dashboard](images/discover-1.png)
    
4.  Rufen Sie die folgende HTTP-Anforderung in der Cloud Shell für Ihren Endpunkt auf. Sie können Anforderungen mehrmals ausführen.
    
        <copy>curl -k https://$(kubectl get gateways.networking.istio.io springboot-springboot-appconf-gw -n springboot -o jsonpath={.spec.servers[0].hosts[0]})/facts; echo</copy>
        
    
    > Was Sie im Ergebnis sehen, können Sie jedes Wort auswählen und im nächsten Schritt dasselbe suchen. In unserem Fall werden wir das Wort _Brücke_ durchsuchen, aber Sie können jedes Wort aus dem Ergebnis suchen.
    
5.  Wählen Sie den Namespace _`verrazzano-application*`_ aus (siehe Abbildung), und suchen Sie dann in der Springboot-Anwendung nach den Logs: `bridge` im Filtertextfeld. Drücken Sie die **Eingabetaste** oder klicken Sie auf **Aktualisieren**. Sie sollten mindestens ein Ergebnis erhalten. ![Anwendungsstatus](images/verrazzano-application.png) ![Problemergebnis](images/log-result.png)
    

## Aufgabe 4: Prometheus-Konsole erkunden

1.  Kehren Sie zur Verrazzano-Homepage zurück, und klicken Sie auf die Konsole **Prometheus**.
    
    ![Prometheus-Link](images/prometheus-link.png)
    
2.  Klicken Sie auf **Fortfahren zum ... Standard XX.XX.XX.XX.nip.io(unsicher)**, wenn Sie dazu aufgefordert werden.
    
3.  Geben Sie auf der Prometheus-Dashboard-Seite _http_ in das Suchfeld ein, und klicken Sie auf die Metrik _`http_server_requests_seconds_count`_ und dann auf _Ausführen_.
    
    ![Prometheus ausführen](images/prometheus-query.png)
    
4.  Klicken Sie auf **Ausführen**, und prüfen Sie das Ergebnis unten. Der aktuelle Wert Ihrer Metrik sollte angezeigt werden, d.h. die Gesamtdauer in Sekunden für die Bereitstellung einer HTTP-Anforderung. Sie können auch zur _Diagrammansicht_ anstatt zum _Konsolenmodus_ wechseln.
    
    ![Prometheus-Wert](images/execute-query.png)
    
    > Sie können Ihrem Dashboard auch eine weitere Metrik hinzufügen. Ermitteln Sie die verfügbaren Standardmetriken in der Liste.
    

## Aufgabe 5: Kiali-Konsole erkunden

1.  Kehren Sie zur Verrazzano-Homepage zurück, und klicken Sie auf die Konsole **Kiali**.
    
    ![Rancher-Link](images/kiali-link.png)
    
2.  Klicken Sie auf **Fortfahren zum ... Standard XX.XX.XX.XX.nip.io(unsicher)**, wenn Sie dazu aufgefordert werden.
    
3.  Klicken Sie auf der linken Seite auf Diagramm.
    
    ![Kiali-Dashboard](images/kiali-dashboard.png " ")
    
4.  Aktivieren Sie in der Dropdown-Liste "Namespace" das Kontrollkästchen für _verrazzano-system_, und verschieben Sie den Curser außerhalb der Dropdown-Liste. ![Bobs-Namespace](images/verrazzano-namespace.png " ")
    
5.  Sie können die grafische Ansicht des Namespace _verrazzano-system_ anzeigen. Klicken Sie auf _Legende_, um die Ansicht "Legende" anzuzeigen.
    
    ![Grafische Ansicht](images/graphical-view.png " ")
    
6.  Hier können Sie anzeigen, was jede Ausprägung darstellt. Beispiel: Kreis stellt die _Workloads_ dar.
    
    ![Legendenansicht](images/legend-view.png " ")
    
7.  Klicken Sie auf der linken Seite auf _Anwendungen_, um die bereitgestellten Anwendungen anzuzeigen.
    
    ![Anwendungen](images/applications.png " ")
    

## Aufgabe 6: Entdecken Sie die Keycloak-Konsole

1.  Kehren Sie zur Verrazzano-Homepage zurück, und klicken Sie auf die Konsole **Keycloak**.
    
    ![Schlüsselanhänger](images/keycloak-link.png)
    
2.  Klicken Sie auf **Fortfahren zum ... Standard XX.XX.XX.XX.nip.io(unsicher)**, wenn Sie dazu aufgefordert werden.
    
3.  Klicken Sie auf der Seite "Willkommen bei Keycloak" auf _Administrationskonsole_. ![Schlüsselanhänger](images/keycloak-home.png)
    
4.  Jetzt benötigen wir den Benutzernamen und das Passwort für die Keycloak-Konsole. _Benutzername_ ist _keycloakadmin_. Um das Kennwort zu ermitteln, gehen Sie zurück zu _Cloud Shell_, und fügen Sie den folgenden Befehl ein, um das Kennwort für die _Keycloak-Konsole_ zu ermitteln.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  Kopieren Sie das Passwort, und kehren Sie zum Browser zurück, in dem die _Keycloak-Konsole_ geöffnet ist. Fügen Sie das Kennwort in das Feld _Kennwort_ ein, und geben Sie _keycloakadmin_ als _Benutzername_ ein. Klicken Sie dann auf **Anmelden**.
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  Hier können Sie die Standardkonfiguration von Verrazzano anzeigen. ![Schlüsselanhänger Home](images/keycloak-realms.png)
    

Herzlichen Glückwunsch, Sie haben das Deployment der Springboot-Anwendung in Verrazzano Lab abgeschlossen.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
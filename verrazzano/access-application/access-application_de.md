# Greifen Sie auf die Beispielanwendung "Bobby's Books" zu. Entdecken Sie Verrazzano, Grafana und Kiali Console

## Einführung

In Lab 3 haben wir die Bobby's Book-Anwendung eingesetzt. In dieser Übung greifen wir auf die Anwendung zu und prüfen, ob sie funktioniert. Später werden wir die Konsolen Verrazzano und Grafana erkunden.

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Greifen Sie auf die Bobby's Book-Anwendung zu.
*   Entdecken Sie die Verrazzano-Konsole.
*   Entdecken Sie die Grafana-Konsole.

### Voraussetzungen

\* Führen Sie Übung 1 aus, in der ein OKE-Cluster in Oracle Cloud Infrastructure erstellt wird. \* Führen Sie Übung 1 aus, in der kubectl für den Zugriff auf ein OKE-Cluster in Oracle Cloud Infrastructure konfiguriert wird.

*   Führen Sie Übung 2 aus, in der Verrazzano auf einem Kubernetes-Cluster installiert wird.
*   Führen Sie Lab 3 aus, das die Bobby's Book-Anwendung bereitstellt.
*   Sie benötigen einen Texteditor, in dem Sie die Befehle und URLs einfügen und entsprechend Ihrer Umgebung ändern können. Anschließend können Sie die geänderten Befehle zum Ausführen in die _Cloud Shell_ kopieren und einfügen.
*   Wir empfehlen, Firefox zum Öffnen der URLs von Bobby's Books, Verrazano, Grafana und Kiali Console zu verwenden. Wenn Sie jedoch Chrome verwenden möchten, müssen Sie den undokumentierten "thisisunsafe"-Workaround verwenden, damit Chrom das Zertifikat akzeptiert.

## Aufgabe 1: Zugriff auf die Bobby's Book-Anwendung

1.  Wir benötigen eine `EXTERNAL_IP`\-Adresse, über die wir auf die Bobby's Book-Anwendung zugreifen können. Um die `EXTERNAL_IP`\-Adresse des istio-ingressgateway-Service abzurufen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in die _Cloud Shell_ ein.
    
        <copy> kubectl get service \
        -n "istio-system" "istio-ingressgateway" \
        -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo</copy>
        
    
    Die Ausgabe sollte der folgenden ähneln:
    
           $ kubectl get service \
           > -n "istio-system" "istio-ingressgateway" \
           > -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo
           XX.XX.XX.XX
        
2.  Um die Homepage von Robert's Book Store zu öffnen, kopieren Sie die folgende URL, und ersetzen Sie _XX.XX.XX.XX_ durch Ihre _EXTERNAL\_IP_\-Adresse, die wir im letzten Schritt erhalten haben, wie in der folgenden Abbildung dargestellt.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/</copy>
        
    
    ![Seite "Roberts-Bücher"](images/robertbooks.png " ")
    
3.  Klicken Sie wie gezeigt auf _Erweitert_:
    
    ![Robert Advanced](images/robertadvanced.png " ")
    
4.  Wählen Sie _Weiter zu bobs-books.bobs-books. EXTERNAL\_IP .nip.io(unsicher)_, um auf die Anwendung zuzugreifen. Wenn Sie diese Option nicht zum Fortfahren erhalten, geben Sie einfach _thisisunsafe_ ohne Platz irgendwo in diesem Chrome-Browserfenster ein. Wenn Sie das Chrome-Browserfenster eingeben, wird es nicht angezeigt. Sobald Sie jedoch _thisisunsafe_ eingegeben haben, wird die Anwendungsseite sofort angezeigt. Weitere Informationen finden Sie [hier](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Robert Unsafe](images/robertunsafe.png " ") ![Robert Page](images/robertpage.png " ")
    
5.  Um die Bobby's Book Store-Homepage zu öffnen, öffnen Sie eine neue Registerkarte, kopieren Sie die folgende URL, und ersetzen Sie _XX.XX.XX.XX_ durch Ihre `EXTERNAL_IP`\-Adresse, wie in der folgenden Abbildung dargestellt.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![Bobbys-URL](images/bobbysurl.png " ")
    
    ![Bobby Buchhandlung](images/bobbysbookstore.png " ")
    
    > Lassen Sie diese Seite geöffnet, da wir sie in Übung 8 verwenden.
    
6.  Um die Benutzeroberfläche von Bobby's Book Order Manager zu öffnen, öffnen Sie eine neue Registerkarte, kopieren Sie die folgende URL, und ersetzen Sie _XX.XX.XX.XX_ durch Ihre _EXTERNAL\_IP_\-Adresse, wie in der folgenden Abbildung dargestellt.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobs-bookstore-order-manager/orders</copy>
        
    
    ![Auftragsmanager](images/ordermanager.png " ")
    
7.  Gehen Sie zurück zur Seite _Bobby's Books_ und kaufen Sie ein Buch. Klicken Sie auf _Bücher_, wie in der folgenden Abbildung dargestellt.
    
    ![Bestellung auschecken](images/checkoutorder.png " ")
    
8.  Wählen Sie das Bild für das Buch _Twilight_ aus, wie in der folgenden Abbildung dargestellt.
    
    ![Kaufbuch](images/purchasebook.png " ")
    
9.  Klicken Sie zuerst auf _In den Warenkorb_ und dann auf _Checkout_, wie in der folgenden Abbildung dargestellt.
    
    ![Klicken Sie auf addcart](images/clickaddcart.png " ")
    
10.  Geben Sie die Details für den Kauf des Buchs ein. Geben Sie unter _Ihr Bundesland_ Ihren zweistelligen Bundeslandcode ein, und klicken Sie auf _Bestellung weiterleiten_.
    

![Bestellung weiterleiten](images/submitorder.png " ") 11. Kehren Sie zur Seite _Auftragsmanager_ zurück, und wählen Sie die Schaltfläche _Aktualisieren_, um zu prüfen, ob Ihre Bestellung erfolgreich im Auftragsmanager erfasst wurde.

![Auftrag prüfen](images/verify-order.png " ")

## Aufgabe 2: Rancher-Konsole erkunden

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
    
10.  Auf der Homepage der Rancher-Konsole können Sie die Verrazzano-Links anzeigen. Klicken Sie auf _Hamburger-Menü_ -> _Verrazzano_.
    
    ![Rancher Home](images/rancher-home.png)
    
11.  Klicken Sie auf _Anwendungen_. In diesem Abschnitt werden alle Anwendungen mit ihrem Namespace angezeigt und von Verrazzano verwaltet. Klicken Sie im Namespace _bobs-books_ auf die Anwendung _bobs-books_. ![Bobs-Anwendung](images/bobs-application.png)
    
12.  Sie können die Pods anzeigen, die mit der Anwendung verknüpft sind. Der Podname enthält eine automatisch generierte eindeutige Zeichenfolge zur Identifizierung dieses bestimmten Replikats. Um die Logs der Pods _bobbys-helidon-stok-application_ anzuzeigen, klicken Sie auf _Three dots_ -> _View Logs_. ![Bobbys-Logs](images/bobs-logs.png)
    
13.  Sie können die Anwendungslogs für die Anwendung anzeigen. Wenn das Anwendungslog nicht angezeigt wird, klicken Sie auf die **Einstellungen** (blaue Schaltfläche mit dem Zahnradsymbol), und ändern Sie den Zeitfilter, um alle Logeinträge ab dem Containerstart anzuzeigen. Um die mit der Anwendung verknüpfte Komponente anzuzeigen, klicken Sie auf _Komponenten_. ![Bobbys-Log](images/bobs-log.png)
    
14.  Hier können Sie alle Komponenten der Anwendung _bobs-books_ anzeigen. Um die zugehörigen Ressourcen anzuzeigen, klicken Sie auf _Zugehörige Ressourcen_. ![Bobs-Ressource](images/bobs-resource.png) ![Bobs-Ressourcen](images/bobs-resources.png)
    
15.  Klicken Sie auf _Hamburgar-Menü_ -> _lokal_, um den _Cluster Explorer_ zu öffnen. Mit dem _Cluster Explorer_ können Sie alle benutzerdefinierten Ressourcen und CRDs in einem Kubernetes-Cluster über die Rancher-UI anzeigen und bearbeiten. ![Verrazzano-Cluster](images/verrazzano-cluster.png)
    
16.  Das Dashboard bietet einen Überblick über das Cluster und die bereitgestellten Anwendungen. Die Anzahl der Ressourcen gehört zu den _Benutzernamensräumen_, bei denen es sich praktisch um fast alle Ressourcen einschließlich des Systems handelt. Sie können oben im Dashboard nach Namespace filtern. Dies ist jedoch jetzt nicht erforderlich. Klicken Sie im linken Menü auf das Element **Knoten**, um einen Überblick über die aktuelle Last der Knoten zu erhalten.
    
    ![Cluster-Explorer](images/cluster-dashboard.png)
    
17.  Das gesamte Deployment hat keine Auswirkungen auf das OKE-Cluster. Klicken Sie nun im linken Menü auf das Element **Deployment**, um die bereitgestellten Anwendungen zu prüfen.
    
    ![Industrieknoten](images/cluster-nodes.png)
    
18.  Es werden mehrere Deployments angezeigt.
    
    ![Deployments](images/cluster-deployments.png)
    

## Aufgabe 3: Grafana-Konsole kennenlernen

1.  Klicken Sie auf _Hamburgar Menü_ -> _Home_, um die Rancher-Homepage zu öffnen. ![Startseite](images/ranchar-menu.png)
    
2.  Auf der Homepage wird der Link zum Öffnen der _Grafana-Konsole_ angezeigt. Klicken Sie wie gezeigt auf den Link für die **Grafana-Konsole**:
    
    ![Grafana-Homepage](images/grafana-link.png)
    
3.  Klicken Sie auf _Erweitert_.
    
    ![Grafana - Erweitert](images/grafanaadvanced.png " ")
    
4.  Wählen Sie _grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)_. Wenn Sie diese Option nicht zum Fortfahren erhalten, geben Sie einfach _thisisunsafe_ ohne Leerzeichen ein.
    
    ![Grafana fortfahren](images/grafanaproceed.png " ")
    
5.  Die Grafana-Homepage wird geöffnet. Wählen Sie oben links _Home_ aus.
    
    ![Grafana-Homepage](images/grafana-homepage.png " ")
    
6.  Geben Sie _WebLogic_ ein. Das _WebLogic Server-Dashboard_ wird unter _Allgemein_ angezeigt. Wählen Sie _WebLogic Server-Dashboard_.
    
    !\[Suchen WebLogic\](Bilder/Suche-weblogic.png" ")
    
    Hier können Sie die beiden Domains unter _Domain_ und Running Servers, Deployed Applications, Servername und deren Status, Heap-Nutzung, Laufzeit, JVM Heap beobachten. Wenn Ihre Anwendung Ressourcen wie JDBC und JMS enthält, können Sie hier auch Details dazu abrufen.
    
    ![WebLogic-Dashboard](images/weblogic-dashboard.png " ")
    
7.  Wählen Sie jetzt das WebLogic Server-Dashboard aus, und geben Sie _Helidon_ ein. Daraufhin wird das _Helidon-Überwachungs-Dashboard_ angezeigt. Wählen Sie _Helidon-Überwachungs-Dashboard_ aus.
    
    ![Helidon](images/search-helidon.png " ")
    
    Hier sehen Sie verschiedene Details wie den _Status_ Ihrer Anwendung und deren _Betriebszeit_, Garbage Collector und Mark Sweep Total sowie deren Zeit und Threadanzahl.
    
    ![Helidon-Dashboard](images/helidon-dashboard.png " ")
    
8.  Wählen Sie jetzt das Helidon-Überwachungs-Dashboard aus, und geben Sie _Coherence_ ein. Daraufhin wird _Coherence-Dashboard - Hauptattribute_ angezeigt. Wählen Sie _Coherence Dashboard - Hauptmenü_ aus.
    
    ![Kohärenz](images/search-coherence.png " ")
    
9.  Hier werden die Details des _Coherence-Clusters_ angezeigt. Für die Bobby's Books-Anwendung haben wir zwei Coherence-Cluster, einen für Bobby's Books und einen für Robert's Books. Sie müssen das Dropdown-Menü für _Clustername_ auswählen, um beide Cluster anzuzeigen.
    
    ![Bobby-Cluster](images/bobby-cluster.png " ")
    
    ![Robert Cluster](images/robert-cluster.png " ")
    

## Aufgabe 4: Kiali-Konsole erkunden

1.  Kehren Sie zur Verrazzano-Homepage zurück, und klicken Sie auf die Konsole **Kiali**.
    
    ![Rancher-Link](images/kiali-link.png)
    
2.  Klicken Sie auf der linken Seite auf Diagramm.
    
    ![Kiali-Dashboard](images/kiali-dashboard.png " ")
    
3.  Aktivieren Sie in der Dropdown-Liste "Namespace" das Kontrollkästchen für _Bobs-Books_, und verschieben Sie den Curser außerhalb der Dropdown-Liste. ![Bobs-Namespace](images/bobs-namespace.png " ")
    
4.  Sie können die grafische Ansicht der Anwendung _bobs-books_ anzeigen. Klicken Sie auf _Legende_, um die Ansicht "Legende" anzuzeigen.
    
    ![Grafische Ansicht](images/graphical-view.png " ")
    
5.  Hier können Sie anzeigen, was jede Ausprägung darstellt. Beispiel: Kreis stellt die _Workloads_ dar.
    
    ![Legendenansicht](images/legend-view.png " ")
    
6.  Klicken Sie auf der linken Seite auf _Anwendungen_, um alle bereitgestellten Anwendungen anzuzeigen.
    
    ![Anwendungen](images/kiali-applications.png " ")
    

## Aufgabe 5: OpenSearch-Dashboards untersuchen

1.  Kehren Sie zur Verrazzano-Homepage zurück, und klicken Sie auf die Konsole **OpenSearch Dashboards**.
    
    ![Kibana-Link](images/opensearch-link.png)
    
2.  Klicken Sie bei Bedarf auf _Weiter zu ... Standard XX.XX.XX.nip.io(unsicher)_.
    
3.  Klicken Sie auf der OpenSearch-Homepage auf **Home** -> **Discover**.
    
    ![Klicken auf Kibana-Dashboard](images/discover-1.png)
    
4.  Wählen Sie den Namespace _`verrazzano-applications*`_ wie dargestellt aus, suchen Sie nach _Büchern_, und drücken Sie die **Eingabetaste**, oder klicken Sie auf **Aktualisieren**. Sie müssen die Logs mit _Büchern_ abrufen. ![Problemergebnis](images/search-books.png)
    

## Aufgabe 6: Prometheus-Konsole erkunden

1.  Kehren Sie zur Verrazzano-Homepage zurück, und klicken Sie auf die Konsole **Prometheus**.
    
    ![Prometheus-Link](images/prometheus-link.png)
    
2.  Klicken Sie auf **Fortfahren zum ... Standard XX.XX.XX.XX.nip.io(unsicher)**, wenn Sie dazu aufgefordert werden.
    
3.  Geben Sie auf der Prometheus-Dashboard-Seite _get_ in das Suchfeld ein, und klicken Sie auf Ihre benutzerdefinierte Metrik _`application_org_books_bobby_BookResource_getBook_total`_.
    
    ![Prometheus ausführen](images/prometheus-query.png)
    
4.  Klicken Sie auf **Ausführen**, und prüfen Sie das Ergebnis unten. Sie sollten den aktuellen Wert Ihrer Metrik anzeigen, was bedeutet, wie viele Anforderungen von Ihrem Endpunkt abgeschlossen wurden. Sie können auch zur _Diagrammansicht_ anstatt zum _Konsolenmodus_ wechseln.
    
    ![Prometheus-Wert](images/execute-query.png)
    
    > Sie können Ihrem Dashboard auch eine weitere Metrik hinzufügen. Ermitteln Sie die verfügbaren Standardmetriken in der Liste.
    

## Aufgabe 7: Entdecken Sie die Keycloak-Konsole

1.  Kehren Sie zur Verrazzano-Homepage zurück, und klicken Sie auf die Konsole **Keycloak**.
    
    ![Schlüsselanhänger](images/keycloak-link.png)
    
2.  Klicken Sie auf **Fortfahren zum ... Standard XX.XX.XX.XX.nip.io(unsicher)**, wenn Sie dazu aufgefordert werden.
    
3.  Klicken Sie auf der Seite "Willkommen bei Keycloak" auf _Administrationskonsole_. ![Schlüsselanhänger](images/keycloak-home.png)
    
4.  Jetzt benötigen wir den Benutzernamen und das Passwort für die Keycloak-Konsole. _Benutzername_ ist _keycloakadmin_. Um das Kennwort zu ermitteln, gehen Sie zurück zu _Cloud Shell_, und fügen Sie den folgenden Befehl ein, um das Kennwort für die _Keycloak-Konsole_ zu ermitteln.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  Kopieren Sie das Passwort, und kehren Sie zum Browser zurück, in dem die _Keycloak-Konsole_ geöffnet ist. Fügen Sie das Kennwort in das Feld _Kennwort_ ein, und geben Sie _keycloakadmin_ als _Benutzername_ ein. Klicken Sie dann auf **Anmelden**.
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  Hier können Sie die Standardkonfiguration von Verrazzano anzeigen. ![Schlüsselanhänger Home](images/keycloak-realms.png)
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
# Übungsumgebung einrichten

## Einführung

In dieser Übung erstellen Sie ein Kubernetes-Cluster mit 3 Knoten, das mit allen erforderlichen Netzwerkressourcen konfiguriert ist. Außerdem erstellen Sie ein Repository in Oracle Cloud Container Image Registry. Anschließend generieren Sie ein Authentifizierungstoken. Außerdem akzeptieren Sie die Lizenzvereinbarung für WebLogic Server-Images in Oracle Container Registry.

Geschätzte Zeit: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Übungsumgebung einrichten](videohub:1_zhvohpqq)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Erstellen Sie ein Oracle Kubernetes-Cluster.
*   Erstellen Sie ein Repository in Oracle Cloud Container Image Registry.
*   Authentifizierungstoken generieren.
*   Akzeptieren Sie die Lizenz für WebLogic Server-Images in Oracle Container Registry.

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.
*   Sie benötigen einen Oracle-Account.
*   Sie benötigen einen Texteditor.

## Aufgabe 1: Oracle Kubernetes-Cluster erstellen

Das Feature _Schnellerstellung_ verwendet die Standardeinstellungen, um nach Bedarf ein _schnelles Cluster_ mit neuen Netzwerkressourcen zu erstellen. Dieser Ansatz ist der schnellste Weg, um ein neues Cluster zu erstellen. Wenn Sie alle Standardwerte akzeptieren, können Sie mit nur wenigen Klicks ein neues Cluster erstellen. Neue Netzwerkressourcen für das Cluster werden automatisch zusammen mit einem Knotenpool und drei Worker-Knoten erstellt.

1.  Wählen Sie in der Konsole _Hamburger Menü -> Developer Services -> Kubernetes Clusters (OKE)_, wie gezeigt.
    
    ![Hamburger-Menü](images/hamburger-menu.png " ")
    
2.  Wählen Sie auf der Seite "Clusterliste" das Compartment aus, und klicken Sie auf _Cluster erstellen_.
    
    > Sie müssen ein Compartment auswählen, in dem Sie ein Cluster erstellen dürfen, sowie ein Repository in der Oracle Container Registry.
    
    ![Compartment auswählen](images/select-compartment.png " ")
    
3.  Wählen Sie im Dialogfeld "Clusterlösung erstellen" die Option _Schnellerstellung_ aus, und klicken Sie auf _Weiterleiten_.
    
    ![Workflow starten](images/launch-workflow.png " ")
    
    Mit _Erstellen_ wird ein neues Cluster mit den Standardeinstellungen zusammen mit neuen Netzwerkressourcen für das neue Cluster erstellt.
    
    Geben Sie die folgenden Konfigurationsdetails auf der Seite "Cluster erstellen" an (bitte achten Sie auf den Wert, den Sie im Feld _Ausprägung_ eingeben):
    
    *   **Name**: Der Name des Clusters. Übernehmen Sie den Standardwert.
    *   **Compartment**: Der Name des Compartments. Wählen Sie das Compartment aus, in dem Sie Ressourcen erstellen dürfen.
    *   **Kubernetes-Version**: Die Version von Kubernetes. Wählen Sie _1.26.2_ als Kubernetes-Version aus.
    *   **Kubernetes-API-Endpunkt**: Werden die Clustermasterknoten routbar oder nicht. Wählen Sie den Wert _Öffentlicher Endpunkt_ aus.
    *   **Knotyp**: Wählen Sie _Verwaltet_ als Knotentyp aus.
    *   **Kubernetes-Worker-Knoten**: Werden die Cluster-Worker-Knoten routbar oder nicht. Übernehmen Sie den Standardwert für _Private Mitarbeiter_.
    *   **Ausprägung**: Die Ausprägung, die für jeden Knoten im Knotenpool verwendet werden soll. Die Ausprägung bestimmt die Anzahl der CPUs und die Speichermenge, die jedem Knoten zugewiesen ist. In der Liste werden nur die in Ihrem Mandanten verfügbaren Ausprägungen angezeigt, die von OKE unterstützt werden. Wählen Sie _VM.Standard.E4. Flex_ (in der Regel im Oracle Free Tier-Account verfügbar). Wählen Sie die 1 OCPUs und 16 GB als Arbeitsspeichermenge aus.
    *   **Image**: Wählen Sie das Standardimage mit Kubernetes-Version _1.26.2_ aus.
    *   **Anzahl Knoten**: Die Anzahl der zu erstellenden Worker-Knoten. Übernehmen Sie den Standardwert _3_.
    
    ![Schnellcluster](images/quick-cluster1.png " ") ![Daten eingeben](images/enter-data.png " ")
    
4.  Klicken Sie auf _Weiter_, um die Details zu prüfen, die Sie für das neue Cluster eingegeben haben.
    
5.  Aktivieren Sie auf der Seite _Prüfen_ das Kontrollkästchen _Basiscluster erstellen_, und klicken Sie dann auf _Cluster erstellen_, um die neuen Netzwerkressourcen und das neue Cluster zu erstellen.
    
    ![Cluster prüfen](images/review-cluster.png " ")
    
    > Die für Sie erstellten Netzwerkressourcen werden angezeigt. Warten Sie, bis die Anforderung zum Erstellen des Knotenpools initiiert wurde, und klicken Sie dann auf _Schließen_.
    
    ![Netzwerkressource](images/network-resource.png " ")
    
    > Anschließend wird das neue Cluster auf der Seite _Clusterdetails_ angezeigt. Wenn die Masterknoten erstellt werden, erhält das neue Cluster den Status _Aktiv_ (es dauert etwa 7 Minuten). Sie müssen nicht warten. Fahren Sie mit der nächsten Aufgabe fort.
    
    ![Clusterzugriff](images/cluster-access.png " ")
    

## Aufgabe 2: Repository erstellen

In dieser Aufgabe erstellen Sie ein öffentliches Repository. In Übung 5 werden wir Auxiliary Image in dieses Repository übertragen.

1.  Wählen Sie in der Konsole das _Hamburger-Menü_ -> _Developer Services_ -> _Container Registry_ wie gezeigt. ![Container Registry](images/container-registry.png)
    
2.  Wählen Sie Ihr Compartment aus, in dem Sie das Repository erstellen dürfen. Klicken Sie auf _Repository erstellen_. ![Repository erstellen](images/create-repository.png)
    
3.  Geben Sie _`test-model-your_firstname`_ als Repository-Namen und Zugriff als _Öffentlich_ ein, und klicken Sie auf _Erstellen_. ![Repository-Details](images/repository-details.png)
    
4.  Sobald das Repository bereit ist. Notieren Sie sich den Mandanten-Namespace in der Textdatei im Texteditor. ![Notizmandant NameSpace](images/tenancy-namespace.png)
    

## Aufgabe 3: Authentifizierungstoken generieren

In dieser Aufgabe generieren wir ein _Authentifizierungstoken_. In Übung 5 verwenden wir dieses Authentifizierungstoken, um ein Auxiliary Image in das Oracle Cloud Container Registry Repository zu übertragen.

1.  Wählen Sie das Benutzersymbol in der oberen rechten Ecke aus, und wählen Sie _MyProfile_ aus.
    
    ![Mein Profil](images/my-profile.png)
    
2.  Scrollen Sie nach unten, wählen Sie _Authentifizierungstoken_ aus, und klicken Sie auf _Token generieren_.
    
    ![Authentifizierungstoken](images/auth-token.png)
    
3.  Kopieren Sie _`test-model-your_first_name`_, fügen Sie es in das Feld _Beschreibung_ ein, und klicken Sie auf _Token generieren_.
    
    ![Token erstellen](images/create-token.png)
    
4.  Wählen Sie unter "Generiertes Token" die Option _Kopieren_ aus, und fügen Sie es in Ihren Texteditor ein. Wir können es später nicht kopieren. Klicken Sie auf _Schließen_.
    
    ![Token kopieren](images/copy-token.png)
    

## Aufgabe 4: Lizenz für WebLogic Server-Images akzeptieren

In dieser Aufgabe akzeptieren wir die Lizenzvereinbarung für WebLogic Server-Images in Oracle Container Registry. Wie in Übung 3 verwenden wir das Image WebLogic Server 12.2.1.3.0 als primäres Image. Um Zugriff auf WebLogic Server-Images zu erhalten, akzeptieren wir die Lizenzvereinbarung.

1.  Klicken Sie auf den Link für die Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/), und melden Sie sich an. Dazu benötigen Sie einen Oracle Account. ![Container Registry-Anmeldung](images/container-registry-sign-in.png)
    
2.  Geben Sie Ihre _Oracle-Accountzugangsdaten_ in die Felder "Benutzername" und "Kennwort" ein, und klicken Sie auf _Anmelden_. ![Anmelde-Container Registry](images/login-container-registry.png)
    
3.  Suchen Sie auf der Homepage von Oracle Container Registry nach _weblogic_. ![WebLogic durchsuchen](images/search-weblogic.png)
    
4.  Klicken Sie wie dargestellt auf _Weblogic_, und wählen Sie _Englisch_ als Sprache aus. Klicken Sie dann auf _Weiter_. ![Klicken Sie auf WebLogic.](images/click-weblogic.png) ![Sprache auswählen](images/select-language.png)
    
5.  Klicken Sie auf _Akzeptieren_, um den Lizenzvertrag zu akzeptieren. ![Lizenz akzeptieren](images/accept-license.png)
    

## Weitere Informationen

_Informationen zu Oracle Cloud Infrastructure Container Engine for Kubernetes_

Oracle Cloud Infrastructure Container Engine for Kubernetes ist ein vollständig verwalteter, skalierbarer und hochverfügbarer Service, mit dem Sie Ihre Containeranwendungen in der Cloud bereitstellen können. Verwenden Sie Container Engine for Kubernetes (manchmal abgekürzt OKE), wenn Ihr Entwicklungsteam cloudnative Anwendungen zuverlässig erstellen, bereitstellen und verwalten möchte. Sie geben die Compute-Ressourcen an, die Ihre Anwendungen benötigen, und OKE stellt sie in Oracle Cloud Infrastructure in einem vorhandenen OCI-Mandanten bereit.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
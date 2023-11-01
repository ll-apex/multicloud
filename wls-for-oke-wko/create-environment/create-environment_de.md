# Übungsumgebung einrichten

## Einführung

In dieser Übung erstellen Sie ein Repository in Oracle Cloud Container Image Registry. Außerdem akzeptieren Sie die Lizenzvereinbarung für WebLogic Server-Images in Oracle Container Registry.

Geschätzte Zeit: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Erstellen Sie ein Repository in Oracle Cloud Container Image Registry.
*   Akzeptieren Sie die Lizenz für WebLogic Server-Images in Oracle Container Registry.

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.
*   Sie benötigen einen Oracle-Account.
*   Sie benötigen einen Texteditor.

## Aufgabe 1: Repository erstellen

In dieser Aufgabe erstellen Sie ein öffentliches Repository. In Übung 5 werden wir Auxiliary Image in dieses Repository übertragen.

1.  Um Firefox zu öffnen, klicken Sie in Remotedesktop auf **Aktivitäten**, geben Sie **Feuer** in das Suchfeld ein, und klicken Sie wie dargestellt auf **Firefox**. ![open firefox](images/open-firefox.png)
    
2.  Melden Sie sich mit Ihren Zugangsdaten bei der [Oracle Cloud-Konsole](https://cloud.oracle.com) an.
    
3.  Wählen Sie in der Konsole das _Hamburger-Menü_ -> _Developer Services_ -> _Container Registry_ wie gezeigt. ![Container Registry](images/container-registry.png)
    
4.  Wählen Sie Ihr Compartment aus, in dem Sie das Repository erstellen dürfen. Klicken Sie auf _Repository erstellen_. ![Repository erstellen](images/create-repository.png)
    
5.  Geben Sie _`test-model-your_firstname`_ als Repository-Namen und Zugriff als _Öffentlich_ ein, und klicken Sie auf _Erstellen_. ![Repository-Details](images/repository-details.png)
    
6.  Sobald das Repository bereit ist. Notieren Sie sich den Mandanten-Namespace in Ihrer Textdatei. ![Notizmandant NameSpace](images/tenancy-namespace.png)
    

## Aufgabe 2: Lizenz für WebLogic Server-Images akzeptieren

In dieser Aufgabe akzeptieren wir die Lizenzvereinbarung für WebLogic Server-Images in Oracle Container Registry. Wie in Übung 5 verwenden wir das Image WebLogic Server 12.2.1.3.0 als primäres Image. Um Zugriff auf WebLogic Server-Images zu erhalten, akzeptieren wir die Lizenzvereinbarung.

1.  Kopieren Sie den Link für die Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/), und fügen Sie ihn in den Browser ein, und melden Sie sich an. Dazu benötigen Sie einen Oracle Account. ![Container Registry-Anmeldung](images/container-registry-sign-in.png)
    
2.  Geben Sie Ihre _Oracle-Accountzugangsdaten_ in die Felder "Benutzername" und "Kennwort" ein, und klicken Sie auf _Anmelden_. ![Anmelde-Container Registry](images/login-container-registry.png)
    
3.  Suchen Sie auf der Homepage von Oracle Container Registry nach _weblogic_. ![WebLogic durchsuchen](images/search-weblogic.png)
    
4.  Klicken Sie wie dargestellt auf _Weblogic_, und wählen Sie _Englisch_ als Sprache aus. Klicken Sie dann auf _Weiter_. ![Klicken Sie auf WebLogic.](images/click-weblogic.png) ![Sprache auswählen](images/select-language.png)
    
5.  Klicken Sie auf _Akzeptieren_, um den Lizenzvertrag zu akzeptieren. ![Lizenz akzeptieren](images/accept-license.png)
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Oktober 2023
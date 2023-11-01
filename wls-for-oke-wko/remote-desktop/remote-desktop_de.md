# Compute-Instanz einrichten

## Einführung

In dieser Übung wird gezeigt, wie Sie einen **Oracle WebLogic Suite for OKE BYOL**\-Stack einrichten, der die Oracle Cloud-Objekte generiert, die zum Ausführen Ihres Workshops erforderlich sind.

Geschätzte Zeit: 15 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Authentifizierungstoken generieren.
*   Secret in einem Vault erstellen
*   Stack erstellen: Oracle WebLogic Suite für OKE BYOL
*   Verbindung zur Compute-Instanz herstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Oracle Cloud-Account
*   Sie haben das SSH-Schlüsselpaar generiert.
*   Sie haben Folgendes abgeschlossen: **Übung: Setup vorbereiten**

## Aufgabe 1: Authentifizierungstoken generieren

In dieser Aufgabe generieren wir ein _Authentifizierungstoken_. In Übung 5 verwenden wir dieses Authentifizierungstoken, um ein Auxiliary Image in das Oracle Cloud Container Registry Repository zu übertragen. Außerdem verwenden wir dieses Authentifizierungstoken, um WebLogic-Dockerimages in Oracle Cloud Infrastructure Registry (auch als Container Registry bezeichnet) abzurufen.

1.  Wählen Sie das Benutzersymbol in der oberen rechten Ecke aus, und wählen Sie _MyProfile_ aus.
    
    ![Mein Profil](images/my-profile.png)
    
2.  Scrollen Sie nach unten, wählen Sie _Authentifizierungstoken_ aus, und klicken Sie auf _Token generieren_.
    
    ![Authentifizierungstoken](images/auth-token.png)
    
3.  Kopieren Sie _`test-model-your_first_name`_, fügen Sie es in das Feld _Beschreibung_ ein, und klicken Sie auf _Token generieren_.
    
    ![Token erstellen](images/create-token.png)
    
4.  Wählen Sie unter "Generiertes Token" die Option _Kopieren_ aus, und fügen Sie es in Ihre Textdatei ein. Wir können es später nicht kopieren. Klicken Sie auf _Schließen_.
    
    ![Token kopieren](images/copy-token.png)
    
    > In der nächsten Aufgabe wird dieses Authentifizierungstoken im **Secret** gespeichert.
    

## Aufgabe 2: Secret in einem Vault erstellen

Secrets sind Zugangsdaten wie Kennwörter, Zertifikate, SSH-Schlüssel oder **Authentifizierungstoken**, die Sie mit Oracle Cloud Infrastructure-Services verwenden.

1.  Klicken Sie in der OCI-Konsole auf **Hamburger-Menü** -> **Identität und Sicherheit** -> **Vault**. ![Menü-Vault](images/menu-vault.png)
    
2.  Wählen Sie das Compartment unter **Listengeltungsbereich** aus, und klicken Sie auf **Vault erstellen**.
    
3.  Geben Sie den Namen des Vaults ein, und klicken Sie auf **Vault erstellen**. ![Vault erstellen](images/create-vault.png)
    
4.  Wenn der erstellte Vault den Status **Aktiv** aufweist, klicken Sie wie unten gezeigt auf den Vault-Namen. ![Vault-Name](images/vault-name.png)
    
5.  Klicken Sie im Vault auf **Hauptverschlüsselungsschlüssel** und dann auf **Schlüssel erstellen**. ![Menü mek](images/menu-mek.png)
    
6.  Geben Sie den Namen für den **Schlüssel** ein, und klicken Sie wie unten gezeigt auf **Schlüssel erstellen**. ![mek erstellen](images/create-mek.png)
    
7.  Sobald der Status für die Änderung des neuen Masterverschlüsselungsschlüssels in **Aktiviert** angezeigt wird, klicken Sie wie unten gezeigt unter **Ressourcen** auf **Secrets**. ![Menü Secrets](images/menu-secret.png)
    
8.  Klicken Sie auf **Secret erstellen**.
    
9.  Geben Sie den Namen für das Secret ein, und wählen Sie den neuen Verschlüsselungsschlüssel aus. Geben Sie das Authentifizierungstoken wie unten gezeigt als **Secret-Inhalt** ein. Klicken Sie auf **Secret erstellen**. ![Secret erstellen](images/create-secret.png)
    

## Aufgabe 3: Stack erstellen: Oracle WebLogic Suite für OKE BYOL

1.  Klicken Sie in der OCI-Konsole auf **Hamburger-Menü** -> **Marketplace** -> **Alle Anwendungen**. ![Menü-Marktplatz](images/menu-marketplace.png)
    
2.  Geben Sie **WebLogic OKE** in das Suchfeld ein, und klicken Sie auf **Oracle WebLogic Suite für OKE BYOL**. ![Menüstapel](images/menu-stack.png)
    
3.  Wählen Sie die neueste verfügbare Version aus, wählen Sie Ihr Compartment aus, und aktivieren Sie das Kontrollkästchen, um die Bedingungen zu akzeptieren. Klicken Sie auf **Stack starten**. ![Stack starten](images/launch-stack.png)
    
4.  Übernehmen Sie im Abschnitt "Stackinformationen" den Standardwert, und klicken Sie auf **Weiter**. ![Stackinformationen](images/stack-info.png)
    
5.  Geben Sie **wko** als Ressourcennamenpräfix ein, und klicken Sie auf "Durchsuchen", um den SSH-Public Key auszuwählen. ![Präfix-SSH](images/prefix-ssh.png)
    
    > Stellen Sie sicher, dass Sie **wko** als Präfix verwenden. Dies wird in weiteren Übungen erläutert.
    
6.  Lassen Sie in der Verrazzano-Integration das Kontrollkästchen **Verrazzano aktivieren** deaktiviert. ![verrazzano deaktivieren](images/uncheck-verrazzano.png)
    
7.  Wählen Sie im Netzwerk die Option **Neues VCN erstellen** als virtuelle Cloud-Netzwerkstrategie aus, und übernehmen Sie den Standardwert. ![Netzwerk](images/network.png)
    
8.  Wählen Sie in der Container Cluster-(OKE-)Konfiguration Folgendes aus.
    
    **Kubernetes-Version:** Übernehmen Sie die Standardeinstellung **v1.26.2**.
    
    **Ausprägung des Knotenpools als WebLogic (erforderlich)**: Wählen Sie **VM.Standard.E4 aus. Flex** als Ausprägung und Chooke **1** als OCPU-Anzahl und **16** als Arbeitsspeichermenge.
    
    **Knoten in NodePool für Nicht-WebLogic-Pods**: Übernehmen Sie den Standardwert **1**.
    
    ![nicht WebLogic](images/non-weblogic.png)
    
    **Ausprägung des Knotenpools WebLogic erstellen** - Lassen Sie das Standardfeld aktiviert.
    
    **WebLogic Ausprägung des Knotenpools (erforderlich)**: Wählen Sie **VM.Standard.E4 aus. Flex** als Ausprägung und Chooke **1** als OCPU-Anzahl und **16** als Arbeitsspeichermenge.
    
    **Knoten im NodePool für WebLogic-Pods**: Übernehmen Sie den Standardwert **1**.
    
    ![WebLogic](images/weblogic-pool.png)
    
9.  Geben Sie unter "Administrationsinstanzen" Folgendes ein, oder wählen Sie Folgendes aus: **Availability-Domain für Compute-Instanzen** - Wählen Sie die Availability-Domain aus der Dropdown-Liste aus.
    
    **Compute-Ausprägung der Administrationsinstanz (erforderlich)** - Wählen Sie **VM.Standard.E4 aus. Flex** als Ausprägung und Chooke **1** als OCPU-Anzahl und **16** als Arbeitsspeichermenge.
    
    ![Admin-Instanz](images/admin-instance.png)
    
    **Ausprägung der Bastioninstanz (erforderlich)**: Wählen Sie **VM.Standard.E4 aus. Flex** als Ausprägung und Chooke **1** als OCPU-Anzahl und **16** als Arbeitsspeichermenge.
    
    ![Bastion](images/bastion.png)
    
10.  Wählen Sie im Abschnitt "Dateisystem" Folgendes aus (siehe Abbildung). **Availability-Domain für Dateisystem** - Wählen Sie die Availability-Domain aus der Dropdown-Liste aus. ![Dateivertrag](images/file-avd.png)
    
11.  Geben Sie im Abschnitt "Registry (OCIR)" Folgendes ein (siehe Abbildung).
    
    **Registrierungsbenutzername** - Der Benutzername, mit dem Kubernetes auf die Containerimage-Registry zugreift, die das Format {identity domain name}/{username} hat. Wenn Ihr Mandant Oracle Identity Cloud Service verwendet, verwenden Sie das Format oracleidentitycloudservice/{username}.
    
    **OCIR-Authentifizierungstoken-Compartment** - Wählen Sie das Compartment aus, in dem Sie das OCIR-Authentifizierungstoken haben.
    
    **Validiertes Secret für OCIR-Authentifizierungstoken** - Das Secret, das das OCIR-Authentifizierungstoken enthält, das Sie für den Benutzer für den Zugriff auf die Image-Registry generiert haben.
    
    ![ocir geheim](images/ocir-secret.png)
    
12.  Aktivieren Sie in OCI-Policys das Kontrollkästchen für **OCI-Policys**. Sie erstellt Policys, um Secrets aus Vault zu lesen und die Autonomous Transaction Processing-Datenbank (falls zutreffend) zu verwalten. Klicken Sie auf **Weiter**.
    
13.  Aktivieren Sie im Abschnitt "Prüfen" das Kontrollkästchen **Anwenden ausführen**, und klicken Sie auf **Erstellen**. ![Apply ausführen](images/run-apply.png)
    
    > Dadurch wird ein Job erstellt, der die erforderlichen Ressourcen im Stack erstellt. Sie müssen nicht warten. Fahren Sie mit der nächsten Aufgabe fort.
    

## Aufgabe 4: Auf den grafischen Remotedesktop zugreifen

Um die Ausführung dieses Workshops zu vereinfachen, wurde Ihre VM-Instanz mit einem Remote-Grafikdesktop vorkonfiguriert, auf den Sie mit einem modernen Browser auf Ihrem Laptop oder Ihrer Workstation zugreifen können. Gehen Sie wie unten beschrieben vor, um sich anzumelden.

1.  Öffnen Sie das Hamburger-Menü oben links. Klicken Sie auf **Entwicklerservices**, und wählen Sie **Resource Manager** > **Stacks**.
    
2.  Klicken Sie auf den Stacknamen, den Sie in Übung 1 erstellt haben. ![Klick auf Stack](images/click-stack.png)
    
3.  Navigieren Sie zur Registerkarte **Anwendungsinformationen**, kopieren Sie die **Remote-Desktop-URL**, und fügen Sie sie in die neue Browserregisterkarte ein. ![Desktop-URL](images/desktop-url.png)
    
    > Jetzt müssen Sie alle Anweisungen in diesem Remote-Desktop befolgen.
    

Sie können jetzt mit der nächsten Übung fortfahren.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Oktober 2023
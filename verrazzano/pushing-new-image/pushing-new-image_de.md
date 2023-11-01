# Docker-Image für bobbys-helidon-stock-application per Push an Oracle Cloud Container Registry übergeben

## Einführung

In Lab 5 haben wir bobbys-helidon-stock-application modifiziert und ein neues Docker-Image erstellt. In dieser Übung übertragen wir dieses Image in ein Repository in der Oracle Cloud Container Registry.

Geschätzte Zeit: 10 Minuten

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Generieren Sie ein Authentifizierungstoken für die Anmeldung bei der Oracle Cloud Container Registry.
*   Übertragen Sie das Docker-Image für Bobbys-helidon-stock-application in Ihr Oracle Cloud Container Registry-Repository.

### Voraussetzungen

Sie benötigen einen Texteditor, in dem Sie die Befehle und URLs einfügen und entsprechend Ihrer Umgebung ändern können. Anschließend können Sie den geänderten Befehl zum Ausführen in die _Cloud Shell_ kopieren und einfügen.

## Aufgabe 1: Authentifizierungstoken für die Anmeldung bei Oracle Cloud Container Registry generieren

In diesem Schritt generieren wir ein _Authentifizierungstoken_, das wir zur Anmeldung bei der Oracle Cloud Container Registry verwenden.

1.  Wählen Sie in der oberen rechten Ecke das Benutzersymbol und dann _Mein Profil_ aus.
    
    ![Benutzereinstellungen](images/user-settings.png " ")
    
2.  Scrollen Sie nach unten, und wählen Sie _Authentifizierungstoken_ aus.
    
    ![Authentifizierungstoken](images/auth-token.png " ")
    
3.  Klicken Sie auf _Token generieren_.
    
    ![Token generieren](images/generate-token.png " ")
    
4.  Kopieren Sie _`helidon-stock-application-your_first_name`_, fügen Sie es in das Feld _Beschreibung_ ein, und klicken Sie auf _Token generieren_.
    
    ![Token erstellen](images/token-create.png " ")
    
5.  Wählen Sie unter "Generiertes Token" die Option _Kopieren_ aus, und fügen Sie es in die Textdatei ein. Wir können es später nicht kopieren.
    
    ![Token kopieren](images/copy-token.png " ")
    

## Aufgabe 2: Docker-Image der bobbys-helidon-stock-application in Ihr Oracle Cloud Container Registry Repository übertragen

1.  In Aufgabe 1 dieser Übung haben Sie eine URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) geöffnet, den Endpunkt für Ihren Regionsnamen bestimmt und in einen Texteditor kopiert. In unserem Beispiel lautet der Regionsname UK South (London). Sie benötigen diese Informationen für diese Aufgabe. ![Endpunkt](images/end-point.png)
    
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in den Texteditor ein. Ersetzen Sie dann **`END_POINT_OF_REGION_NAME`** durch den Endpunkt Ihrer Region.
    
        <copy> docker login `END_POINT_OF_REGION_NAME`</copy>
        

3\. In der vorherigen Übung haben Sie den Mandanten-Namespace bestimmt. Geben Sie den Benutzernamen wie folgt an: \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`/oracleidentitycloudservice/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*. Ersetzen Sie hier \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`\*\* durch den Namespace Ihres Mandanten und \*\*\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\* durch den Benutzernamen Ihres Oracle Cloud-Accounts, kopieren Sie den ersetzten Benutzernamen aus der Textdatei, und fügen Sie ihn in die \*Cloud Shell\* ein. Fügen Sie für das Kennwort das Authentifizierungstoken aus dem Texteditor oder an der Stelle ein, an der es gespeichert wurde. !\[Anmelderegistrierung\](Bilder/Anmeldung-registry.png " ") 3\. In der vorherigen Übung haben Sie den Mandanten-Namespace bestimmt. Erstellen Sie den Benutzernamen wie folgt: \`NAMESPACE\_OF\_YOUR\_TENANCY\`/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`. Sie müssen den Mandanten-Namespace und den Benutzernamen in Kleinbuchstaben verwenden, während Sie die Synchronisierungsanmeldung ausführen. Ersetzen Sie hier "NAMESPACE\_OF\_YOUR\_TENANCY" durch den Namespace Ihres Mandanten und "YOUR\_ORACLE\_CLOUD\_USERNAME" durch den Benutzernamen Ihres Oracle Cloud-Accounts, kopieren Sie den ersetzten Benutzernamen aus dem Texteditor, und fügen Sie ihn in die \*Cloud Shell\* ein. Fügen Sie als Kennwort das Authentifizierungstoken aus Ihrem Texteditor oder an der Stelle ein, an der Sie es gespeichert haben.

4.  Gehen Sie zurück zur Container Registry, und wählen Sie _Hamburger Menü -> Developer Services -> Container Registry_.
    
    ![Container Registry](images/container-registry.png " ")
    
5.  Wählen Sie das Compartment, und klicken Sie auf _Repository erstellen_.
    
    ![Repository-Erstellen](images/repository-create.png " ")
    
6.  Wählen Sie das Compartment aus, und geben Sie _`helidon-stock-application-your_first_name`_ als Repository-Namen ein. Wählen Sie dann "Zugriff" als _Öffentlich_ aus, und klicken Sie auf _Erstellen_.
    
    ![Repository-Daten](images/repository-data.png " ")
    
    Nachdem das Repository _`helidon-stock-application-your_first_name`_ erstellt wurde, können Sie den Namespace prüfen. Er muss mit dem Namespace Ihres Mandanten identisch sein.
    
    !\[Namespace verifizieren\](images/verify-namespace.png" ")
    
7.  Im Rahmen von Übung 5 haben Sie den vollständigen Namen des Docker-Images in Ihren Texteditor kopiert. Um das Docker-Image in das Repository in der Oracle Cloud Container Registry zu übertragen, kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihren Texteditor ein. Ersetzen Sie dann _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`helidon-stock-application-your_first_name`:1.0_ durch den vollständigen Docker-Imagenamen, den Sie in Ihrem Texteditor gespeichert haben.
    
        <copy>docker push `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/helidon-stock-application-your_first_name:1.0</copy>
        
    
    ![Docker-Push](images/docker-push.png " ")
    
    Nachdem der Befehl _docker push_ erfolgreich ausgeführt wurde, blenden Sie das Repository _`helidon-stock-application-your_first_name`_ ein, und Sie werden feststellen, dass ein neues Image in dieses Repository hochgeladen wurde.
    
    Lassen Sie die _Cloud Shell_\- und Container Registry-Repository-Seite geöffnet. Sie werden für die nächsten Übungen benötigt.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, März 2023
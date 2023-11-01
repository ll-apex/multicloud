# Auxiliary Image erstellen und in Oracle Container Image Registry übertragen

## Einführung

**Primäres Image** - Das Image, das die Oracle Fusion Middleware-Software enthält. Sie wird als Basis aller Container verwendet, die WebLogic-Server für die Domain ausführen.

**Auxiliary Image** - Das Image, das die WebLogic Deploy Tooling-Software und die Modelldateien bereitstellt. Zur Laufzeit wird der Inhalt des Auxiliary-Images mit dem Inhalt des primären Images zusammengeführt. ![Bildstruktur](images/image-structure.png)

In dieser Übung verwenden wir das Image 12.2.1.3.0-ol8 des WebLogic-Servers als primäres Image. Außerdem erstellen wir ein Auxiliary Image und übergeben es mit dem generierten Authentifizierungstoken an das Oracle Container Image Registry-Repository.

Geschätzte Zeit: 10 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Erstellen von Images für OKE auf OCI](videohub:1_y5o56oe5)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Erstellen Sie ein Zusatzimage, und übertragen Sie das Image per Push an Oracle Cloud Container Image Registry.

## Aufgabe 1: Zusatzbild vorbereiten und Zusatzbild per Push übertragen

In dieser Aufgabe erstellen wir ein Zusatzimage, das an die Oracle Cloud Container Registry übertragen wird.

1.  Klicken Sie auf _Bild_. Für "Primäres Image" verwenden wir die folgende _Weblogik_ Image.So. Übernehmen Sie die Standardwerte unter dem Abschnitt _Primäres Image_ (siehe Abbildung).
    
        <copy>container-registry.oracle.com/middleware/weblogic:12.2.1.3-ol8</copy>
        
    
    ![Primäres Bild](images/primary-image.png)
    
    > **Nur zu Informationszwecken:**  
    > Das primäre Image wird zum Ausführen der Domain verwendet. Ein primäres Image kann für Hunderte von Domains wiederverwendet werden. Das primäre Image enthält die BS-, JDK- und FMW-Softwareinstallationen.
    
2.  Um das Zusatzbildtag zu erstellen, benötigen wir die folgenden Informationen:
    
    *   Endpunkt für die Region
    *   Mandanten-Namespace
3.  Suchen Sie den _Endpunkt für Ihre Region_. Weitere Informationen finden Sie in der Tabelle, die unter dieser URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) dokumentiert ist. Im dargestellten Beispiel lautet der Endpunkt für die Region _UK South (London)_ (als Regionsname), und der Endpunkt ist _lhr.ocir.io_. Suchen Sie den Endpunkt für Ihren eigenen _Regionsnamen_, und speichern Sie ihn in der Textdatei. Sie werden es auch für das nächste Labor benötigen.
    
    ![Endpunkte](images/end-point.png " ")
    
    > Jetzt haben Sie sowohl den Mandanten-Namespace als auch den Endpunkt für Ihre Region.
    
4.  In Übung 3 haben Sie den Mandanten-Namespace bereits in der Textdatei notiert. Wenn nicht, wählen Sie zum Suchen des Namespace des Mandanten das Hamburger-Menü _Menü_ -> _Entwicklerservices_ -> _Container Registry_, wie gezeigt. Wählen Sie das erstellte Repository aus. Der Namespace wird wie dargestellt angezeigt. ![Mandanten-Namespace](images/tenancy-namespace.png)
    
5.  Jetzt haben Sie sowohl den Mandanten-Namespace als auch den Endpunkt für Ihre Region. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in Ihre Textdatei ein. Ersetzen Sie dann `END_POINT_OF_YOUR_REGION` durch den Endpunkt Ihres Regionsnamens, `NAMESPACE_OF_YOUR_TENANCY`, durch den Namespace Ihres Mandanten. Klicken Sie auf die Registerkarte _Auxiliary Image_ (siehe Abbildung). ![Registerkarte "Auxiliary"](images/auxiliary-tab.png)
    
        <copy>END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/test-model-your_first_name:v1</copy>
        

> Beispiel: In meinem Fall lautet das Auxiliary Image-Tag `lhr.ocir.io/tenancynamespace/test-model-ankit:v1`.

6.  In Schritt 4 haben Sie auch den Mandanten-Namespace bestimmt. Geben Sie den Pushbenutzernamen für die Auxiliary Image Registry wie folgt ein: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    

*   Ersetzen Sie `NAMESPACE_OF_YOUR_TENANCY` durch den Namespace Ihres Mandanten
*   Ersetzen Sie `YOUR_ORACLE_CLOUD_USERNAME` durch Ihren Oracle Cloud-Accountbenutzernamen, kopieren Sie den ersetzten Benutzernamen aus der Textdatei, und fügen Sie ihn in den _Pushbenutzernamen der Auxiliary Image Registry_ ein.

> Beispiel: In meinem Fall lautet **Auxiliary Image Registry Push Username** `tenancynamespace/lab.user@oracle.com`.

*   Kopieren Sie das Authentifizierungstoken für das Kennwort, und fügen Sie es aus Ihrer Textdatei (oder wo auch immer Sie es gespeichert haben) ein, und fügen Sie es in den **Pushbenutzernamen der Zusatzbild-Registry** ein. ![Zusatzbilddetails](images/auxiliary-image-details.png)

7.  Klicken Sie auf _Auxiliary-Image erstellen_. ![Zusatzbild erstellen](images/create-auxiliary-image.png)
    
8.  Da das Modell bereits in Übung 2 vorbereitet wurde, klicken Sie auf _Nein_. ![Modell vorbereiten](images/prepare-model.png)
    
9.  Wählen Sie den Ordner _Downloads_ aus, in dem _WebLogic Deployer_ gespeichert werden soll, und klicken Sie wie dargestellt auf _Auswählen_. ![WDT-Standort](images/wdt-location.png)
    
10.  Nachdem Auxiliary-Images erfolgreich erstellt wurden, klicken Sie im Fenster _Auxiliary-Image erstellen abgeschlossen_ auf _OK_. ![Zusatz erstellt](images/auxiliary-created.png)
    
    > **Nur zu Informationszwecken:**  
    > Ein Auxiliary-Image ist domänenspezifisch. Das Auxiliary Image enthält die Daten, mit denen die Domain definiert wird.
    
11.  Klicken Sie auf _Auxiliary-Image pushen_, um das Image in das Repository in der Oracle Cloud Container Image Registry zu übertragen. ![Hilfsmittel pushen](images/push-auxiliary.png)
    
12.  Nachdem das Image erfolgreich übertragen wurde, klicken Sie im Fenster _Pushimage abgeschlossen_ auf _OK_. ![Hilfsmittel gedrückt](images/auxiliary-pushed.png)
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
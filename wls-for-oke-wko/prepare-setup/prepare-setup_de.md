# Setup vorbereiten

## Einführung

In dieser Übung wird gezeigt, wie Sie die ZIP-Datei für den Oracle Resource Manager-(ORM-)Stack herunterladen, die zum Einrichten der für die Ausführung dieses Workshops erforderlichen Ressource erforderlich ist. Anschließend erstellen Sie eine Compute-Instanz und ein virtuelles Cloud-Netzwerk (VCN), über das Sie auf einen Remotedesktop zugreifen können.

Geschätzte Zeit: 10 Minuten

### Ziele

*   ORM-Stack herunterladen
*   Compute + Networking mit Resource Manager Stack erstellen

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier- oder kostenpflichtige Cloud-Accounts von Oracle

## Aufgabe 1: ZIP-Datei für Oracle Resource Manager-(ORM-)Stack herunterladen

1.  Klicken Sie auf den folgenden Link, um die Resource Manager-ZIP-Datei herunterzuladen, die Sie zum Erstellen Ihrer Umgebung benötigen:
    
    _Hinweis 1:_ Wenn Sie einen einzelnen Stack-Download für den Workshop bereitstellen, verwenden Sie diesen einfachen Ausdruck.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  Speichern Sie in Ihrem Download-Ordner.
    

## Aufgabe 2: Stack erstellen: Compute + Networking

1.  Identifizieren Sie die in **Aufgabe 1: ZIP-Datei für den Oracle Resource Manager-(ORM-)Stack herunterladen** heruntergeladene ORM-Stackdatei.
    
2.  Öffnen Sie das Hamburger-Menü oben links. Klicken Sie auf **Entwicklerservices**, und wählen Sie **Resource Manager** > **Stacks**. Wählen Sie das Compartment aus, in dem Sie den Stack installieren möchten. Klicken Sie auf **Stack erstellen**. ![Menüstapel](images/menu-stack.png) ![Compartment auswählen](images/select-compartment.png)
    
3.  Wählen Sie **Meine Konfiguration** und dann **. Schaltfläche "ZIP-Datei"**, klicken Sie auf den Link **Durchsuchen**, und wählen Sie die heruntergeladene ZIP-Datei oder ziehen Sie sie per Drag-and-Drop für den Datei-Explorer. Klicken Sie auf **Weiter**. ![Nach ZIP durchsuchen](images/browse-zip.png)
    
4.  Geben Sie Folgendes ein, oder wählen Sie es aus, und klicken Sie auf **Weiter**.
    
    **Instanzanzahl:** Übernehmen Sie den Standardwert 1.
    
    **Verfügbarkeitsdomain auswählen:** Wählen Sie eine Availability-Domain aus der Dropdown-Liste aus.
    
    **Benötigen Sie Remotezugriff über SSH?** Deaktivieren Sie "Nur Remote Desktop-Zugriff - Standard".
    
    **Flexible Instanzausprägung mit anpassbarer OCPU-Anzahl verwenden?:** Behalten Sie den Standardwert bei (es sei denn, Sie möchten eine feste Ausprägung verwenden).
    
    **Instanzausprägung:** Behalten Sie den Standardwert bei, oder wählen Sie ihn aus der Liste der Flex-Ausprägungen im Dropdown-Menü aus (z.B. VM.Standard.E4). Flexibel).
    
    **Anzahl OCPUs pro Instanz auswählen:** Übernehmen Sie den angezeigten Standardwert. Beispiel: (1) stellt 1 OCPUs und 16 GB Arbeitsspeicher bereit.
    
    **Vorhandenes VCN verwenden?:** Übernehmen Sie den Standardwert, indem Sie diese Option nicht aktivieren. Dadurch wird ein neues VCN erstellt. ![Hauptkonfiguration](images/main-config.png) ![Instanzausprägung](images/instance-shape.png)
    
5.  Wählen Sie **Apply ausführen**, und klicken Sie auf **Erstellen**. ![Apply ausführen](images/run-apply.png)
    

Sie können jetzt mit der nächsten Übung fortfahren.

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Oktober 2023
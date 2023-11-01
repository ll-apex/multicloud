# Setup vorbereiten

## Einführung

In dieser Übung wird gezeigt, wie Sie die ZIP-Datei für den Oracle Resource Manager-(ORM-)Stack herunterladen, die zum Einrichten der für die Ausführung dieses Workshops erforderlichen Ressource erforderlich ist. Für diesen Workshop sind eine Compute-Instanz und ein virtuelles Cloud-Netzwerk (VCN) erforderlich.

Geschätzte Zeit: 10 Minuten

### Ziele

*   ORM-Stack herunterladen
*   Vorhandenes virtuelles Cloud-Netzwerk (VCN) konfigurieren

### Voraussetzungen

In dieser Übung wird Folgendes vorausgesetzt:

*   Free Tier- oder kostenpflichtige Cloud-Accounts von Oracle

## Aufgabe 1: ZIP-Datei für Oracle Resource Manager-(ORM-)Stack herunterladen

1.  Klicken Sie auf den folgenden Link, um die Resource Manager-ZIP-Datei herunterzuladen, die Sie zum Erstellen Ihrer Umgebung benötigen:
    
    _Hinweis 1:_ Wenn Sie einen einzelnen Stack-Download für den Workshop bereitstellen, verwenden Sie diesen einfachen Ausdruck.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  Speichern Sie in Ihrem Download-Ordner.
    

Wir empfehlen dringend, mit diesem Stack ein eigenständiges/dediziertes VCN mit Ihren Instanzen zu erstellen. Gehen Sie zu _Aufgabe 3_, um unsere Empfehlungen zu befolgen. Wenn Sie lieber ein vorhandenes VCN verwenden möchten, fahren Sie wie unten angegeben mit dem nächsten Schritt fort, um das vorhandene VCN mit den erforderlichen Egress-Regeln zu aktualisieren.

## Aufgabe 2: Sicherheitsregeln zu einem vorhandenen VCN hinzufügen

Für diesen Workshop muss eine bestimmte Anzahl von Ports verfügbar sein. Dies ist eine Anforderung, die mit der ORM-Standardstackausführung erfüllt werden kann, die ein dediziertes VCN erstellt. Um ein vorhandenes VCN zu verwenden, müssen die folgenden Ports zu Ingress-Regeln hinzugefügt werden.

| Hafen | Beschreibung |
| :-- | :-- |
| 22 | SSH |
| 80 | noVNC Remotedesktop (NGINX-Proxy) |
| (6080) | noVNC Remotedesktop |

1.  Gehen Sie zu _Networking >> Virtuelle Cloud-Netzwerke_.
2.  Wählen Sie Ihr Netzwerk aus.
3.  Wählen Sie unter "Resources" die Option "Security Lists".
4.  Klicken Sie unter der Schaltfläche "Create Security List" auf "Default Security Lists".
5.  Klicken Sie auf die Schaltfläche "Add Ingress Rule".
6.  Geben Sie Folgendes ein:
    *   Quell-CIDR: 0.0.0.0/0
    *   Zielportbereich: _Siehe obige Tabelle_
7.  Klicken Sie auf die Schaltfläche "Add Ingress Rules".

## Aufgabe 3: Compute einrichten

Fahren Sie mit den Details aus den beiden oben genannten Aufgaben mit der Übung _Umgebungssetup_ fort, um Ihre Workshopumgebung mit Oracle Resource Manager (ORM) und einer der folgenden Optionen einzurichten:

*   Stack erstellen: _Compute + Networking_
*   Stack erstellen: _Nur Compute_ mit einem vorhandenen VCN, in dem Sicherheitslisten gemäß _Aufgabe 2_ oben aktualisiert wurden

Sie können jetzt mit der nächsten Übung fortfahren.

## Danksagungen

*   **Autor** - Rene Fontcha, LiveLabs Platform Lead, NA-Technologie
*   **Mitwirkende** - Meghana Banka
*   **Zuletzt aktualisiert am/um** - Rene Fontcha, LiveLabs Platform Lead, NA Technology, Februar 2022
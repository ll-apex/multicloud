# Ingress-Controller für Oracle Kubernetes-Cluster (OKE) bereitstellen

## Einführung

In dieser Übung installieren wir den _Traefik_ Ingress Controller. Später aktualisieren wir die _Ingress-Routen_, um auf die Anwendung und den Admin-Server zuzugreifen.

Geschätzte Zeit: 10 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Ingress Controller für OKE-Cluster bereitstellen](videohub:1_4kih00fi)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Installieren Sie Ingress Controller auf dem Kubernetes-Cluster.
*   Aktualisieren Sie die Ingress-Routen.

## Aufgabe 1: Ingress-Controller im Oracle Kubernetes-Cluster installieren

In dieser Aufgabe installieren wir den _Ingress Controller_.

1.  Klicken Sie auf _Ingress-Controller_. Einige vorab ausgefüllte Werte werden angezeigt, bleiben unverändert, und klicken Sie auf _Ingress-Controller installieren_. ![Ingress-Controller installieren](images/install-ingress-controller.png)
    
    > **Nur zu Ihrer Information:**  
    > Dadurch wird der Ingress-Controller _traefik-operator_ erfolgreich in Kubernetes-Namespace _traefik-ns_ installiert.
    
2.  Wenn _Ingress Controller-Installation abgeschlossen_ angezeigt wird, klicken Sie auf _OK_. ![Ingress-Controller installiert](images/ingress-controller-installed.png)
    

## Aufgabe 2: Ingress-Routen für den Zugriff auf die Anwendung aktualisieren

In dieser Aufgabe fügen wir die Ingress-Routen für den Zugriff auf die Admin Console, Anwendung, hinzu. Am Ende erhalten wir die zugänglichen Endpunkte.

1.  Scrollen Sie nach unten, und klicken Sie auf _+_, um die _Ingress-Routenkonfiguration_ hinzuzufügen. ![Ingress-Routen hinzufügen](images/add-ingress-routes.png)
    
2.  Klicken Sie auf das Symbol "Bearbeiten", um die Werte zu ändern. ![Ingress bearbeiten](images/edit-ingress.png)
    
3.  Geben Sie die folgenden Details ein, und klicken Sie auf _OK_.  
    Name: console  
    Pfadausdruck: /console  
    Zielservice-Namespace: test-domain-ns  
    Zielservice: test-domain-admin-server  
    Zielport: 7001  
    
    ![Konsoleningress](images/console-ingress.png)
    
4.  Fügen Sie auf ähnliche Weise auch die folgenden _opdemo_\-Ingress-Routen hinzu:  
    Name: opdemo  
    Pfadausdruck: /opdemo  
    Zielservice-Namespace: test-domain-ns  
    Zielservice: test-domain-cluster-cluster-1  
    Zielport: 8001  
    ![Opdemo-Ingress](images/opdemo-ingress.png)
    
5.  Fügen Sie auf ähnliche Weise auch die folgenden Ingress-Routen der _Remote-Konsole_ hinzu:  
    Name: remote-console  
    Pfadausdruck: /  
    Zielservice-Namespace: test-domain-ns  
    Zielservice: test-domain-admin-server  
    Zielport: 7001  
    ![Ingress der Remotekonsole](images/remote-console-ingress.png)
    
6.  Um die Ingress-Routen zu aktualisieren, klicken Sie auf _Ingress-Routen aktualisieren_. ![Ingress-Routen aktualisieren](images/update-ingress-routes.png)
    
7.  Klicken Sie unter _Vorhandene Ingress-Routen aktualisieren_ auf _Ja_.
    
8.  Wenn das Fenster _Aktualisierung der Ingress-Routen abgeschlossen_ angezeigt wird, klicken Sie auf _OK_. ![Update-Ingress abgeschlossen](images/update-ingress-complete.png)
    
    > Sie müssen diese IP notieren und in Textdatei speichern.
    

## Danksagungen

*   **Autor** - Ankit Pandey
*   **Mitwirkende** - Maciej Gruszka, Sid Joshi
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, August 2023
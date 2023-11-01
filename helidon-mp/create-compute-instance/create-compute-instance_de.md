# Natives Dockerimage der Helidon-Anwendung in Compute-Instanz ausführen

## Einführung

In dieser Übung wird beschrieben, wie Sie ein Docking-Image aus der Oracle Container Image Registry abrufen und in einer virtuellen Maschine in Oracle Cloud Infrastructure ausführen.

Geschätzte Zeit: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Natives Dockerimage der Helidon-Anwendung in Compute-Instanz ausführen](videohub:1_dsfd22u5)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Compute-Instanz erstellen
*   Docker in Compute-Instanz installieren
*   Führen Sie den nativen Image-Docker-Container der Anwendung innerhalb der Compute-Instanz aus.

### Voraussetzungen

Um diese Übung ausführen zu können, benötigen Sie:

*   Oracle Cloud-Account
*   Mit einer Ressource zum Erstellen einer Compute-Instanz
*   In Container gepackte Helidon-Anwendung _myproject-your\_first\_name_ in einer Container-Registry verfügbar.

## Aufgabe 1: Compute-Instanz erstellen

1.  Klicken Sie in der Cloud-Konsole auf _Compute_ -> _Instanzen_. ![Compute-Instanzen](images/compute-instance.png)
    
2.  Wählen Sie das korrekte Compartment aus, und klicken Sie auf _Instanz erstellen_. ![Instanz erstellen](images/create-instance.png)
    
3.  Select the following values and click _Create_.  
    **Name:** Leave default.  
    **Create in compatment** Select your own compartment. **Image:** Select _Oracle Linux 8_ image.  
    **Shape:** Select the shape **VM.Standard.E4.Flex** and then select 1 OCPU with 16GB memory.  
    **Primary network:** select _Create new virtual cloud network_ and leave default values.  
    **Subnet:** select _Create new public subnet_ and leave default values.  
    **Public IP address:** select _Assign a public IPv4 address_.  
    **Add SSH keys:** select _Generate a key pair for me_ and click _Save private key_ and _Save public key_ to save the key pair in your local machine. you will need to copy the private key to Code Editor later.
    
4.  Nachdem die Instanz erstellt wurde, klicken Sie auf _Kopieren_, um die öffentliche IP der Instanz zu kopieren. ![IP kopieren](images/copy-ip.png)
    

## Aufgabe 2: Docker auf Compute-Instanz installieren

1.  Führen Sie im Terminal im Codeeditor den folgenden Befehl aus, um einen Private Key zu erstellen.
    
        <copy>vi ~/opc.key</copy>
        
2.  Drücken Sie _i_, um in den Einfügemodus zu wechseln und den Inhalt des Private Keys einzufügen, den Sie in Aufgabe 1 dieser Übung heruntergeladen haben. Drücken Sie die _Querformat_\-Taste, und geben Sie dann _:wq_ ein, um den Inhalt der Datei zu speichern.
    
3.  Ändern Sie die Dateiberechtigung, indem Sie den folgenden Befehl im Terminal ausführen.
    
        <copy>chmod 600 ~/opc.key</copy>
        
4.  Führen Sie den folgenden Befehl mit Ihrer eigenen _PUBLIC-IP_ aus, um eine Verbindung zur gerade erstellten Compute-Instanz herzustellen.
    
        <copy>ssh -i ~/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        
    
    ![SSH-OPC](images/ssh-opc.png)
    
    > **Im kostenlosen Tier-Account ist der _.ssh_\-Ordner standardmäßig nicht vorhanden, sodass Sie einen anderen als den Screenshot ausgegeben haben.**
    
5.  Führen Sie den folgenden Befehl aus, um Docker als Root-Benutzer in der Compute-Instanz zu installieren und um die _opc_\-Benutzerfunktion zur Ausführung von Docker bereitzustellen.
    
        <copy>sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
        sudo dnf remove -y runc
        sudo dnf install -y docker-ce --nobest
        sudo systemctl enable docker.service
        sudo systemctl start docker.service
        sudo usermod -aG docker opc
        sudo reboot</copy>
        
6.  Warten Sie 1-2 Minuten, bis der Rechner neu gestartet und erneut eine Verbindung zur Compute-Instanz hergestellt wird, indem Sie den folgenden Befehl ausführen:
    
        <copy>ssh -i ~/.ssh/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        

## Aufgabe 3: Begrüßungs-App in der Compute-Instanz abrufen und ausführen

1.  Führen Sie den folgenden Befehl aus, um das Synchronisierungsimage aus der Oracle Container Image Registry abzurufen.
    
        <copy>docker pull ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    Sie erhalten eine ähnliche Ausgabe wie unten gezeigt. ![Image abrufen](images/docker-pull.png)
    
2.  Führen Sie den folgenden Befehl aus, um die Anwendung auszuführen:
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
3.  Sie können ein neues Terminal und SSH öffnen, um eine Instanz zu berechnen, und die folgenden Befehle ausführen, um die Anwendung auszuüben.
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        
    
    Herzlichen Glückwunsch, Sie haben das Helidon-Anwendungs-Deployment auf Compute-Instanz in Oracle Cloud Infrastructure abgeschlossen.
    

## Danksagungen

*   **Autor** - Dmitry Aleksandrov
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, April 2023
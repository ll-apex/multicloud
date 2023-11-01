# Exécuter l'image docker native de l'application Helidon dans une instance de calcul

## Présentation

Cet atelier vous guide tout au long du processus d'extraction d'une image docker à partir d'Oracle Container Image Registry et de son exécution dans une machine virtuelle dans Oracle Cloud Infrastructure.

Temps estimé : 15 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Exécution de l'image docker native de l'application Helidon dans l'instance de calcul](videohub:1_dsfd22u5)

### Objectifs

Dans cet exercice, vous allez :

*   Créer une instance de calcul
*   Installer Docker dans l'instance de calcul
*   Exécutez le conteneur docker d'image natif de l'application dans l'instance de calcul.

### Prérequis

Pour exécuter cet exercice, vous devez disposer des éléments suivants :

*   Compte Oracle Cloud
*   Disposer d'une ressource pour créer une instance de calcul
*   Application Helidon _myproject-your\_first\_name_ packagée par conteneur disponible dans un registre de conteneurs.

## Tâche 1 : créer une instance Compute

1.  Dans la console cloud, cliquez sur _Compute_ -> _Instances_. ![instances de calcul](images/compute-instance.png)
    
2.  Sélectionnez le compartiment approprié, puis cliquez sur _Créer une instance_. ![créer l'instance](images/create-instance.png)
    
3.  Select the following values and click _Create_.  
    **Name:** Leave default.  
    **Create in compatment** Select your own compartment. **Image:** Select _Oracle Linux 8_ image.  
    **Shape:** Select the shape **VM.Standard.E4.Flex** and then select 1 OCPU with 16GB memory.  
    **Primary network:** select _Create new virtual cloud network_ and leave default values.  
    **Subnet:** select _Create new public subnet_ and leave default values.  
    **Public IP address:** select _Assign a public IPv4 address_.  
    **Add SSH keys:** select _Generate a key pair for me_ and click _Save private key_ and _Save public key_ to save the key pair in your local machine. you will need to copy the private key to Code Editor later.
    
4.  Une fois l'instance créée, cliquez sur _Copier_ pour copier l'adresse IP publique de l'instance. ![copier l'adresse IP](images/copy-ip.png)
    

## Tâche 2 : installer docker sur l'instance de calcul

1.  Dans le terminal de l'éditeur de code, exécutez la commande suivante pour créer une clé privée.
    
        <copy>vi ~/opc.key</copy>
        
2.  Appuyez sur _i_ pour passer en mode insertion et coller le contenu de la clé privée que vous avez téléchargée dans la tâche 1 de cet exercice. Appuyez sur la touche _escape_, puis saisissez _:wq_ pour enregistrer le contenu du fichier.
    
3.  Modifiez l'autorisation du fichier en exécutant la commande suivante dans le terminal.
    
        <copy>chmod 600 ~/opc.key</copy>
        
4.  Exécutez la commande suivante avec votre propre _adresse IP publique_ pour vous connecter à l'instance de calcul que vous venez de créer.
    
        <copy>ssh -i ~/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        
    
    ![ssh opc](images/ssh-opc.png)
    
    > **Dans le compte Niveau gratuit, nous n'avons pas de dossier _.ssh_ par défaut. Vous obtiendrez donc une sortie différente de celle de la capture d'écran.**
    
5.  Exécutez la commande suivante pour installer docker en tant qu'utilisateur root dans l'instance de calcul et pour permettre à l'utilisateur _opc_ d'exécuter docker.
    
        <copy>sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
        sudo dnf remove -y runc
        sudo dnf install -y docker-ce --nobest
        sudo systemctl enable docker.service
        sudo systemctl start docker.service
        sudo usermod -aG docker opc
        sudo reboot</copy>
        
6.  Attendez 1 à 2 minutes jusqu'à ce que la machine redémarre et se reconnecte à l'instance de calcul en exécutant la commande suivante :
    
        <copy>ssh -i ~/.ssh/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        

## Tâche 3 : extraire et exécuter l'application Greeting dans l'instance Compute

1.  Exécutez la commande suivante pour extraire l'image docker à partir d'Oracle Container Image Registry.
    
        <copy>docker pull ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    Vous obtiendrez un résultat similaire à celui indiqué ci-dessous. ![extraire l'image](images/docker-pull.png)
    
2.  Exécutez la commande suivante pour exécuter l'application :
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
3.  Vous pouvez ouvrir un nouveau terminal et SSH pour calculer une instance et exécuter les commandes suivantes pour exercer l'application.
    
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
        
    
    Félicitations pour votre déploiement d'application Helidon sur l'instance Compute dans Oracle Cloud Infrastructure.
    

## Accusés de réception

*   **Auteur** - Dmitry Aleksandrov
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, avril 2023
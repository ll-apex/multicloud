# Tester le déploiement d'application

## Présentation

### A propos de WebLogic Remote Console

WebLogic Remote Console est une console open source légère que vous pouvez utiliser pour gérer votre domaine de serveur WebLogic exécuté n'importe où, par exemple sur une machine physique ou virtuelle, dans un conteneur, Kubernetes ou dans Oracle Cloud. Il n'est pas nécessaire de colocaliser la console distante WebLogic avec le domaine de serveur WebLogic.

Vous pouvez installer et exécuter WebLogic Remote Console n'importe où, et vous connecter à votre domaine à l'aide des API REST WebLogic. Il vous suffit de lancer l'application de bureau et de vous connecter au serveur d'administration de votre domaine. Vous pouvez également démarrer le serveur de la console, lancer la console dans un navigateur, puis vous connecter au serveur d'administration.

La console distante WebLogic est entièrement prise en charge avec le serveur WebLogic 12.2.1.3, 12.2.1.4 et 14.1.1.0.

**Principales fonctionnalités de la console distante WebLogic**

*   Configurer des instances et des clusters de serveurs WebLogic
*   Créer ou modifier des modèles de métadonnées WDT
*   Configurer des services de serveur WebLogic, tels que la connectivité à la base de données (JDBC) et la messagerie (JMS)
*   Déployer et annuler le déploiement des applications
*   Démarrer et arrêter des serveurs et des applications
*   Surveiller les performances de serveur et d'application

Au cours de cet exercice, nous accédons à l'application _opdemo_ et vérifions que la migration d'un domaine sur site hors ligne a réussi. Nous vérifions également l'équilibrage de charge entre les pods de serveur géré. Ensuite, nous utiliserons WebLogic Remote Console pour vérifier que le déploiement des ressources du domaine de test dans l'environnement kubernetes a réussi.

Temps estimé : 10 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Test du déploiement d'application](videohub:1_1khcsrbq)

### Objectifs

Dans cet exercice, vous allez :

*   Accédez à l'application via le navigateur.
*   Explorez le domaine WebLogic à l'aide de WebLogic Remote Console.

## Tâche 1 : accéder à l'application via le navigateur

Dans cette tâche, nous accédons à l'application _opdemo_. Cliquez sur l'icône d'actualisation pour effectuer plusieurs demandes à l'application afin de vérifier l'équilibrage de charge entre deux pods de serveur géré.

1.  Copiez l'URL ci-dessous et remplacez _XX.XX.XX.XX_ par votre adresse IP, que vous avez notée au cours du dernier exercice. Vous pouvez voir la sortie ci-dessous.
    
        <copy>http://XX.XX.XX.XX/opdemo/?dsname=testDatasource</copy>
        
    
    ![Ouvrir l'application](images/open-application.png)
    
2.  Si vous cliquez sur l'icône Actualiser, l'équilibrage de charge entre deux pods de serveur géré s'affiche. ![Afficher l'équilibrage de charge](images/show-load-balancing.png)
    

## Tâche 2 : exploration du domaine WebLogic sur un cluster Kubernetes à l'aide de WebLogic Remote Console

Dans cette tâche, nous explorons la console distante WebLogic. Nous créons une connexion au _serveur d'administration_ dans Remote Console et vérifions les ressources dans le domaine WebLogic. Cela permet de vérifier que la migration d'un domaine sur site vers le cluster Oracle Kubernetes a réussi.

1.  Pour ouvrir WebLogic Remote Console, cliquez sur _Activités_, saisissez _WebLogic_ dans la zone de recherche et cliquez sur l'icône _WebLogic Remote Console_.
    
2.  Cliquez sur `Three dots` sous _Kiosk_ et sélectionnez _Ajouter un fournisseur de connexion de serveur d'administration_, puis cliquez sur _Choisir_. ![Connexion au serveur d'administration](images/adminserver-connection.png)
    
3.  Entrez les données suivantes et cliquez sur _OK_.  
    Nom du fournisseur de connexions : AdminServer  
    Nom utilisateur : weblogic  
    Mot de passe : welcome1  
    URL : `Copy_IP_From_TextFile`  
    ![Détails de connexion](images/connection-details.png)
    
4.  Cliquez sur l'icône _Modifier l'arborescence_, puis sélectionnez _Services_ -> _Sources de données_. Vous pouvez observer le même Datasouce, que nous avions vu dans le domaine sur site. ![Vérifier les origines de données](images/verify-datasources.png)
    
5.  Pour afficher les serveurs en cours d'exécution dans votre domaine. Cliquez sur l'icône **Arborescence de surveillance** comme indiqué, puis sélectionnez **Environnement** -> **Serveurs**. Vous pouvez constater que **Admin Server** et 2 pods Managed Server sont en cours d'exécution. ![Serveurs en cours d'exécution](images/running-server-status.png)
    
6.  Cliquez sur **admin-server** pour voir que la version de WebLogic est **12.2.1.3.0**. ![Statut du serveur](images/wls-version.png)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
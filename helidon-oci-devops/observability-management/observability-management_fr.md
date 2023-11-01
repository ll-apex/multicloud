# Explorer les journaux et l'explorateur de mesures

## Présentation

Dans cet exercice, vous allez vérifier la réussite du déploiement de l'application Helidon. Vous explorerez également le service d'explorateur de journaux et de mesures.

Durée estimée : 5 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Explorer l'explorateur de journaux et de mesures](videohub:1_7a0qaaif)

### Objectifs

Dans cet exercice, vous allez :

*   Vérifiez que le déploiement de l'application Helidon a réussi.
*   Explorer les mesures OCI
*   Explorer le service OCI Logging
*   Valider la vivacité et la préparation de l'application Helidon

### Prérequis

*   Un compte cloud Oracle Free Tier (essai), payant ou LiveLabs
*   Connaissance de la console OCI

## Tâche 1 : accéder à l'application Helidon

Dans cette tâche, nous accédons à l'application à l'aide de curl pour effectuer des demandes HTTP GET et PUT.

1.  Revenez à l'**éditeur de code**, copiez et collez la commande suivante dans le terminal pour configurer le noeud de déploiement **`PUBLIC_IP`** en tant que variable d'environnement.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Copiez et collez la commande suivante pour placer une **demande GET**. Vous obtiendrez un résultat similaire à celui indiqué ci-dessous.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  Bonjour à un nom, à savoir **Joe**. Vous obtiendrez un résultat similaire à celui indiqué ci-dessous.
    
        <copy>curl http://$PUBLIC_IP:8080/greet/Joe</copy>
        {"message":"Hello Joe!","date":[2023,5,23]}
        
4.  Remplacez Hello par un autre mot de bienvenue, à savoir **Hola**. Vous obtiendrez un résultat similaire à celui indiqué ci-dessous.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,23]}
        

## Tâche 2 : explorer OCI Metrics Explorer

Dans cette tâche, vous allez vérifier que les **mesures Helidon** sont propagées vers le **service OCI Monitoring**.

1.  Dans la console cloud, cliquez sur _Menu Hamburger_ -> _Observation & gestion_ -> _Explorateur de mesures_ sous **Surveillance** comme indiqué ci-dessous. ![explorateur de mesures](images/metrics-explorer.png)
    
2.  Faites défiler la page vers le bas pour accéder à la section **Requête**, sélectionnez le compartiment approprié, puis sélectionnez **`helidon_metrics`** en tant qu'espace de noms de mesures et **`request.count_counter`** en tant que nom de mesure pour créer la requête. Cliquez ensuite sur _Mettre à jour le graphique_ comme indiqué ci-dessous. ![créer une requête](images/create-query.png)
    
3.  Faites défiler l'affichage vers le haut et vous verrez un résultat similaire comme indiqué ci-dessous. Cela vous fournit des données au format Graphique pour le compteur de demandes. ![éditeur de requête](images/query-editor.png)
    
4.  Pour afficher les données dans la table de données, basculez le bouton **Afficher la table de données** comme indiqué ci-dessous. Lorsque vous basculez le bouton, _Afficher la table de données_ est remplacé par **Afficher le graphique**. ![table de données](images/data-table.png)
    

## Tâche 3 : exploration du service OCI Logging

Dans cette tâche, nous validons l'intégration du **kit SDK de journalisation OCI** pour transmettre des messages au service Logging en explorant le journal **app-log-helidon-ocw-hol** sur le groupe de journaux **app-log-group-helidon-ocw-hol**.

1.  Dans la console cloud, cliquez sur _Menu Hamburger_ -> _Observation & gestion_ -> _Journaux_. ![menu des journaux](images/logs-menu.png)
    
2.  Sélectionnez le compartiment approprié, puis sélectionnez _`app-log-group-helidon-ocw-hol`_ en tant que **groupe de journaux**, puis cliquez sur **app-log-helidon-ocw-hol** comme indiqué ci-dessous. ![sélectionner un compartiment](images/select-compartment.png)
    
3.  Dans **Filtrer par heure**, sélectionnez **Aujourd'hui** dans la liste déroulante et vous pouvez voir les journaux d'application. ![visualiser les journaux](images/view-logs.png)
    

## Tâche 4 : vérification de l'état de l'application Helidon pour valider la vivacité et la préparation

L'application **oci-mp** Helidon ajoute une fonctionnalité de **vérification de l'état** pour valider la **vérité** et/ou la **préparation**. Vous pouvez vérifier les fichiers de classe _GreetLivenessCheck_ et _GreetReadinessCheck_ respectivement dans le projet pour voir comment ils sont réalisés. Cela sera particulièrement utile lors de l'exécution de l'application en tant que microservice sur un environnement d'orchestrateur tel que Kubernetes pour déterminer si le microservice doit être redémarré s'il n'est pas en bon état. Spécifique à cet exercice, la vérification de la préparation est exploitée dans la spécification de pipeline de déploiement DevOps après le démarrage de l'application _pour déterminer si l'application Helidon a démarré avec succès_. Consultez le code de la _ligne 79_ dans **deployment\_spec.yaml** pour le voir en action.

1.  Revenez à l'**éditeur de code**, copiez et collez la commande suivante dans le terminal pour configurer le noeud de déploiement **`PUBLIC_IP`** en tant que variable d'environnement.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Copiez et collez la commande suivante pour vérifier la **livité**. Vous obtiendrez un résultat similaire à celui indiqué ci-dessous.
    
        <copy>curl http://$PUBLIC_IP:8080/health/live</copy>
        {"status":"UP","checks":[{"name":"CustomLivenessCheck","status":"UP","data":{"time":1684391639448}}]}
        
3.  Copiez et collez la commande suivante pour vérifier la **préparation**. Vous obtiendrez un résultat similaire à celui indiqué ci-dessous.
    
        <copy>curl http://$PUBLIC_IP:8080/health/ready</copy>
        {"status":"UP","checks":[{"name":"CustomReadinessCheck","status":"UP","data":{"time":1684391438298}}]}
        

Vous pouvez maintenant **passer à l'exercice suivant**.

## En savoir plus

*   [Application OCI avec Helidon](https://medium.com/helidon/oci-application-with-helidon-caa78cacaee5)
*   [Mesures MP Helidon](https://helidon.io/docs/v3/#/mp/metrics/metrics)
*   [Guide des mesures Helidon MP](https://helidon.io/docs/v3/#/mp/guides/metrics)
*   [Etat Helidon MP](https://helidon.io/docs/v3/#/mp/health)
*   [Guide de vérification de l'état Helidon MP](https://helidon.io/docs/v3/#/mp/guides/health)

## Accusés de réception

*   **Auteur** - Keith Lustria
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
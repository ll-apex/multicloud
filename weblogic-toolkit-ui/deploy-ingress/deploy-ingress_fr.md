# Déployer le contrôleur entrant vers le cluster Oracle Kubernetes (OKE)

## Présentation

Dans cet exercice, nous installons le contrôleur d'entrée _Traefik_. Par la suite, nous mettons à jour les _routages entrants_ pour accéder à l'application et au serveur d'administration.

Temps estimé : 10 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Déploiement du contrôleur entrant vers le cluster OKE](videohub:1_4kih00fi)

### Objectifs

Dans cet exercice, vous allez :

*   Installez le contrôleur d'entrée sur le cluster Kubernetes.
*   Mettez à jour les routages entrants.

## Tâche 1 : installation du contrôleur entrant sur le cluster Oracle Kubernetes

Dans cette tâche, nous installons le _contrôleur d'entrée_.

1.  Cliquez sur _Contrôleur entrant_. Vous pouvez voir certaines valeurs préremplies, les laisser rester identiques et cliquer sur _Installer le contrôleur d'entrée_. ![Installer le contrôleur d'entrée](images/install-ingress-controller.png)
    
    > **Pour information uniquement :**  
    > Cette opération a réussi à installer le contrôleur d'entrée _traefik-operator_ dans l'espace de noms Kubernetes _traefik-ns_.
    
2.  Une fois l'_installation du contrôleur d'entrée terminée_, cliquez sur _OK_. ![Contrôleur d'entrée installé](images/ingress-controller-installed.png)
    

## Tâche 2 : mise à jour des routages entrants pour accéder à l'application

Dans cette tâche, nous ajoutons les routages entrants pour accéder à la console d'administration, Application. A la fin, nous obtenons les adresses accessibles.

1.  Faites défiler la page vers le bas et cliquez sur _+_ icone pour ajouter la _configuration de routage entrant_. ![Ajouter des routages entrants](images/add-ingress-routes.png)
    
2.  Cliquez sur l'icône Modifier comme indiqué pour modifier les valeurs. ![Modifier l'entrée](images/edit-ingress.png)
    
3.  Entrez les détails suivants et cliquez sur _OK_.  
    Nom : console  
    Expression de chemin : /console  
    Espace de noms du service cible : test-domain-ns  
    Service cible : test-domain-admin-server  
    Port cible : 7001  
    
    ![Entrée de console](images/console-ingress.png)
    
4.  De la même manière, ajoutez les routes entrantes _opdemo_ suivantes :  
    Nom : opdemo  
    Expression de chemin : /opdemo  
    Espace de noms du service cible : test-domain-ns  
    Service cible : test-domain-cluster-cluster-1  
    Port cible : 8001  
    ![Entrée Opdemo](images/opdemo-ingress.png)
    
5.  De la même manière, ajoutez les routages entrants _remote-console_ suivants :  
    Nom : remote-console  
    Expression de chemin : /  
    Espace de noms de service cible : test-domain-ns  
    Service cible : test-domain-admin-server  
    Port cible : 7001  
    ![Entrée de la console distante](images/remote-console-ingress.png)
    
6.  Pour mettre à jour les routages entrants, cliquez sur _Mettre à jour les routages entrants_. ![Mettre à jour les routages entrants](images/update-ingress-routes.png)
    
7.  Dans _Mettre à jour les routages entrants existants_, cliquez sur _Oui_.
    
8.  Une fois la fenêtre _Mise à jour des routages entrants terminée_, cliquez sur _OK_. ![Mise à jour en entrée terminée](images/update-ingress-complete.png)
    
    > Vous devez noter cette adresse IP et l'enregistrer dans un fichier texte.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
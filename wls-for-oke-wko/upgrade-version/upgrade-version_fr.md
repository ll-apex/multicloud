# Effectuer une mise à niveau dans la version du serveur WebLogic (facultatif)

## Présentation

Dans cet exercice, nous modifions l'image principale. Nous utilisons l'image de serveur WebLogic avec la balise _12.2.1.4-slim-ol8_. Ensuite, redéployez le domaine à l'aide de l'interface utilisateur Kubernetes Toolkit WebLogic. Enfin, nous vérifions que les nouveaux pods de serveur géré utilisent les images de serveur WebLogic mises à jour via WebLogic Remote Console.

Temps estimé : 10 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Utilisez l'image du serveur WebLogic (12.2.1.4) en tant qu'image principale.
*   Redéployez le domaine WebLogic.

### Prérequis

*   Accès à noVNC Remote Desktop créé dans l'exercice 1.

## Tâche 1 : entrer les détails de la nouvelle image de serveur WebLogic en tant qu'image principale

Dans cette tâche, nous mettons à jour l'image principale pour qu'elle utilise l'image WebLogic Server 12.2.1.4.0 mise à niveau.

1.  Revenez à l'interface utilisateur du kit d'outils Kubernetes WebLogic, cliquez sur _Domaine WebLogic_. La balise de serveur WebLogic a été remplacée par _12.2.1.4-slim-ol8_. ![Mettre à jour la balise d'image principale](images/update-primary-image-tag.png)

## Tâche 2 : mettre à jour une application déployée par un redémarrage non simultané des pods du serveur

Dans cette tâche, nous redéployons le domaine WebLogic. Nous utiliserons ensuite la console distante WebLogic pour vérifier que les pods de serveur utilisent l'image WebLogic Server 12.2.1.4.0 mise à jour.

1.  Cliquez sur _Déployer le domaine_. Cela va redéployer le domaine. ![Redéployer le domaine](images/redeploy-domain.png)
    
    > **Pour information uniquement :**  
    > A mesure que nous modifions notre image principale, nous remarquons un redémarrage non simultané des serveurs un par un. Lorsque vous cliquez sur _Déployer le domaine_, il démarre un _travail d'introspecteur_, qui met fin aux pods du serveur d'administration en cours d'exécution, et crée un pod pour le serveur d'administration qui utilise l'image WebLogic Server 12.2.1.4.0. Introspector effectue le même processus avec les deux serveurs gérés.
    
2.  Une fois la fenêtre _Déploiement de domaine WebLogic vers Kubernetes terminé_ affichée, cliquez sur _OK_. ![Déploiement terminé](images/deployment-complete.png)
    
3.  Revenez à _Terminal_ et copiez la commande ci-dessous et collez-la dans le terminal. Vous remarquerez le redémarrage non simultané des serveurs un par un. Tout d'abord, les pods du serveur d'administration se terminent et prennent l'état _En cours d'exécution_.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Afficher les pods](images/view-pods.png)
    
4.  Pour vérifier que les pods de serveur d'administration et de serveur géré utilisent l'image de serveur WebLogic mise à jour, cliquez sur l'icône de l'arborescence de surveillance, puis sélectionnez _Environnement_ -> _Serveurs_ -> _admin-server_. Vous pouvez voir, il utilise 12.2.1.4.0. ![Version WLS](images/wls-version.png)
    

Congratulation !!!

C'est la fin de l'atelier.

Nous espérons que vous avez trouvé cet atelier utile.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, octobre 2023
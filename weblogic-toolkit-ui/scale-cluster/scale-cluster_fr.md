# Redimensionnement d'un cluster WebLogic (facultatif)

## Présentation

Dans cet exercice, nous redimensionnons un cluster WebLogic. Ici, nous modifions la valeur sur _3_ et redéployons le domaine.

Durée estimée : 5 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Redimensionnement d'un cluster WebLogic](videohub:1_mcl3p6td)

### Objectifs

Dans cet exercice, vous allez :

*   Redimensionnez un cluster WebLogic.

## Tâche 1 : redimensionnement d'un cluster WebLogic à l'aide de l'interface utilisateur WebLogic Kubernetes Toolkit

Dans cette tâche, il vous suffit de modifier la valeur _Replica_ de 2 à 3 et de redéployer le domaine.

1.  Revenez à l'interface utilisateur Kubernetes Toolkit WebLogic et cliquez sur _Domaine WebLogic_. Accédez à la section _Clusters_ et cliquez sur l'icône _Modifier_.  
    ![Redimensionnement de cluster](images/cluster-resize.png)
    
2.  Faites passer les répliques de _2_ à _3_, puis cliquez sur _OK_. ![Modifier les répliques](images/change-replicas.png)
    
3.  Pour redéployer le domaine, cliquez sur _Déployer le domaine_. ![Redéployer le domaine](images/redeploy-domain.png)
    
4.  Une fois la fenêtre _Déploiement de domaine WebLogic vers Kubernetes terminé_ affichée, cliquez sur _OK_. ![Déploiement terminé](images/deployment-complete.png)
    
5.  Revenez à la fenêtre _Terminal_, cliquez sur _Activities_ et sélectionnez la fenêtre _Terminal_. Copiez la commande suivante et collez-la dans le terminal.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Voir l'évolutivité](images/view-scaling.png)
    
    > Vous pouvez voir, le redéploiement du domaine, démarre le travail d'introspecteur, qui démarre le processus de création du pod pour test-domain-managed-server3 et, dans un certain temps, ce pod passe au statut _En cours d'exécution_.
    
6.  Revenez au navigateur, où la page de l'application est ouverte. Cliquez sur le bouton Actualiser pour voir l'équilibrage de charge entre trois serveurs gérés. ![nouveau serveur](images/new-server.png)
    
7.  Revenez à WebLogic Remote Console, cliquez sur _Monitoring Tree_ -> _Environment_ -> _Servers_. Vous remarquerez managed-server3 ici également. ![Console à distance](images/remote-console.png)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
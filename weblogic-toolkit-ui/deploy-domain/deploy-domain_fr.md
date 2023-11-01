# Déployer le domaine WebLogic vers le cluster Kubernetes Oracle (OKE)

## Présentation

Dans cet exercice, nous déployons le domaine WebLogic vers le cluster kubernetes. Dans la section d'image principale, nous indiquons les informations d'identification de compte oracle. Dans la section des images auxiliaires, nous indiquons les informations d'identification de compte oracle cloud. Ici, nous spécifions également la réplique pour le cluster.

Temps estimé : 10 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Déploiement du domaine WebLogic vers le cluster OKE](videohub:1_wz94de1l)

### Objectifs

Dans cet exercice, vous allez :

*   Déployez le domaine WebLogic sur le cluster Kubernetes.

## Tâche 1 : déploiement du domaine WebLogic sur le cluster Oracle Kubernetes

Dans cette tâche, nous déployons la ressource personnalisée Kubernetes pour le domaine WebLogic vers le cluster Kubernetes.

1.  Faites défiler la page vers le bas, entrez les informations suivantes dans la section Image principale, entrez _domain-secret_ en tant que _nom de clé secrète d'extraction d'image_, et utilisez le nom utilisateur et le mot de passe du compte Oracle dans _Nom utilisateur d'extraction du registre d'images_ et _Mot de passe d'extraction du registre d'images_. Entrez votre ID courriel Oracle dans _Adresse électronique d'extraction du registre des images_. Il s'agit des mêmes informations d'identification que celles utilisées pour accepter la licence des images _weblogic_ dans Oracle Container Registry. ![Détails de l'image principale](images/primary-image-details.png)
    
    > **Pour information uniquement :**  
    > nous extrayons l'image à partir d'Oracle Container Registry. Nous indiquons donc les informations d'identification, que nous avons utilisées pour accepter le contrat de licence pour les images de serveur WebLogic.
    
2.  Faites défiler la page vers le bas, dans la section Image auxiliaire, décochez la case **Spécifier les informations d'identification d'extraction d'image auxiliaire**.
    
3.  Dans la section _Clusters_, cliquez sur l'icône _Modifier_ comme indiqué. ![Redimensionnement de cluster](images/cluster-resize.png)
    
4.  Entrez _2_ en tant que _répliques_, puis cliquez sur _OK_. La taille de la réplique détermine le nombre de serveurs gérés dans l'état _En cours d'exécution_ après le déploiement réussi du domaine WebLogic vers le cluster Kubernetes. ![Répliques de cluster](images/cluster-replicas.png)
    
5.  Dans la section Sources de données, cliquez deux fois pour modifier les _mots de passe_ de deux sources de données. Vous pouvez indiquer _tiger_ comme mot de passe dans les deux sources de données. Une fois que vous avez terminé, cliquez sur _Déployer le domaine_. ![Mot de passe de l'origine des données](images/datasource-password.png)
    
    > Ce déploiement déploie le domaine de test WebLogic vers l'espace de noms Kubernetes _test-domain-ns_.
    
6.  Une fois la fenêtre _Déploiement de domaine WebLogic vers Kubernetes terminé_ affichée, cliquez sur _OK_. ![Déploiement terminé](images/deployment-complete.png)
    
7.  Revenez au terminal, cliquez sur _Activités_ et sélectionnez la fenêtre _Terminal_. Copiez la commande suivante et collez-la sur le terminal. Vous devriez voir la sortie similaire, où le pod pour introspector s'exécute d'abord pour le serveur d'administration et les pods ultérieurs pour le serveur géré passent à l'état _En cours d'exécution_.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Statut de pod](images/pod-status.png)
    
8.  Vous pouvez également obtenir le statut du domaine via l'_interface utilisateur WebLogic Kubernetes Toolkit_. Revenez à l'_interface utilisateur WebLogic Kubernetes Toolkit_ et cliquez sur _Obtenir le statut du domaine_. ![Statut du domaine](images/domain-status.png)
    
9.  Dans la fenêtre Domain Status, faites défiler la page vers le bas pour voir le statut de tous les pods de serveur, puis cliquez sur _OK_. ![Statut du serveur](images/server-status.png)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
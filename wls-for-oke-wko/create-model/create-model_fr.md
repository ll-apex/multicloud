# Modifier un projet d'interface utilisateur WKT et créer un fichier de modèle

## Présentation

Dans cet atelier, nous explorons le domaine WebLogic on-premise. Nous naviguons dans la console d'administration pour afficher l'application déployée, les sources de données et les serveurs dans _test-domain_. Nous ouvrons également le fichier _`base_project.wktproj`_ pré-créé, qui contient déjà des valeurs préremplies pour la section _Paramètres de projet_. Ensuite, nous créons le fichier de modèle, par introspection d'un domaine sur site hors ligne. Enfin, nous validons le modèle et préparons le modèle à déployer sur Oracle Kubernetes Cluster (OKE).

Temps estimé : 15 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Explorez le domaine WebLogic sur site _test-domain_.
*   Ouvrez le projet WKT de base.
*   Introspection d'un domaine sur site hors ligne.
*   Valider et préparer le modèle.

### Prérequis

Pour exécuter cet exercice, vous devez disposer des éléments suivants :

*   Accès à noVNC Remote Desktop créé dans l'exercice 2.

## Tâche 1 : Explorer le domaine sur site

Dans cette tâche, nous parcourons les ressources dans _test-domain_ sur site à l'aide de la console d'administration WebLogic.

1.  Sur le côté gauche, cliquez sur _Icône Flèche_. ![Presse-papiers](images/clipboard.png)

> **Important :** vous pouvez voir le _papier_, pour copier-coller entre l'ordinateur hôte et le bureau distant, nous utilisons le _papier_. Par exemple, si vous voulez copier à partir de l'ordinateur hôte et que vous voulez le coller à l'intérieur du bureau distant, vous devez d'abord le coller dans le presse-papiers, puis vous pouvez le coller dans le bureau distant. Cliquez à nouveau sur _Icône Flèche_ pour masquer l'option _Paramètres_.

2.  Cliquez sur l'onglet _Oracle WebLogic Server_ et entrez _weblogic/Welcome1%_ en tant que `Username/Password`, puis cliquez sur _Connexion_. Vous pouvez voir que nous avons WebLogic Server version _12.2.1.3.0_.  
    ![Console d'administration de connexion](images/login-admin-console.png)
    
3.  Pour afficher les serveurs disponibles, développez _Environnement_ et cliquez sur _Serveurs_. Vous pouvez voir, nous avons un cluster dynamique avec 5 serveurs gérés. ![Visualiser les serveurs](images/view-servers.png)
    
4.  Pour afficher les sources de données, développez _Services_ et cliquez sur _Sources de données_. ![Voir les origines de données](images/view-datasources.png)
    
5.  Pour visualiser l'application déployée, cliquez sur _Déploiement_. Vous pouvez voir que nous avons _opdemo_ en tant qu'application déployée. ![Afficher les déploiements](images/view-deployments.png)
    

## Tâche 2 : Ouvrir le projet d'interface utilisateur WKT de base

Pour plus de simplicité, nous avons créé _`base_project.wktproj`_, qui prédéfinit l'emplacement de docker, Java, le répertoire de base Oracle et la balise d'image principale. Dans cette tâche, nous ouvrons le projet _`base_project.wktproj`_.

1.  Cliquez sur _Activités_, puis saisissez **WebLogic** dans la zone de recherche. Cliquez sur l'icône de l'_interface utilisateur WebLogic Kubernetes Toolkit_. ![Ouvrir WKTUI](images/open-wktui.png)
    
2.  Pour ouvrir le projet _base\_project.wktproj_, cliquez sur _Fichier_ -> _Ouvrir un projet_. ![Ouvrir un projet](images/open-project.png)
    
3.  Cliquez sur _Téléchargements_ dans la partie gauche, puis choisissez _base\_project.wktproj_ et cliquez sur _Ouvrir le projet_. ![Emplacement de projet](images/project-location.png)
    
    > **For your information only:**  
    > As _Credential Story Policy_, we select **Store in Native OS Credentials Store**. It means the credentials (like for WebLogic Server and datasources) are only stored on the local machine.  
    > For _Where would you like the target Oracle Fusion Middleware domain to live?_, we select **Created in the container from the model in the image**. In this case, the set of model-related files are added to the image. So when the WebLogic Kubernetes Operator domain object is deployed, its inspector process runs and creates the WebLogic Server domain inside a running container on-the-fly.  
    > ![Paramètres de projet](images/project-settings.png) As _Kubernetes Environment Target Type_, we select **WebLogic Kubernetes Operator**. This means, you want this domain to be deployed in Kubernetes managed by the WebLogic Kubernetes Operator. This settings also determine what sections and their associated actions within the application, to display.  
    > we also specify the location for _JAVA HOME_ and _ORACLE\_HOME_. WebLogic Kubernetes Toolkit UI uses this directory when invoking the WebLogic Deployer Tooling and WebLogic Image Tool.  
    > To build new images, inspect images and interact with image repositories, the WKT UI application uses an image build tool, which defaults to docker.  
    > ![Type de cluster Kubernetes](images/kubernetes-cluster-type.png)
    
4.  Entrez _welcome1_ en tant que **mot de passe**, puis cliquez sur _Déverrouiller_. ![déverrouiller](images/unlock.png)
    

## Tâche 3 : Introspection d'un domaine hors ligne sur site

Dans cette tâche, nous effectuons l'introspection d'un domaine sur site, ce qui crée un fichier modèle composé de la configuration du domaine.

1.  Dans l'interface utilisateur du kit d'outils Kubernetes WebLogic, cliquez sur _Modèle_. ![Modèle](images/click-model.png)
    
2.  Cliquez sur _Fichier_ -> _Ajouter un modèle_ -> _Repérer le modèle (hors ligne)_. ![Repérage du modèle](images/discover-model.png)
    
3.  Cliquez sur l'_icône_ Ouvrir le dossier pour ouvrir le _répertoire de base du domaine_. ![Domaine ouvert Hom](images/open-domain-home.png)
    
4.  Dans le dossier d'accueil, accédez au répertoire _`/home/opc/Oracle/Middleware/Oracle_Home/user_projects/domains/`_, sélectionnez le dossier _test-domain_, puis cliquez sur _Sélectionner_. Cliquez sur _OK_. ![Emplacement de navigation](images/navigate-location.png) ![Indiquer l'emplacement](images/specify-location.png)
    
    > Si vous regardez dans la console, vous verrez que l'outil de déploiement WebLogic est appelé pour analyser la configuration du domaine en mode hors ligne.
    
5.  Vous pouvez voir la fenêtre comme indiqué ci-dessous, à la fin, vous aurez un modèle prêt pour vous. ![Modèle de vue](images/view-model.png)
    
    > Le résultat de cette introspection WDT est model (représentation de métadonnées de votre configuration de domaine), placeholder, où vous pouvez spécifier les valeurs (comme le mot de passe pour la source de données) et l'application dans l'archive d'application.
    

## Tâche 4 : Valider et préparer le modèle

Dans cette tâche, nous validons le modèle et préparons le modèle à déployer sur le cluster Kubernetes Oracle (OKE).

1.  Cliquez sur _Vue de code_ et pour valider le modèle, cliquez sur _Valider le modèle_. ![Valider le modèle](images/validate-model.png)
    
    > **Pour information uniquement :**  
    > Valider le modèle appelle l'[outil Valider le modèle](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/validate/) WDT, qui vérifie que le modèle et ses artefacts associés sont bien formés et fournit de l'aide sur les attributs et sous-dossiers valides pour un emplacement de modèle particulier.
    
2.  Une fois que la fenêtre _Valider le modèle terminé_ s'affiche, cliquez sur _OK_. ![Validation terminée](images/validate-complete.png)
    
3.  Pour préparer le modèle à déployer sur le cluster Kubernetes, cliquez sur _Préparer le modèle_. ![Préparer le modèle](images/prepare-model.png)
    
    > **Pour information uniquement :**  
    > La préparation du modèle appelle l'[outil de préparation de modèle](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/prepare/) WDT pour modifier le modèle afin qu'il fonctionne dans un cluster Kubernetes avec WebLogic Kubernetes Operator ou Verrazzano installé.  
    > Prepare Model effectue les opérations suivantes :
    
    *   Supprime les sections de modèle et les champs qui ne sont pas compatibles avec l'environnement cible.
    *   Remplace les valeurs d'adresse par des jetons de modèle qui référencent des variables.
    *   Remplace les valeurs d'informations d'identification par des jetons de modèle qui référencent un champ dans une clé secrète Kubernetes ou une variable.
    *   Fournit des valeurs par défaut pour les champs affichés dans les variables de l'application, les remplacements de variable et les éditeurs de clé secrète.
    *   Extrait les informations de topologie vers l'application qu'il utilise pour générer le fichier de ressources utilisé pour déployer le domaine.
4.  Une fois que la fenêtre _Préparer le modèle terminé_ s'affiche, cliquez sur _OK_. ![Préparation terminée](images/prepare-complete.png)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, octobre 2023
# Configurer une instance de calcul

## Présentation

Cet atelier vous montrera comment configurer une pile **Oracle WebLogic Suite for OKE BYOL** qui générera les objets Oracle Cloud nécessaires à l'exécution de votre atelier.

Temps estimé : 15 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Générez un jeton d'authentification.
*   Créer une clé secrète dans un coffre
*   Créer une pile : Oracle WebLogic Suite pour OKE BYOL
*   Connexion à l'instance de calcul

### Prérequis

Cet exercice suppose que vous disposez des éléments suivants :

*   Un compte Oracle Cloud
*   Vous avez généré la paire de clés SSH.
*   Vous avez terminé : **Laboratoire : Préparer la configuration**

## Tâche 1 : générer un jeton d'authentification

Dans cette tâche, nous allons générer un _jeton d'authentification_. Au cours de l'exercice 5, nous utiliserons ce jeton d'authentification pour propager l'image auxiliaire dans le référentiel Oracle Cloud Container Registry. En outre, nous utilisons ce jeton d'authentification pour extraire les images de docker WebLogic vers Oracle Cloud Infrastructure Registry (également appelé Container Registry).

1.  Sélectionnez l'icône Utilisateur dans l'angle supérieur droit, puis sélectionnez _MyProfile_.
    
    ![Mon profil](images/my-profile.png)
    
2.  Faites défiler la page vers le bas et sélectionnez _Jetons d'authentification_, puis cliquez sur _Générer un jeton_.
    
    ![jeton d'authentification](images/auth-token.png)
    
3.  Copiez _`test-model-your_first_name`_ et collez-le dans la zone _Description_, puis cliquez sur _Générer un jeton_.
    
    ![Créer un jeton](images/create-token.png)
    
4.  Sélectionnez _Copier_ sous Jeton généré et collez-le dans votre fichier texte. Nous ne pourrons pas le copier plus tard. Cliquez sur _Fermer_.
    
    ![Copier le jeton](images/copy-token.png)
    
    > Dans la tâche suivante, nous stockerons ce jeton d'authentification dans **Secret**.
    

## Tâche 2 : créer une clé secrète dans un coffre

Les clés secrètes sont les informations d'identification telles que les mots de passe, les certificats, les clés SSH ou les **jetons d'authentification** que vous utilisez avec les services Oracle Cloud Infrastructure.

1.  Dans la console OCI, cliquez sur **Menu Hamburger** -> **Identité et sécurité** -> **Coffre**. ![coffre de menu](images/menu-vault.png)
    
2.  Sélectionnez le compartiment sous **Portée de la liste**, puis cliquez sur **Créer un coffre**.
    
3.  Entrez le nom du coffre et cliquez sur **Créer un coffre**. ![créer un coffre](images/create-vault.png)
    
4.  Une fois que le coffre que vous avez créé est à l'état **Actif**, cliquez sur le nom du coffre comme indiqué ci-dessous. ![nom de coffre](images/vault-name.png)
    
5.  Dans le coffre, cliquez sur **clés de cryptage maître**, puis sur **Créer une clé**. ![menu](images/menu-mek.png)
    
6.  Entrez le nom de la **clé**, puis cliquez sur **Créer une clé** comme indiqué ci-dessous. ![créer un mek](images/create-mek.png)
    
7.  Une fois que vous voyez l'état de la nouvelle clé de cryptage maître passer à **Activé**, cliquez sur **Clés secrètes** sous **Ressources**, comme indiqué ci-dessous. ![clés secrètes de menu](images/menu-secret.png)
    
8.  Cliquez sur **Créer une clé secrète**.
    
9.  Entrez le nom de la clé secrète et choisissez la nouvelle clé de cryptage, puis entrez le jeton d'authentification **Contenu de la clé secrète** comme indiqué ci-dessous. Cliquez sur **Créer une clé secrète**. ![créer une clé secrète](images/create-secret.png)
    

## Tâche 3 : créer une pile : Oracle WebLogic Suite pour OKE BYOL

1.  Dans la console OCI, cliquez sur **Menu Hamburger** -> **Marketplace** -> **Toutes les applications**. ![marché des menus](images/menu-marketplace.png)
    
2.  Saisissez **WebLogic OKE** dans la zone de recherche, puis cliquez sur **Oracle WebLogic Suite pour OKE BYOL**. ![pile de menus](images/menu-stack.png)
    
3.  Sélectionnez la dernière version disponible, choisissez votre compartiment et cochez la case pour accepter les conditions générales. Cliquez sur **Lancer la pile**. ![lancer la pile](images/launch-stack.png)
    
4.  Dans la section Informations sur la pile, conservez toutes les valeurs par défaut et cliquez sur **Suivant**. ![informations sur la pile](images/stack-info.png)
    
5.  Entrez **wko** comme préfixe de nom de ressource et cliquez sur Parcourir pour choisir la clé publique SSH. ![préfixe-ssh](images/prefix-ssh.png)
    
    > Veillez à utiliser **wko** comme préfixe, car nous l'utiliserons dans d'autres exercices.
    
6.  Dans l'intégration Verrazzano, ne cochez pas la case **Activer Verrazzano** par défaut. ![décocher verrazzano](images/uncheck-verrazzano.png)
    
7.  Dans Réseau, sélectionnez **Créer un VCN** en tant que stratégie de réseau cloud virtuel et conservez toutes les valeurs par défaut. ![réseau](images/network.png)
    
8.  Dans Configuration de cluster de conteneurs (OKE), sélectionnez les éléments suivants.
    
    **Version de Kubernetes :** conservez la version par défaut **v1.26.2**.
    
    **Forme de pool de noeuds non WebLogic (obligatoire) :** sélectionnez **VM.Standard.E4. Flex** en tant que forme et chooke **1** en tant que nombre d'OCPU et **16** en tant que quantité de mémoire.
    
    **Noeud dans NodePool pour les pods autres que WebLogic** - Laissez la valeur par défaut **1**.
    
    ![non WebLogic](images/non-weblogic.png)
    
    **Créer une forme de pool de noeuds WebLogic :** laissez la case par défaut cochée.
    
    **WebLogic Forme du pool de noeuds (obligatoire) :** sélectionnez **VM.Standard.E4. Flex** en tant que forme et chooke **1** en tant que nombre d'OCPU et **16** en tant que quantité de mémoire.
    
    **Noeud dans NodePool pour les pods WebLogic :** conservez la valeur par défaut **1**.
    
    ![WebLogic](images/weblogic-pool.png)
    
9.  Dans Administration Instances, entrez ou sélectionnez les éléments suivants comme indiqué. **Domaine de disponibilité pour les instances de calcul :** sélectionnez le domaine de disponibilité dans la liste déroulante.
    
    **Forme de calcul de l'instance d'administration (obligatoire) :** sélectionnez **VM.Standard.E4. Flex** en tant que forme et chooke **1** en tant que nombre d'OCPU et **16** en tant que quantité de mémoire.
    
    ![instance d'administrateur](images/admin-instance.png)
    
    **Forme d'instance de bastion (obligatoire) :** sélectionnez **VM.Standard.E4. Flex** en tant que forme et chooke **1** en tant que nombre d'OCPU et **16** en tant que quantité de mémoire.
    
    ![bastion](images/bastion.png)
    
10.  Dans la section Système de fichiers, sélectionnez les éléments suivants comme indiqué. **Domaine de disponibilité pour le système de fichiers :** sélectionnez le domaine de disponibilité dans la liste déroulante. ![fichier avd](images/file-avd.png)
    
11.  Dans la section Registry (OCIR), entrez les informations suivantes, comme indiqué.
    
    **Nom utilisateur de registre :** nom utilisateur utilisé par Kubernetes pour accéder au registre d'images de conteneur, au format {identity domain name}/{username}. Si votre location utilise Oracle Identity Cloud Service, utilisez le format oracleidentitycloudservice/{username}.
    
    **Compartiment du jeton d'authentification OCIR :** sélectionnez le compartiment dans lequel vous disposez du jeton d'authentification OCIR.
    
    **Clé secrète validée pour le jeton d'authentification OCIR :** clé secrète contenant le jeton d'authentification OCIR que vous avez généré pour que l'utilisateur puisse accéder au registre d'images.
    
    ![secret ocir](images/ocir-secret.png)
    
12.  Dans Stratégies OCI, cochez la case **Stratégies OCI**. Il crée des stratégies pour lire les clés secrètes à partir de Vault et gérer la base de données Autonomous Transaction Processing (le cas échéant). Cliquez sur **Suivant**.
    
13.  Dans la section Révision, cochez la case **Exécuter l'application**, puis cliquez sur **Créer**. ![exécuter l'application](images/run-apply.png)
    
    > Cela créera un travail, qui créera les ressources requises dans la pile. Vous n'avez pas besoin d'attendre, veuillez passer à la tâche suivante,
    

## Tâche 4 : accéder au bureau à distance graphique

Pour faciliter l'exécution de cet atelier, votre instance de machine virtuelle a été préconfigurée avec un bureau graphique distant accessible à l'aide de n'importe quel navigateur moderne sur votre ordinateur portable ou votre poste de travail. Procédez comme indiqué ci-dessous pour vous connecter.

1.  Ouvrez le menu hamburger dans le coin supérieur gauche. Cliquez sur **Services de développeur**, puis sélectionnez **Gestionnaire de ressources** > **Piles**.
    
2.  Cliquez sur le nom de pile que vous avez créé dans l'exercice 1. ![cliquer sur la pile](images/click-stack.png)
    
3.  Accédez à l'onglet **Informations sur l'application**, copiez l'**URL du bureau à distance** et collez-la dans le nouvel onglet du navigateur. ![URL de bureau](images/desktop-url.png)
    
    > Maintenant, vous devez suivre toutes les instructions à l'intérieur de ce bureau distant.
    

Vous pouvez maintenant passer à l'exercice suivant.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, octobre 2023
# Générer l'application Helidon MP

## Présentation

Cet atelier vous explique comment créer une application **Helidon MP** à l'aide de l'**interface de ligne de commande Helidon**. Vous allez également modifier l'application Helidon pour afficher son intégration aux services **OCI Logging and Monitoring**.

Durée estimée : 15 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Générer une application MP Helidon](videohub:1_i4j33ed4)

### Objectifs

Dans cet exercice, vous allez :

*   Télécharger l'interface de ligne de commande Helidon
*   Générer une application Helidon MP à l'aide de la CLI Helidon
*   Effectuer l'intégration avec le service OCI Logging et Metrics
*   Poussez le code de l'application vers le référentiel de code OCI
*   Déclencher le pipeline DevOps

### Prérequis

*   Un compte cloud Oracle Free Tier (essai), payant ou LiveLabs
*   Connaissance des commandes git

## Tâche 1 : téléchargement de l'interface de ligne de commande Helidon

1.  Ouvrez un nouveau terminal dans l'**éditeur de code OCI** et collez la commande suivante pour accéder au dossier d'accueil.
    
        <copy>cd ~</copy>
        
2.  Copiez et collez la commande suivante pour télécharger l'**interface de ligne de commande Helidon** et modifiez les **autorisations** pour la rendre exécutable.
    
        <copy>curl -L -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon</copy>
        

## Tâche 2 : génération d'une application de microprofil Helidon à l'aide de la CLI Helidon

1.  Exécutez l'interface de ligne de commande pour **générer un projet d'application Helidon Microprofile**.
    
        <copy>./helidon init</copy>
        
2.  Lorsqu'il vous invite à indiquer la _version Helidon_, entrez _4_ pour afficher toutes les versions, puis entrez _35_ pour sélectionner la version _4.0.0-ALPHA6_ comme indiqué ci-dessous.
    
        Helidon versions
        (1) 3.2.2 
        (2) 2.6.2 
        (3) 4.0.0-M1 
        (4) Show all versions 
        Enter selection (default: 1): 4
        -----------------------------
        -----------------------------
        (33) 2.0.1 
        (34) 2.0.0 
        (35) 4.0.0-ALPHA6 
        (36) 4.0.0-M1 
        Enter selection (default: 1): 35 
        
3.  Lorsque vous êtes invité à _Sélectionner une variante_, copiez et collez la valeur ci-dessous sur le terminal.
    
            | Helidon Flavor
        
            Select a Flavor
            (1) se   | Helidon SE
            (2) mp   | Helidon MP
            (3) nima | Helidon Níma
            Enter selection (default: 1): <copy>2</copy>
        
4.  Lorsque vous êtes invité à _Sélectionner un type d'application_, copiez et collez la valeur ci-dessous dans le terminal.
    
            Select an Application Type
        (1) quickstart | Quickstart
        (2) database   | Database
        (3) custom     | Custom
        (4) oci        | OCI
        Enter selection (default:1):<copy>4</copy>
        
5.  Lorsque vous êtes invité à saisir le _projet groupId_, le _projet artifactId_ et la _version du projet_, il vous suffit d'**accepter les valeurs par défaut**.
    
6.  Lorsque vous êtes invité à saisir le _nom du package Java_, copiez et collez la valeur ci-dessous dans le terminal.
    
        Java package name (default: me.username.mp.oci): <copy>ocw.hol.mp.oci</copy>
        
7.  Une fois promu pour _Start development loop ? (default : n) :_, appuyez sur _Entrée_ pour sélectionner la valeur par défaut.
    
    > Une fois terminé, un projet **oci-mp** est généré.
    

## Tâche 3 : modifier l'application Helidon pour la journalisation et l'explorateur de mesures

1.  Pour ouvrir le projet _oci-mp_ dans l'**éditeur de code**, cliquez sur _Fichier_ -> _Ouvrir_. ![ouvrir un projet](images/open-project.png)
    
2.  Cliquez sur la _flèche Haut_ pour accéder au dossier parent, puis sélectionnez le dossier _oci-mp_ et cliquez sur _Ouvrir_. ![mp oci ouvert](images/open-ocimp.png)
    
    > L'application _oci-mp_ sera ouverte dans l'explorateur.
    
3.  Ouvrez un nouveau terminal, copiez et collez la commande suivante pour **copier les spécifications de pipeline de build et de déploiement** à partir du dossier _`devops_helidon_to_instance_ocw_hol`_.
    
        <copy>cd ~/oci-mp/
        cp ~/devops_helidon_to_instance_ocw_hol/pipeline_specs/* .</copy>
        
4.  Ajoutez **.gitignore** afin que les fichiers et répertoires qui ne sont pas nécessaires pour faire partie du référentiel soient ignorés par git.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/.gitignore .</copy>
        
5.  Copiez et collez la commande suivante pour exécuter le script utilitaire à partir du dossier principal _`devops_helidon_to_instance_ocw_hol`_ afin de mettre à jour les paramètres **config**. Lorsqu'il demande **Enter the Helidon MP project's root directory**, appuyez sur Entrée pour sélectionner la valeur **default**.
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/update_config_values.sh</copy>
        
    
    Vous obtiendrez un résultat similaire à celui indiqué ci-dessous : ![mettre à jour la configuration](images/update-config.png)
    
    > **Veuillez lire :-**
    
    *   L'appel de ce script effectuera les opérations suivantes :
    *   Mises à jour dans le fichier de configuration _~/oci-mp/server/src/main/resources/application.yaml_ pour configurer une fonctionnalité Helidon qui envoie des mesures générées par Helidon au service de surveillance OCI.
        *   **compartmentId** : OCID de compartiment utilisé pour cette démonstration
        *   **namespace** - Il peut s'agir de n'importe quelle chaîne, mais pour cette démonstration, elle sera définie sur helidon\_metrics.
    *   Mises à jour dans le fichier de configuration _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ pour configurer les paramètres de configuration utilisés par le code d'application Helidon afin d'effectuer l'intégration avec le service OCI Logging et Metrics.
        *   **oci.monitoring.compartmentId** : OCID de compartiment utilisé pour cette démonstration
        *   **oci.monitoring.namespace** : il peut s'agir de n'importe quelle chaîne, mais pour cette démonstration, elle sera définie sur helidon\_application.
        *   **oci.logging.id :** ID de journal d'application provisionné par les scripts Terraform.
    *   Mettez à jour le fichier de configuration _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ pour configurer la propriété _oci.bucket.name_ afin qu'elle contienne le nom du bucket Object Storage provisionné par les scripts terraform qui seront utilisés dans un exercice ultérieur pour démontrer la prise en charge d'Object Storage à partir d'une application Helidon.
6.  Ouvrez le fichier _~/oci-mp/server/src/main/resources/application.yaml_ dans l'éditeur de code pour vérifier que la valeur de **compartmentId** et **namespace** sont mises à jour. ![application yaml](images/application-yaml.png)
    
7.  Ouvrez le fichier _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ dans l'éditeur de code pour vérifier que la valeur de **oci.monitoring.compartmentId**, **oci.monitoring.namespace**, **oci.logging.id** et **oci.bucket.name** sont mises à jour. _oci.bucket.name_ sera utilisé ultérieurement dans l'atelier 5.
    

## Tâche 4 : générer un jeton d'authentification pour propager le code vers le référentiel de code OCI

Au cours de cette étape, nous allons générer un _jeton d'authentification_ que nous utiliserons pour propager le code d'application Helidon vers le _référentiel de code OCI_.

1.  Sélectionnez l'_icône Utilisateur_ dans l'angle supérieur droit, puis _Mon profil_. ![Icône d'utilisateur](images/user-icon.png)
    
2.  Faites défiler la page vers le bas et sélectionnez _Jetons d'authentification_. ![Jetons d'authentification](images/auth-tokens.png)
    
3.  Cliquez sur _Générer un jeton_. ![générer un jeton](images/generate-token.png)
    
4.  Copiez _oci-mp_ et collez-le dans la zone Description, puis cliquez sur _Générer un jeton_. ![description du jeton](images/token-description.png)
    
5.  Sélectionnez **Copier** sous Jeton généré et collez-le/enregistrez-le dans un fichier texte à l'aide de l'éditeur de votre choix. N'oubliez pas que le jeton ne peut pas être récupéré ultérieurement. Il est donc important de conserver une copie de ce jeton maintenant. Une fois que vous avez terminé, cliquez sur **Fermer**. ![copier le jeton](images/copy-token.png)
    

## Tâche 5 : synchroniser le projet d'application Helidon avec le référentiel de code OCI dans votre projet DevOps

1.  Ouvrez un nouveau terminal, copiez et collez la commande suivante pour accéder au répertoire _oci-mp_.
    
        <copy>cd ~/oci-mp</copy>
        
2.  Initialisez le répertoire du projet _oci-mp_ pour devenir un **référentiel git**.
    
        <copy>git init</copy>
        
3.  Définissez la branche sur **main** pour qu'elle corresponde à la branche distante correspondante.
    
        <copy>git checkout -b main</copy>
        
4.  Définissez le **référentiel distant**. Utilisez l'URL HTTPS du référentiel de code OCI affichée à partir de la dernière sortie terraform ou utilisez l'outil get.sh de `devops_helidon_to_instance_ocw_hol` pour extraire cette valeur.
    
        <copy>git remote add origin $(~/devops_helidon_to_instance_ocw_hol/main/get.sh code_repo_https_url)
        git remote -v</copy>
        
    
    > **git remote -v** permet de vérifier que l'origine a été définie.
    
5.  Configurez git pour qu'il utilise le magasin **credential helper** afin que le nom utilisateur et le mot de passe du référentiel OCI ne soient saisis qu'une seule fois sur les commandes git qui en ont besoin. Définissez également **user.name** et **user.email** requis par la validation git.
    
        <copy>git config credential.helper store
        git config --global user.email "my.name@example.com"
        git config --global user.name "FIRST_NAME LAST_NAME"</copy>
        
6.  Synchronisez le journal git du référentiel oci avec le référentiel local à l'aide de **git pull**.
    
        <copy>git pull origin main</copy>
        
7.  Vous serez invité à saisir un nom d'utilisateur et un mot de passe. Utilisez **nom de location**/**nom utilisateur** pour le nom utilisateur et le **jeton d'authentification** de l'utilisateur OCI généré à la **tâche 4** pour le mot de passe. ![nom utilisateur git](images/git-username.png) ![mot de passe git](images/git-password.png)
    
8.  Vous obtiendrez une sortie similaire à la suivante. Si ce n'est pas le cas, vérifiez que vous avez exécuté chaque commande git correctement. ![synchronisation d'extraction Git](images/git-pull-sync.png)
    

## Tâche 6 : propager le code d'application Helidon et déclencher le pipeline DevOps

1.  **Préparer** tous les fichiers pour la validation git.
    
        <copy>git add .
        git status</copy>
        
    
    > git status affichera tous les fichiers du référentiel.
    
2.  Exécutez la première commande **commit**.
    
        <copy>git commit -m "Helidon oci-mp first commit"</copy>
        
3.  **Transmettez** les modifications au référentiel distant.
    
        <copy>git push -u origin main</copy>
        
    
    > Cette action déclenche le démarrage du pipeline de build par DevOps.
    
4.  Ouvrez la [console cloud](https://cloud.oracle.com/) dans le nouvel onglet, cliquez sur _Menu Hamburger_ -> _Services de développeur_ -> _Projets_ sous **DevOps**. ![projet devops](images/devops-project.png)
    
5.  Sélectionnez le compartiment que vous avez créé dans l'**exercice 1**, puis cliquez sur _devops-project-helidon-ocw-hol-string_ pour ouvrir le **projet DevOps**. ![sélectionner un compartiment](images/select-compartment.png)
    
    > Actualisez le navigateur, si vous ne voyez pas le nouveau compartiment.
    
6.  Sous _Dernier historique de build_, les _exécutions_ et le statut sont _Accepté/En cours_. Cliquez sur les dernières exécutions comme indiqué ci-dessous. ![historique des builds](images/build-history.png)
    
7.  Une fois que le pipeline de build a terminé les trois étapes, vous verrez la sortie comme indiqué ci-dessous. Vous pouvez cliquer sur la flèche juste avant les étapes pour afficher l'action qu'elles effectuent. Cette action a été définie dans le fichier _`build_spec.yaml`_ du dossier _oci-mp_. ![première exécution de build](images/build-run-first.png)
    
8.  Dans la progression de l'exécution de build, dans la troisième étape, cliquez sur **Trois points**, puis sur **Visualiser le déploiement** comme indiqué ci-dessous. Le **pipeline de déploiement** est alors ouvert. ![visualiser le déploiement](images/view-deployment.png)
    
9.  Ici, vous pouvez voir la _progression du déploiement_. Une fois le pipeline de déploiement terminé, vous verrez la sortie comme indiqué ci-dessous. Vous pouvez cliquer sur la flèche juste avant la phase pour afficher l'action qu'ils effectuent. Cette action a été définie dans le fichier _`deployment_spec.yaml`_ du dossier _oci-mp_. ![exécution du déploiement](images/deployment-run.png)
    
    > Cette opération déploie l'application Helidon vers des **instances Compute** dans OCI.
    
10.  Pour visualiser les journaux du pipeline de déploiement, cliquez sur **Trois points** près de la phase de déploiement, puis sur **Visualiser les détails** comme indiqué ci-dessous. ![visualiser les journaux](images/view-logs.png)
    
11.  Faites défiler les journaux vers le bas et vérifiez que la variante JDK est **Open JDK**. Notez également dans les journaux que l'application Helidon tire parti de la nouvelle fonctionnalité **Threads virtuels** dans Java, comme indiqué ci-dessous. ![jdk ouvert](images/open-jdk.png)
    
    > Dans le cadre de l'**atelier 4**, nous remplacerons **Open JDK** par **Oracle JDK**.
    

Vous pouvez maintenant **passer à l'exercice suivant**.

## En savoir plus

*   [CLI Helidon](https://helidon.io/docs/v3/#/about/cli)
*   [Guide de démarrage rapide Helidon MP](https://helidon.io/docs/v3/#/mp/guides/quickstart)
*   [Sources de configuration MP Helidon](https://helidon.io/docs/v3/#/mp/config/advanced-configuration)
*   [Sources de configuration MP Helidon](https://helidon.io/docs/v3/#/mp/guides/config)

## Accusés de réception

*   **Auteur** - Keith Lustria
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
# Provisionnement de l'infrastructure

## Présentation

Dans cet exercice, vous allez créer un **compartiment**, des **groupes dynamiques**, un **groupe d'utilisateurs et des stratégies**. Vous allez ensuite créer un **projet DevOps** et ses ressources associées à l'aide du service Terraform dans l'**éditeur de code OCI**.

Durée estimée : 10 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Provisionner l'infrastructure](videohub:1_tnhjvsxr)

### Objectifs

Dans cet exercice, vous allez :

*   Ouvrez l'éditeur de code pour télécharger le script Terraform.
*   Provisionnez le compartiment, les groupes dynamiques, le groupe d'utilisateurs et les stratégies.
*   Provisionner les ressources requises pour le projet DevOps

### Prérequis

*   Un compte cloud Oracle Free Tier (essai), payant ou LiveLabs
*   Bonne connaissance des compartiments, des groupes dynamiques, des groupes d'utilisateurs et des stratégies

## Tâche 1 : ouvrez l'éditeur de code et téléchargez le code source

1.  Dans la console cloud, cliquez sur l'icône _Outils de développement_ comme indiqué, puis sur _Editeur de code_. ![ouvrir l'éditeur de code](images/open-codeeditor.png)
    
2.  Cliquez sur _Terminal_\-> _New Terminal_ pour ouvrir le terminal. Pendant l'atelier, il vous sera demandé d'ouvrir un nouveau terminal. De cette façon, vous pouvez ouvrir un nouveau terminal dans l'éditeur de code. ![ouvrir un terminal](images/open-terminal.png)
    
3.  Copiez et collez la commande suivante sur le terminal pour télécharger le code source. Ce code source contient les scripts terraform qui créent les ressources OCI requises pour cet atelier.
    
        <copy>curl -O https://objectstorage.uk-london-1.oraclecloud.com/p/IxV3cO7Uf5VnbniLEmx8zOBhr6liix40QWNTnTy0TTBcGdrLaRNSt2IJYxBPHqdw/n/lrv4zdykjqrj/b/ankit-bucket/o/devops_helidon_to_instance_ocw_hol.zip
        unzip ~/devops_helidon_to_instance_ocw_hol.zip</copy>
        

![télécharger le code source](images/download-sourcecode.png)

4.  Pour ouvrir le code source _`devops_helidon_to_instance_ocw_hol`_ dans l'**éditeur de code**, cliquez sur _Fichier_\-> _Ouvrir_. ![code open source](images/open-sourcecode.png)
    
5.  Sélectionnez _`devops_helidon_to_instance_ocw_hol`_ dans votre répertoire de base et cliquez sur _Ouvrir_. ![ouvrir devops](images/open-devops.png)
    
6.  Cliquez sur le nom de fichier _terraform.tfvars_ dans le dossier _`devops_helidon_to_instance_ocw_hol`_ comme indiqué. Vous pouvez voir que nous avons des variables **`tenancy_ocid`**, **region**, **`compartment_ocid`**, **`user_ocid`** que nous allons personnaliser pour votre compte cloud d'évaluation. ![tfvars ouverts](images/open-tfvars.png)
    

> Si vous ne voyez pas le projet. Vous devrez peut-être cliquer sur l'icône _Explorateur_ dans l'éditeur de code. ![exlorer](images/explorer.png)

7.  Dans votre navigateur, ouvrez un **nouvel onglet** pour la [console cloud](https://cloud.oracle.com/). Nous allons utiliser cet onglet pour obtenir la valeur des variables ci-dessus.
    
8.  Pour obtenir **`tenancy_ocid`**, cliquez sur _Icône Utilisateur_, puis sur _Location_ comme indiqué. ![obtenir l'OCID de location](images/get-tenancyocid.png)
    
9.  Cliquez sur _Copier_ pour copier l'**OCID** de la location et collez-le dans le fichier _terraform.tfvars_ en tant que valeur de _`tenancy_ocid`_. ![copier l'OCID de location](images/copy-tenancyocid.png)
    
10.  Pour obtenir **`user_ocid`**, cliquez sur _Icône Utilisateur_, puis sur _Mon profil_ comme indiqué. ![mon profil](images/my-profile.png)
    
11.  Cliquez sur _Copier_ pour copier l'**OCID** de l'utilisateur et le coller dans le fichier _terraform.tfvars_ en tant que valeur de _`user_ocid`_. ![copier l'OCID utilisateur](images/copy-userocid.png)
    
12.  Pour rechercher le nom de la région, cliquez sur **Gérer les régions** comme indiqué ci-dessous. Copiez ensuite l'**identificateur de région** de votre région d'origine et collez-le dans le fichier _terraform.tfvars_ en tant que valeur de _region_. ![gérer une région](images/manage-region.png) ![nom de région](images/region-name.png)
    
13.  Enfin, votre fichier _terraform.tfvars_ doit ressembler à ceci. Laissez la valeur de _`compartment_ocid`_ telle quelle. Nous remplacerons la valeur une fois que le compartiment sera créé dans le cadre de la tâche 2. ![tfvars init](images/init-tfvars.png)
    

## Tâche 2 : création d'un compartiment, de groupes dynamiques, de groupes d'utilisateurs et de stratégies

L'objectif de cette tâche est de préparer l'environnement pour la configuration DevOps en créant un compartiment, un groupe dynamique, un groupe d'utilisateurs et des stratégies. Cette section nécessite un utilisateur disposant du privilège d'administrateur. Si vous ne l'avez pas, demandez à un autre utilisateur disposant de ce privilège de l'exécuter pour vous.

1.  Ouvrez un terminal, copiez et collez la commande suivante pour accéder au dossier _init_.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/init/</copy>
        
2.  Dans l'éditeur de code, vous pouvez visualiser différents fichiers dans le dossier _init_. Il s'agit des scripts terraform qui créent le compartiment, les groupes dynamiques, le groupe d'utilisateurs et les stratégies. ![fichiers init](images/init-files.png)
    
3.  Copiez et collez les commandes suivantes pour provisionner le compartiment, les groupes dynamiques, le groupe d'utilisateurs et les stratégies.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    Vous verrez une sortie similaire à celle ci-dessous. Veuillez **observer la sortie** pour savoir ce que le script terraform crée. Vous pouvez également référencer le code pour voir son implémentation. ![init créé](images/init-created.png)
    
    > Si des erreurs se produisent, vérifiez que vous avez correctement défini les valeurs dans le fichier **terraform.tfvars**.
    

## Tâche 3 : créer un projet DevOps et ses ressources

1.  Dans le terminal, copiez et collez la commande suivante pour accéder au dossier _main_.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main/</copy>
        
2.  Copiez et collez la commande suivante pour mettre à jour le fichier **compartment\_ocid** d'un compartiment nouvellement créé dans _terraform.tfvars_ dans le dossier _`devops_helidon_to_instance_ocw_hol`_.
    
        <copy>../utils/update_compartment.sh</copy>
        
3.  Copiez et collez la commande suivante dans le terminal pour provisionner toutes les ressources DevOps.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    > **Veuillez lire :** cela fournira les ressources suivantes requises pour DevOps :
    
    *   **Service OCI DevOps**
        *   **Projet OCI DevOps** contenant tous les composants DevOps nécessaires pour ce projet.
        *   **Référentiel de code OCI** qui hébergera le projet de code source de l'application.
        *   **Pipeline de build DevOps** avec les étapes suivantes :
            *   **Gérer le build :** exécute les étapes permettant de télécharger JDK20, maven et de créer l'application Helidon
            *   **Fournir des artefacts :** télécharge l'application Helidon créée et le déploiement vers le référentiel d'artefacts
            *   **Déclencher le déploiement :** déclenche le pipeline de déploiement
        *   **DevOps Pipeline de déploiement** qui effectue les opérations suivantes sur l'environnement cible :
            *   Télécharger JDK20
            *   Installez l'interface de ligne de commande OCI et utilisez-la pour télécharger le livrable d'application
            *   Exécuter l'application
        *   **Environnement de groupe d'instances DevOps** qui sera utilisé par le pipeline de déploiement pour identifier l'instance de calcul OCI créée en tant que cible de déploiement.
        *   **Déclencheur DevOps** qui appelle le cycle de vie du pipeline du début à la fin lorsqu'un événement Push se produit sur le référentiel de code OCI.
    *   **Registre d'artefacts OCI**
        *   **Référentiel d'artefacts OCI** qui hébergera les fichiers binaires d'application Helidon et le manifeste de déploiement en tant qu'artefacts avec numéro de version.
    *   **Plate-forme OCI**
        *   **Instance de calcul OCI** qui ouvre le port 8080 à partir du pare-feu. C'est là que l'application sera finalement déployée.
    *   **OCI Virtual Cloud Network (VCN) avec liste de sécurité** contenant une entrée qui ouvre le port 8080. Le port 8080 permet d'accéder à l'application Helidon. Le VCN OCI sera utilisé par l'instance de calcul OCI pour ses besoins réseau.
4.  Le diagramme ci-dessous illustre le fonctionnement de la configuration DevOps : ![diagramme devops](images/devops-diagram.png)
    
5.  Vous obtiendrez une sortie similaire comme indiqué ci-dessous. ![sortie tf](images/tf-output.png)
    

Vous pouvez maintenant **passer à l'exercice suivant**.

## Accusés de réception

*   **Auteur** - Keith Lustria
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
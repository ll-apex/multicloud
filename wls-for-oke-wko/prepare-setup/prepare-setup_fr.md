# Préparer la configuration

## Présentation

Cet atelier vous explique comment télécharger le fichier ZIP de pile Oracle Resource Manager (ORM) nécessaire pour configurer la ressource nécessaire à l'exécution de cet atelier. Vous créez ensuite une instance de calcul et un réseau cloud virtuel (VCN) qui vous donne accès à un bureau distant.

Temps estimé : 10 minutes

### Objectifs

*   Télécharger la pile ORM
*   Créer un calcul + une mise en réseau à l'aide de la pile de gestionnaires de ressources

### Prérequis

Cet exercice suppose que vous disposez des éléments suivants :

*   Un compte Oracle Free Tier ou Paid Cloud

## Tâche 1 : télécharger le fichier ZIP de la pile Oracle Resource Manager (ORM)

1.  Cliquez sur le lien ci-dessous pour télécharger le fichier ZIP Resource Manager dont vous avez besoin pour créer votre environnement :
    
    _Remarque 1 :_ si vous fournissez un seul téléchargement de pile pour l'atelier, utilisez cette expression simple.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  Enregistrez dans le dossier de téléchargement.
    

## Tâche 2 : créer une pile : calcul + mise en réseau

1.  Identifiez le fichier ZIP de pile ORM téléchargé dans **Tâche 1 : téléchargement du fichier ZIP de pile ORM (Oracle Resource Manager)**.
    
2.  Ouvrez le menu hamburger dans le coin supérieur gauche. Cliquez sur **Services de développeur**, puis sélectionnez **Gestionnaire de ressources** > **Piles**. Choisissez le compartiment dans lequel installer la pile. Cliquez sur **Créer une pile**. ![pile de menus](images/menu-stack.png) ![sélectionner un compartiment](images/select-compartment.png)
    
3.  Sélectionnez **Ma configuration**, puis choisissez **. Bouton Zip** du fichier, cliquez sur le lien **Parcourir** et sélectionnez le fichier zip que vous avez téléchargé ou glissez-déplacez pour l'explorateur de fichiers. Cliquez sur **Suivant**. ![parcourir le fichier ZIP](images/browse-zip.png)
    
4.  Entrez ou sélectionnez l'une des options suivantes, puis cliquez sur **Suivant**.
    
    **Nombre d'instances :** acceptez la valeur par défaut, 1.
    
    **Sélectionner un domaine de disponibilité :** sélectionnez un domaine de disponibilité dans la liste déroulante.
    
    **Besoin d'un accès distant via SSH ?** Ne cochez pas cette case pour l'accès Bureau à distance uniquement - Valeur par défaut.
    
    **Utiliser une forme d'instance flexible avec un nombre d'OCPU ajustable ? :** conservez la valeur par défaut cochée (sauf si vous prévoyez d'utiliser une forme fixe).
    
    **Forme d'instance :** conservez la valeur par défaut ou sélectionnez-la dans la liste des formes flexibles du menu déroulant (par exemple, VM.Standard.E4). Champ flexible).
    
    **Sélectionner le nombre d'OCPU par instance :** acceptez la valeur par défaut affichée. Par exemple, (1) provisionnera 1 OCPU et 16 Go de mémoire.
    
    **Utiliser le VCN existant ? :** acceptez la valeur par défaut en ne la cochant pas. Cela créera un VCN. ![configuration principale](images/main-config.png) ![forme d'instance](images/instance-shape.png)
    
5.  Sélectionnez **Exécuter l'application** et cliquez sur **Créer**. ![exécuter l'application](images/run-apply.png)
    

Vous pouvez maintenant passer à l'exercice suivant.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, octobre 2023
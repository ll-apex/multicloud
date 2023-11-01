# Préparer la configuration

## Présentation

Cet atelier vous explique comment télécharger le fichier ZIP de pile Oracle Resource Manager (ORM) nécessaire pour configurer la ressource nécessaire à l'exécution de cet atelier. Cet atelier nécessite une instance de calcul et un réseau cloud virtuel (VCN).

Temps estimé : 10 minutes

### Objectifs

*   Télécharger la pile ORM
*   Configuration d'un réseau cloud virtuel (VCN) existant

### Prérequis

Cet exercice suppose que vous disposez des éléments suivants :

*   Un compte Oracle Free Tier ou Paid Cloud

## Tâche 1 : télécharger le fichier ZIP de la pile Oracle Resource Manager (ORM)

1.  Cliquez sur le lien ci-dessous pour télécharger le fichier ZIP Resource Manager dont vous avez besoin pour créer votre environnement :
    
    _Remarque 1 :_ si vous fournissez un seul téléchargement de pile pour l'atelier, utilisez cette expression simple.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  Enregistrez dans le dossier de téléchargement.
    

Nous vous recommandons vivement d'utiliser cette pile pour créer un VCN autonome/dédié avec vos instances. Passez à la _tâche 3_ pour suivre nos recommandations. Si vous préférez utiliser un VCN existant, passez à l'étape suivante comme indiqué ci-dessous pour mettre à jour votre VCN existant avec les règles sortantes requises.

## Tâche 2 : ajout de règles de sécurité à un VCN existant

Cet atelier nécessite la disponibilité d'un certain nombre de ports, une exigence qui peut être satisfaite en utilisant l'exécution de pile ORM par défaut qui crée un VCN dédié. Pour utiliser un VCN existant, les ports suivants doivent être ajoutés aux règles entrantes.

| Port | Description |
| :-- | :-- |
| 22 | SSH ; |
| 80 | noVNC Bureau à distance (proxy NGINX) |
| (6080) | noVNC Bureau à distance |

1.  Accédez à _Networking >> Virtual Cloud Networks_.
2.  Choisissez votre réseau.
3.  Sous Resources, sélectionnez Security Lists.
4.  Cliquez sur Default Security Lists sous le bouton Create Security List (Créer une liste de sécurité).
5.  Cliquez sur le bouton Add Ingress Rule (Ajouter une règle entrante).
6.  Entrez ce qui suit :
    *   CIDR source : 0.0.0.0/0
    *   Plage de ports de destination : _reportez-vous au tableau ci-dessus_
7.  Cliquez sur le bouton Add Ingress Rules (Ajouter des règles entrantes).

## Tâche 3 : Configurer le calcul

A l'aide des détails des deux tâches ci-dessus, passez à l'exercice _Configuration de l'environnement_ pour configurer votre environnement d'atelier à l'aide d'Oracle Resource Manager (ORM) et de l'une des options suivantes :

*   Créer une pile : _Compute + Networking_
*   Créer une pile : _calculer uniquement_ avec un VCN existant où les listes de sécurité ont été mises à jour conformément à la _tâche 2_ ci-dessus

Vous pouvez maintenant passer à l'exercice suivant.

## Accusés de réception

*   **Auteur** - Rene Fontcha, LiveLabs Responsable de la plate-forme, NA Technology
*   **Contributeurs** - Meghana Banka
*   **Dernière mise à jour par/date** - Rene Fontcha, LiveLabs Platform Lead, NA Technology, février 2022
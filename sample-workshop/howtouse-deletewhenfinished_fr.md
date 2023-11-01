# Atelier avec un seul ensemble d'ateliers

## Instructions - Supprimer ce fichier lorsque vous avez terminé

1.  Ouvrir le modèle échantillon-atelier dans Atom ou Visual Studio Code
2.  Nous avons pré-créé 5 dossiers. Un atelier est créé à partir de plusieurs exercices.
3.  Supprimez les commentaires comme celui-ci : _Répertoriez les objectifs de cet exercice_
4.  Veillez à utiliser des noms de dossier et de fichier minuscules et des tirets pour les espaces (setup-adb NOT Setup\_ADB)
5.  Les noms de vos images doivent avoir des noms descriptifs. Pas seulement adb1, adb2, adb3. Pour l'accessibilité aux personnes handicapées, nous avons besoin des descriptions d'images pour expliquer à quoi ressemble l'image. N'oubliez pas les minuscules et les tirets.
6.  Téléchargez notre document d'assurance qualité à partir de WMS. Nous constatons que les ateliers entrent en production plus rapidement lorsque vous savez ce qui est nécessaire pour passer en production et que vous utilisez le squelette.

PS : vous n'avez pas besoin de Readme.md. Les fichiers Readme existent uniquement aux niveaux supérieurs de la bibliothèque. Nous dirigeons tout le trafic vers LiveLabs car nous ne pouvons pas suivre l'utilisation sur GitHub. Ne créez pas de liens directs vers GitHub, votre atelier peut être très populaire, mais nous ne pouvons pas le suivre afin que personne ne le sache.

## Chemin absolu pour la navigation dans le menu Oracle Cloud

**Exercice 1 : Provisionner une instance -> Etape 0 : Utiliser ces images standardisées pour la navigation dans Oracle Cloud (communément pour le provisionnement)** : nous avons inclus la liste des captures d'écran courantes pour la navigation dans le menu Oracle Cloud. Veuillez lire cette section et utiliser les images de chemin absolu pertinentes, le cas échéant. Votre atelier sera pérenne en cas de mises à jour de l'interface utilisateur Oracle Cloud.

## Structure de dossiers

Dans cet exemple, l'objectif est de créer plusieurs ateliers "enfants"à partir d'un atelier"parent" plus long. Les enfants sont composés de parties du parent.

atelier-échantillon/ -- laboratoires individuels

        provision/
        setup/
        dataload/
        query/
        introduction/
          introduction.md       -- description of the everything workshop, note that it is a "lab" since there is only one
    
    workshops/
       freetier/                -- freetier version of the workshop
        index.html
        manifest.json
       livelabs/                -- livelabs version of the workshop
        index.html
        manifest.json
    

### FreeTier et LiveLabs

*   "FreeTier" - inclut les essais gratuits, les comptes payants et, pour certains ateliers, les comptes Always Free (bouton brun)
*   "LiveLabs" : ateliers qui utilisent les locations fournies par Oracle (bouton vert)
*   "Bureau" : il s'agit d'un nouveau déploiement dans lequel l'atelier est encapsulé dans un environnement NoVNC exécuté dans une instance de calcul

### A propos de l'atelier

L'atelier comprend les 6 laboratoires individuels en une seule séquence.

La structure de dossiers comprend un "laboratoire" d'introduction qui décrit l'atelier comme un ensemble complet de 6 exercices. Remarque : vous n'avez peut-être pas besoin d'avoir une introduction différente pour chacune des versions parent et enfant des ateliers, ceci n'est qu'illustration.

Consultez le dossier product-name-workshop/freetier et consultez le fichier manifest.json pour voir la structure.

> **Remarque :** l'utilisation de "Lab n :" dans les titres est facultative

Le prérequis "lab" est le premier exercice d'un dossier commun sur le référentiel oracle-livelabs/common. Comme cet atelier existe déjà, nous pouvons utiliser une URL RAW/absolue à la place :

    "filename": "https://oracle-livelabs.github.io/common/labs/cloud-login/cloud-login-livelabs2.md"        },
    

Le fichier manifest.json doit connaître l'emplacement de chaque exercice par rapport à son emplacement dans la hiérarchie. Dans cette structure, les exercices sont situés à deux niveaux supérieurs, par exemple :

    "filename": "../../provision/provision.md"
    

### Par exemple :

Cet [atelier APEX](https://oracle-livelabs.github.io/apex/spreadsheet/workshops/freetier/) est un bon exemple d'atelier avec un seul ensemble d'exercices : [https://github.com/oracle-livelabs/apex/tree/main/spreadsheet](https://github.com/oracle-livelabs/apex/tree/main/spreadsheet).
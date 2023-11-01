# Simuler un scénario de patch

## Présentation

Cet exercice va tenter de simuler un scénario d'application de patches. Prenez le cas où l'environnement initial utilise _Open JDK_ à des fins de test et se déplace éventuellement vers _Oracle JDK_ lorsqu'il est prêt pour la production. Le précédent pipeline de build et de déploiement définit déjà _Open JDK 20_ comme version Java à utiliser. Dans cet exercice, il sera remplacé par _Oracle JDK 20_.

Durée estimée : 10 minutes

Regardez la vidéo ci-dessous pour une présentation rapide du laboratoire. [Simuler un scénario de patch](videohub:1_4pecvhmf)

### Objectifs

Dans cet exercice, vous allez :

*   Modifier la version de JDK
*   Vérifier la nouvelle version de JDK dans le pipeline de déploiement

### Prérequis

*   Un compte cloud Oracle Free Tier (essai), payant ou LiveLabs

## Tâche 1 : modifier le programme d'installation de JDK

1.  Comme nous l'avons vu dans l'**atelier 2**, dans le **journal de pipeline de déploiement** précédemment terminé, et avons observé que près du bas du journal, il utilisait **Open JDK**. ![jdk ouvert](images/open-jdk.png)
    
2.  Dans l'**éditeur de code**, cliquez sur le nom de fichier **`build_spec.yaml`** pour l'ouvrir. Mettez en commentaire la variable d'environnement **`JDK20_TAR_GZ_INSTALLER`** qui définit le chemin d'URL vers un programme d'installation **JDK ouvert** à la ligne numéro 15 et le commentaire **`JDK20_TAR_GZ_INSTALLER`** qui définit le chemin d'URL vers un programme d'installation **Oracle JDK** à la ligne numéro 16 comme indiqué ci-dessous. ![modifier jdk](images/modify-jdk.png)
    

## Tâche 2 : propager la modification et déclencher le pipeline DevOps

1.  Copiez et collez la commande suivante dans le terminal **pour valider et appliquer** la modification.
    
        <copy>git add .
        git status
        git commit -m "Replace OpenJDK 20 with OracleJDK 20"
        git push</copy>
        
    
    > Le pipeline sera déclenché par cette poussée git.
    

## Tâche 3 : vérifier la nouvelle variante JDK dans le pipeline de déploiement

1.  Ouvrez la [console cloud](https://cloud.oracle.com/) dans le nouvel onglet, cliquez sur _Menu Hamburger_ -> _Services de développeur_ -> _Projets_ sous **DevOps**. ![projet devops](images/devops-project.png)
    
2.  Sélectionnez le compartiment que vous avez créé dans l'**exercice 1**, puis cliquez sur _devops-project-helidon-ocw-hol-string_ pour ouvrir le **projet DevOps**. ![sélectionner un compartiment](images/select-compartment.png)
    
3.  Sous **Dernier historique de build**, les **exécutions** et le statut sont **Accepté/En cours**. Cliquez sur les dernières exécutions comme indiqué ci-dessous. ![historique des builds](images/build-history.png)
    
4.  Une fois que le pipeline de build **a terminé les trois étapes**, vous verrez la sortie comme indiqué ci-dessous. ![première exécution de build](images/build-run-first.png)
    
5.  Dans la progression de l'exécution de build, dans la troisième étape, cliquez sur **Trois points**, puis sur **Visualiser le déploiement** comme indiqué ci-dessous. Le pipeline de déploiement sera ouvert. ![visualiser le déploiement](images/view-deployment.png)
    
6.  Ici, vous pouvez voir la **progression du déploiement**. Une fois le pipeline de déploiement terminé, vous verrez la sortie comme indiqué ci-dessous. ![exécution du déploiement](images/deployment-run.png)
    
    > Cette opération déploie l'application Helidon vers les instances Compute dans OCI.
    
7.  Pour visualiser les journaux du pipeline de déploiement, cliquez sur **Trois points** près de la phase de déploiement, puis sur **Visualiser les détails** comme indiqué ci-dessous. ![visualiser les journaux](images/view-logs.png)
    
8.  Faites défiler les journaux et vérifiez la version de JDK. Il doit s'agir d'**Oracle JDK** comme indiqué ci-dessous. ![oracle jdk](images/oracle-jdk.png)
    

Vous pouvez maintenant **passer à l'exercice suivant**.

## Accusés de réception

*   **Auteur** - Keith Lustria
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
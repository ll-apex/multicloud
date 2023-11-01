# Installer Verrazzano

## Présentation

Cet atelier vous guide tout au long des étapes d'installation de Verrazzano sur un cluster Kubernetes dans Oracle Cloud Infrastructure.

Durée estimée : 20 minutes

### A propos du produit/technologie

Verrazzano est une plate-forme de conteneur d'entreprise de bout en bout pour le déploiement d'applications cloud natives et traditionnelles dans des environnements multicloud et hybrides. Il est composé d'un ensemble de composants open source sélectionnés - beaucoup que vous pouvez déjà utiliser et faire confiance, et certains qui ont été écrits spécifiquement pour rassembler toutes les pièces qui font de Verrazzano une plate-forme cohérente et facile à utiliser.

Verrazzano inclut les fonctionnalités suivantes :

*   Gestion de la charge de travail hybride et multicluster
*   Gestion spéciale pour les applications WebLogic, Coherence et Helidon
*   Gestion d'infrastructure multicluster
*   Surveillance intégrée et pré-câblée des applications
*   Sécurité intégrée
*   Activation de DevOps et GitOps

### Objectifs

Dans cet exercice, vous allez :

*   Installez l'outil de ligne de commande Verrazzano.
*   Installez le profil de développement (`dev`) de Verrazzano.
*   Vérifiez que l'installation de Verrazzano a réussi.

### Prérequis

Verrazzano requiert les éléments suivants :

*   Cluster Kubernetes et `kubectl` compatible.
*   Au moins 2 UC, 100 Go de stockage sur disque et 16 Go de RAM disponibles sur les noeuds de processus actif Kubernetes. Cela suffit pour installer le profil de développement de Verrazzano. Selon les besoins en ressources des applications que vous déployez, cela peut ou non suffire au déploiement de vos applications.

Au cours de l'atelier 1, vous avez créé un cluster Kubernetes sur Oracle Cloud Infrastructure. Vous utiliserez ce cluster Kubernetes, \*cluster1\*, pour installer le profil de développement de Verrazzano. Au cours de l'atelier 1, vous avez créé un fichier de configuration pour accéder au cluster Kubernetes sur Oracle Cloud Infrastructure. Vous utiliserez ce cluster Kubernetes, \*cluster1\*, pour installer le profil de développement de Verrazzano.

## Tâche 1 : installation de la CLI vz

1.  Téléchargez la dernière interface de ligne de commande vz.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz</copy>
        
    
    La sortie doit ressembler à ceci :
    
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
        

0 0 0 0 0 0 0 0 -- :-- :-- :-- :-- -- :-- :-- :-- :-- 0 100 39.7M 100 39.7M 0 0 23.6M 0 0:00:01 0:00:01 -- :-- :-- 32.7M \`\`\`

2.  Téléchargez le fichier de somme de contrôle.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        

La sortie doit ressembler à ceci :

    ```bash
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    

0 0 0 0 0 0 0 0 -- :-- :-- :-- :-- -- :-- :-- :-- :-- 0 100 102 100 102 0 0 218 0 -- :-- :-- :-- :-- :-- -- :-- :-- :-- :-- 218 \`\`

3.  Validez le fichier binaire par rapport au fichier de somme de contrôle.
    
        <copy>sha256sum -c verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        
    
    La sortie doit ressembler à ceci :
    
        verrazzano-1.6.2-linux-amd64.tar.gz: OK
        
4.  Déballage et déplacement vers le répertoire binaire vz,
    
        <copy>tar xvf verrazzano-1.6.2-linux-amd64.tar.gz
        cd ~/verrazzano-1.6.2/bin/</copy>
        
5.  Testez pour vous assurer que la version que vous avez installée est à jour.
    
        <copy>./vz version</copy>
        
    
    La sortie doit ressembler à ceci :
    
        Version: v1.6.2
        BuildDate: 2023-07-21T12:06:02Z
        GitCommit: 5cd8a2f8670000d2ef0b02404e3169699d87f26b
        

## Tâche 2 : installation du profil de développement Verrazzano

Un profil d'installation est une configuration bien connue des paramètres de Verrazzano qui peut être référencée par nom, qui peut ensuite être personnalisée si nécessaire.

Verrazzano prend en charge les profils d'installation suivants : développement (`dev`), production (`prod`) et cluster géré (`managed-cluster`).

L'image suivante décrit les profils d'installation de Verrazzano. ![Installer le profil](images/installprofile.png)

Pour modifier les profils dans l'une des commandes suivantes, définissez la variable d'environnement _VZ\_PROFILE_ sur le nom du profil à installer.

Pour obtenir une description complète des options de configuration de Verrazzano, reportez-vous à la [définition de ressource personnalisée Verrazzano](https://verrazzano.io/docs/reference/api/verrazzano/verrazzano/).

Dans cet atelier, nous allons installer le _profil de développement de Verrazzano_, qui présente les caractéristiques suivantes :

*   DNS générique (nip.io)
*   Certificats auto-signés
*   Pile d'observabilité partagée utilisée par les composants système et toutes les applications
*   Stockage éphémère pour la pile d'observabilité (si les pods sont redémarrés, vous perdez tous vos journaux et mesures)
*   Il a une installation légère.
*   C'est à des fins d'évaluation.
*   Topologie de cluster Opensearch à noeud unique.

Verrazzano installe un ensemble de composants open source sélectionnés. Le tableau suivant répertorie chaque composant, sa version et une brève description.

![Profil Verrazzano](images/verrazzano-profile.png " ") ![Profils Verrazzano](images/verrazzano-profiles.png " ")

Selon notre choix de DNS, nous pouvons utiliser nip.io (DNS générique) ou [Oracle OCI DNS](https://docs.cloud.oracle.com/en-us/iaas/Content/DNS/Concepts/dnszonemanagement.htm). Dans cet atelier, nous allons procéder à l'installation à l'aide de nip.io (DNS générique).

Un contrôleur d'entrée permet de fournir un accès aux conteneurs Docker au monde extérieur (en fournissant une adresse IP). L'entrée achemine l'adresse IP vers différents clusters.

1.  Installation à l'aide de la méthode DNS nip.io. Copiez la commande suivante et collez-la dans _Cloud Shell_ pour installer Verrazzano.
    
        <copy>./vz install -f - <<EOF
        apiVersion: install.verrazzano.io/v1beta1
        kind: Verrazzano
        metadata:
          name: example-verrazzano
        spec:
          profile: dev
        EOF
        </copy>
        
    
    La sortie doit ressembler à ceci :
    
        Installing Verrazzano version v1.6.2
        Applying the file https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-platform-operator.yaml
        customresourcedefinition.apiextensions.k8s.io/verrazzanos.install.verrazzano.io created
        namespace/verrazzano-install created
        serviceaccount/verrazzano-platform-operator created
        clusterrole.rbac.authorization.k8s.io/verrazzano-managed-cluster created
        clusterrolebinding.rbac.authorization.k8s.io/verrazzano-platform-operator created
        service/verrazzano-platform-operator created
        service/verrazzano-platform-operator-webhook created
        deployment.apps/verrazzano-platform-operator created
        deployment.apps/verrazzano-platform-operator-webhook created
        mutatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-mysql-backup created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-operator-webhook created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-mysqlinstalloverrides created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-requirements-validator created
        Waiting for verrazzano-platform-operator to be ready before starting install - 23 seconds
        \> Il faut environ 15 à 20 minutes pour terminer l'installation. Cette commande installe l'opérateur de plate-forme Verrazzano et applique la ressource personnalisée Verrazzano. \> L'installation prend environ 8 à 10 minutes. Cette commande installe l'opérateur de plate-forme Verrazzano et applique la ressource personnalisée Verrazzano.
2.  Attendez la fin de l'installation. Les journaux d'installation sont transmis à la fenêtre de commande jusqu'à la fin de l'installation ou jusqu'à ce que le délai d'expiration par défaut (30 m) soit atteint.
    

## Tâche 3 : Vérification d'une installation Verrazzano réussie

Verrazzano installe plusieurs objets dans plusieurs espaces de noms. Les composants Verrazzano sont installés dans l'espace de noms _verrazzano-system_.

1.  Vérifiez que tous les pods associés aux objets multiples ont le statut _En cours d'exécution_.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $   kubectl get pods -n verrazzano-system
        NAME                                    READY   STATUS  RESTARTS AGE
        coherence-operator-b5dc669c6-rk2sm      1/1     Running   1       23m
        fluentd-54f5x                           2/2     Running   1       11m
        fluentd-h7mgh                           2/2     Running   1       11m
        fluentd-xcdfz                           2/2     Running   0       11m
        oam-kubernetes-runtime-5b48f944b-cx7b9  1/1     Running   0       24m
        verrazzano-application-operator-665c5   1/1     Running   0       22m
        verrazzano-application-operator-webhook 1/1     Running   0       22m
        verrazzano-authproxy-67776ff58b-8l      3/3     Running   0       21m
        verrazzano-cluster-operator-67dc5695    1/1     Running   0       14m
        verrazzano-cluster-operator-webhook     1/1     Running   0       14m
        verrazzano-console-7d95c98cb9-9ql2      2/2     Running   0       20m
        verrazzano-monitoring-operator-59f      2/2     Running   0       21m
        vmi-system-es-master-0                  2/2     Running   0       19m
        vmi-system-grafana-7fd956b585-2tbgl     3/3     Running   0       19m
        vmi-system-kiali-dd87546d6-ddxss        2/2     Running   0       22m
        vmi-system-osd-7687d6fccf-nm7kt         2/2     Running   0       14m
        weblogic-operator-54979449f4-njgrq      2/2     Running   0       22m
        weblogic-operator-webhook-f7ff8c8cf     1/1     Running   0       22m
        
    
    Laissez _Cloud Shell_ ouvert. Nous en avons besoin pour l'atelier 4.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
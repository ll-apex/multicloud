# Présentation

## A propos de cet atelier

Dans cet atelier, vous apprendrez à créer, tenir à jour et mettre à niveau une **application de microservice Helidon** du début à la fin à l'aide du **service OCI DevOps**. Nous allons montrer comment accélérer et rationaliser l'ensemble du processus de gestion du cycle de vie, en utilisant JDK20 avec la technologie des threads virtuels.

Durée estimée : 5 minutes

### Objectifs

Dans cet atelier, vous allez :

*   Créer un compartiment, un groupe dynamique et des stratégies
*   Créer un projet DevOps et les ressources associées à l'aide de Terraform
*   Créez et déployez une application Helidon MP sur des instances Compute à l'aide d'OCI DevOps
*   Afficher l'intégration du kit SDK OCI Monitoring
*   Valider l'intégration du kit SDK OCI Logging pour transmettre les messages au service Logging
*   Simuler un scénario d'application de patches
*   Ajouter un accès Object Storage à partir de l'application Helidon MP

### Prérequis

*   Un compte cloud Oracle Free Tier (essai), payant ou LiveLabs
*   [Connaissance de la console OCI](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
*   [Présentation de Networking](https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm)
*   [Bonne connaissance des compartiments](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/concepts.htm)
*   [Services OCI DevOps](https://docs.oracle.com/en-us/iaas/Content/devops/using/home.htm)

## Oracle DevOps

Le service Oracle Cloud Infrastructure DevOps fournit une plate-forme d'intégration continue et de déploiement continu de bout en bout pour les développeurs. Les services OCI DevOps couvrent largement tous les besoins essentiels du cycle de vie d'un logiciel. Tel que

*   **Pipelines de déploiement OCI :** automatisez les versions avec des stratégies de publication de pipeline déclaratives vers des plates-formes OCI telles que VM et Baremetals, Oracle Container Engine for Kubernetes (OKE) et OCI Functions
*   **Référentiels d'artefacts OCI :** emplacement de stockage des artefacts avec numéro de version, y compris les artefacts non mutables.
*   **Référentiels de code OCI :** OCI a fourni un service de référentiel de code évolutif.
*   **Pipelines de build OCI** : service sans serveur et évolutif permettant d'automatiser les appels de build, de test, d'artefacts et de déploiement.

![Architecture Devops](images/oci-devops.png)

## Architecture de jeu de rôles

Vous allez créer et déployer l'architecture ci-dessous à l'aide des services et fonctionnalités OCI.

![Diagramme Devops](images/devops-diagram.png)

Vous pouvez maintenant **passer à l'exercice suivant**.

## En savoir plus

*   [Architecture de référence : comprendre les stratégies de déploiement d'applications innovantes avec Oracle Cloud Infrastructure DevOps](https://docs.oracle.com/en/solutions/mod-app-deploy-strategies-oci/index.html)
*   [https://helidon.io/](https://helidon.io/)
*   [https://medium.com/helidon](https://medium.com/helidon)
*   [https://github.com/helidon-io/helidon](https://github.com/helidon-io/helidon)
*   [https://www.youtube.com/Helidon\_Project](https://www.youtube.com/Helidon_Project)
*   [https://helidon.slack.com](https://helidon.slack.com)

## Accusés de réception

*   **Auteur** - Keith Lustria
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, mai 2023
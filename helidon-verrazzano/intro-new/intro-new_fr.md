# Présentation

## A propos de cet atelier

Cet atelier propose aux participants une session pratique d'introduction à Oracle Kubernetes Engine, Verrazzano et Helidon. Vous apprendrez à déployer un exemple d'application hello helidon sur OKE à l'aide de Verrazzano.

Au cours de cet atelier, vous allez configurer Oracle Kubernetes Engine (OKE) sur Oracle Cloud Infrastructure (OCI) à l'aide du compte Oracle Cloud Free Tier. Le compte Niveau gratuit est suffisant pour explorer et apprendre à exécuter et à utiliser des applications de microservices au niveau de l'entreprise.

L'objectif de cet atelier est d'apprendre les bases de l'utilisation d'OKE et de Verrazzano et de comprendre comment ils peuvent vous aider dans votre entreprise. Si vous souhaitez en savoir plus sur les fonctionnalités de ces projets, poursuivez l'exploration à l'aide de votre compte cloud Oracle Free Tier et d'Oracle Cloud Infrastructure.

Cet atelier est conçu pour être aussi explicite que possible, mais n'hésitez pas à demander des éclaircissements ou de l'aide en cours de route.

Temps estimé : 60 minutes

### Objectifs

*   Configurez votre compte Oracle Cloud Free Tier (si vous ne l'avez pas encore fait).
*   Configurez une instance Oracle Kubernetes Engine sur Oracle Cloud Infrastructure.
*   Installez la plate-forme Verrazzano.
*   Déployez l'application _Simple Hello Helidon_.
*   Surveillez l'application _Simple Hello Helidon_ à l'aide des outils Verrazzano, notamment Rancher, Opensearch, Grafana et Prometheus.

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.

## À propos de Helidon

Helidon est une structure de microservices open source introduite par Oracle qui fournit un ensemble de bibliothèques Java conçues pour créer des applications légères et rapides basées sur des microservices. La structure prend en charge deux modèles de programmation pour l'écriture de microservices : Helidon SE et Helidon MP.

Alors que Helidon SE est conçu pour être une microstructure qui prend en charge le modèle de programmation réactive, Helidon MP est une implémentation de la spécification MicroProfile. Etant donné que MicroProfile a ses racines dans Java EE, les API MicroProfile suivent une approche déclarative familière avec une utilisation intensive des annotations. Cela en fait un bon choix pour les développeurs Java EE.

Les fonctionnalités MicroProfile visent l'implémentation de microservices. Vous pouvez trouver des API pour définir des clients REST, surveiller l'application, lire des statistiques techniques et fonctionnelles et configurer l'application. Helidon a également ajouté des API supplémentaires à l'ensemble central d'API Microprofile, vous offrant toutes les fonctionnalités dont vous avez besoin pour écrire des applications cloud natives modernes.

> La norme [MicroProfile](https://microprofile.io/) s'appuie sur Jakarta EE. Comme Jakarta EE, MicroProfile est open source et est développé par la Fondation Eclipse. L'implémentation avec MicroProfile a lieu dans les bibliothèques ou les serveurs d'applications implémentant la norme, tout comme Jakarta EE.

## À propos de Verrazzano

Verrazzano est une plate-forme de conteneur d'entreprise de bout en bout pour le déploiement d'applications cloud natives et traditionnelles dans des environnements multicloud et hybrides. Il est composé d'un ensemble de composants open source sélectionnés - beaucoup que vous pouvez déjà utiliser et faire confiance, et certains qui ont été écrits spécifiquement pour rassembler toutes les pièces qui font de Verrazzano une plate-forme cohérente et facile à utiliser.

Verrazzano inclut les fonctionnalités suivantes :

*   Gestion de la charge de travail hybride et multicluster
*   Gestion spéciale pour les applications WebLogic, Coherence et Helidon
*   Gestion d'infrastructure multicluster
*   Surveillance intégrée et pré-câblée des applications
*   Sécurité intégrée
*   Activation de DevOps et GitOps

![Verrazzano](images/verrazzano.png)

## En savoir plus

*   [https://helidon.io](https://helidon.io)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, mars 2023
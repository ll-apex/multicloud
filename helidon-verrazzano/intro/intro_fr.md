# Présentation

## A propos de cet atelier

Cet atelier propose aux participants une session pratique de présentation avec Helidon et Verrazzano. Vous apprendrez à créer, assembler et déployer des services sur une plate-forme de conteneur d'entreprise.

Il s'agit d'une session BYOL (Apportez votre ordinateur portable). Apportez votre ordinateur portable Windows, OSX ou Linux. JDK 11+, Apache Maven (3.6) et Docker doivent être installés avant de commencer.

Au cours de cet exercice, vous allez installer les outils de la CLI Helidon et développer l'application de microservice HTTP. Pour Verrazzano, vous allez configurer Oracle Kubernetes Engine (OKE) sur Oracle Cloud Infrastructure à l'aide du compte Niveau gratuit d'Oracle Cloud. Le compte Niveau gratuit est suffisant pour explorer et apprendre à exécuter et à utiliser des applications de microservices au niveau de l'entreprise.

L'objectif de cet atelier est d'apprendre les bases de l'utilisation de Helidon et de Verrazzano et de comprendre comment ils peuvent vous aider dans vos projets. Si vous souhaitez en savoir plus sur les fonctionnalités de ces projets, poursuivez l'exploration à l'aide de votre compte cloud Oracle Free Tier et d'Oracle Cloud Infrastructure.

Cet atelier est conçu pour être aussi explicite que possible, mais n'hésitez pas à demander des éclaircissements ou de l'aide en cours de route.

> Le provisionnement du moteur Kubernetes Oracle (OKE) et l'installation de Verrazzano peuvent prendre plusieurs minutes. Pour gagner du temps, vous serez invité à effectuer votre développement et la configuration de l'environnement en parallèle. Suivez les instructions et passez d'une tâche à l'autre lorsque cela est nécessaire.

Temps estimé : 90 minutes

### Objectifs

*   Configurez votre compte Oracle Cloud Free Tier (si vous ne l'avez pas encore fait).
*   Configurez une instance Oracle Kubernetes Engine sur Oracle Cloud Infrastructure.
*   Installez la plate-forme Verrazzano.
*   Déployez l'application _Helidon MP_.
*   Surveillez l'application _Helidon MP_ à l'aide des outils Verrazzano, notamment OpenSearch, Grafana et Prometheus.

### Prérequis

Cet exercice suppose que les éléments suivants sont installés sur votre ordinateur :

*   JDK 11 et versions ultérieures
*   Apache Maven (3.6)
*   Docker

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
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
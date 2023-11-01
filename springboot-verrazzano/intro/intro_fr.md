# Présentation

## A propos de cet atelier

Dans cet atelier, nous allons découvrir comment déployer des applications Spring Boot vers un cluster Oracle Kubernetes Engine (OKE) à l'aide de Verrazzano.

À l'ère du numérique, les entreprises cherchent des moyens de développer et de déployer des applications plus rapidement et plus efficacement. Kubernetes est devenu la norme de facto pour l'orchestration de conteneurs, permettant aux entreprises de déployer, de gérer et de dimensionner des applications en conteneur. Oracle Kubernetes Engine (OKE) fournit un service Kubernetes géré qui simplifie le déploiement et la gestion des applications en conteneur sur Oracle Cloud Infrastructure (OCI).

Spring Boot est une structure Java populaire utilisée pour créer des applications Web et des microservices. Il offre une expérience de développement simplifiée avec une approche conventionnelle sur configuration, ce qui en fait un excellent choix pour la création et le déploiement d'applications en conteneur sur Kubernetes.

Verrazzano est une plate-forme open source native de Kubernetes qui fournit une solution complète de bout en bout pour le déploiement et la gestion d'applications cloud natives. Il simplifie le déploiement d'applications complexes à plusieurs composants en fournissant une vue unifiée de la topologie des applications, ainsi qu'un ensemble d'outils intégrés pour le déploiement, la surveillance et la gestion.

Cet atelier est conçu pour être aussi explicite que possible, mais n'hésitez pas à demander des éclaircissements ou de l'aide en cours de route.

Temps estimé : 90 minutes

### Objectifs

*   Configurez votre compte Oracle Cloud Free Tier (si vous ne l'avez pas encore fait).
*   Configuration d'un cluster OKE sur OCI
*   Installation et configuration de Verrazzano sur le cluster
*   Création d'une image de conteneur pour une application Spring Boot
*   Déployer l'application sur le cluster OKE à l'aide de Verrazzano
*   Surveiller et gérer l'application à l'aide des outils intégrés de Verrazzano

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.

## En savoir plus

**À propos de Springboot**

Spring Boot est une structure Java populaire utilisée pour créer des applications Web et des microservices. Il offre une expérience de développement simplifiée avec une approche conventionnelle sur configuration, ce qui en fait un excellent choix pour la création et le déploiement d'applications en conteneur sur Kubernetes.

Spring Boot offre une variété de fonctionnalités qui facilitent la création et le déploiement rapides d'applications, telles que les serveurs intégrés, la configuration automatique et un large éventail de bibliothèques et d'outils pour la création d'applications Web, de systèmes de messagerie et d'applications orientées données. Il offre également un riche ensemble d'outils de test et de débogage, ce qui facilite l'écriture de code robuste et fiable.

L'un des principaux avantages de Spring Boot est sa capacité à s'intégrer facilement à d'autres technologies et structures, notamment les systèmes de base de données, les files d'attente de messages et les systèmes de mise en cache. Cela permet aux développeurs de créer des systèmes distribués complexes capables de gérer des charges de travail à grande échelle et de répondre aux exigences des applications modernes basées sur le cloud.

En mettant l'accent sur la simplicité et la facilité d'utilisation, Spring Boot est devenu un choix populaire parmi les développeurs et les organisations qui cherchent à créer des applications cloud natives modernes. Elle compte une communauté dynamique de contributeurs et d'utilisateurs et évolue continuellement pour répondre aux besoins changeants de l'industrie.

**À propos de Verrazzano**

Verrazzano est une plate-forme de conteneur d'entreprise de bout en bout pour le déploiement d'applications cloud natives et traditionnelles dans des environnements multicloud et hybrides. Il est composé d'un ensemble de composants open source sélectionnés - beaucoup que vous pouvez déjà utiliser et faire confiance, et certains qui ont été écrits spécifiquement pour rassembler toutes les pièces qui font de Verrazzano une plate-forme cohérente et facile à utiliser.

Verrazzano inclut les fonctionnalités suivantes :

*   Gestion de la charge de travail hybride et multicluster
*   Gestion spéciale pour les applications WebLogic, Coherence et Helidon
*   Gestion d'infrastructure multicluster
*   Surveillance intégrée et pré-câblée des applications
*   Sécurité intégrée
*   Activation de DevOps et GitOps

![Verrazzano](images/verrazzano.png)

*   [https://spring.io/](https://spring.io/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, avril 2023
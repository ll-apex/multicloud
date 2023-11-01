# Présentation

## A propos de cet atelier

Cet atelier explique comment installer la plate-forme Verrazzano sur un seul cluster Kubernetes et déployer un exemple d'application à l'aide des concepts Verrazzano.

L'exemple d'application [Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/) se compose de trois parties principales :

*   Application back-end de "traitement des commandes", qui est une application Java EE avec des services REST et une interface utilisateur JSP très simple, qui stocke les données dans une base de données MySQL. Cette application est exécutée sur le serveur WebLogic.
*   Une boutique en ligne front-end "Robert's Books", qui est un vendeur de livres général. Il est implémenté en tant que microservice Helidon, qui extrait les données des livres de Coherence, utilise une banque de cache Coherence pour conserver les données pour le gestionnaire de commandes et dispose d'une interface utilisateur Web React.
*   Une boutique en ligne front-end "Bobby's Books", qui est une librairie spécialisée pour enfants. Il est implémenté en tant que microservice Helidon, qui obtient les données de livre à partir d'une (différente) banque de cache Coherence, s'interface directement avec le gestionnaire de commandes et dispose d'une interface utilisateur Web JSF exécutée sur le serveur WebLogic.

### A propos du produit/technologie

Verrazzano est une plate-forme de conteneur d'entreprise de bout en bout pour le déploiement d'applications cloud natives et traditionnelles dans des environnements multicloud et hybrides. Il est composé d'un ensemble de composants open source sélectionnés - beaucoup que vous pouvez déjà utiliser et faire confiance, et certains qui ont été écrits spécifiquement pour rassembler toutes les pièces qui font de Verrazzano une plate-forme cohérente et facile à utiliser.

Verrazzano inclut les fonctionnalités suivantes :

*   Gestion de la charge de travail hybride et multicluster
*   Gestion spéciale pour les applications WebLogic, Coherence et Helidon
*   Gestion d'infrastructure multicluster
*   Surveillance intégrée et pré-câblée des applications
*   Sécurité intégrée
*   Activation de DevOps et GitOps

![Verrazzano](images/verrazzano.png)

### Objectifs

1\. Configurez une instance Oracle Kubernetes Engine sur Oracle Cloud Infrastructure. 1\. Configurez "kubectl" pour interagir avec l'instance Oracle Kubernetes Engine sur Oracle Cloud Infrastructure. 2\. Installez et apprenez à connaître la plate-forme Verrazzano. 3. Déployez l'exemple d'application \*Bobby's Books\*. 4. Modifiez et redéployez le composant d'application \*Stock\* basé sur Helidon.

## En savoir plus

*   [Verrazzano](https://verrazzano.io/)

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
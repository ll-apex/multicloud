# Présentation

## A propos de cet atelier

Le déploiement d'une application Tomcat sur un cluster Kubernetes peut être un processus complexe qui implique la gestion de plusieurs conteneurs, la configuration de la mise en réseau et la garantie d'une haute disponibilité et d'une évolutivité. Pour simplifier ce processus, de nombreuses entreprises se tournent vers des technologies de conteneurisation telles que Docker et Kubernetes.

Docker permet la création d'images de conteneur portables pouvant être déployées sur n'importe quelle plate-forme, tandis que Kubernetes fournit un puissant moteur d'orchestration capable de gérer le déploiement et la mise à l'échelle des conteneurs.

Oracle Verrazzano est une plate-forme cloud native qui fournit une expérience de gestion unifiée pour le déploiement et l'exploitation d'applications en conteneur sur des clusters Kubernetes. Il simplifie le déploiement d'applications complexes en fournissant un ensemble cohérent d'outils et d'API pouvant être utilisés pour gérer et surveiller des applications sur plusieurs clusters.

Dans cet atelier, nous explorerons le processus de déploiement d'une image Docker d'application Tomcat vers un cluster Oracle Kubernetes à l'aide de Verrazzano. Nous aborderons les étapes de création d'une image Docker, de configuration de Verrazzano, de déploiement de l'application et de surveillance de ses performances.

A la fin de cet atelier, vous saurez comment déployer des applications en conteneur sur un cluster Oracle Kubernetes à l'aide de Verrazzano.

Cet atelier est conçu pour être aussi explicite que possible, mais n'hésitez pas à demander des éclaircissements ou de l'aide en cours de route.

Temps estimé : 90 minutes

### Objectifs

*   Configurez votre compte Oracle Cloud Free Tier (si vous ne l'avez pas encore fait).
*   Configurez une instance Oracle Kubernetes Engine sur Oracle Cloud Infrastructure.
*   Installez le profil de production Verrazzano.
*   Créez un exemple d'image de docker d'application Tomcat.
*   Déployez l'application Tomcat sur OKE à l'aide de Verrazzano.
*   Explorez Grafana, Prometheus et la console OpenSearch Dashboard.

### Prérequis

*   Vous devez avoir un compte [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) activé.

## En savoir plus

**À propos de Tomcat**

Apache Tomcat est un serveur Web open source et un conteneur de servlets largement utilisé pour déployer des applications Web basées sur Java. Tomcat est conçu pour être léger, efficace et facile à utiliser, et fournit un riche ensemble de fonctionnalités pour la gestion et le déploiement d'applications Web.

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

*   [https://tomcat.apache.org/](https://tomcat.apache.org/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
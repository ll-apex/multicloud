# Présentation

## A propos de cet atelier

Dans cet exercice, vous allez utiliser Oracle Code Editor pour créer, exécuter et modifier des microservices à l'aide de Helidon Nima WebServer disponible dans Helidon 4. Nima est écrit à partir de zéro pour tirer parti des threads virtuels de Java 19 et fournit un modèle de programmation de blocage simple.

Vous verrez également comment il se compare à une programmation réactive plus complexe.

Au cours de cet atelier, vous allez utiliser Helidon Project Starter pour développer une application Helidon Microprofile. Il est hautement personnalisable et propose diverses options permettant aux utilisateurs de sélectionner les fonctionnalités Helidon qu'ils souhaitent ajouter à leur projet. Ensuite, vous allez migrer l'application vers Helidon 4 s'exécutant sur le nouveau Helidon Nima WebServer à l'aide de threads virtuels.

Cet atelier est conçu pour être aussi explicite que possible, mais n'hésitez pas à demander des éclaircissements ou de l'aide en cours de route.

Temps estimé : 90 minutes

### Objectifs

*   Explorer l'éditeur de code OCI
*   Construire et exécuter l'application Nima
*   Créer et exécuter l'application réactive
*   Analyser la simplicité de l'application Nima
*   Comparer la trace de pile pour l'application Nima et réactive
*   Générer une application Helidon MP
*   Migrer l'application Helidon 3 vers Helidon 4

**À propos de Helidon Nima**

Helidon est une structure de microservices open source introduite par Oracle qui fournit un ensemble de bibliothèques Java conçues pour créer une application légère et rapide basée sur les microservices.

Dans les versions antérieures de Helidon, les développeurs pouvaient choisir parmi deux modèles de programmation. [Helidon MP](https://helidon.io/docs/v3/#/mp/introduction) est une implémentation de la norme Eclipse MicroProfile avec des API familières aux développeurs Jakarta EE. De plus, [Helidon SE](https://helidon.io/docs/v3/#/se/introduction) est un framework réactif et allégé.

Dans Helidon 4, Oracle présente Níma, un serveur Web écrit de A à Z pour utiliser des threads virtuels JEP 425. Nima fournit un modèle de programmation facile à utiliser avec des performances exceptionnelles. Avec les threads virtuels, les threads ne sont plus une ressource rare à regrouper et à gérer avec soin. Il s'agit plutôt d'une ressource abondante qui peut être créée au besoin pour gérer des demandes simultanées presque illimitées. Chaque demande s'exécutant dans son thread dédié, il est libre d'effectuer des opérations de blocage, telles que l'appel d'une base de données ou d'un autre service. Et il peut le faire d'une manière synchrone simple sans craindre de bloquer un fil de plate-forme et de mourir de faim d'autres demandes. Vous n'avez plus besoin de recourir à du code asynchrone complexe pour implémenter un service peu coûteux et très simultané.

Le serveur web Helidon Níma a l'intention de remplacer Netty dans l'écosystème Helidon. Il peut également être utilisé par d'autres frameworks en tant que composant de serveur Web intégré.

### Prérequis

Cet exercice suppose que vous disposez des éléments suivants :

*   Compte Oracle Cloud

## En savoir plus

*   [https://helidon.io](https://helidon.io)

## Accusés de réception

*   **Auteur** - Joe DiPol
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, février 2023
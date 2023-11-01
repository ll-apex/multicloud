# Présentation

## A propos de cet atelier

Au cours de cet atelier, vous allez utiliser Helidon Project Starter pour développer l'application de microservice HTTP. Il est hautement personnalisable et propose diverses options permettant aux utilisateurs de sélectionner les fonctionnalités Helidon qu'ils souhaitent ajouter au projet.

Ensuite, vous allez explorer l'éditeur de code OCI et y ouvrir l'application Helidon. Vous apprendrez également à utiliser GraalVM Enterprise Edition dans Oracle Cloud Infrastructure (OCI) Cloud Shell.

Vous allez créer une image native GraalVM pour une application Helidon MP à l'aide de maven. Vous allez ensuite modifier l'application en ajoutant une adresse personnalisée dans la classe Java existante. Vous créerez ensuite une image docker native de cette application et la propagerez vers Oracle Cloud Container Image Registry. Vous allez également créer une instance de calcul, extraire l'image docker ici et exécuter le conteneur docker pour cette application.

Cet atelier est conçu pour être aussi explicite que possible, mais n'hésitez pas à demander des éclaircissements ou de l'aide en cours de route.

Temps estimé : 90 minutes

### Objectifs

*   Générer une application Helidon MP
*   Explorer l'éditeur de code OCI
*   Créer une image native pour l'application Helidon MP
*   Créer une image docker native GraalVM pour l'application Helidon
*   Créer une machine virtuelle
*   Exécuter le conteneur docker pour l'application MP Helidon

### Prérequis

Cet exercice suppose que vous disposez des éléments suivants :

*   Compte Oracle Cloud

## À propos de Helidon

Helidon est une structure de microservices open source introduite par Oracle qui fournit un ensemble de bibliothèques Java conçues pour créer des applications légères et rapides basées sur des microservices. La structure prend en charge deux modèles de programmation pour l'écriture de microservices : Helidon SE et Helidon MP.

Alors que Helidon SE est conçu pour être une microstructure qui prend en charge le modèle de programmation réactive, Helidon MP est une implémentation de la spécification MicroProfile. Etant donné que MicroProfile a ses racines dans Java EE, les API MicroProfile suivent une approche déclarative familière avec une utilisation intensive des annotations. Cela en fait un bon choix pour les développeurs Java EE.

Les fonctionnalités MicroProfile visent l'implémentation de microservices. Vous pouvez trouver des API pour définir des clients REST, surveiller l'application, lire des statistiques techniques et fonctionnelles et configurer l'application. Helidon a également ajouté des API supplémentaires à l'ensemble central d'API Microprofile, vous offrant toutes les fonctionnalités dont vous avez besoin pour écrire des applications cloud natives modernes.

> La norme [MicroProfile](https://microprofile.io/) s'appuie sur Jakarta EE. Comme Jakarta EE, MicroProfile est open source et est développé par la Fondation Eclipse. L'implémentation avec MicroProfile a lieu dans les bibliothèques ou les serveurs d'applications implémentant la norme, tout comme Jakarta EE.

## En savoir plus

*   [https://helidon.io](https://helidon.io)

## Accusés de réception

*   **Auteur** - Dmitry Aleksandrov
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, avril 2023
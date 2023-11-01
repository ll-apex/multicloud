# Présentation

## A propos de cet atelier

Cet atelier présente un atelier de bout en bout sur la migration d'un domaine de serveur WebLogic sur site vers les conteneurs et le rend exécutable dans OCI avec la pile **Oracle WebLogic Suite for OKE BYOL**. Nous montrons l'interface graphique de l'interface utilisateur WebLogic Kubernetes Toolkit, ainsi que WebLogic Deployer Tool et Weblogic Kubernetes Operator. Nous montrons comment simplifier et accélérer le processus de migration à l'aide d'un ensemble d'outils orientés DevOps.

![Flux de laboratoire](images/lab-flow.png)

Temps estimé : 120 minutes

### A propos du produit/technologie

WebLogic Kubernetes Toolkit (WKT) est un ensemble d'outils open source qui vous aident à provisionner des applications basées sur WebLogic pour les exécuter dans des conteneurs Linux sur un cluster Kubernetes. WKT comprend les outils suivants :  

*   [WebLogic Deploy Tooling (WDT) :](https://github.com/oracle/weblogic-deploy-tooling) ensemble d'outils de cycle de vie à usage unique qui fonctionnent à partir d'une représentation de modèle de métadonnées unique d'un domaine WebLogic.
*   [WebLogic Image Tool (WIT)](https://github.com/oracle/weblogic-image-tool) - Outil permettant de créer des images de conteneur Linux pour exécuter des domaines WebLogic.
*   [WebLogic Opérateur Kubernetes :](https://github.com/oracle/weblogic-kubernetes-operator) opérateur Kubernetes qui permet aux domaines WebLogic de s'exécuter de manière native dans un cluster Kubernetes.

L'interface utilisateur WKT fournit une interface utilisateur graphique qui encapsule les outils WKT, Docker, Helm et le client Kubernetes (kubectl) et vous guide tout au long du processus de création et de modification d'un modèle. de votre domaine WebLogic, en créant une image de conteneur Linux à utiliser pour exécuter le domaine, ainsi qu'en configurant et en déployant le logiciel et la configuration nécessaires pour déployer le domaine dans votre cluster Kubernetes et y accéder.

### Objectifs

*   Création d'une machine virtuelle à partir d'une image Marketplace
*   Configuration d'une instance Oracle Kubernetes Engine sur Oracle Cloud Infrastructure
*   Modification d'un projet d'interface utilisateur WKT et création d'un fichier de modèle
*   Créer une image auxiliaire et la propager dans Oracle Container Image Registry
*   Déployer l'opérateur WebLogic sur le cluster Oracle Kubernetes (OKE)
*   Déployer le domaine WebLogic vers le cluster Kubernetes Oracle (OKE)
*   Déployer le contrôleur entrant vers le cluster Oracle Kubernetes (OKE)
*   Affichage de l'équilibrage de charge entre les pods de serveur géré et exploration de WebLogic-Remote-Console
*   Redimensionner un cluster WebLogic
*   Mettre à jour une application déployée par un redémarrage non simultané de la nouvelle image

## En savoir plus

*   [WebLogic Interface utilisateur de Kubernetes Toolkit](https://oracle.github.io/weblogic-toolkit-ui/)
    
*   [Oracle WebLogic Suite pour OKE](https://docs.oracle.com/en/cloud/paas/weblogic-container/user/oracle-weblogic-server-oke.html)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, octobre 2023
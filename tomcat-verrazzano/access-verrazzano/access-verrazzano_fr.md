# Utiliser Verrazzano pour accéder à l'application et la surveiller

## Présentation

Vous avez déployé l'application Tomcat. Dans cet exercice, nous allons accéder à l'application et la vérifier à l'aide de plusieurs outils de gestion fournis par Verrazzano.

Temps estimé : 15 minutes

**Grafana**

Grafana est une solution open source pour l'analyse des données, l'extraction de mesures qui donnent du sens à la quantité massive de données et pour surveiller nos applications à l'aide de jolis tableaux de bord personnalisables. L'outil permet d'étudier, d'analyser et de surveiller les données au fil du temps, techniquement appelé analyse de séries temporelles. Utile pour suivre le comportement de l'utilisateur, le comportement de l'application, la fréquence des erreurs apparaissant en production ou dans un environnement de pré-production, le type d'erreurs apparaissant et les scénarios contextuels en fournissant des données relatives.

[https://grafana.com/grafana/](https://grafana.com/grafana/)

**OpenSearch Tableaux de bord**

OpenSearch Les tableaux de bord sont un tableau de bord de visualisation pour le contenu indexé sur un cluster OpenSearch. Verrazzano crée un déploiement de tableaux de bord OpenSearch pour fournir une interface utilisateur permettant d'interroger et de visualiser les données de journal collectées dans OpenSearch.

Pour accéder à la console OpenSearch Dashboards, reportez-vous à [Accès à Verrazzano](https://verrazzano.io/latest/docs/access/).

Pour afficher les enregistrements d'un index OpenSearch via les tableaux de bord OpenSearch, créez un modèle d'index pour filtrer les enregistrements sous l'index souhaité.

Dans la tâche 3, nous allons explorer les tableaux de bord OpenSearch.

[https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)

**Prometheus**

Prometheus est une boîte à outils de surveillance et d'alerte. Prometheus collecte et stocke ses métriques sous forme de données de séries temporelles, c'est-à-dire que les informations de métriques sont stockées avec l'horodatage auquel elles ont été enregistrées, ainsi que des paires clé-valeur facultatives appelées étiquettes.

[https://prometheus.io/](https://prometheus.io/)

**Rancher**

Rancher est une plate-forme qui permet à Verrazzano d'exécuter des conteneurs sur plusieurs clusters Kubernetes en production. Il permet l'hybride, ce qui signifie une gestion centralisée des clusters sur site et hébergée sur des services cloud.

[https://rancher.com/](https://rancher.com/)

**Kiali**

Kiali est l'interface utilisateur d'Istio, et il est très simple à utiliser. Depuis la console Verrazzano, vous pouvez accéder directement à Kiali. De là, vous pouvez voir les flux de trafic et les points chauds ou les zones de problèmes. Vous pouvez ensuite effectuer une analyse descendante pour voir les détails de chacun. Kiali utilise les données de métriques d'Envoy stockées dans Prometheus pour construire le modèle graphique

### Objectifs

Dans cet exercice, vous allez :

*   Explorez la console Rancher.
*   Explorez la console Prometheus.
*   Explorez la console Grafana.
*   Explorez les tableaux de bord OpenSearch.
*   Explorez la console Kiali.
*   Explorez la console Keycloak.

### Prérequis

*   Cluster Kubernetes (OKE) exécuté sur Oracle Cloud Infrastructure.
*   Verrazzano installé sur un cluster Kubernetes (OKE).
*   Exemple d'application Tomcat déployée.

## Tâche 1 : Explorer la console Rancher

Verrazzano installe plusieurs consoles. Les adresses d'une installation sont stockées dans le champ `Status` de la ressource personnalisée Verrazzano installée.

1.  Vous pouvez obtenir les adresses de ces consoles à l'aide de la commande suivante :
    
        <copy>kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $ kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .
        {
        "consoleUrl": "https://verrazzano.default.xx.xx.xx.xx.nip.io",
        "grafanaUrl": "https://grafana.vmi.system.default.xx.xx.xx.xx.nip.io",
        "keyCloakUrl": "https://keycloak.default.xx.xx.xx.xx.nip.io",
        "kialiUrl": "https://kiali.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchDashboardsUrl": "https://osd.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchUrl": "https://opensearch.vmi.system.default.xx.xx.xx.xx.nip.io",
        "prometheusUrl": "https://prometheus.vmi.system.default.xx.xx.xx.xx.nip.io",
        "rancherUrl": "https://rancher.default.xx.xx.xx.xx.nip.io"
        }
        $
        
2.  Utilisez `https://rancher.default.YOUR_UNIQUE_IP.nip.io` pour ouvrir la console Rancher.
    
3.  Le profil _dev_ de Verrazzano utilise des certificats autosignés. Vous devez donc cliquer sur **Avancé** pour accepter le risque et ignorer l'avertissement.
    
    ![Avancé](images/rancher-advanced.png)
    
4.  Cliquez sur **Proceed to runcher default XX.XX.XX.XX.nip.io(unsafe)**. Si vous n'obtenez pas cette option pour continuer, il vous suffit de saisir _thisisunsafe_ sans espace à l'intérieur de cette fenêtre de navigateur chromée. Lorsque vous tapez dans la fenêtre du navigateur chromée, vous ne pouvez pas le voir, mais dès que vous avez fini de saisir _thisisunsafe_, vous pouvez voir la page suivante immédiatement. Vous trouverez plus de détails [ici](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Continuer](images/rancher-proceed.png)
    
5.  Cliquez sur _Log in with Keycloak_. ![Rancher la connexion](images/keycloak-login.png)
    
6.  Comme il est redirigé vers l'URL de la console Keycloak pour l'authentification, cliquez sur **Avancé**.
    
    ![Authentification Keycloak](images/keycloak-advanced.png)
    
7.  Cliquez sur **Proceed to Keycloak default XX.XX.XX.XX.nip.io(unsafe)**. Si vous n'obtenez pas cette option pour continuer, il vous suffit de saisir _thisisunsafe_ sans espace à l'intérieur de cette fenêtre de navigateur chromée. Lorsque vous tapez dans la fenêtre du navigateur chromée, vous ne pouvez pas le voir, mais dès que vous avez fini de saisir _thisisunsafe_, vous pouvez voir la page suivante immédiatement. Vous trouverez plus de détails [ici](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Continuer](images/keycloak-proceed.png)
    
8.  Maintenant, nous avons besoin du nom d'utilisateur et du mot de passe pour la console Verrazzano. Le _nom utilisateur_ est _verrazzano_. Pour connaître le mot de passe, revenez à _Cloud Shell_ et collez la commande suivante afin de connaître le mot de passe de la _console d'analyse_.
    
        <copy>kubectl get secret --namespace verrazzano-system verrazzano -o jsonpath={.data.password} | base64 --decode; echo</copy>
        
9.  Copiez le mot de passe et revenez au navigateur, où la _console d'analyse_ est ouverte. Collez le mot de passe dans le champ _Mot de passe_ et entrez _verrazzano_ sous _Nom utilisateur_, puis cliquez sur **Connexion**.
    
    ![SignIn](images/verrazzano-sign-in.png)
    
10.  Sur la page d'accueil de la console du programme d'exécution, vous pouvez afficher les liens Verrazzano. Vous pouvez accéder à l'une de ces consoles à partir du programme d'exécution console.Click _Menu Hamburger_ -> _Verrazzano_.
    
    ![Accueil Rancher](images/rancher-home.png)
    
11.  Cliquez sur _Applications_. Cette section affiche toutes les applications avec leur espace de noms et est gérée par Verrazzano. Cliquez sur l'application _tomcat-appconf_ dans l'espace de noms _tomcat-ns_. ![Application Tomcat](images/tomcat-application.png)
    
12.  Vous pouvez afficher les pods associés à l'application. Le nom du pod contient une chaîne unique générée automatiquement pour identifier cette réplique particulière. Pour afficher les journaux des pods, cliquez sur _Trois points_ -> _Visualiser les journaux_. ![Composants Tomcat](images/view-pod-logs.png)
    
13.  Vous pouvez afficher les journaux de l'application. Si vous ne voyez pas le journal de l'application, cliquez sur **Paramètres** (bouton bleu avec l'icône d'engrenage) et modifiez le filtre d'heure pour afficher toutes les entrées de journal à partir du démarrage du conteneur. Pour afficher le composant associé à l'application, cliquez sur _Composants_. ![Journaux d'application](images/view-pod-log.png)
    
14.  Cette application ne dispose que d'une seule vue component.To des ressources associées. Cliquez sur _Ressources associées_. ![Ressource Tomcat](images/tomcat-resource.png) ![Ressources Tomcat](images/tomcat-resources.png)
    
15.  Cliquez sur _Menu Hambourg_ -> _local_ pour ouvrir l'_explorateur de cluster_. L'_explorateur de cluster_ vous permet d'afficher et de manipuler toutes les ressources personnalisées et tous les CRD d'un cluster Kubernetes à partir de l'interface utilisateur de l'analyseur. ![Cluster Verrazzano](images/verrazzano-cluster.png)
    
16.  Le tableau de bord présente le cluster et les applications déployées. Le nombre de ressources appartient aux _espaces de noms utilisateur_, qui sont pratiquement toutes les ressources, y compris le système. Vous pouvez filtrer par espace de noms en haut du tableau de bord, mais ce n'est pas nécessaire maintenant. Cliquez sur l'élément **Noeuds** dans le menu de gauche pour obtenir un aperçu de la charge actuelle des noeuds.
    
    ![Explorateur de clusters](images/cluster-dashboard.png)
    
17.  L'ensemble du déploiement n'a aucun impact sur le cluster OKE. Cliquez à présent sur l'élément **Déploiement** dans le menu de gauche pour vérifier l'application déployée.
    
    ![Noeuds](images/cluster-nodes.png)
    
18.  Vous pouvez voir plusieurs déploiements.
    
    ![Déploiements](images/deployment.png)
    

## Tâche 2 : Explorer la console Grafana

1.  Cliquez sur _Menu Hambourg_ -> _Accueil_. ![Accueil](images/ranchar-menu.png)
    
2.  Sur la page d'accueil, vous verrez le lien permettant d'ouvrir la _console Grafana_. Cliquez sur le lien de la **console Grafana** comme indiqué :
    
    ![Accueil Grafana](images/grafana-link.png)
    
3.  Cliquez sur **Avancé**.
    
    ![Avancé](images/grafana-advanced.png)
    
4.  Cliquez sur **Proceed to grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)**. Si vous n'obtenez pas cette option pour continuer, il vous suffit de saisir _thisisunsafe_ sans espace à l'intérieur de cette fenêtre de navigateur chromée. Lorsque vous tapez dans la fenêtre du navigateur chromée, vous ne pouvez pas le voir, mais dès que vous avez fini de saisir _thisisunsafe_, vous pouvez voir la page suivante immédiatement. Vous trouverez plus de détails [ici](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![continuer](images/grafana-proceed.png)
    
5.  Cliquez sur l'icône _+_, puis sur _Importer_ comme indiqué ci-dessous. ![importer](images/import.png)
    
6.  Entrez la valeur _6340_ pour _Importer via grafana.com_ et cliquez sur _Charger_. ![ID de chargement](images/load-id.png)
    
7.  Sélectionnez _Prometheus_ comme source de données par défaut et cliquez sur _Importer_. ![Importer une source de données](images/import-datasource.png)
    
8.  Sélectionnez _tomcat-ns_ en tant qu'espace de noms et _tomcat-container_ en tant que conteneur. Vous pourrez alors voir la _portion de mémoire utilisée_, la _mémoire validée_ et d'autres données comme indiqué ci-dessous. ![sélectionner un espace de noms](images/select-namespace.png)
    
    > Dans Grafana.com, vous pouvez obtenir plus d'ID à importer dans la console grafana.
    

## Tâche 3 : Explorer les tableaux de bord OpenSearch

1.  Revenez à la page d'accueil de Verrazzano et cliquez sur la console **OpenSearch Dashboards**.
    
    ![Lien Kibana](images/opensearch-link.png)
    
2.  Cliquez sur _Passer à ... par défaut XX.XX.XX.XX.nip.io(unsafe)_ si nécessaire. La première fois que _OpenSearch Dashboards_ affiche la page de bienvenue. Il propose des exemples de données intégrés pour essayer OpenSearch, mais vous pouvez sélectionner l'option **Explorer par moi-même** car Verrazzano a terminé la configuration nécessaire et les données d'application sont déjà disponibles.
    
    ![Page de bienvenue Kibana](images/opensearch-proceed.png)
    
3.  Sur la page d'accueil OpenSearch, cliquez sur **Accueil** -> **Repérer**.
    
    ![Cliquez sur le tableau de bord Kibana](images/discover-1.png)
    
4.  Appelez la demande HTTP suivante dans Cloud Shell sur votre adresse. Vous pouvez exécuter des demandes plusieurs fois.
    
        <copy>curl -k https://$(kubectl get gateways.networking.istio.io tomcat-ns-tomcat-appconf-gw -n tomcat-ns -o jsonpath={.spec.servers[0].hosts[0]})/sample-webapp/log; echo</copy>
        
5.  Sélectionnez l'espace de noms _`verrazzano-application*`_ comme indiqué, puis saisissez la valeur d'entrée de journal personnalisée que vous avez créée dans l'application tomcat : `Logs` dans la zone de texte du filtre. Appuyez sur **Entrée** ou cliquez sur **Actualiser**. Vous devriez obtenir au moins un résultat. ![Résultat du journal](images/log-result.png)
    

## Tâche 4 : Explorer la console Prometheus

1.  Revenez à la page d'accueil de Verrazzano et cliquez sur la console **Prometheus**.
    
    ![Lien Prométhée](images/prometheus-link.png)
    
2.  Cliquez sur **Passer à ... par défaut XX.XX.XX.XX.nip.io(unsafe)** si vous y êtes invité.
    
3.  Sur la page du tableau de bord Prometheus, saisissez _tomcat_ dans le champ de recherche, cliquez sur la mesure _tomcat\_requestcount\_total_, puis sur _Exécuter_.
    
    ![Exécution de Prométhée](images/prometheus-query.png)
    
4.  Cliquez sur **Exécuter** et vérifiez le résultat ci-dessous. Vous devez voir la valeur en cours de votre mesure, ce qui signifie le nombre de demandes terminées par votre adresse. Vous pouvez également passer à la vue _Graphique_ au lieu du mode _Console_.
    
    ![Valeur de Prométhée](images/execute-query.png)
    
    > Vous pouvez également ajouter une autre mesure à votre tableau de bord. Repérez les mesures disponibles par défaut dans la liste.
    

## Tâche 5 : Explorer la console Kiali

1.  Revenez à la page d'accueil de Verrazzano et cliquez sur la console **Kiali**.
    
    ![Lien Rancher](images/kiali-link.png)
    
2.  Cliquez sur **Passer à ... par défaut XX.XX.XX.XX.nip.io(unsafe)** si vous y êtes invité.
    
3.  Sur le côté gauche, cliquez sur Graph.
    
    ![Tableau de bord Kiali](images/kiali-dashboard.png " ")
    
4.  Dans la liste déroulante Espace de noms, cochez la case _verrazzano-system_ et faites en sorte que le curseur se déplace en dehors de la liste déroulante. ![Espace de noms Bobs](images/verrazzano-namespace.png " ")
    
5.  Vous pouvez afficher la vue graphique de l'espace de noms _verrazzano-system_. Cliquez sur _Légende_ pour afficher la vue Légende.
    
    ![Vue graphique](images/graphical-view.png " ")
    
6.  Ici, vous pouvez voir ce que chaque forme représente, comme le cercle représente les _charges globales_.
    
    ![Vue Légende](images/legend-view.png " ")
    
7.  Sur la gauche, cliquez sur _Applications_ pour visualiser les applications déployées.
    
    ![Applications](images/applications.png " ")
    

## Tâche 6 : Explorer la console Keycloak

1.  Revenez à la page d'accueil de Verrazzano et cliquez sur la console **Keycloak**.
    
    ![Lien Keycloak](images/keycloak-link.png)
    
2.  Cliquez sur **Passer à ... par défaut XX.XX.XX.XX.nip.io(unsafe)** si vous y êtes invité.
    
3.  Sur la page Bienvenue dans Keycloak, cliquez sur _Console d'administration_. ![Maison Keycloak](images/keycloak-home.png)
    
4.  Maintenant, nous avons besoin du nom d'utilisateur et du mot de passe pour la console Keycloak. Le _nom utilisateur_ est _keycloakadmin_. Pour connaître le mot de passe, revenez à _Cloud Shell_ et collez la commande suivante afin de connaître le mot de passe de la _console Keycloak_.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  Copiez le mot de passe et revenez au navigateur, où la _console Keycloak_ est ouverte. Collez le mot de passe dans le champ _Password_ et entrez _keycloakadmin_ sous _Username_, puis cliquez sur **Sign In**.
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  Ici, vous pouvez afficher la configuration par défaut effectuée par Verrazzano. ![Accueil Keycloak](images/keycloak-realms.png)
    

Félicitations, vous avez terminé le déploiement de l'application tomcat sur le laboratoire Verrazzano.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
# Accéder à l'exemple d'application Bobby's Books Explorer Verrazzano, Grafana et la console Kiali

## Présentation

Dans Lab 3, nous avons déployé l'application Bobby's Book. Dans cet exercice, nous allons accéder à l'application et vérifier qu'elle fonctionne. Plus tard, nous explorerons les consoles Verrazzano et Grafana.

Durée estimée : 10 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Accédez à l'application Livre de Bobby.
*   Explorez la console Verrazzano.
*   Explorez la console Grafana.

### Prérequis

\* Exécutez l'atelier 1, qui crée un cluster OKE sur Oracle Cloud Infrastructure. \* Exécutez l'atelier 1, qui configure kubectl pour accéder à un cluster OKE sur Oracle Cloud Infrastructure.

*   Exécutez Lab 2, qui installe Verrazzano sur un cluster Kubernetes.
*   Exécutez Lab 3, qui déploie l'application Bobby's Book.
*   Vous devez disposer d'un éditeur de texte, dans lequel vous pouvez coller les commandes et les URL et les modifier, conformément à votre environnement. Vous pouvez ensuite copier et coller les commandes modifiées pour les exécuter dans _Cloud Shell_.
*   Nous vous recommandons d'utiliser Firefox pour ouvrir les URL de l'application Bobby's Books, Verrazano, Grafana et Kiali Console. Cependant, si vous souhaitez utiliser Chrome, vous devez utiliser la solution de contournement "thisisunsafe" non documentée pour permettre au chrome d'accepter le certificat.

## Tâche 1 : accéder à l'application Bobby's Book

1.  Nous avons besoin d'une adresse `EXTERNAL_IP` à travers laquelle nous pouvons accéder à l'application Bobby's Book. Pour obtenir l'adresse `EXTERNAL_IP` du service istio-ingressgateway, copiez la commande suivante et collez-la dans _Cloud Shell_.
    
        <copy> kubectl get service \
        -n "istio-system" "istio-ingressgateway" \
        -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo</copy>
        
    
    La sortie doit ressembler à ceci :
    
           $ kubectl get service \
           > -n "istio-system" "istio-ingressgateway" \
           > -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo
           XX.XX.XX.XX
        
2.  Pour ouvrir la page d'accueil de Robert's Book Store, copiez l'URL suivante et remplacez _XX.XX.XX.XX_ par l'adresse _EXTERNAL\_IP_ que nous avons obtenue à la dernière étape, comme indiqué dans l'image suivante.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/</copy>
        
    
    ![Page des livres Roberts](images/robertbooks.png " ")
    
3.  Cliquez sur _Avancé_, comme indiqué ci-dessous :
    
    ![Robert Advanced](images/robertadvanced.png " ")
    
4.  Sélectionnez _Passer à bobs-books.bobs-books. EXTERNAL\_IP .nip.io(non sécurisé)_ pour accéder à l'application. Si vous n'obtenez pas cette option pour continuer, tapez simplement _thisisunsafe_ sans aucun espace à l'intérieur de cette fenêtre de navigateur chromée. Lorsque vous tapez dans la fenêtre du navigateur chromé, vous ne pouvez pas le voir, mais dès que vous avez fini de saisir _thisisunsafe_, vous pouvez voir la page de l'application immédiatement. Vous trouverez plus de détails [ici](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Robert Unsafe](images/robertunsafe.png " ") ![Robert Page](images/robertpage.png " ")
    
5.  Pour ouvrir la page d'accueil de Bobby's Book Store, ouvrez un nouvel onglet et copiez l'URL suivante et remplacez _XX.XX.XX.XX_ par votre adresse `EXTERNAL_IP`, comme indiqué dans l'image suivante.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![URL Bobbys](images/bobbysurl.png " ")
    
    ![Librairie Bobby](images/bobbysbookstore.png " ")
    
    > Laissez cette page ouverte car nous l'utiliserons dans l'atelier 8.
    
6.  Pour ouvrir l'interface utilisateur de Bobby's Book Order Manager, ouvrez un nouvel onglet et copiez l'URL suivante et remplacez _XX.XX.XX.XX_ par votre adresse _EXTERNAL\_IP_ comme indiqué dans l'image suivante.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobs-bookstore-order-manager/orders</copy>
        
    
    ![responsable des commandes](images/ordermanager.png " ")
    
7.  Revenez à la page _Livres de Bobby_ et achetons un livre. Cliquez sur _Livres_ comme indiqué dans l'image suivante.
    
    ![Extraire la commande](images/checkoutorder.png " ")
    
8.  Sélectionnez l'image de la liasse _Twilight_, comme indiqué dans l'image suivante.
    
    ![Livre d'achat](images/purchasebook.png " ")
    
9.  Tout d'abord, cliquez sur _Ajouter au panier_, puis sur _Extraire_ comme indiqué dans l'image suivante.
    
    ![Cliquez sur addcart](images/clickaddcart.png " ")
    
10.  Saisissez le détail de l'achat du livre. Dans _Votre état_, entrez votre code d'état à deux chiffres, puis cliquez sur _Soumettre la commande_.
    

![Soumettre la commande](images/submitorder.png " ") 11. Revenez à la page _Gestionnaire de commandes_ et cliquez sur le bouton _Actualiser_ pour vérifier si votre commande a été enregistrée avec succès dans le gestionnaire de commandes.

![Vérifier la commande](images/verify-order.png " ")

## Tâche 2 : Explorer la console Rancher

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
    
10.  Sur la page d'accueil de la console de runcher, vous pouvez afficher les liens Verrazzano. Cliquez sur _Menu Hamburger_ -> _Verrazzano_.
    
    ![Accueil Rancher](images/rancher-home.png)
    
11.  Cliquez sur _Applications_. Cette section affiche toutes les applications avec leur espace de noms et est gérée par Verrazzano. Cliquez sur l'application _bobs-books_ dans l'espace de noms _bobs-books_. ![Application Bobs](images/bobs-application.png)
    
12.  Vous pouvez afficher les pods associés à l'application. Le nom du pod contient une chaîne unique générée automatiquement pour identifier cette réplique particulière. Pour afficher les journaux des pods _bobbys-helidon-stok-application_, cliquez sur _Trois points_ -> _Visualiser les journaux_. ![Bobbys Logs](images/bobs-logs.png)
    
13.  Vous pouvez afficher les journaux de l'application. Si vous ne voyez pas le journal de l'application, cliquez sur **Paramètres** (bouton bleu avec l'icône d'engrenage) et modifiez le filtre d'heure pour afficher toutes les entrées de journal à partir du démarrage du conteneur. Pour afficher le composant associé à l'application, cliquez sur _Composants_. ![Bobbys Log](images/bobs-log.png)
    
14.  Vous pouvez visualiser tous les composants de l'application _bobs-books_ ici. Pour visualiser les ressources associées, cliquez sur _Ressources associées_. ![Ressources Bobs](images/bobs-resource.png) ![Ressources Bobs](images/bobs-resources.png)
    
15.  Cliquez sur _Menu Hambourg_ -> _local_ pour ouvrir l'_explorateur de cluster_. L'_explorateur de cluster_ vous permet d'afficher et de manipuler toutes les ressources personnalisées et tous les CRD d'un cluster Kubernetes à partir de l'interface utilisateur de l'analyseur. ![Cluster Verrazzano](images/verrazzano-cluster.png)
    
16.  Le tableau de bord présente le cluster et les applications déployées. Le nombre de ressources appartient aux _espaces de noms utilisateur_, qui sont pratiquement toutes les ressources, y compris le système. Vous pouvez filtrer par espace de noms en haut du tableau de bord, mais ce n'est pas nécessaire maintenant. Cliquez sur l'élément **Noeuds** dans le menu de gauche pour obtenir un aperçu de la charge actuelle des noeuds.
    
    ![Explorateur de clusters](images/cluster-dashboard.png)
    
17.  L'ensemble du déploiement n'a aucun impact sur le cluster OKE. Cliquez à présent sur l'élément **Déploiement** dans le menu de gauche pour vérifier les applications déployées.
    
    ![nœuds clustr](images/cluster-nodes.png)
    
18.  Vous pouvez voir plusieurs déploiements.
    
    ![Déploiements](images/cluster-deployments.png)
    

## Tâche 3 : Explorer la console Grafana

1.  Cliquez sur _menu Hambourg_ -> _Accueil_ pour ouvrir la page d'accueil de Rancher. ![Accueil](images/ranchar-menu.png)
    
2.  Sur la page d'accueil, vous verrez le lien permettant d'ouvrir la _console Grafana_. Cliquez sur le lien de la **console Grafana** comme indiqué :
    
    ![Accueil Grafana](images/grafana-link.png)
    
3.  Cliquez sur _Avancé_.
    
    ![Grafana avancé](images/grafanaadvanced.png " ")
    
4.  Sélectionnez _grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)_. Si vous n'obtenez pas cette option pour continuer, tapez simplement _thisisunsafe_ sans espace.
    
    ![Grafana continuer](images/grafanaproceed.png " ")
    
5.  La page d'accueil Grafana apparaît. Sélectionnez _Accueil_ en haut à gauche.
    
    ![Page d'accueil Grafana](images/grafana-homepage.png " ")
    
6.  Saisissez _WebLogic_ et vous verrez _WebLogic Server Dashboard_ sous _General_. Sélectionnez _WebLogic Tableau de bord du serveur_.
    
    !\[Rechercher WebLogic\](images/search-weblogic.png")
    
    Ici, vous pouvez observer les deux domaines sous _Domaine_ et Serveurs en cours d'exécution, Applications déployées, Nom du serveur et leur statut, Utilisation de la portion de mémoire, Temps d'exécution, Portion de mémoire JVM. Si votre application dispose de ressources telles que JDBC et JMS, vous pouvez également obtenir des détails à ce sujet ici.
    
    ![WebLogic Tableau de bord](images/weblogic-dashboard.png " ")
    
7.  Maintenant, sélectionnez WebLogic Tableau de bord du serveur et saisissez _Helidon_. Vous verrez _Tableau de bord de surveillance Helidon_. Sélectionnez _Tableau de bord de surveillance Helidon_.
    
    ![Helidon](images/search-helidon.png " ")
    
    Ici, vous pouvez voir différents détails tels que le _statut_ de votre application et son _temps d'activité_, le processus Garbage Collector et le total de marquage du nivellement, ainsi que son temps et son nombre de threads.
    
    ![Tableau de bord Helidon](images/helidon-dashboard.png " ")
    
8.  Maintenant, sélectionnez Tableau de bord de surveillance Helidon et saisissez _Coherence_. Vous verrez _Tableau de bord Coherence principal_. Sélectionnez _Tableau de bord Coherence - Principal_.
    
    ![Coherence](images/search-coherence.png " ")
    
9.  Ici, vous pouvez voir les détails du _cluster Coherence_. Pour l'application Bobby's Books, nous avons deux clusters Coherence, l'un pour Bobby's Books et l'autre pour Robert's Books. Vous devez sélectionner le menu déroulant _Nom de cluster_ pour visualiser les deux clusters.
    
    ![Groupe Bobby](images/bobby-cluster.png " ")
    
    ![Robert Cluster](images/robert-cluster.png " ")
    

## Tâche 4 : Explorer la console Kiali

1.  Revenez à la page d'accueil de Verrazzano et cliquez sur la console **Kiali**.
    
    ![Lien Rancher](images/kiali-link.png)
    
2.  Sur le côté gauche, cliquez sur Graph.
    
    ![Tableau de bord Kiali](images/kiali-dashboard.png " ")
    
3.  Dans la liste déroulante Espace de noms, cochez la case _bobs-books_ et faites en sorte que le curseur se déplace en dehors de la liste déroulante. ![Espace de noms Bobs](images/bobs-namespace.png " ")
    
4.  Vous pouvez visualiser la vue graphique de l'application _bobs-books_. Cliquez sur _Légende_ pour afficher la vue Légende.
    
    ![Vue graphique](images/graphical-view.png " ")
    
5.  Ici, vous pouvez voir ce que chaque forme représente, comme le cercle représente les _charges globales_.
    
    ![Vue Légende](images/legend-view.png " ")
    
6.  Sur la gauche, cliquez sur _Applications_ pour visualiser toutes les applications déployées.
    
    ![Applications](images/kiali-applications.png " ")
    

## Tâche 5 : Explorer les tableaux de bord OpenSearch

1.  Revenez à la page d'accueil de Verrazzano et cliquez sur la console **OpenSearch Dashboards**.
    
    ![Lien Kibana](images/opensearch-link.png)
    
2.  Cliquez sur _Passer à ... par défaut XX.XX.XX.XX.nip.io(unsafe)_ si nécessaire.
    
3.  Sur la page d'accueil OpenSearch, cliquez sur **Accueil** -> **Repérer**.
    
    ![Cliquez sur le tableau de bord Kibana](images/discover-1.png)
    
4.  Sélectionnez l'espace de noms _`verrazzano-applications*`_ comme indiqué, puis recherchez des _livres_ et appuyez sur **Entrée** ou cliquez sur **Actualiser**. Vous devez obtenir les journaux contenant des _livres_. ![Résultat du journal](images/search-books.png)
    

## Tâche 6 : Explorer la console Prometheus

1.  Revenez à la page d'accueil de Verrazzano et cliquez sur la console **Prometheus**.
    
    ![Lien Prométhée](images/prometheus-link.png)
    
2.  Cliquez sur **Passer à ... par défaut XX.XX.XX.XX.nip.io(unsafe)** si vous y êtes invité.
    
3.  Dans le tableau de bord Prometheus, saisissez _get_ dans le champ de recherche et cliquez sur la mesure personnalisée _`application_org_books_bobby_BookResource_getBook_total`_.
    
    ![Exécution de Prométhée](images/prometheus-query.png)
    
4.  Cliquez sur **Exécuter** et vérifiez le résultat ci-dessous. Vous devez voir la valeur en cours de votre mesure, ce qui signifie le nombre de demandes terminées par votre adresse. Vous pouvez également passer à la vue _Graphique_ au lieu du mode _Console_.
    
    ![Valeur de Prométhée](images/execute-query.png)
    
    > Vous pouvez également ajouter une autre mesure à votre tableau de bord. Repérez les mesures disponibles par défaut dans la liste.
    

## Tâche 7 : Explorer la console Keycloak

1.  Revenez à la page d'accueil de Verrazzano et cliquez sur la console **Keycloak**.
    
    ![Lien Keycloak](images/keycloak-link.png)
    
2.  Cliquez sur **Passer à ... par défaut XX.XX.XX.XX.nip.io(unsafe)** si vous y êtes invité.
    
3.  Sur la page Bienvenue dans Keycloak, cliquez sur _Console d'administration_. ![Maison Keycloak](images/keycloak-home.png)
    
4.  Maintenant, nous avons besoin du nom d'utilisateur et du mot de passe pour la console Keycloak. Le _nom utilisateur_ est _keycloakadmin_. Pour connaître le mot de passe, revenez à _Cloud Shell_ et collez la commande suivante afin de connaître le mot de passe de la _console Keycloak_.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  Copiez le mot de passe et revenez au navigateur, où la _console Keycloak_ est ouverte. Collez le mot de passe dans le champ _Password_ et entrez _keycloakadmin_ sous _Username_, puis cliquez sur **Sign In**.
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  Ici, vous pouvez afficher la configuration par défaut effectuée par Verrazzano. ![Accueil Keycloak](images/keycloak-realms.png)
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
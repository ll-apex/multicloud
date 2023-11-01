# ALTERNATE VERSION avec solution simplifiée

## Présentation

Dans cet exercice, nous allons utiliser l'image bobbys-helidon-stock-application modifiée. Cette image est stockée dans notre référentiel public dans Oracle Cloud Container Registry. Nous utilisons également le fichier bobs-books-comp-mod.yaml modifié, qui pointe vers l'image bobbys-helidon-stock-application modifiée.

### Objectifs

Dans cet exercice, vous allez :

*   Téléchargez le fichier bobs-books-comp-mod.yaml modifié.
*   Appliquer les modifications à l'aide de kubectl

### Prérequis

*   Exécutez l'atelier 1, qui crée un cluster OKE sur Oracle Cloud Infrastructure.
*   Exécutez Lab 2, qui installe Verrazzano sur le cluster Kubernetes.
*   Exécutez Lab 3, qui déploie l'application Bobs-Books.
*   Vous devez disposer d'un éditeur de texte, dans lequel vous pouvez coller les commandes et les URL et les modifier, conformément à votre environnement. Vous pouvez ensuite copier et coller les commandes modifiées pour les exécuter dans _Cloud Shell_.

## Tâche 1 : télécharger le fichier bobs-books-comp-mod.yaml modifié

1.  Exécutez la commande suivante pour télécharger le fichier bobs-books-comp-mod.yaml modifié.
    
        <copy>curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/alternate-version/bobs-books-comp-mod.yaml >~/bobs-books-comp-mod.yaml</copy>
        
    
    ![Télécharger le fichier](images/downloadfile.png " ")
    

## Tâche 2 : appliquer les modifications à l'aide de kubectl

1.  Pour appliquer les modifications, copiez et collez la commande suivante dans _Cloud Shell_. Lorsque vous appliquerez la modification, un nouveau pod sera initialisé pour traiter les demandes de nouveau composant, tandis que le pod associé à l'ancien composant continuera à traiter les demandes. Plus tard, une fois que le nouveau pod atteint l'état _En cours d'exécution_, l'ancien pod commence à être _interrompu_. Finalement, seul le nouveau pod sera à l'état _En cours d'exécution_.
    
        <copy>kubectl apply -f ~/bobs-books-comp-mod.yaml -n bobs-books</copy>
        
    
    ![Appliquer les modifications](images/applychanges.png " ")
    
    Vous pouvez observer dans la sortie ; seul _component.core.oam.dev/bobby-helidon_ est configuré et les autres composants ne sont pas modifiés.
    
2.  Pour visualiser la façon dont le nouveau pod est initialisé et dont l'ancien est à l'état _Terminaison_, copiez et collez la commande suivante dans _Cloud Shell_.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    La sortie doit ressembler à ce qui suit :
    
        vera_zano@cloudshell:~ (us-ashburn-1)$ kubectl get pods -n bobs-books
        NAME                                               READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                                 2/2    Running  0         130m
        bobbys-front-end-adminserver                       4/4    Running  0         127m
        bobbys-front-end-managed-server1                   4/4    Running  0         126m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  0/2    PodInitializing  0         10s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Running  0         130m
        bobs-bookstore-adminserver                         4/4    Running  0         127m
        bobs-bookstore-managed-server1                     4/4    Running  0         126m
        mysql-65d864bf8c-xf64p                             2/2    Running  0         130m
        robert-helidon-bfdfb58b8-58qfs                     2/2    Running  0         130m
        robert-helidon-bfdfb58b8-lkw8m                     2/2    Running  0         130m
        roberts-coherence-0                                2/2    Running  0         130m
        roberts-coherence-1                                2/2    Running  0         130m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  1/2    Running  0         28s
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  2/2    Running  0         34s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        vera_zano@cloudshell:~ (us-ashburn-1)$
        
    
    Une fois que vous constatez que tous les pods ont le statut _Running_ (En cours d'exécution), appuyez sur _CTRL + C_ pour arrêter cette commande.
    

Laissez _Cloud Shell_ ouvert car nous en avons également besoin pour notre dernier atelier.

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, février 2023
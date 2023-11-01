# Modification du fichier YAML de configuration des livres de Bobby

## Présentation

Dans l'atelier 6, nous avons propagé l'image Docker mise à jour pour _bobby-helidon-stock-application_. Désormais, nous voulons que Verrazzano redéploie l'application et les composants mis à jour sans affecter les services. Pour cela, nous devons configurer le fichier YAML afin que Verrazzano récupère la nouvelle image et démarre un pod pour elle. Une fois que le pod est à l'état _En cours d'exécution_, il met fin au pod associé à l'application et aux composants précédents.

Durée estimée : 10 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Modifiez le fichier `bobs-books-comp.yaml`.
*   Appliquez les modifications à l'aide de `kubectl`.

### Prérequis

Vous devez disposer d'un éditeur de texte, dans lequel vous pouvez coller les commandes et les URL et les modifier, conformément à votre environnement. Vous pouvez ensuite copier et coller les commandes modifiées pour les exécuter dans _Cloud Shell_.

## Tâche 1 : modifier le fichier bobs-books-comp.yaml

1.  Nous disposons d'un fichier de configuration d'application, _bobs-books-comp.yaml_. Dans Lab 2, nous avons téléchargé les fichiers yaml de l'application. Pour passer au répertoire de base contenant le fichier yaml, copiez la commande suivante et collez-la dans _Cloud Shell_.
    
        <copy>cd ~</copy>
        
2.  A cet emplacement, nous avons le fichier de configuration pour l'application bobs-books. Dans le cadre du laboratoire 5, nous avons modifié l'application bobbys-helidon-stock et créé une nouvelle image Docker pour ce composant. Au cours de l'atelier 6, nous avons propagé cette image Docker vers le référentiel Oracle Cloud Container Registry. Dans cet exercice, nous allons modifier le fichier _bobs-books-comp.yaml_ de sorte qu'il prenne la nouvelle image Docker mise à jour du référentiel Oracle Cloud Container Registry. Pour modifier le fichier _bobs-books-comp.yaml_, copiez la commande suivante et collez-la dans _Cloud Shell_.
    
        <copy>vi bobs-books-comp.yaml</copy>
        
    
    ![Ouvrir le fichier](images/openfile.png " ")
    
3.  Dans le cadre de l'atelier 5, vous avez enregistré le nom complet de l'image Docker. Vous devez copier la ligne suivante et la coller dans votre éditeur de texte. Vous devez ensuite remplacer `docker image full name` par le nom de l'image Docker. Copiez ensuite la ligne modifiée et appuyez sur _i_ pour insérer le texte dans le fichier `*bobs-books-comp.yaml*`. Collez la sortie à la ligne numéro 148 (assurez-vous de conserver l'indentation) et commentez la sortie de la ligne numéro 147 avec _#_ comme indiqué sur l'image suivante, puis appuyez sur _Echap_ et tapez _:wq_ pour enregistrer le fichier.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Insérer une ligne](images/insert-line.png " ")
    

## Tâche 2 : appliquer les modifications à l'aide de `kubectl`

1.  Pour appliquer les modifications, copiez et collez la commande suivante dans _Cloud Shell_. Lorsque vous appliquerez la modification, un nouveau pod sera initialisé pour traiter les demandes de nouveau composant, tandis que le pod associé à l'ancien composant continuera à traiter les demandes. Plus tard, une fois que le nouveau pod atteint l'état _En cours d'exécution_, l'ancien pod commence à être _interrompu_. Finalement, seul le nouveau pod sera à l'état _En cours d'exécution_.
    
        <copy>kubectl apply -f bobs-books-comp.yaml -n bobs-books</copy>
        
    
    La sortie doit ressembler à ceci :
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh unchanged
        component.core.oam.dev/robert-helidon unchanged
        component.core.oam.dev/bobby-coh unchanged
        component.core.oam.dev/bobby-helidon configured
        component.core.oam.dev/bobby-wls unchanged
        component.core.oam.dev/bobs-mysql-configmap unchanged
        component.core.oam.dev/bobs-mysql-service unchanged
        component.core.oam.dev/bobs-mysql-deployment unchanged
        component.core.oam.dev/bobs-orders-configmap unchanged
        component.core.oam.dev/bobs-orders-wls unchanged
        $
        
    
    Vous pouvez observer dans la sortie ; seul _component.core.oam.dev/bobby-helidon_ est configuré et les autres composants ne sont pas modifiés.
    
2.  Pour visualiser la façon dont le nouveau pod est initialisé et dont l'ancien est à l'état _Terminaison_, copiez et collez la commande suivante dans _Cloud Shell_.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    La sortie doit ressembler à ce qui suit :
    
        $ kubectl get pods -n bobs-books
        NAME                                         READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                           2/2    Running      0         130m
        bobbys-front-end-adminserver                 4/4    Running      0         127m
        bobbys-front-end-managed-server1             4/4    Running      0         126m
        bobbys-helidon-stock-application-64fb55-zzp  0/2    PodInitializing  0     10s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Running      0         130m
        bobs-bookstore-adminserver                   4/4    Running      0         127m
        bobs-bookstore-managed-server1               4/4    Running      0         126m
        mysql-65d864bf8c-xf64p                       2/2    Running      0         130m
        robert-helidon-bfdfb58b8-58qfs               2/2    Running      0         130m
        robert-helidon-bfdfb58b8-lkw8m               2/2    Running      0         130m
        roberts-coherence-0                          2/2    Running      0         130m
        roberts-coherence-1                          2/2    Running      0         130m
        bobbys-helidon-stock-application-64fb55-zzp  1/2    Running      0         28s
        bobbys-helidon-stock-application-64fb55-zzp  2/2    Running      0         34s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        $
        
    
    Une fois que vous constatez que tous les pods ont le statut _Running_ (En cours d'exécution), appuyez sur _CTRL + C_ pour arrêter cette commande.
    
    Laissez _Cloud Shell_ ouvert car nous en avons également besoin pour notre dernier atelier.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
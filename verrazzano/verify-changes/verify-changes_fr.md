# Vérifier les modifications via l'accès à l'application et la pile organisée

## Présentation

Au cours de l'atelier 7, nous avons appliqué les modifications apportées à l'application bobbys-helidon-stock et le pod correspondant est à l'état _Running_. Dans cet exercice, nous allons vérifier les modifications apportées à l'application, à la console Verrazzano et à la console Grafana.

Durée estimée : 05 minutes

### Objectifs

Dans cet exercice, vous allez :

*   Vérifiez les modifications dans l'application Bobby's Books.
*   Vérifiez les modifications dans la console Grafana.

### Prérequis

*   Vous devez disposer d'un éditeur de texte, dans lequel vous pouvez coller les commandes et les URL et les modifier, conformément à votre environnement. Vous pouvez ensuite copier et coller les commandes modifiées pour les exécuter dans _Cloud Shell_.

## Tâche 1 : vérifier les modifications apportées à l'application Livres de Bobby

1.  Ouvrez l'onglet Livres de Bobby et sélectionnez Actualiser. Si vous avez fermé cet onglet, copiez et collez la commande suivante dans l'éditeur de texte et remplacez XX.XX.XX.XX par EXTERNAL\_IP pour l'application. Vous remarquerez que le nom du livre est en majuscules.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![Livre de Bobby](images/bobbysbooks.png " ")
    

## Tâche 2 : vérifier les modifications dans la console Grafana

1.  Retournez à la page d'accueil de Verrazzano.
    
    ![Accueil Verrazzano](images/verrazzao-home.png " ")
    
2.  Sélectionnez le lien Grafana pour ouvrir la _console Grafana_.
    
    ![Lien Grafana](images/grafana-link.png " ")
    
3.  Sélectionnez _Accueil_, saisissez _Helidon_, puis sélectionnez _Tableau de bord de surveillance Helidon_.
    
    ![Rechercher dans Helidon](images/search-helidon.png " ")
    
4.  Dans ServiceID, sélectionnez _bobs-books\_default\_bobs-books\_bobby-helidon_ et, dans l'instance, sélectionnez l'instance nouvellement créée. Dans ce cas, vous obtiendrez des informations sur l'application _bobby-helidon-stock-application_ modifiée.
    
    ![Nouveau composant](images/new-component.png " ")
    
    Félicitations ! Vous avez terminé les exercices.
    

## Accusés de réception

*   **Auteur** - Ankit Pandey
*   **Contributeurs** - Maciej Gruszka, Sid Joshi
*   **Dernière mise à jour par/date** - Ankit Pandey, août 2023
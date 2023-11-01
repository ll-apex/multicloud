# Nettoyage des ressources OCI (facultatif)

## Présentation

Cet atelier vous guide tout au long des étapes de nettoyage de la ressource OCI créée dans cet atelier, lorsque l'environnement n'est pas nécessaire.

Durée estimée : 15 minutes

Regardez la vidéo ci-dessous pour une présentation rapide de l'atelier. [Nettoyez les ressources](videohub:1_cyuzrf2g)

### Objectifs

Dans cet exercice, vous allez :

*   Destruction des ressources créées pour le projet DevOps
*   Nettoyez le compartiment, les groupes dynamiques, les groupes d'utilisateurs et les stratégies.

### Prérequis

*   Un compte cloud Oracle Free Tier (essai), payant ou LiveLabs

## Tâche 1 : détruire les ressources créées pour le projet DevOps

1.  Pour ouvrir le projet _`devops_helidon_to_instance_ocw_hol`_ dans l'**éditeur de code**, cliquez sur _Fichier_ -> _Ouvrir_. ![ouvrir le fichier](images/open-file.png)
    
2.  Cliquez sur la _flèche vers le haut_ pour accéder au dossier parent, puis sélectionnez le dossier **`devops_helidon_to_instance_ocw_hol`** et cliquez sur _Ouvrir_. ![ouvrir devops](images/open-devops.png)
    
3.  Ouvrez un nouveau terminal, copiez et collez la commande suivante pour accéder au dossier **`devops_helidon_to_instance_ocw_hol`**.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main</copy>
        
4.  Copiez et collez la commande suivante pour détruire les ressources liées au projet **DevOps**, telles que la rubrique, le référentiel de code, etc. Observez la sortie d'une commande pour connaître les ressources qu'elle supprime.
    
        <copy>./destroy.sh</copy>
        Deleting all artifacts from 'artifact-repo-helidon-ocw-hol'
        Deleted 6 artifacts
        
        Deleting all objects from 'app-bucket-helidon-ocw-hol-UkUz'
        Deleted 1 objects
        
        Begin Terraform destroy...
        
        tls_private_key.public_private_key_pair: Refreshing state... [id=2aab23ed7159c8162b122d90fee8d5e1ebeda044]
        random_string.random_value: Refreshing state... [id=UkUz]
        data.oci_core_images.compute_instance_images: Reading...
        oci_core_virtual_network.vcn: Refreshing state... [id=ocid1.vcn.oc1.ap-hyderabad-1.ama]
        oci_logging_log_group.devops_log_group: Refreshing state... [id=ocid1.loggroup.oc1.ap-hyderabad-1.ama]
        data.oci_objectstorage_namespace.object_storage_namespace: Reading...
        oci_logging_log_group.application_log_group: Refreshing state... [id=ocid1.loggroup.oc1.ap-hyderabad-1.amaa]
        data.oci_identity_availability_domains.ads: Reading...
        oci_ons_notification_topic.devops_notification_topic: Refreshing state... [id=ocid1.onstopic.oc1.ap-hyderabad-1.aaaaaa]
        oci_artifacts_repository.artifact_repo: Refreshing state... [id=ocid1.artifactrepository.oc1.ap-hyderabad-1.0.amaa]
        data.oci_objectstorage_namespace.object_storage_namespace: Read complete after 0s [id=ObjectStorageNamespaceDataSource-1900185788]
        oci_objectstorage_bucket.application_bucket: Refreshing state... [id=n/axebow4vbabr/b/app-bucket-helidon-ocw-hol-UkUz]
        data.oci_identity_availability_domains.ads: Read complete after 0s [id=IdentityAvailabilityDomainsDataSource-2783498200]
        oci_logging_log.application_log: Refreshing state... [id=ocid1.log.oc1.ap-hyderabad-1.amaaa]
        oci_devops_project.devops_project: Refreshing state... [id=ocid1.devopsproject.oc1.ap-hyderabad-1.amaa]
        oci_core_internet_gateway.ig: Refreshing state... [id=ocid1.internetgateway.oc1.ap-hyderabad-1.aaaa]
        oci_core_security_list.sl: Refreshing state... [id=ocid1.securitylist.oc1.ap-hyderabad-1.aaaa]
        
    
    > L'exécution de ce script prend 16 minutes.
    

## Tâche 2 : nettoyage du compartiment, du groupe dynamique, du groupe d'utilisateurs et des stratégies

1.  Copiez et collez la commande suivante dans le terminal pour accéder au dossier _init_.
    
        <copy>cd ../init</copy>
        
2.  Copiez et collez la commande suivante dans le terminal pour supprimer le compartiment, les groupes dynamiques, les groupes et les stratégies.
    
        <copy>terraform destroy -auto-approve</copy>
        random_string.random_value: Refreshing state... [id=8emL]
        oci_identity_group.user_group: Refreshing state... [id=ocid1.group.oc1..aaa]
        oci_identity_compartment.devops_demo_compartment: Refreshing state... [id=ocid1.compartment.oc1..aaa]
        oci_identity_dynamic_group.devops_dynamic_group: Refreshing state... [id=ocid1.dynamicgroup.oc1..aaaa]
        oci_identity_dynamic_group.instance_dynamic_group: Refreshing state... [id=ocid1.dynamicgroup.oc1..aaaa]
        oci_identity_user_group_membership.user_group_membership[0]: Refreshing state... [id=ocid1.groupmembership.oc1..aaaa]
        oci_identity_policy.user_group_policy: Refreshing state... [id=ocid1.policy.oc1..aaaa]
        oci_identity_policy.instance_policy: Refreshing state... [id=ocid1.policy.oc1..aaaa]
        oci_identity_policy.devops_policy: Refreshing state... [id=ocid1.policy.oc1..aaa]
        

## Accusés de réception

*   **Auteur** - Keith Lustria
*   **Contributeurs** - Ankit Pandey, Maciej Gruszka
*   **Dernière mise à jour par/date** - Ankit Pandey, mai 2023
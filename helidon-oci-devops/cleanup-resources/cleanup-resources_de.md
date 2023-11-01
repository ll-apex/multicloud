# OCI-Ressourcen bereinigen (optional)

## Einführung

In dieser Übung werden Sie durch die Schritte zum Bereinigen der OCI-Ressource geführt, die in diesem Workshop erstellt wurde, wenn die Umgebung nicht benötigt wird.

Geschätzte Zeit: 15 Minuten

In dem folgenden Video erhalten Sie einen kurzen Überblick über die Übung. [Ressourcen bereinigen](videohub:1_cyuzrf2g)

### Ziele

In dieser Übung führen Sie folgende Schritte aus:

*   Löschen Sie die für das Projekt DevOps erstellten Ressourcen.
*   Bereinigen Sie das Compartment, dynamische Gruppen, Benutzergruppen und Policys.

### Voraussetzungen

*   Free Tier-(Testversion), kostenpflichtiger oder LiveLabs Cloud-Account für Oracle

## Aufgabe 1: Löschen Sie die für das Projekt DevOps erstellten Ressourcen.

1.  Um das Projekt _`devops_helidon_to_instance_ocw_hol`_ im **Codeeditor** zu öffnen, klicken Sie auf _Datei_ -> _Öffnen_. ![Datei öffnen](images/open-file.png)
    
2.  Klicken Sie auf den _Pfeil nach oben_, um zum übergeordneten Ordner zu navigieren. Wählen Sie dann den Ordner **`devops_helidon_to_instance_ocw_hol`**, und klicken Sie auf _Öffnen_. ![Offene Devops](images/open-devops.png)
    
3.  Öffnen Sie ein neues Terminal, kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um zum Ordner **`devops_helidon_to_instance_ocw_hol`** zu navigieren.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main</copy>
        
4.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn ein, um die Ressourcen für das **DevOps-Projekt** wie Thema, Code-Repository usw. zu löschen. Beachten Sie die Ausgabe eines Befehls, um die gelöschten Ressourcen zu kennen.
    
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
        
    
    > Die Ausführung dieses Skripts dauert 16 Minuten.
    

## Aufgabe 2: Compartment, dynamische Gruppe, Benutzergruppe und Policys bereinigen

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um zum Ordner _init_ zu navigieren.
    
        <copy>cd ../init</copy>
        
2.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in das Terminal ein, um das Compartment, dynamische Gruppen, Gruppen und Policys zu löschen.
    
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
        

## Danksagungen

*   **Autor** - Keith Lustria
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, Mai 2023
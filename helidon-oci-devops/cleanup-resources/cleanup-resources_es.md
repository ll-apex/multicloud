# Limpieza de los recursos de OCI (opcional)

## Introducción

Este laboratorio le guiará por los pasos para limpiar el recurso de OCI creado en este taller, cuando no sea necesario el entorno.

Tiempo estimado: 15 minutos

Vea el siguiente vídeo para obtener una breve introducción al laboratorio. [Elimine los recursos](videohub:1_cyuzrf2g)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Destruir los recursos creados para el proyecto DevOps
*   Limpie el compartimento, los grupos dinámicos, los grupos de usuarios y las políticas.

### Requisitos

*   Una cuenta en la nube de Oracle Free Tier (prueba), de pago o LiveLabs

## Tarea 1: Destruir los recursos creados para el proyecto DevOps

1.  Para abrir el proyecto _`devops_helidon_to_instance_ocw_hol`_ en **Editor de códigos**, haga clic en _Archivo_ -> _Abrir_. ![abrir archivo](images/open-file.png)
    
2.  Haga clic en la _Flecha Arriba_ para navegar a la carpeta principal y, a continuación, seleccione la carpeta **`devops_helidon_to_instance_ocw_hol`** y haga clic en _Abrir_. ![abrir devops](images/open-devops.png)
    
3.  Abra un nuevo terminal, copie y pegue el siguiente comando para navegar a la carpeta **`devops_helidon_to_instance_ocw_hol`**.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main</copy>
        
4.  Copie y pegue el siguiente comando para destruir los recursos relacionados con el **proyecto DevOps**, como el tema, el repositorio de código, etc. Observe la salida de un comando para conocer los recursos que suprime.
    
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
        
    
    > Este script tarda 16 minutos en completarse.
    

## Tarea 2: Limpiar el compartimento, el grupo dinámico, el grupo de usuarios y las políticas

1.  Copie y pegue el siguiente comando en el terminal para navegar a la carpeta _init_.
    
        <copy>cd ../init</copy>
        
2.  Copie y pegue el siguiente comando en el terminal para suprimir el compartimento, los grupos dinámicos, los grupos y las políticas.
    
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
        

## Reconocimientos

*   **Autor**: Keith Lustria
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, mayo de 2023
# Limpar os Recursos do OCI (Opcional)

## Introdução

Este laboratório orienta você pelas etapas para limpar o recurso do OCI criado neste workshop, quando o ambiente não é necessário.

Tempo estimado: 15 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [limpe recursos](videohub:1_cyuzrf2g)

### Objetivos

Neste laboratório, você vai:

*   Destrua os recursos criados para o projeto DevOps
*   Limpe o compartimento, os grupos dinâmicos, os grupos de usuários e as políticas.

### Pré-requisitos

*   Uma Conta do Oracle Free Tier(Trial), Paga ou LiveLabs Cloud

## Tarefa 1: Destruir os recursos criados para o projeto DevOps

1.  Para abrir o projeto _`devops_helidon_to_instance_ocw_hol`_ no **Editor de Códigos**, clique em _Arquivo_ -> _Abrir_. ![abrir arquivo](images/open-file.png)
    
2.  Clique na _Seta para Cima_ para navegar até a pasta pai e selecione a pasta **`devops_helidon_to_instance_ocw_hol`** e clique em _Abrir_. ![devops abertos](images/open-devops.png)
    
3.  Abra um novo terminal, Copie e cole o comando a seguir para navegar até a pasta **`devops_helidon_to_instance_ocw_hol`**.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main</copy>
        
4.  Copie e cole o comando a seguir para destruir os recursos relacionados ao **projeto DevOps** como o tópico, repositório de código etc. Observe a saída de um comando para saber os recursos que ele exclui.
    
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
        
    
    > Este script leva 16 minutos para ser concluído.
    

## Tarefa 2: Limpar o compartimento, o grupo dinâmico, o grupo de usuários e as políticas

1.  Copie e cole o comando a seguir no terminal para navegar até a pasta _init_.
    
        <copy>cd ../init</copy>
        
2.  Copie e cole o seguinte comando no terminal para excluir o compartimento, os grupos dinâmicos, os grupos e as políticas.
    
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
        

## Confirmação

*   **Autor** - Keith Lustria
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, maio de 2023
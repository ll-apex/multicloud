# 清除 OCI 资源（可选）

## 简介

此实验室将指导您完成清理研讨会中创建的 OCI 资源的步骤（如果不需要此环境）。

估计时间：15 分钟

观看下面的视频，快速浏览实验室。[清理资源](videohub:1_cyuzrf2g)

### 目标

在本练习中，您将：

*   销毁为 DevOps 项目创建的资源
*   清除区间、动态组、用户组和策略。

### 先备条件

*   Oracle 免费套餐（试用版）、付费版或 LiveLabs 云账户

## 任务 1：销毁为 DevOps 项目创建的资源

1.  要在_代码编辑器_中打开 **`devops_helidon_to_instance_ocw_hol`** 项目，请单击_文件_ -> _打开_。 ![打开文件](images/open-file.png)
    
2.  单击_上箭头_以导航到父文件夹，然后选择 **`devops_helidon_to_instance_ocw_hol`** 文件夹并单击_打开_。 ![开放的开发者](images/open-devops.png)
    
3.  打开新终端，复制并粘贴以下命令以导航到 **`devops_helidon_to_instance_ocw_hol`** 文件夹。
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main</copy>
        
4.  复制并粘贴以下命令以销毁与 **DevOps 项目**相关的资源，例如主题、代码系统信息库等。请查看命令的输出以了解其删除的资源。
    
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
        
    
    > 此脚本需要 16 分钟才能完成。
    

## 任务 2：清除区间、动态组、用户组和策略

1.  在终端中复制并粘贴以下命令以导航到 _init_ 文件夹。
    
        <copy>cd ../init</copy>
        
2.  在终端中复制并粘贴以下命令，以删除区间、动态组、组和策略。
    
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
        

## 确认

*   **作者** - Keith Lustria
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 5 月
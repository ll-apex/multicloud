# 清除 OCI 資源 (選擇性)

## 簡介

此實驗室會在不需要環境時，逐步引導您清理在此研討會中建立的 OCI 資源。

預估時間：15 分鐘

請觀看下方影片快速瞭解實驗室的逐步解說。[清除資源](videohub:1_cyuzrf2g)

### 目標

在此實驗室中，您將：

*   毀棄為 DevOps 專案建立的資源
*   清除區間、動態群組、使用者群組以及原則。

### 先決條件

*   一個 Oracle Free Tier (Trial)、Paid 或 LiveLabs 雲端帳戶

## 作業 1：毀棄為 DevOps 專案建立的資源

1.  若要在_程式碼編輯器_中開啟 **`devops_helidon_to_instance_ocw_hol`** 專案，請按一下 \[ _檔案_ \] -> \[ _開啟_ \]。 ![開啟檔案](images/open-file.png)
    
2.  按一下_向上鍵_以瀏覽至上層資料夾，然後選取 **`devops_helidon_to_instance_ocw_hol`** 資料夾，再按一下_開啟_。 ![開放式裝置](images/open-devops.png)
    
3.  開啟新的終端機，複製並貼上下列指令以瀏覽至 **`devops_helidon_to_instance_ocw_hol`** 資料夾。
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main</copy>
        
4.  複製並貼上下列命令，以銷毀與 **DevOps 專案**相關的資源，例如主題、程式碼儲存區域等。請觀察指令的輸出，以瞭解指令所刪除的資源。
    
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
        
    
    > 此命令檔需要 16 分鐘完成。
    

## 作業 2：清除區間、動態群組、使用者群組以及原則

1.  在終端機中複製並貼上下列指令，以瀏覽至 _init_ 資料夾。
    
        <copy>cd ../init</copy>
        
2.  在終端機中複製並貼上下列命令，以刪除區間、動態群組、群組以及原則。
    
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
        

## 確認

*   **作者** - Keith Lustria
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 5 月
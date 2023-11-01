# OCIリソースのクリーン・アップ(オプション)

## 概要

このラボでは、環境が不要な場合に、このワークショップで作成されたOCIリソースをクリーンアップするステップについて説明します。

見積時間: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[リソースをクリーンアップ](videohub:1_cyuzrf2g)

### 目的

この演習では、次のことを行います。

*   DevOpsプロジェクト用に作成されたリソースを破棄します。
*   コンパートメント、動的グループ、ユーザー・グループおよびポリシーをクリーン・アップします。

### 事前設定

*   Oracle Free Tier(トライアル)、有料またはLiveLabsクラウド・アカウント

## タスク1: DevOpsプロジェクト用に作成されたリソースの破棄

1.  _コード・エディタ_で**`devops_helidon_to_instance_ocw_hol`**プロジェクトを開くには、_「ファイル」_→_「開く」_をクリックします。 ![ファイルを開く](images/open-file.png)
    
2.  _上矢印_をクリックして親フォルダに移動し、**`devops_helidon_to_instance_ocw_hol`**フォルダを選択して_「開く」_をクリックします。 ![オープンなDevOps](images/open-devops.png)
    
3.  新しい端末を開き、次のコマンドをコピーして貼り付けて、**`devops_helidon_to_instance_ocw_hol`**フォルダに移動します。
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main</copy>
        
4.  次のコマンドをコピーして貼り付け、トピック、コードリポジトリなどの **DevOpsプロジェクト**に関連するリソースを破棄します。コマンドの出力を確認して、削除するリソースを確認してください。
    
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
        
    
    > このスクリプトの完了には16分かかります。
    

## タスク2: コンパートメント、動的グループ、ユーザー・グループおよびポリシーのクリーンアップ

1.  次のコマンドをコピーして端末に貼り付け、_init_フォルダに移動します。
    
        <copy>cd ../init</copy>
        
2.  コンパートメント、動的グループ、グループおよびポリシーを削除するには、ターミナルで次のコマンドをコピーして貼り付けます。
    
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

*   **著者** - Keith Lustria
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年5月
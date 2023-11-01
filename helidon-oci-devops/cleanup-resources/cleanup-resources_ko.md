# OCI 리소스 정리(선택사항)

## 소개

이 실습에서는 환경이 필요하지 않은 경우 이 워크샵에서 생성된 OCI 리소스를 정리하는 단계를 안내합니다.

예상 시간: 15분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [리소스 정리](videohub:1_cyuzrf2g)

### 목표

이 실습에서는 다음을 수행합니다.

*   DevOps 프로젝트에 대해 생성된 리소스를 삭제합니다.
*   구획, 동적 그룹, 사용자 그룹 및 정책을 정리합니다.

### 필요 조건

*   Oracle Free Tier(체험판), 유료 또는 LiveLabs 클라우드 계정

## 작업 1: DevOps 프로젝트에 대해 생성된 리소스를 삭제합니다.

1.  _코드 편집기_에서 **`devops_helidon_to_instance_ocw_hol`** 프로젝트를 열려면 _파일_ -> _열기_를 누릅니다. ![파일 열기](images/open-file.png)
    
2.  _위쪽 화살표_를 눌러 상위 폴더로 이동한 다음 **`devops_helidon_to_instance_ocw_hol`** 폴더를 선택하고 _열기_를 누릅니다. ![열린 DevOps](images/open-devops.png)
    
3.  새 터미널을 열고 다음 명령을 복사하여 붙여넣어 **`devops_helidon_to_instance_ocw_hol`** 폴더로 이동합니다.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main</copy>
        
4.  다음 명령을 복사하여 붙여넣어 항목, 코드 저장소 등과 같이 **DevOps 프로젝트**와 관련된 리소스를 삭제합니다. 삭제된 리소스를 확인하려면 명령의 출력을 확인하십시오.
    
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
        
    
    > 이 스크립트를 완료하려면 16분 정도 걸립니다.
    

## 작업 2: 구획, 동적 그룹, 사용자 그룹 및 정책 정리

1.  터미널에서 다음 명령을 복사하여 붙여넣어 _init_ 폴더로 이동합니다.
    
        <copy>cd ../init</copy>
        
2.  터미널에서 다음 명령을 복사하여 붙여넣어 구획, 동적 그룹, 그룹 및 정책을 삭제합니다.
    
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
        

## 확인

*   **작성자** - Keith Lustria
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **Last Updated By/Date** - Ankit Pandey, 2023년 5월
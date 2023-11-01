# 인프라 프로비전

## 소개

이 실습에서는 **구획**, **동적 그룹**, **사용자 그룹 및 정책**을 생성합니다. 그런 다음 **OCI Code Editor**의 Terraform 서비스를 사용하여 **DevOps 프로젝트** 및 관련 리소스를 생성합니다.

예상 시간: 10분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [인프라 프로비저닝](videohub:1_tnhjvsxr)

### 목표

이 실습에서는 다음을 수행합니다.

*   코드 편집기를 열어 Terraform 스크립트를 다운로드합니다.
*   구획, 동적 그룹, 사용자 그룹 및 정책을 프로비전합니다.
*   DevOps 프로젝트에 필요한 리소스를 프로비전합니다.

### 필요 조건

*   Oracle Free Tier(체험판), 유료 또는 LiveLabs 클라우드 계정
*   구획, 동적 그룹, 사용자 그룹 및 정책에 대한 기본적인 이해

## 작업 1: 코드 편집기를 열고 소스 코드를 다운로드합니다.

1.  클라우드 콘솔에서 표시된 대로 _개발자 툴_ 아이콘을 누른 다음 _코드 편집기_를 누릅니다. ![코드 편집기 열기](images/open-codeeditor.png)
    
2.  _터미널_\-> _새 터미널_을 눌러 터미널을 엽니다. 워크샵 중 새 터미널을 열지 묻는 메시지가 표시됩니다. 이렇게 하면 Code Editor에서 새 터미널을 열 수 있습니다. ![터미널 열기](images/open-terminal.png)
    
3.  다음 명령을 복사하여 터미널에 붙여 넣어 소스 코드를 다운로드합니다. 이 소스 코드에는 이 워크샵에 필요한 OCI 리소스를 생성하는 terraform 스크립트가 포함되어 있습니다.
    
        <copy>curl -O https://objectstorage.uk-london-1.oraclecloud.com/p/IxV3cO7Uf5VnbniLEmx8zOBhr6liix40QWNTnTy0TTBcGdrLaRNSt2IJYxBPHqdw/n/lrv4zdykjqrj/b/ankit-bucket/o/devops_helidon_to_instance_ocw_hol.zip
        unzip ~/devops_helidon_to_instance_ocw_hol.zip</copy>
        

![소스 코드 다운로드](images/download-sourcecode.png)

4.  소스 코드 _`devops_helidon_to_instance_ocw_hol`_을 **Code Editor**에서 열려면 _File_\-> _Open_을 누릅니다. ![오픈 소스 코드](images/open-sourcecode.png)
    
5.  홈 디렉토리에서 _`devops_helidon_to_instance_ocw_hol`_을 선택하고 _Open_을 누릅니다. ![열린 DevOps](images/open-devops.png)
    
6.  그림과 같이 _`devops_helidon_to_instance_ocw_hol`_ 폴더 내에서 _terraform.tfvars_ 파일 이름을 누릅니다. 체험판 클라우드 계정에 대해 사용자정의할 변수 **`tenancy_ocid`**, **region**, **`compartment_ocid`**, **`user_ocid`**이 있습니다. ![tfvars 열기](images/open-tfvars.png)
    

> 프로젝트가 표시되지 않는 경우 코드 편집기에서 _탐색기_ 아이콘을 눌러야 할 수도 있습니다. ![외관](images/explorer.png)

7.  브라우저에서 [Cloud Console](https://cloud.oracle.com/)용 **새 탭**을 엽니다. 이 탭을 사용하여 위 변수의 값을 가져옵니다.
    
8.  **`tenancy_ocid`**을 가져오려면 _사용자 아이콘_을 누른 다음 표시된 대로 _테넌시_를 누릅니다. ![테넌시 OCID 가져오기](images/get-tenancyocid.png)
    
9.  _Copy(복사)_를 눌러 테넌시에 대한 **OCID**를 복사하고 _terraform.tfvars_ 파일에 _`tenancy_ocid`_ 값으로 붙여넣습니다. ![테넌시 OCID 복사](images/copy-tenancyocid.png)
    
10.  **`user_ocid`**을 가져오려면 _사용자 아이콘_을 누른 다음 표시된 대로 _내 프로파일_을 누릅니다. ![내 프로파일](images/my-profile.png)
    
11.  _Copy(복사)_를 눌러 사용자에 대한 **OCID**를 복사하고 _terraform.tfvars_ 파일에 _`user_ocid`_ 값으로 붙여넣습니다. ![사용자 ocid 복사](images/copy-userocid.png)
    
12.  영역 이름을 찾으려면 아래와 같이 **영역 관리**를 누릅니다. 그런 다음 홈 영역의 **지역 식별자**를 복사하여 _terraform.tfvars_ 파일에 _지역_ 값으로 붙여 넣습니다. ![영역 관리](images/manage-region.png) ![영역 이름](images/region-name.png)
    
13.  마지막으로 _terraform.tfvars_은 다음과 같아야 합니다. _`compartment_ocid`_ 값은 그대로 둡니다. 구획이 작업 2의 일부로 생성되면 값을 바꿉니다. ![초기화 tfvars](images/init-tfvars.png)
    

## 작업 2: 구획, 동적 그룹, 사용자 그룹 및 정책 생성

이 작업의 목표는 구획, 동적 그룹, 사용자 그룹 및 정책을 생성하여 DevOps 설정에 대한 환경을 준비하는 것입니다. 이 섹션에는 관리자 권한이 있는 사용자가 필요합니다. 권한이 없는 경우 해당 권한을 가진 다른 사용자에게 이 작업을 실행하도록 요청하십시오.

1.  터미널을 열고 다음 명령을 복사하여 붙여넣어 _init_ 폴더로 이동합니다.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/init/</copy>
        
2.  Code Editor에서 _init_ 폴더의 다양한 파일을 볼 수 있습니다. 구획, 동적 그룹, 사용자 그룹 및 정책을 생성하는 Terraform 스크립트입니다. ![init 파일](images/init-files.png)
    
3.  다음 명령을 복사하여 붙여넣어 구획, 동적 그룹, 사용자 그룹 및 정책을 프로비전합니다.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    아래와 유사한 출력이 표시됩니다. Terraform 스크립트가 생성하는 항목을 확인하려면 **출력을 검색**하십시오. 또한 코드를 참조하여 구현을 확인할 수 있습니다. ![init 생성됨](images/init-created.png)
    
    > 오류가 있으면 **terraform.tfvars** 파일에서 값을 올바르게 설정했는지 확인합니다.
    

## 작업 3: DevOps 프로젝트 및 해당 리소스 생성

1.  터미널에서 다음 명령을 복사하여 붙여넣어 _main_ 폴더로 이동합니다.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main/</copy>
        
2.  다음 명령을 복사하여 붙여넣어 **`devops_helidon_to_instance_ocw_hol`** 폴더 내 _terraform.tfvars_에서 새로 생성된 구획의 _compartment\_ocid_을 업데이트합니다.
    
        <copy>../utils/update_compartment.sh</copy>
        
3.  다음 명령을 복사하여 터미널에 붙여넣어 모든 DevOps 리소스를 프로비전합니다.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    > **Please Read:-** DevOps에 필요한 다음 리소스를 제공합니다.
    
    *   **OCI DevOps 서비스**
        *   **OCI DevOps 프로젝트** - 이 프로젝트에 필요한 모든 DevOps 구성요소를 포함합니다.
        *   애플리케이션 소스 코드 프로젝트를 호스트할 **OCI 코드 저장소**입니다.
        *   **DevOps 빌드 파이프라인**(다음 단계 포함):
            *   **빌드 관리** - JDK20 다운로드, Maven 및 Helidon 애플리케이션 구축 단계를 실행합니다.
            *   **아티팩트 전달** - 빌드된 Helidon 앱 및 배치를 아티팩트 저장소에 업로드합니다.
            *   **배치 트리거** - 배치 파이프라인을 트리거합니다.
        *   대상 환경에서 다음을 수행할 **DevOps 배치 파이프라인**:
            *   다운로드 JDK20
            *   OCI CLI를 설치하고 애플리케이션 결과물을 다운로드하는 데 사용
            *   애플리케이션 실행
        *   배치 파이프라인에서 생성된 OCI 컴퓨트 인스턴스를 배치 대상으로 식별하는 데 사용할 **DevOps 인스턴스 그룹 환경**입니다.
        *   **DevOps 트리거**: OCI 코드 저장소에서 푸시 이벤트가 발생할 때 시작부터 완료까지 파이프라인 수명 주기를 호출합니다.
    *   **OCI 아티팩트 레지스트리**
        *   빌드된 Helidon 앱 바이너리 및 배치 매니페스트를 버전 지정된 아티팩트로 호스트할 **OCI 아티팩트 저장소**입니다.
    *   **OCI 플랫폼**
        *   **OCI 컴퓨트 인스턴스** - 방화벽에서 포트 8080을 엽니다. 그러면 응용 프로그램이 최종적으로 배치됩니다.
    *   **보안 목록이 있는 OCI VCN(가상 클라우드 네트워크)**에 포트 8080을 여는 수신이 포함되어 있습니다. 포트 8080은 Helidon 응용 프로그램에 액세스할 위치입니다. OCI VCN은 OCI 컴퓨트 인스턴스가 네트워크 요구사항에 사용합니다.
4.  아래 다이어그램은 DevOps 설정의 작동 방식을 보여줍니다. ![devops 다이어그램](images/devops-diagram.png)
    
5.  아래와 유사한 출력이 표시됩니다. ![tf 출력](images/tf-output.png)
    

이제 **다음 실습으로 진행할 수 있습니다.**

## 확인

*   **작성자** - Keith Lustria
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
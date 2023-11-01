# 컴퓨트 인스턴스 설정

## 소개

이 실습에서는 워크샵 실행에 필요한 Oracle Cloud 객체를 생성하는 **Oracle WebLogic Suite for OKE BYOL** 스택을 설정하는 방법을 보여줍니다.

예상 시간: 15분

### 목표

이 실습에서는 다음을 수행합니다.

*   인증 토큰을 생성합니다.
*   저장소에 암호 생성
*   스택 생성: OKE BYOL용 Oracle WebLogic Suite
*   컴퓨트 인스턴스에 접속

### 필요 조건

이 실습에서는 다음 사항이 있다고 가정합니다.

*   Oracle Cloud 계정
*   SSH 키 쌍을 생성했습니다.
*   **Lab: Prepare Setup**을 완료했습니다.

## 작업 1: 인증 토큰 생성

이 작업에서는 _인증 토큰_을 생성합니다. 실습 5에서는 이 인증 토큰을 사용하여 보조 이미지를 Oracle Cloud Container Registry Repository로 푸시합니다. 또한 이 인증 토큰을 사용하여 WebLogic 도커 이미지를 Oracle Cloud Infrastructure Registry(컨테이너 레지스트리라고도 함)로 가져옵니다.

1.  오른쪽 맨 위에 있는 사용자 아이콘을 선택한 다음 _MyProfile_를 선택합니다.
    
    ![내 프로파일](images/my-profile.png)
    
2.  아래로 스크롤하여 _인증 토큰_을 선택한 다음 _토큰 생성_을 누릅니다.
    
    ![인증 토큰](images/auth-token.png)
    
3.  _`test-model-your_first_name`_을 복사하여 _설명_ 상자에 붙여넣고 _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/create-token.png)
    
4.  생성된 토큰에서 _복사_를 선택하고 텍스트 파일에 붙여넣습니다. 나중에 복사할 수 없습니다. _닫기_를 누릅니다.
    
    ![토큰 복사](images/copy-token.png)
    
    > 다음 작업에서는 이 인증 토큰을 **Secret**에 저장합니다.
    

## 작업 2: 저장소에 암호 만들기

암호는 암호, 인증서, SSH 키 또는 Oracle Cloud Infrastructure 서비스와 함께 사용하는 **인증 토큰**과 같은 자격 증명입니다.

1.  OCI 콘솔에서 **햄버거 메뉴** -> **ID 및 보안** -> **저장소**를 누릅니다. ![메뉴 볼트](images/menu-vault.png)
    
2.  **목록 범위**에서 구획을 선택한 다음 **저장소 생성**을 누릅니다.
    
3.  저장소 이름을 입력하고 **저장소 생성**을 누릅니다. ![저장소 생성](images/create-vault.png)
    
4.  생성한 저장소가 **활성** 상태로 표시되면 아래와 같이 저장소 이름을 누릅니다. ![저장소 이름](images/vault-name.png)
    
5.  저장소 내에서 **마스터 암호화 키**를 누른 다음 **키 생성**을 누릅니다. ![메뉴 메뉴 메뉴 메뉴](images/menu-mek.png)
    
6.  **key**의 이름을 입력한 다음 아래와 같이 **Create Key**를 누릅니다. ![MEK 생성](images/create-mek.png)
    
7.  새 마스터 암호화 키의 상태가 **Enabled(사용)**로 변경된 후 아래와 같이 **Resources(리소스)** 아래의 **Secrets(암호)**를 누릅니다. ![메뉴 암호](images/menu-secret.png)
    
8.  **암호 생성**을 누릅니다.
    
9.  암호 이름을 입력하고 새 암호화 키를 선택한 후 아래와 같이 인증 토큰을 **비밀 콘텐츠**로 입력합니다. **비밀 생성**을 누릅니다. ![암호 생성](images/create-secret.png)
    

## 작업 3: 스택 생성: OKE BYOL용 Oracle WebLogic Suite

1.  OCI 콘솔에서 **햄버거 메뉴** -> **마켓플레이스** -> **모든 애플리케이션**을 누릅니다. ![메뉴 마켓플레이스](images/menu-marketplace.png)
    
2.  검색 상자에 **WebLogic OKE**를 입력한 다음 **Oracle WebLogic Suite for OKE BYOL**을 누릅니다. ![메뉴 스택](images/menu-stack.png)
    
3.  사용 가능한 최신 버전을 선택하고 구획을 선택한 다음 조건 및 조항에 동의하는 체크박스를 선택하십시오. **Launch Stack(스택 실행)**을 누릅니다. ![스택 실행](images/launch-stack.png)
    
4.  스택 정보 섹션에서 모든 항목을 기본값으로 두고 **다음**을 누릅니다. ![스택 정보](images/stack-info.png)
    
5.  Resource Name Prefix로 **wko**를 입력하고 Browse를 눌러 SSH 공용 키를 선택합니다. ![접두어-ssh](images/prefix-ssh.png)
    
    > 추가 연습에서 사용할 수 있으므로 **wko**를 접두어로 사용해야 합니다.
    
6.  Verrazzano 통합의 경우 **Verrazzano 사용**에 대해 기본적으로 확인란을 선택하지 않은 상태로 둡니다. ![확인 안 함 verrazzano](images/uncheck-verrazzano.png)
    
7.  네트워크에서 **새 VCN 생성**을 가상 클라우드 네트워크 전략으로 선택하고 모든 항목을 기본값으로 둡니다. ![네트워크](images/network.png)
    
8.  OKE(컨테이너 클러스터) 구성에서 다음을 선택합니다.
    
    **Kubernetes 버전:** - 기본값인 **v1.26.2**를 그대로 둡니다.
    
    **Non-WebLogic Node Pool Shape (Required)** - **VM.Standard.E4을 선택합니다. Flex**는 구성이고 **1**은 OCPU 수로, **16**은 메모리 양으로 선택합니다.
    
    **Node in the NodePool for non-WebLogic pods** - 기본값 **1**을 그대로 둡니다.
    
    ![비WebLogic](images/non-weblogic.png)
    
    **WebLogic 노드 풀 구성 만들기** - 기본 상자를 선택된 상태로 둡니다.
    
    **WebLogic Node Pool Shape (Required)** - **VM.Standard.E4을 선택합니다. Flex**는 구성이고 **1**은 OCPU 수로, **16**은 메모리 양으로 선택합니다.
    
    **WebLogic Pod의 경우 NodePool의 노드** - 기본값 **1**을 그대로 둡니다.
    
    ![WebLogic](images/weblogic-pool.png)
    
9.  Administration Instances(관리 인스턴스)에서 다음과 같이 입력하거나 선택합니다. **컴퓨트 인스턴스에 대한 가용성 도메인** - 드롭다운에서 가용성 도메인을 선택합니다.
    
    **Administration Instance Compute Shape (Required)** - **VM.Standard.E4를 선택합니다. Flex**는 구성이고 **1**은 OCPU 수로, **16**은 메모리 양으로 선택합니다.
    
    ![관리 인스턴스](images/admin-instance.png)
    
    **Bastion Instance Shape(Required)** - **VM.Standard.E4를 선택합니다. Flex**는 구성이고 **1**은 OCPU 수로, **16**은 메모리 양으로 선택합니다.
    
    ![배스천](images/bastion.png)
    
10.  File System 섹션에서 다음과 같이 선택합니다. **Availability Domain for File system(파일 시스템의 가용성 도메인)** - 드롭다운에서 가용성 도메인을 선택합니다. ![파일 평균](images/file-avd.png)
    
11.  레지스트리(OCIR) 섹션에서 다음과 같이 입력합니다.
    
    **레지스트리 사용자 이름** - Kubernetes가 컨테이너 이미지 레지스트리에 액세스하는 데 사용하는 사용자 이름으로, {identity domain name}/{username} 형식입니다. 테넌시에서 Oracle Identity Cloud Service를 사용 중인 경우 oracleidentitycloudservice/{username} 형식을 사용합니다.
    
    **OCIR 인증 토큰 구획** - OCIR 인증 토큰이 있는 구획을 선택합니다.
    
    **OCIR 인증 토큰에 대한 검증된 암호** - 사용자가 이미지 레지스트리에 액세스하기 위해 생성한 OCIR 인증 토큰이 포함된 암호입니다.
    
    ![OCIR 비밀](images/ocir-secret.png)
    
12.  OCI 정책에서 **OCI 정책** 확인란을 선택합니다. Vault에서 암호를 읽고 Autonomous Transaction Processing 데이터베이스(해당하는 경우)를 관리하기 위한 정책을 생성합니다. **다음**을 누릅니다.
    
13.  \[검토\] 섹션에서 **적용 실행** 확인란을 선택한 다음 **생성**을 누릅니다. ![적용 실행](images/run-apply.png)
    
    > 그러면 스택에 필요한 리소스가 생성되는 작업이 생성됩니다. 기다릴 필요가 없습니다. 다음 태스크로 진행하십시오.
    

## 작업 4: 그래픽 원격 데스크탑 액세스

이 워크샵을 쉽게 실행하기 위해 VM 인스턴스는 노트북이나 워크스테이션의 최신 브라우저를 사용하여 액세스할 수 있는 원격 그래픽 데스크탑으로 사전 구성되어 있습니다. 로그인하려면 아래에 설명된 대로 진행합니다.

1.  왼쪽 상단 모서리에 햄버거 메뉴를 엽니다. **개발자 서비스**를 누르고 **리소스 관리자** > **스택**을 선택합니다.
    
2.  lab 1에서 생성한 스택 이름을 누릅니다. ![스택 클릭](images/click-stack.png)
    
3.  **애플리케이션 정보** 탭으로 이동하고 **원격 데스크톱 URL**을 복사하여 새 브라우저 탭에 붙여 넣습니다. ![데스크탑 URL](images/desktop-url.png)
    
    > 이제 이 원격 데스크탑 내의 모든 지침을 따라야 합니다.
    

이제 다음 실습을 진행할 수 있습니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 10월
# 실습 환경 설정

## 소개

이 실습에서는 필요한 모든 네트워크 리소스로 구성된 3노드 Kubernetes 클러스터를 생성합니다. 또한 Oracle Cloud Container Image Registry 내에 저장소를 생성합니다. 그런 다음 인증 토큰을 생성합니다. 또한 Oracle Container Registry에서 WebLogic 서버 이미지에 대한 라이센스 합의서에 동의하게 됩니다.

예상 시간: 15분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [Lab 환경 설정](videohub:1_zhvohpqq)

### 목표

이 실습에서는 다음을 수행합니다.

*   Oracle Kubernetes 클러스터를 생성합니다.
*   Oracle Cloud 컨테이너 이미지 레지스트리 내에 저장소를 생성합니다.
*   인증 토큰을 생성합니다.
*   Oracle Container Registry의 WebLogic 서버 이미지에 대한 라이센스를 수락하십시오.

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.
*   Oracle 계정이 있어야 합니다.
*   텍스트 편집기가 있어야 합니다.

## 작업 1: Oracle Kubernetes 클러스터 만들기

_빠른 만들기_ 기능은 기본 설정을 사용하여 필요에 따라 새 네트워크 리소스로 _빠른 클러스터_를 만듭니다. 이 접근 방식은 새 클러스터를 만드는 가장 빠른 방법입니다. 모든 기본값을 적용하면 몇 번의 클릭만으로 새 클러스터를 만들 수 있습니다. 클러스터에 대한 새 네트워크 리소스는 노드 풀 및 3개의 워커 노드와 함께 자동으로 생성됩니다.

1.  그림과 같이 콘솔에서 _햄버거 메뉴 -> 개발자 서비스 -> Kubernetes 클러스터(OKE)_를 선택합니다.
    
    ![작업 목록 메뉴](images/hamburger-menu.png " ")
    
2.  \[클러스터 목록\] 페이지에서 구획을 선택한 다음 _클러스터 생성_을 누릅니다.
    
    > 클러스터를 생성할 수 있는 구획과 Oracle Container Registry 내의 저장소를 선택해야 합니다.
    
    ![구획 선택](images/select-compartment.png " ")
    
3.  Create Cluster Solution 대화 상자에서 _Quick Create_를 선택하고 _Submit_을 누릅니다.
    
    ![워크플로우 실행](images/launch-workflow.png " ")
    
    _빠른 생성_을 선택하면 새 클러스터에 대한 새 네트워크 리소스와 함께 기본 설정으로 새 클러스터가 생성됩니다.
    
    Cluster Creation(클러스터 만들기) 페이지에서 다음 구성 세부 정보를 지정합니다. _Shape_ 필드에 지정한 값에 주의하십시오.
    
    *   **Name(이름)**: 클러스터의 이름입니다. 기본값은 그대로 둡니다.
    *   **구획**: 구획의 이름입니다. 리소스를 생성할 수 있는 구획을 선택합니다.
    *   **Kubernetes 버전**: Kubernetes의 버전입니다. Kubernetes 버전으로 _1.26.2_를 선택합니다.
    *   **Kubernetes API 끝점**: 클러스터 마스터 노드의 경로 지정 가능 여부입니다. _공용 끝점_ 값을 선택합니다.
    *   **노드 유형**: _관리됨_을 노드 유형으로 선택합니다.
    *   **Kubernetes 작업자 노드**: 클러스터 작업자 노드를 경로 지정할 수 있는지 여부입니다. 기본 _Private Workers_ 값은 그대로 둡니다.
    *   **구성**: 노드 풀의 각 노드에 사용할 구성입니다. 구성에 따라 각 노드에 할당되는 CPU 수와 메모리 양이 결정됩니다. 이 목록에는 OKE에서 지원하는 테넌시에서 사용 가능한 구성만 표시됩니다. _VM.Standard.E4을 선택합니다. Flex_(일반적으로 Oracle Free Tier 계정에서 사용 가능) OCPU 1개와 메모리 양으로 16GB를 선택합니다.
    *   **이미지**: Kubernetes 버전이 _1.26.2_인 기본 이미지를 선택합니다.
    *   **노드 수**: 생성할 워커 노드 수입니다. 기본값 _3_을 그대로 사용합니다.
    
    ![빠른 클러스터](images/quick-cluster1.png " ") ![데이터 입력](images/enter-data.png " ")
    
4.  _다음_을 눌러 새 클러스터에 대해 입력한 세부정보를 검토합니다.
    
5.  _Review(검토)_ 페이지에서 _Create a Basic cluster(기본 클러스터 만들기)_ 확인란을 선택한 다음 _Create Cluster(클러스터 만들기)_를 눌러 새 네트워크 리소스와 새 클러스터를 만듭니다.
    
    ![클러스터 검토](images/review-cluster.png " ")
    
    > 생성된 네트워크 리소스가 표시됩니다. 노드 풀 생성 요청이 시작될 때까지 기다렸다가 _Close_를 누릅니다.
    
    ![네트워크 리소스](images/network-resource.png " ")
    
    > 그런 다음 새 클러스터가 _Cluster Details(클러스터 세부정보)_ 페이지에 표시됩니다. 마스터 노드가 만들어지면 새 클러스터가 _Active_ 상태로 설정됩니다(7분 정도 걸림). 기다릴 필요가 없습니다. 다음 태스크로 진행하십시오.
    
    ![클러스터 액세스](images/cluster-access.png " ")
    

## 작업 2: 저장소 만들기

이 작업에서는 공용 저장소를 생성합니다. 실습 5에서는 보조 이미지를 이 저장소로 푸시합니다.

1.  그림과 같이 콘솔에서 _햄버거 메뉴_ -> _개발자 서비스_ -> _컨테이너 레지스트리_를 선택합니다. ![컨테이너 레지스트리](images/container-registry.png)
    
2.  저장소를 생성할 수 있는 구획을 선택합니다. _저장소 생성_을 누릅니다. ![저장소 생성](images/create-repository.png)
    
3.  저장소 이름으로 _`test-model-your_firstname`_를 입력하고 _Public_으로 Access를 입력한 다음 _Create_를 누릅니다. ![저장소 세부정보](images/repository-details.png)
    
4.  저장소가 준비되면 텍스트 편집기 내의 텍스트 파일에서 테넌시 네임스페이스를 확인하십시오. ![노트 테넌시 NameSpace](images/tenancy-namespace.png)
    

## 작업 3: 인증 토큰 생성

이 작업에서는 _인증 토큰_을 생성합니다. 실습 5에서는 이 인증 토큰을 사용하여 보조 이미지를 Oracle Cloud Container Registry Repository로 푸시합니다.

1.  오른쪽 맨 위에 있는 사용자 아이콘을 선택한 다음 _MyProfile_를 선택합니다.
    
    ![내 프로파일](images/my-profile.png)
    
2.  아래로 스크롤하여 _인증 토큰_을 선택한 다음 _토큰 생성_을 누릅니다.
    
    ![인증 토큰](images/auth-token.png)
    
3.  _`test-model-your_first_name`_을 복사하여 _설명_ 상자에 붙여넣고 _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/create-token.png)
    
4.  생성된 토큰에서 _복사_를 선택하고 텍스트 편집기에 붙여넣습니다. 나중에 복사할 수 없습니다. _닫기_를 누릅니다.
    
    ![토큰 복사](images/copy-token.png)
    

## 작업 4: WebLogic 서버 이미지에 대한 라이센스 수락

이 작업에서는 Oracle Container Registry에 있는 WebLogic 서버 이미지의 라이센스 계약에 동의합니다. 실습 3에서와 같이 WebLogic Server 12.2.1.3.0 이미지를 기본 이미지로 사용합니다. 따라서 WebLogic Server Images에 액세스하려면 사용권 계약에 동의합니다.

1.  Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/)에 대한 링크를 누르고 사인인합니다. 이를 위해서는 Oracle 계정이 필요합니다. ![컨테이너 레지스트리 사인인](images/container-registry-sign-in.png)
    
2.  \[사용자 이름\] 및 \[비밀번호\] 필드에 _Oracle 계정 인증서_를 입력한 다음 _사인인_을 누릅니다. ![로그인 컨테이너 레지스트리](images/login-container-registry.png)
    
3.  Oracle Container Registry의 홈 페이지에서 _weblogic_을 검색합니다. ![검색 WebLogic](images/search-weblogic.png)
    
4.  표시된 대로 _weblogic_을 누르고 언어로 _English_를 선택한 다음 _Continue_를 누릅니다. ![WebLogic을 누릅니다.](images/click-weblogic.png) ![언어 선택](images/select-language.png)
    
5.  _수락_을 눌러 라이센스 계약에 동의합니다. ![라이센스 동의](images/accept-license.png)
    

## 자세히 알아보기

_Oracle Cloud Infrastructure Container Engine for Kubernetes 정보_

Oracle Cloud Infrastructure Container Engine for Kubernetes는 컨테이너 애플리케이션을 클라우드에 배포하는 데 사용할 수 있는 확장 가능한 완전 관리형 고가용성 서비스입니다. 개발 팀이 클라우드 전용 애플리케이션을 안정적으로 구축, 배포 및 관리하려는 경우 Container Engine for Kubernetes(일부로 OKE 축약)를 사용합니다. 애플리케이션에 필요한 컴퓨트 리소스를 지정하면 OKE가 기존 OCI 테넌시의 Oracle Cloud Infrastructure에서 프로비저닝합니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
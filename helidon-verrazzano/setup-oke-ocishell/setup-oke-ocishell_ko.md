# Oracle Cloud Infrastructure에서 Oracle Kubernetes Engine 인스턴스 설정

## 소개

이 실습에서는 필요한 모든 네트워크 리소스로 구성된 3노드 Kubernetes 클러스터를 생성합니다. 고가용성을 보장하기 위해 노드가 다른 장애 도메인에 배치됩니다.

OKE 및 사용자정의 클러스터 배치에 대한 자세한 내용은 [Oracle Kubernetes Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm) 설명서를 참조하십시오.

예상 시간: 15분

### 제품/기술 정보

Oracle Cloud Infrastructure Container Engine for Kubernetes는 컨테이너 애플리케이션을 클라우드에 배포하는 데 사용할 수 있는 확장 가능한 완전 관리형 고가용성 서비스입니다. 개발 팀이 클라우드 전용 애플리케이션을 안정적으로 구축, 배포 및 관리하려는 경우 Container Engine for Kubernetes(일부로 OKE 축약)를 사용합니다. 애플리케이션에 필요한 컴퓨트 리소스를 지정하면 OKE가 기존 OCI 테넌시의 Oracle Cloud Infrastructure에서 프로비저닝합니다.

### 목표

_빠른 생성_ 클러스터 기능을 사용하여 필요한 네트워크 리소스, 노드 풀 및 세 개의 워커 노드가 있는 OKE(Oracle Kubernetes Engine) 인스턴스를 생성합니다. _빠른 만들기_ 접근 방법은 새 클러스터를 만드는 가장 빠른 방법입니다. 모든 기본값을 적용하면 몇 번의 클릭만으로 새 클러스터를 만들 수 있습니다.

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.

## 작업 1: OKE 클러스터 만들기

_빠른 만들기_ 기능은 기본 설정을 사용하여 필요에 따라 새 네트워크 리소스로 _빠른 클러스터_를 만듭니다. 이 접근 방식은 새 클러스터를 만드는 가장 빠른 방법입니다. 모든 기본값을 적용하면 몇 번의 클릭만으로 새 클러스터를 만들 수 있습니다. 클러스터에 대한 새 네트워크 리소스는 노드 풀 및 3개의 워커 노드와 함께 자동으로 생성됩니다.

1.  그림과 같이 콘솔에서 _햄버거 메뉴 -> 개발자 서비스 -> Kubernetes 클러스터(OKE)_를 선택합니다.
    
    ![작업 목록 메뉴](images/hamburger-menu.png " ")
    
2.  \[클러스터 목록\] 페이지에서 선택한 구획을 선택하고, 여기서 클러스터를 생성할 수 있습니다. 그런 다음 _클러스터 생성_을 누릅니다.
    
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
    *   **노드 유형**: 노드 유형으로 _관리됨_을 선택합니다.
    *   **Kubernetes 작업자 노드**: 클러스터 작업자 노드를 경로 지정할 수 있는지 여부입니다. 기본 _Private Workers_ 값은 그대로 둡니다.
    *   **구성**: 노드 풀의 각 노드에 사용할 구성입니다. 구성에 따라 각 노드에 할당되는 CPU 수와 메모리 양이 결정됩니다. 이 목록에는 OKE에서 지원하는 테넌시에서 사용 가능한 구성만 표시됩니다. _VM.Standard.E4을 선택합니다. Flex_(일반적으로 Oracle Free Tier 계정에서 사용 가능) OCPU 2개와 메모리 양으로 32GB를 선택합니다.
    *   **이미지**: Kubernetes 버전이 _1.26.2_인 기본 이미지를 선택합니다.
    *   **노드 수**: 생성할 워커 노드 수입니다. 기본값 _3_을 그대로 사용합니다.
    
    > _기본 모양을 남기지 않도록 주의하십시오. 기본 모양은 모든 VERRAZZANO 구성 요소에 맞게 너무 작습니다._
    
    ![빠른 클러스터](images/quick-cluster.png " ") ![노드 번호](images/node-number.png " ")
    
4.  _다음_을 눌러 새 클러스터에 대해 입력한 세부정보를 검토합니다.
    
5.  _Review(검토)_ 페이지에서 _Create a Basic cluster(기본 클러스터 만들기)_ 확인란을 선택한 다음 _Create Cluster(클러스터 만들기)_를 눌러 새 네트워크 리소스와 새 클러스터를 만듭니다.
    
    ![클러스터 검토](images/review-cluster.png " ")
    
    > 생성된 네트워크 리소스가 표시됩니다. 노드 풀 생성 요청이 시작될 때까지 기다렸다가 _Close_를 누릅니다.
    
    ![네트워크 리소스](images/network-resource.png " ")
    
    > 그런 다음 새 클러스터가 _Cluster Details(클러스터 세부정보)_ 페이지에 표시됩니다. 마스터 노드가 만들어지면 새 클러스터가 _Active_ 상태로 설정됩니다(7분 정도 걸림). 그런 다음 실습을 계속할 수 있습니다.
    
    ![클러스터 프로비전](images/cluster-provision.png " ")
    
    ![클러스터 액세스](images/cluster-access.png " ")
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
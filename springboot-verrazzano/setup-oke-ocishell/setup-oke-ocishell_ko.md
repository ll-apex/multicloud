# Oracle Cloud Infrastructure에서 Oracle Kubernetes Engine 인스턴스 설정

## 소개

이 실습에서는 Oracle Cloud Infrastructure에서 관리형 Kubernetes 환경을 생성하는 단계를 안내합니다.

예상 시간: 15분

### 제품/기술 정보

Oracle Cloud Infrastructure Container Engine for Kubernetes는 컨테이너 애플리케이션을 클라우드에 배포하는 데 사용할 수 있는 확장 가능한 완전 관리형 고가용성 서비스입니다. 개발 팀이 클라우드 전용 애플리케이션을 안정적으로 구축, 배포 및 관리하려는 경우 Container Engine for Kubernetes(일부로 OKE 축약)를 사용합니다. 애플리케이션에 필요한 컴퓨트 리소스를 지정하면 OKE가 기존 OCI 테넌시의 Oracle Cloud Infrastructure에서 프로비저닝합니다.

### 목표

이 실습에서는 다음을 수행합니다.

*   OKE(Oracle Kubernetes Engine) 인스턴스를 생성합니다.
*   OCI Cloud Shell을 열고 `kubectl`를 구성하여 Kubernetes 클러스터와 상호 작용합니다.

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.

OKE(Container Engine for Kubernetes)를 생성하려면 다음 단계를 완료하십시오.

*   네트워크 리소스(VCN, 서브넷, 보안 목록 등)를 생성합니다.
*   클러스터를 만듭니다.
*   `NodePool`를 만듭니다.

이 실습에서는 _빠른 시작_ 기능이 3노드 Kubernetes 클러스터에 필요한 모든 리소스를 생성하고 구성하는 방법을 보여줍니다. 고가용성을 보장하기 위해 모든 노드가 서로 다른 가용성 도메인에 배치됩니다.

OKE 및 사용자정의 클러스터 배치에 대한 자세한 내용은 [Oracle Container Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm) 설명서를 참조하십시오.

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
    

## 작업 2: `kubectl` 구성(Kubernetes 클러스터 CLI)

OCI(Oracle Cloud Infrastructure) Cloud Shell은 웹 브라우저 기반 터미널로, Oracle Cloud 콘솔에서 액세스할 수 있습니다. Cloud Shell은 사전 인증된 Oracle Cloud Infrastructure CLI 및 기타 유용한 도구(_Git, kubectl, helm, OCI CLI_)를 사용하여 Verrazzano 자습서를 완료하는 Linux 셸에 대한 액세스를 제공합니다. Cloud Shell은 콘솔에서 액세스할 수 있습니다. Cloud Shell은 Oracle Cloud 콘솔에 영구 프레임으로 표시되며, 다른 콘솔 페이지로 이동할 때 활성 상태로 유지됩니다.

_Cloud Shell_을 사용하여 이 워크샵을 완료합니다.

Cloud Shell을 사용하여 원격으로 클러스터를 관리하기 위해 `kubectl`를 사용합니다. `kubeconfig` 파일이 필요합니다. 사전 인증된 OCI CLI를 사용하여 생성되므로 사용을 시작하기 전에 설정을 수행할 필요가 없습니다.

1.  클러스터 세부정보 페이지에서 _클러스터 액세스_를 누릅니다.
    
    > 해당 페이지에서 다른 위치로 이동한 다음 탐색 메뉴를 열고 _개발자 서비스_에서 _OKE(Kubernetes Clusters)_를 선택합니다. 클러스터를 선택하고 세부정보 페이지로 이동합니다.
    
    ![클러스터에 액세스](images/access-cluster.png " ")
    
    > Kubernetes 구성 파일을 만들기 위해 Cloud Shell을 열고 실행해야 하는 사용자정의된 OCI 명령을 포함하는 대화상자가 표시됩니다.
    
2.  기본 _Cloud Shell 액세스_를 그대로 두고 먼저 _복사_ 링크를 선택하여 `oci ce...` 명령을 Cloud Shell에 복사합니다.
    
    ![kubectl 구성 복사](images/copy-config.png " ")
    
3.  이제 _Cloud Shell 실행_을 눌러 내장 콘솔을 엽니다. 그런 다음 _Cloud Shell_에 명령을 붙여넣기 전에 구성 대화 상자를 닫습니다.
    
    ![Cloud Shell 실행](images/launch-cloudshell.png " ")
    
4.  클립보드(Ctrl+V 또는 마우스 오른쪽 단추로 눌러서 복사)에서 Cloud Shell로 명령을 복사하고 명령을 실행합니다.
    
    예를 들어 이 명령은 다음과 같습니다.
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl 구성](images/kube-config.png " ")
    
5.  이제 `kubectl`가 작동하는지 확인합니다(예: `get node` 명령 사용). 다음과 유사한 출력이 표시될 때까지 이 명령을 여러 번 실행해야 할 수 있습니다.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.101   Ready    node    11m   v1.26.2
        10.0.10.29    Ready    node    11m   v1.26.2
        10.0.10.37    Ready    node    11m   v1.26.2
        
    
    > 노드 정보가 표시되면 구성이 성공한 것입니다.
    
6.  Cloud Shell의 오른쪽 상단 모서리에 있는 컨트롤을 사용하여 언제든지 터미널 크기를 최소화하고 복원할 수 있습니다.
    
    ![클라우드 셸](images/cloudshell.png " ")
    

이 _Cloud Shell_은 열어 둡니다. 추가 실습에 사용됩니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 3월
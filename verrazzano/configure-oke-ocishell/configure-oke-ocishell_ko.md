# Oracle Cloud Infrastructure(OCI)에서 OKE(Oracle Container Engine for Kubernetes)와 상호 작용하도록 KUBECTL 구성

## 소개

이 실습에서는 Oracle Cloud Infrastructure에서 Kubernetes 환경에 액세스할 수 있는 구성 파일 생성 단계를 안내합니다.

### 제품/기술 정보

Oracle Cloud Infrastructure Container Engine for Kubernetes는 컨테이너 애플리케이션을 클라우드에 배포하는 데 사용할 수 있는 확장 가능한 완전 관리형 고가용성 서비스입니다. 개발 팀이 클라우드 전용 애플리케이션을 안정적으로 구축, 배포 및 관리하려는 경우 Container Engine for Kubernetes(일부로 OKE 축약)를 사용합니다. 애플리케이션에 필요한 컴퓨트 리소스를 지정하면 OKE가 기존 OCI 테넌시의 Oracle Cloud Infrastructure에서 프로비저닝합니다.

### 목표

이 실습에서는 다음을 수행합니다.

*   OCI Cloud Shell을 열고 `kubectl`를 구성하여 Kubernetes 클러스터와 상호 작용합니다.

### 필요 조건

*   이 워크샵은 LiveLabs에서 이 워크샵을 예약했다고 가정합니다.

## 작업 1: `kubectl` 구성(Kubernetes 클러스터 CLI)

OCI(Oracle Cloud Infrastructure) Cloud Shell은 웹 브라우저 기반 터미널로, Oracle Cloud 콘솔에서 액세스할 수 있습니다. Cloud Shell은 사전 인증된 Oracle Cloud Infrastructure CLI 및 기타 유용한 도구(_Git, kubectl, helm, OCI CLI_)를 사용하여 Verrazzano 자습서를 완료하는 Linux 셸에 대한 액세스를 제공합니다. Cloud Shell은 콘솔에서 액세스할 수 있습니다. Cloud Shell은 Oracle Cloud 콘솔에 영구 프레임으로 표시되며, 다른 콘솔 페이지로 이동할 때 활성 상태로 유지됩니다.

_Cloud Shell_을 사용하여 이 워크샵을 완료합니다.

Cloud Shell을 사용하여 원격으로 클러스터를 관리하기 위해 `kubectl`를 사용합니다. `kubeconfig` 파일이 필요합니다. 사전 인증된 OCI CLI를 사용하여 생성되므로 사용을 시작하기 전에 설정을 수행할 필요가 없습니다.

1.  그림과 같이 콘솔에서 _햄버거 메뉴 -> 개발자 서비스 -> Kubernetes 클러스터(OKE)_를 선택합니다.
    
    ![작업 목록 메뉴](../setup-oke-ocishell/images/hamburgermenu.png " ")
    
2.  지정된 구획을 선택합니다. 그런 다음 _cluster1_ 클러스터를 누릅니다.
    
3.  클러스터 세부정보 페이지에서 _클러스터 액세스_를 누릅니다.
    
    ![클러스터에 액세스](../setup-oke-ocishell/images/accesscluster.png " ")
    
    > Kubernetes 구성 파일을 만들기 위해 Cloud Shell을 열고 실행해야 하는 사용자정의된 OCI 명령을 포함하는 대화상자가 표시됩니다.
    
4.  기본 _Cloud Shell 액세스_를 그대로 두고 먼저 _복사_ 링크를 선택하여 `oci ce...` 명령을 Cloud Shell에 복사합니다.
    
    ![kubectl 구성 복사](../setup-oke-ocishell/images/copyconfig.png " ")
    
5.  이제 _Cloud Shell 실행_을 눌러 내장 콘솔을 엽니다. 그런 다음 _Cloud Shell_에 명령을 붙여넣기 전에 구성 대화 상자를 닫습니다.
    
    ![Cloud Shell 실행](../setup-oke-ocishell/images/launchcloudshell.png " ")
    
6.  클립보드(Ctrl+V 또는 마우스 오른쪽 단추로 눌러서 복사)에서 Cloud Shell로 명령을 복사하고 명령을 실행합니다.
    
    예를 들어 이 명령은 다음과 같습니다.
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl 구성](../setup-oke-ocishell/images/kubeconfig.png " ")
    
7.  이제 `kubectl`가 작동하는지 확인합니다(예: `get node` 명령 사용). 다음과 유사한 출력이 표시될 때까지 이 명령을 여러 번 실행해야 할 수 있습니다.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.17    Ready    node    11m   v1.21.5
        10.0.10.184   Ready    node    11m   v1.21.5
        10.0.10.31    Ready    node    11m   v1.21.5
        
    
    > 노드 정보가 표시되면 구성이 성공한 것입니다.
    
8.  Cloud Shell의 오른쪽 상단 모서리에 있는 컨트롤을 사용하여 언제든지 터미널 크기를 최소화하고 복원할 수 있습니다.
    
    ![클라우드 셸](../setup-oke-ocishell/images/cloudshell.png " ")
    

이 _Cloud Shell_은 열어 둡니다. 추가 실습에 사용됩니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 2월
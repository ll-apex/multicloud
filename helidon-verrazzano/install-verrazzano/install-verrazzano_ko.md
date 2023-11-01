# Verrazzano 설치

## 소개

이 실습에서는 Oracle Cloud Infrastructure의 Kubernetes 클러스터에 Verrazzano를 설치하는 단계를 안내합니다.

예상 시간: 15분

### 제품/기술 정보

Verrazzano는 멀티 클라우드 및 하이브리드 환경에서 클라우드 네이티브 및 기존 애플리케이션을 배포하기 위한 엔드투엔드 엔터프라이즈 컨테이너 플랫폼입니다. 그것은 선별 된 오픈 소스 구성 요소들로 구성되어 있습니다 - 많은 당신이 이미 사용하고 신뢰 할 수 있습니다, 그리고 일부는 Verrazzano를 응집하고 사용하기 쉬운 플랫폼으로 만드는 모든 조각을 모으기 위해 특별히 작성되었습니다.

Verrazzano에는 다음과 같은 기능이 포함되어 있습니다.

*   하이브리드 및 다중 클러스터 워크로드 관리
*   WebLogic, Coherence 및 Helidon 애플리케이션에 대한 특별 처리
*   다중 클러스터 Infrastructure 관리
*   사전 유선 통합 애플리케이션 모니터링
*   통합된 보안
*   DevOps 및 GitOps 역량 강화

Verrazzano는 개발(`dev`), 운영(`prod`) 및 관리 클러스터(`managed-cluster`)의 설치 프로파일을 지원합니다.

다음 그림은 Verrazzano 설치 프로파일을 설명합니다. ![설치 프로필](images/installprofile.png)

다음 명령에서 프로파일을 변경하려면 _VZ\_PROFILE_ 환경 변수를 설치할 프로파일 이름으로 설정합니다.

Verrazzano 구성 옵션에 대한 전체 설명은 [Verrazzano Custom Resource Definition](https://verrazzano.io/docs/reference/api/verrazzano/verrazzano/)을 참조하십시오.

이 실습에서는 다음과 같은 특성을 가진 _Verrazzano의 개발 프로파일_을 설치합니다.

*   와일드카드(nip.io) DNS
*   자체 서명된 인증서
*   시스템 구성 요소 및 모든 응용 프로그램에서 사용되는 공유 관찰성 스택
*   관찰성 스택을 위한 임시 스토리지(팟이 재시작되면 모든 로그 및 측정항목이 손실됨)
*   경량 설치가 있습니다.
*   평가 목적으로 사용됩니다.
*   단일 노드 Opensearch 클러스터 토폴로지입니다.

Verrazzano는 선별된 오픈 소스 구성요소 세트를 설치합니다. 다음 표에는 각 구성 요소, 해당 버전 및 간략한 설명이 나열되어 있습니다.

![Verrazzano 프로필](images/verrazzano-profile.png " ") ![Verrazzano 프로필](images/verrazzano-profiles.png " ")

DNS 선택에 따라 nip.io(와일드카드 DNS) 또는 [Oracle OCI DNS](https://docs.cloud.oracle.com/en-us/iaas/Content/DNS/Concepts/dnszonemanagement.htm)를 사용할 수 있습니다. 이 실습에서는 nip.io(와일드카드 DNS)를 사용하여 설치합니다.

수신 컨트롤러는 IP 주소를 제공하여 외부에 Docker 컨테이너에 대한 액세스를 제공하는 데 도움이 됩니다. 수신 경로는 IP 주소를 다른 클러스터로 지정합니다.

### 목표

이 실습에서는 다음을 수행합니다.

*   Oracle Kubernetes Engine 클러스터를 사용하도록 `kubectl` 설정
*   Verrazzano vz CLI를 설치합니다.
*   Verrazzano의 개발(`dev`) 프로필을 설치합니다.

### 필요 조건

Verrazzano의 요구 사항:

*   Kubernetes 클러스터 및 호환되는 `kubectl`.
*   Kubernetes 작업자 노드에서 사용 가능한 CPU 2개, 100GB 디스크 스토리지 및 16GB RAM. 이것은 Verrazzano의 개발 프로필을 설치하기에 충분합니다. 배치하는 응용 프로그램의 리소스 요구 사항에 따라 응용 프로그램 배치에 충분하거나 충분하지 않을 수 있습니다.
*   실습 1에서는 Oracle Cloud Infrastructure에 Kubernetes 클러스터를 생성했습니다. 해당 Kubernetes 클러스터인 _cluster1_를 사용하여 Verrazzano의 개발 프로파일을 설치합니다.

## 작업 1: `kubectl` 구성(Kubernetes 클러스터 CLI)

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
    

## 작업 2: Verrazzano CLI 설치

1.  최신 vz CLI를 다운로드합니다.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
        

0 0 0 0 0 0 0 0 --:--:-- --:--:-- --:--:-- 0 100 39.7M 100 39.7M 0 0 23.6M 0 0:00:01 0:00:01 --:--:-- 32.7M \`\`\`

2.  체크섬 파일을 다운로드합니다.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        

다음과 같은 결과가 표시됩니다.

    ```bash
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    

0 0 0 0 0 0 0 0 --:--:-- --:--:-- --:--:-- 0 100 102 100 102 0 0 218 0 --:--:-- --:--:--:--:--:--:-- 218 \`\`\`

3.  체크섬 파일에 대해 이진을 검증합니다.
    
        <copy>sha256sum -c verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        verrazzano-1.6.2-linux-amd64.tar.gz: OK
        
4.  압축을 풀고 vz 바이너리 디렉토리로 이동합니다.
    
        <copy>tar xvf verrazzano-1.6.2-linux-amd64.tar.gz
        cd ~/verrazzano-1.6.2/bin/</copy>
        
5.  설치한 버전이 최신인지 테스트합니다.
    
        <copy>./vz version</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        Version: v1.6.2
        BuildDate: 2023-07-21T12:06:02Z
        GitCommit: 5cd8a2f8670000d2ef0b02404e3169699d87f26b
        

## 작업 3: Verrazzano 개발 프로필 설치

설치 프로파일은 이름으로 참조할 수 있는 잘 알려진 Verrazzano 설정 구성으로, 필요에 따라 사용자 정의할 수 있습니다.

Verrazzano는 개발(`dev`), 운영(`prod`) 및 관리 클러스터(`managed-cluster`)의 설치 프로파일을 지원합니다.

1.  nip.io DNS 메소드를 사용하여 설치합니다. 다음 명령을 복사하여 _Cloud Shell_에 붙여 넣어 Verrazzano를 설치합니다.
    
        <copy>./vz install -f - <<EOF
        apiVersion: install.verrazzano.io/v1beta1
        kind: Verrazzano
        metadata:
          name: example-verrazzano
        spec:
          profile: dev
        EOF
        </copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        Installing Verrazzano version v1.6.2
        Applying the file https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-platform-operator.yaml
        customresourcedefinition.apiextensions.k8s.io/verrazzanos.install.verrazzano.io created
        namespace/verrazzano-install created
        serviceaccount/verrazzano-platform-operator created
        clusterrole.rbac.authorization.k8s.io/verrazzano-managed-cluster created
        clusterrolebinding.rbac.authorization.k8s.io/verrazzano-platform-operator created
        service/verrazzano-platform-operator created
        service/verrazzano-platform-operator-webhook created
        deployment.apps/verrazzano-platform-operator created
        deployment.apps/verrazzano-platform-operator-webhook created
        mutatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-mysql-backup created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-operator-webhook created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-mysqlinstalloverrides created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-requirements-validator created
        Waiting for verrazzano-platform-operator to be ready before starting install - 23 seconds
        
    
    > 설치를 완료하는 데는 15~20분 정도 걸립니다. 이 명령은 Verrazzano 플랫폼 운영자를 설치하고 Verrazzano 사용자정의 리소스를 적용합니다. 설치가 완료되거나 기본 시간 초과(30m)에 도달할 때까지 설치 로그가 명령 창으로 스트리밍됩니다.
    
2.  _Cloud Shell_을 열어 두고 설치를 실행합니다.
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
# Verrazzano 설치

## 소개

이 실습에서는 Oracle Cloud Infrastructure의 Kubernetes 클러스터에 Verrazzano를 설치하는 단계를 안내합니다.

예상 시간: 20분

### 제품/기술 정보

Verrazzano는 멀티클라우드 및 하이브리드 환경에서 클라우드 네이티브 및 기존 애플리케이션을 배포하기 위한 엔드투엔드 엔터프라이즈 컨테이너 플랫폼입니다. 그것은 선별 된 오픈 소스 구성 요소들로 구성되어 있습니다 - 많은 당신이 이미 사용하고 신뢰 할 수 있습니다, 그리고 일부는 Verrazzano를 응집하고 사용하기 쉬운 플랫폼으로 만드는 모든 조각을 모으기 위해 특별히 작성되었습니다.

Verrazzano에는 다음과 같은 기능이 포함되어 있습니다.

*   하이브리드 및 다중 클러스터 워크로드 관리
*   WebLogic, Coherence 및 Helidon 애플리케이션에 대한 특별 처리
*   다중 클러스터 Infrastructure 관리
*   사전 유선 통합 애플리케이션 모니터링
*   통합된 보안
*   DevOps 및 GitOps 역량 강화

### 목표

이 실습에서는 다음을 수행합니다.

*   Verrazzano 명령줄 도구를 설치합니다.
*   Verrazzano의 개발(`dev`) 프로필을 설치합니다.
*   성공적인 Verrazzano 설치를 확인합니다.

### 필요 조건

Verrazzano의 요구 사항:

*   Kubernetes 클러스터 및 호환되는 `kubectl`.
*   Kubernetes 작업자 노드에서 사용 가능한 CPU 2개, 100GB 디스크 스토리지 및 16GB RAM. 이것은 Verrazzano의 개발 프로필을 설치하기에 충분합니다. 배치하는 응용 프로그램의 리소스 요구 사항에 따라 응용 프로그램 배치에 충분하거나 충분하지 않을 수 있습니다.

실습 1에서는 Oracle Cloud Infrastructure에 Kubernetes 클러스터를 생성했습니다. 해당 Kubernetes 클러스터 \*cluster1\*를 사용하여 Verrazzano의 개발 프로파일을 설치합니다. 실습 1에서는 Oracle Cloud Infrastructure의 Kubernetes 클러스터에 액세스하기 위한 구성 파일을 만들었습니다. 해당 Kubernetes 클러스터 \*cluster1\*를 사용하여 Verrazzano의 개발 프로파일을 설치합니다.

## 작업 1: vz CLI 설치

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
        

## 작업 2: Verrazzano 개발 프로필 설치

설치 프로파일은 이름으로 참조할 수 있는 잘 알려진 Verrazzano 설정 구성으로, 필요에 따라 사용자 정의할 수 있습니다.

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
        \> 설치를 완료하는 데는 15~20분 정도 걸립니다. 이 명령은 Verrazzano 플랫폼 운영자를 설치하고 Verrazzano 사용자정의 리소스를 적용합니다. \> 설치를 완료하는 데 8~10분 정도 걸립니다. 이 명령은 Verrazzano 플랫폼 운영자를 설치하고 Verrazzano 사용자정의 리소스를 적용합니다.
2.  설치가 완료될 때까지 기다립니다. 설치가 완료되거나 기본 시간 초과(30m)에 도달할 때까지 설치 로그가 명령 창으로 스트리밍됩니다.
    

## 작업 3: 성공적인 Verrazzano 설치 확인

Verrazzano는 여러 네임스페이스에 여러 객체를 설치합니다. Verrazzano 구성 요소는 이름 공간 _verrazzano-system_에 설치됩니다.

1.  여러 객체와 연관된 모든 Pod가 _실행 중_ 상태인지 확인하십시오. 모든 Pod가 _Running_ 상태로 유지됩니다.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $   kubectl get pods -n verrazzano-system
        NAME                                    READY   STATUS  RESTARTS AGE
        coherence-operator-b5dc669c6-rk2sm      1/1     Running   1       23m
        fluentd-54f5x                           2/2     Running   1       11m
        fluentd-h7mgh                           2/2     Running   1       11m
        fluentd-xcdfz                           2/2     Running   0       11m
        oam-kubernetes-runtime-5b48f944b-cx7b9  1/1     Running   0       24m
        verrazzano-application-operator-665c5   1/1     Running   0       22m
        verrazzano-application-operator-webhook 1/1     Running   0       22m
        verrazzano-authproxy-67776ff58b-8l      3/3     Running   0       21m
        verrazzano-cluster-operator-67dc5695    1/1     Running   0       14m
        verrazzano-cluster-operator-webhook     1/1     Running   0       14m
        verrazzano-console-7d95c98cb9-9ql2      2/2     Running   0       20m
        verrazzano-monitoring-operator-59f      2/2     Running   0       21m
        vmi-system-es-master-0                  2/2     Running   0       19m
        vmi-system-grafana-7fd956b585-2tbgl     3/3     Running   0       19m
        vmi-system-kiali-dd87546d6-ddxss        2/2     Running   0       22m
        vmi-system-osd-7687d6fccf-nm7kt         2/2     Running   0       14m
        weblogic-operator-54979449f4-njgrq      2/2     Running   0       22m
        weblogic-operator-webhook-f7ff8c8cf     1/1     Running   0       22m
        
    
    _Cloud Shell_은 열어 둡니다. 실습 3에 필요합니다.
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
# Verrazzano에 Helidon 애플리케이션 배포

## 소개

이 실습에서는 Hello Helidon 응용 프로그램을 배치하는 과정을 안내합니다.

예상 시간: 10분

### Verrazzano 및 애플리케이션 배포

Verrazzano는 [OAM(Open Application Model)](https://oam.dev/)을 사용하여 애플리케이션 정의를 지원합니다. Verrazzano 애플리케이션은 구성 요소 및 애플리케이션 구성으로 구성됩니다.

Verrazzano로 애플리케이션을 배포할 때 Verrazzano는 지능적인 워크로드 관리를 수행합니다. 이 플랫폼은 연결 및 네트워크 정책, 서비스 메시의 수신 및 모니터링 스택을 연결하여 측정항목, 로그 및 추적을 캡처합니다. Verrazzano는 OAM 구성 요소를 사용하여 시스템의 기능 단위를 정의한 후 연관된 애플리케이션 구성을 정의하여 어셈블 및 구성합니다.

![Verrazzano ](images/Verrazzano_IntelligentWorkload.png)

### 목표

이 실습에서는 다음을 수행합니다.

*   Verrazzano 환경의 성공적인 설치를 확인합니다.
*   Hello Helidon 애플리케이션을 배치합니다.
*   Hello Helidon 응용 프로그램의 배치를 확인합니다.

### 필요 조건

이 실습을 실행하려면 다음이 있어야 합니다.

*   Oracle Cloud Infrastructure에서 실행되는 OKE(Kubernetes) 클러스터입니다.
*   OKE(Kubernetes) 클러스터에서 Verrazzano 설치가 시작되었습니다.

## 작업 1: 성공적인 Verrazzano 설치 확인

Verrazzano는 여러 네임스페이스에 여러 객체를 설치합니다. Verrazzano 구성 요소는 이름 공간 _verrazzano-system_에 설치됩니다.

1.  여러 객체와 연관된 모든 Pod가 _실행 중_ 상태인지 확인하십시오.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $   kubectl get pods -n verrazzano-system
        NAME                                         READY   STATUS RESTARTS AGE
        coherence-operator-b5dc669c6-zm4dw           1/1     Running   1     153m
        fluentd-5s7hh                                2/2     Running   1     144m
        fluentd-bt4t4                                2/2     Running   1     144m
        fluentd-ghmkx                                2/2     Running   1     144m
        oam-kubernetes-runtime-5b48f944b-9bv2v       1/1     Running   0     154m
        verrazzano-application-operator-f976cf96f    1/1     Running   0     152m
        verrazzano-application-operator-webhook      1/1     Running   0     152m
        verrazzano-authproxy-58f8975c5d-jhx72        3/3     Running   0     151m
        verrazzano-cluster-operator-544d494f49       1/1     Running   0     144m
        verrazzano-cluster-operator-webhook-79       1/1     Running   0     144m
        verrazzano-console-6f59f7485d-xrdt8          2/2     Running   0     147m
        verrazzano-monitoring-operator-d88cdd66c     2/2     Running   0     151m
        vmi-system-es-master-0                       2/2     Running   0     151m
        vmi-system-grafana-78cd975dd9-vfjv5          3/3     Running   0     151m
        vmi-system-kiali-57d859dc4b-bnvjh            2/2     Running   0     151m
        vmi-system-osd-7687d6fccf-cjwqr              2/2     Running   0     147m
        weblogic-operator-54979449f4-t9q26           2/2     Running   0     152m
        weblogic-operator-webhook-f7ff8c8cf-nc4r     1/1     Running   0     152m
        

## 태스크 2: Hello Helidon 애플리케이션 배포

1.  Helidon Quickstart-mp 애플리케이션에 대한 `hello-helidon` 네임스페이스를 생성합니다. 모든 Kubernetes 아티팩트는 별도의 네임스페이스에 보관할 것입니다.
    
        <copy>kubectl create namespace hello-helidon</copy>
        
    
    > 네임스페이스는 클러스터를 가상 하위 클러스터로 구성하는 방법입니다. 각 네임스페이스는 논리적으로 다른 네임스페이스와 구분되지만 서로 통신할 수 있는 기능을 사용하여 클러스터 내에 원하는 수의 네임스페이스를 가질 수 있습니다.
    
2.  Verrazzano가 해당 네임스페이스 Verrazzano 아티팩트에 저장함을 인식하도록 해야 합니다. 따라서 Verrazzano에서 관리하는 `hello-helidon` 네임스페이스를 식별하는 레이블을 추가해야 합니다. 레이블은 의미 있고 사용자와 관련된 객체의 식별 속성을 지정하는 데 사용됩니다.
    
    여기서 `hello-helidon` 네임스페이스의 경우 레이블을 첨부하여 이 네임스페이스를 Verrazzano에서 관리되는 것으로 표시합니다. _istio-injection=enabled_는 Istio "sidecar"를 사용으로 설정하므로 Istio 프록시를 설정하는 데 도움이 됩니다. Istio 프록시를 사용하면 Istio 게이트웨이와 같은 다른 Istio 서비스에 액세스할 수 있습니다. 앞에서 언급한 속성을 사용하여 `hello-helidon` 네임스페이스에 레이블을 추가하려면 다음 명령을 복사하여 Cloud Shell에서 실행합니다.
    
        <copy>kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled</copy>
        
3.  이제 Hello Helidon 컨테이너화된 애플리케이션을 _cluster1_에 배포하려고 합니다. 이를 위해서는 Kubernetes 배포 구성이 필요합니다. 이 배치는 Kubernetes가 Hello Helidon 애플리케이션에 대한 인스턴스를 생성하고 업데이트하도록 지시합니다. 여기서는 Kubernetes에 지시하는 `hello-helidon-comp.yaml` 파일이 있습니다.
    
    Hello Helidon 응용 프로그램을 배치하려면 아래와 같이 다음 두 명령을 복사하여 붙여넣습니다. `hello-helidon-comp.yaml` 파일에는 다양한 OAM 구성요소의 정의가 포함되어 있습니다. 여기서 OAM 구성요소는 애플리케이션의 일반 구성 및 환경 요구사항을 설명하는 Kubernetes 사용자정의 리소스입니다.
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    `hello-helidon-app.yaml` 파일은 환경별 사용자정의를 제공하는 Verrazzano 애플리케이션 구성 파일입니다.
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-app.yaml -n hello-helidon</copy>
        
    
    > verrazzano를 사용하여 응용 프로그램을 배치하는 작업은 다음과 같은 지능형 작업 로드 관리 작업을 수행합니다.
    
    *   필요한 경우 서비스 계정 및 권한 부여 정책 만들기
    *   응용 프로그램 배치
    *   필요에 따라 게이트웨이 및 가상 서비스를 설정합니다.
    *   Istio에서 필요에 따라 인증서, 암호, 네트워크 정책을 설정합니다.
    *   자동화된 관찰 가능성 관리 킥
        *   OpenSearch에 로그 폐기
        *   모든 모니터링 정보를 Prometheus 및 Grafana로 가져오기
4.  Pod가 _Running_ 상태가 될 때까지 기다립니다. 이 _kubectl_ 명령을 사용하여 hello-helidon 이름 공간 내에서 모든 Pod가 _Running_ 상태가 될 때까지 기다립니다. 1~2분 정도 걸립니다.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    포드가 준비되면 유사한 응답을 볼 수 있습니다.
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    포드를 직접 나열하여 상태를 확인할 수도 있습니다.
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## 작업 3: Hello Helidon 응용 프로그램의 성공적인 배치 확인

1.  `/greet` 끝점을 확인합니다. 외부/로드 밸런서 IP 및 애플리케이션 구성에서 구성된 URL을 확인하려면 다음 명령을 실행하십시오.
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet</copy>
        
    
    그러면 REST 끝점에 적절한 URL이 인쇄됩니다. 예를 들면 다음과 같습니다.
    
        https://hello-helidon.hello-helidon.xx.xx.xx.xx.nip.io/greet
        
2.  이 링크를 사용하여 브라우저에서 테스트합니다. 하지만 자체 서명된 인증서로 인해 위험을 수락하고 브라우저에서 요청 처리를 계속할 수 있도록 해야 합니다.
    
    응답이 문자열일 뿐이므로 `curl`를 더 쉽게 사용할 수 있습니다.
    
        <copy>curl -k https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet; echo</copy>
        
    
    개발 과정에서 얻은 결과와 동일한 결과가 표시됩니다.
    
        {"message":"Hello World!"}
        
3.  _Cloud Shell_은 열어 둡니다. 다음 실습에 사용됩니다.
    

## Open Application Model에 대해 자세히 알아보기

**Verrazzano 구성요소**

Verrazzano OAM 구성요소는 애플리케이션의 일반 구성 및 환경 요구사항을 설명하는 [Kubernetes 사용자정의 리소스](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)입니다.

다음 코드는 이 실습에서 사용되는 Hello Helidon 응용 프로그램에 대한 간단한 Helidon 응용 프로그램 구성 요소를 보여줍니다. 이 리소스는 단일 끝점을 노출하는 Helidon 애플리케이션을 포함하는 단일 Docker 이미지로 구현되는 구성요소를 설명합니다.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: hello-helidon-component
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: hello-helidon-workload
          labels:
            app: hello-helidon
            version: v1
        spec:
          deploymentTemplate:
            metadata:
              name: hello-helidon-deployment
            podSpec:
              containers:
                - name: hello-helidon-container
                  image: "ghcr.io/verrazzano/example-helidon-greet-app-v1:1.0.0-1-20230126194830-31cd41f"
                  ports:
                    - containerPort: 8080
                      name: http
    

구성요소의 각 필드에 대한 간략한 설명:

*   **apiVersion** - 구성 요소 사용자 정의 리소스 정의의 버전
*   **kind** - 구성 요소 사용자 정의 리소스 정의의 표준 이름입니다.
*   **metadata.name** - 구성 요소의 사용자 정의 리소스를 만드는 데 사용되는 이름입니다.
*   **metadata.namespace** - 이 구성 요소의 사용자 정의 리소스를 만드는 데 사용되는 이름 공간입니다.
*   **spec.workload.kind** - VerrazzanoHelidonWorkload는 Kubernetes의 Stateless 워크로드를 정의합니다.
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - Kubernetes의 Stateless 작업 로드를 생성하는 데 사용되는 이름입니다.
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - 구현 컨테이너
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - 컨테이너에 의해 노출된 포트

**Verrazzano 애플리케이션 구성**

Verrazzano 애플리케이션 구성은 환경별 사용자정의를 제공하는 Kubernetes 사용자정의 리소스입니다. 다음 코드는 이 실습에서 사용된 Helidon _quickstart-mp_ 예에 대한 응용 프로그램 구성을 보여줍니다. 이 리소스는 hello-helidon 이름 공간에 대한 응용 프로그램 배치를 지정합니다.

추가 런타임 기능은 특성을 사용하여 지정되거나 워크로드를 보강하는 런타임 오버레이가 지정됩니다. 예를 들어 수신 특성은 수신 호스트 및 경로를 지정하고, 측정항목 특성은 애플리케이션 관련 측정항목을 가져오는 데 사용되는 Prometheus 스크레이퍼를 제공합니다.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: hello-helidon
      annotations:
        version: v1.0.0
        description: "Hello Helidon application"
    spec:
      components:
        - componentName: hello-helidon-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                    scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: hello-helidon-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/greet"
                          pathType: Prefix
    

애플리케이션 구성의 각 필드에 대한 간략한 설명:

*   **apiVersion** - ApplicationConfiguration 사용자정의 리소스 정의의 버전
*   **kind** - 응용 프로그램 구성 사용자 정의 리소스 정의의 표준 이름입니다.
*   **metadata.name** - 이 응용 프로그램 구성 리소스를 만드는 데 사용되는 이름입니다.
*   **metadata.namespace** - 이 응용 프로그램 구성 사용자 정의 리소스에 사용되는 이름 공간입니다.
*   **spec.components** - 런타임 구성을 지정하는 데 사용되는 응용 프로그램의 구성 요소에 대한 참조
*   **spec.components\[\].traits** - 응용 프로그램의 구성 요소에 대해 지정된 특성

특성을 탐색하기 위해 수신 특성의 필드를 검사할 수 있습니다.

*   **apiVersion** - OAM 특성 사용자정의 리소스 정의의 버전
*   **kind** - IngressTrait는 OAM 애플리케이션 수신 특성 사용자정의 리소스 정의의 이름입니다.
*   **spec.rules.paths** - 응용 프로그램에 액세스하기 위한 컨텍스트 경로

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
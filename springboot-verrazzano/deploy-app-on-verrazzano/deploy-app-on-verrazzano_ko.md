# Verrazzano에 Springboot 애플리케이션 배포

## 소개

이 실습에서는 Springboot 응용 프로그램 도커 이미지를 OKE에 배치하는 과정을 안내합니다.

예상 시간: 10분

### Verrazzano 및 애플리케이션 배포

Verrazzano는 [OAM(Open Application Model)](https://oam.dev/)을 사용하여 애플리케이션 정의를 지원합니다. Verrazzano 애플리케이션은 구성 요소 및 애플리케이션 구성으로 구성됩니다.

Verrazzano로 애플리케이션을 배포할 때 플랫폼은 연결 및 네트워크 정책, 서비스 메시의 수신, 모니터링 스택을 설정하여 측정지표, 로그 및 추적을 캡처합니다. Verrazzano는 OAM 구성 요소를 사용하여 시스템의 기능 단위를 정의한 후 연관된 애플리케이션 구성을 정의하여 어셈블 및 구성합니다.

### Verrazzano 구성요소

Verrazzano OAM 구성요소는 애플리케이션의 일반 구성 및 환경 요구사항을 설명하는 [Kubernetes 사용자정의 리소스](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)입니다.

다음 코드는 이 실습에서 사용되는 Tomcat 샘플 응용 프로그램에 대한 간단한 Springboot 응용 프로그램 구성 요소를 보여줍니다. 이 리소스는 단일 끝점을 노출하는 springboot 응용 프로그램을 포함하는 단일 Docker 이미지로 구현되는 구성 요소를 설명합니다.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: springboot-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: springboot-workload
          labels:
            app: springboot
            version: v1
        spec:
          containers:
          - name: springboot-container
            image: "EndpointOfYourRegion/TenancyNamespace/springboot-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: springboot
    

구성요소의 각 필드에 대한 간략한 설명:

*   **apiVersion** - 구성 요소 사용자 정의 리소스 정의의 버전
*   **kind** - 구성 요소 사용자 정의 리소스 정의의 표준 이름입니다.
*   **metadata.name** - 구성 요소의 사용자 정의 리소스를 만드는 데 사용되는 이름입니다.
*   **spec.workload.kind** - ContainerizedWorkload는 Kubernetes의 Stateless 워크로드를 정의합니다.
*   **spec.workload.spec.containers.ports.containerPort** - 컨테이너에 의해 노출된 포트

### Verrazzano 애플리케이션 구성

Verrazzano 애플리케이션 구성은 환경별 사용자정의를 제공하는 Kubernetes 사용자정의 리소스입니다. 다음 코드는 이 실습에서 사용된 Springboot 응용 프로그램 예에 대한 응용 프로그램 구성을 보여줍니다. 이 리소스는 Springboot 이름 공간에 대한 응용 프로그램 배치를 지정합니다.

추가 런타임 기능은 특성을 사용하여 지정되거나 워크로드를 보강하는 런타임 오버레이가 지정됩니다. 예를 들어 수신 특성은 수신 호스트 및 경로를 지정하고, 측정항목 특성은 애플리케이션 관련 측정항목을 가져오는 데 사용되는 Prometheus 스크레이퍼를 제공합니다.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: springboot-appconf
      annotations:
        version: v1.0.0
        description: "Spring Boot application"
    spec:
      components:
        - componentName: springboot-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                  scraper: verrazzano-system/vmi-system-prometheus-0
                  path: "/actuator/prometheus"
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: springboot-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
    

애플리케이션 구성의 각 필드에 대한 간략한 설명:

*   **apiVersion** - ApplicationConfiguration 사용자정의 리소스 정의의 버전
*   **kind** - 응용 프로그램 구성 사용자 정의 리소스 정의의 표준 이름입니다.
*   **metadata.name** - 이 응용 프로그램 구성 리소스를 만드는 데 사용되는 이름입니다.
*   **spec.components** - 런타임 구성을 지정하는 데 사용되는 응용 프로그램의 구성 요소에 대한 참조
*   **spec.components\[\].traits** - 응용 프로그램의 구성 요소에 대해 지정된 특성

특성을 탐색하기 위해 수신 특성의 필드를 검사할 수 있습니다.

*   **apiVersion** - OAM 특성 사용자정의 리소스 정의의 버전
*   **kind** - IngressTrait는 OAM 애플리케이션 수신 특성 사용자정의 리소스 정의의 이름입니다.
*   **spec.rules.paths** - 응용 프로그램에 액세스하기 위한 컨텍스트 경로

### 목표

이 실습에서는 다음을 수행합니다.

*   springboot 응용 프로그램을 배치합니다.
*   springboot 응용 프로그램의 배치를 확인합니다.

### 필요 조건

이 실습을 실행하려면 다음이 있어야 합니다.

*   Oracle Cloud Infrastructure에서 실행되는 OKE(Kubernetes) 클러스터입니다.
*   OKE(Kubernetes) 클러스터에서 Verrazzano 설치가 시작되었습니다.
*   컨테이너로 패키지화된 _`springboot-your_firstname`_ 애플리케이션을 컨테이너 레지스트리에서 사용할 수 있습니다.

## 작업 1: Spring 부트 응용 프로그램 배치

1.  springboot 응용 프로그램에 대한 `springboot` 이름 공간을 만듭니다. 모든 Kubernetes 아티팩트는 별도의 네임스페이스에 보관할 것입니다.
    
        <copy>kubectl create namespace springboot</copy>
        
    
    > 네임스페이스는 클러스터를 가상 하위 클러스터로 구성하는 방법입니다. 각 네임스페이스는 논리적으로 다른 네임스페이스와 구분되지만 서로 통신할 수 있는 기능을 사용하여 클러스터 내에 원하는 수의 네임스페이스를 가질 수 있습니다.
    
2.  Verrazzano가 해당 네임스페이스 Verrazzano 아티팩트에 저장함을 인식하도록 해야 합니다. 따라서 Verrazzano에서 관리하는 `springboot` 네임스페이스를 식별하는 레이블을 추가해야 합니다. 레이블은 의미 있고 사용자와 관련된 객체의 식별 속성을 지정하는 데 사용됩니다.
    
    여기서 `springboot` 네임스페이스의 경우 레이블을 첨부하여 이 네임스페이스를 Verrazzano에서 관리되는 것으로 표시합니다. _istio-injection=enabled_는 Istio "sidecar"를 사용으로 설정하므로 Istio 프록시를 설정하는 데 도움이 됩니다. Istio 프록시를 사용하면 Istio 게이트웨이와 같은 다른 Istio 서비스에 액세스할 수 있습니다. 앞에서 언급한 속성을 사용하여 `springboot` 네임스페이스에 레이블을 추가하려면 다음 명령을 복사하여 Cloud Shell에서 실행합니다.
    
        <copy>kubectl label namespace springboot verrazzano-managed=true istio-injection=enabled</copy>
        
3.  _~/springboot-app_에는 springboot 응용 프로그램에 대한 구성 파일이 있습니다. Lab 2의 일환으로 애플리케이션 구성요소용 새 Docker 이미지를 구축했습니다. 또한 해당 Docker 이미지를 Oracle Cloud Container Registry 저장소에 푸시했습니다. 이제 이 실습에서는 Oracle Cloud Container Registry 저장소에서 새 Docker 이미지를 가져오도록 _springboot-comp.yaml_ 파일을 수정합니다. _springboot-comp.yaml_ 파일을 수정하려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣습니다.
    
        <copy>vi ~/springboot-app/springboot-comp.yaml</copy>
        
4.  Lab 2의 일부로 Docker 이미지 전체 이름을 저장했습니다. 다음 행을 복사하여 텍스트 파일에 붙여 넣어야 합니다. 그런 다음 `docker image full name`을 Docker 이미지 이름으로 바꾸어야 합니다. 그런 다음 수정된 행을 복사하고 _i_ 키를 눌러 `*springboot-comp.yaml*` 파일에 텍스트를 삽입합니다. 행 번호 **19**에 출력을 붙여 넣은 다음(들여쓰기를 유지해야 함) _Esc_를 누르고 _:wq_를 입력하여 파일을 저장합니다.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![선 삽입](images/insert-line.png " ")
    
5.  이제 _cluster1_에 springboot 컨테이너화된 응용 프로그램을 배치하려고 합니다. 이를 위해서는 Kubernetes 배포 구성이 필요합니다. 이 배치는 Springboot 애플리케이션에 대한 인스턴스를 생성하고 업데이트하도록 Kubernetes에 지시합니다. 여기서는 Kubernetes에 지시하는 `springboot-comp.yaml` 파일이 있습니다.
    
    springboot 응용 프로그램을 배치하려면 다음과 같이 다음 두 명령을 복사하여 붙여넣습니다. `springboot-comp.yaml` 파일에는 다양한 OAM 구성요소의 정의가 포함되어 있습니다. 여기서 OAM 구성요소는 애플리케이션의 일반 구성 및 환경 요구사항을 설명하는 Kubernetes 사용자정의 리소스입니다.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-comp.yaml -n springboot</copy>
        
    
    `springboot-app.yaml` 파일은 환경별 사용자정의를 제공하는 Verrazzano 애플리케이션 구성 파일입니다.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-app.yaml -n springboot</copy>
        
6.  Pod가 _Running_ 상태가 될 때까지 기다립니다. 이 _kubectl_ 명령을 사용하여 모든 포드가 _springboot_ 이름 공간 내에서 _Running_ 상태가 될 때까지 기다립니다. 1~2분 정도 걸립니다.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s</copy>
        
    
    포드가 준비되면 유사한 응답을 볼 수 있습니다.
    
        $ kubectl wait --for=condition=Ready pods --all -n kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s --timeout=600s
        pod/springboot-workload-5c4ffc8954-mbgbb condition met
        pod/springboot-workload-bfb9d487-dwp7q condition met
        
    
    포드를 직접 나열하여 상태를 확인할 수도 있습니다.
    
        <copy>kubectl  get po -n springboot</copy>
        
    
        $ kubectl  get po -n springboot
        NAME                                 READY   STATUS    RESTARTS   AGE
        springboot-workload-bfb9d487-dwp7q   2/2     Running   0          68s
        $
        

## 작업 2: Springboot 응용 프로그램의 성공적인 배치 확인

1.  `/facts` 끝점을 확인합니다. 외부/로드 밸런서 IP 및 애플리케이션 구성에서 구성된 URL을 확인하려면 다음 명령을 실행하십시오.
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io springboot-springboot-appconf-gw -n springboot -o jsonpath={.spec.servers[0].hosts[0]})/facts</copy>
        
    
    그러면 REST 끝점에 적절한 URL이 인쇄됩니다. 예를 들면 다음과 같습니다.
    
        https://springboot-appconf.springboot.xx.xx.xx.xx.nip.io/facts
        
2.  브라우저에 URL을 붙여넣고 브라우저에서 _새로고침_ 버튼을 누릅니다. 언제든지 새로 고침 버튼을 클릭하면 Verrazzano에 대한 무작위 사실이 제공됩니다. ![신청 URL](images/application-url.png) ![다른 사실](images/another-fact.png)
    

이제 **다음 실습으로 진행**할 수 있습니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
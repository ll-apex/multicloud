# Bobby의 Books 샘플 애플리케이션 배포

## 소개

### Bobby's Books 응용 프로그램 정보

![Bobby' Books 애플리케이션](images/bobbyoverview.png " ")

[Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/)는 다음 세 부분으로 구성됩니다.

*   REST 서비스가 제공되는 Java EE 애플리케이션이자 MySQL 데이터베이스에 데이터를 저장하는 매우 간단한 JSP UI인 백엔드 _"주문 처리"_ 애플리케이션입니다. 이 애플리케이션은 WebLogic 서버에서 실행됩니다.
*   일반 서적 판매자인 프런트엔드 웹 스토어 _"Robert's Books"_이는 Coherence에서 장부 데이터를 가져오는 Helidon 마이크로서비스로 구현되고, Coherence 캐시 저장소를 사용하여 주문 관리자의 데이터를 보관하며, React 웹 UI를 사용합니다.
*   프런트엔드 웹 스토어인 _"Bobby's Books"_는 전문 어린이 책장입니다. 이 서비스는 다른 Coherence 캐시 저장소에서 장부 데이터를 가져오고 주문 관리자와 직접 연결하며 WebLogic 서버에서 JSF 웹 UI를 실행하는 Helidon 마이크로서비스로 구현됩니다.

이 응용 프로그램의 소스 코드 및 자세한 내용은 [Verrazzano Examples](https://github.com/verrazzano/examples)를 참조하십시오.

### Verrazzano 및 애플리케이션 배포

Verrazzano는 [OAM(Open Application Model)](https://oam.dev/)을 사용하여 애플리케이션 정의를 지원합니다. Verrrazzano 응용 프로그램은 구성 요소 및 응용 프로그램 구성으로 구성됩니다.

Verrazzano로 애플리케이션을 배포하면 이 플랫폼은 서비스 메시에서 연결, 네트워크 정책 및 수신을 설정하고 모니터링 스택을 활성화하여 측정지표, 로그 및 추적을 캡처합니다. Verrazzano는 OAM 구성 요소를 사용하여 시스템의 기능 단위를 정의한 후 연관된 애플리케이션 구성을 정의하여 어셈블 및 구성합니다.

### Verrazzano 구성요소

Verrazzano OAM 구성요소는 애플리케이션의 일반 구성 및 환경 요구사항을 설명하는 [Kubernetes 사용자정의 리소스](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)입니다.

다음 코드는 이 실습에서 사용되는 Bobby's Books 예제 응용 프로그램에 대한 하나의 구성 요소를 보여줍니다. 이 리소스는 단일 끝점을 노출하는 Helidon 애플리케이션을 포함하는 단일 Docker 이미지로 구현되는 구성요소를 설명합니다.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: bobby-helidon
      namespace: bobs-books
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: bobbys-helidon-stock-application
          labels:
            app: bobbys-helidon-stock-application
        spec:
          deploymentTemplate:
            metadata:
              name: bobbys-helidon-stock-application
            podSpec:
              containers:
                - name: bobbys-helidon-stock-application
                  image: container-registry.oracle.com/verrazzano/example-bobbys-helidon-stock-application:0.1.12-1-20210624160519-017d358
                  imagePullPolicy: IfNotPresent
                  ports:
                    - containerPort: 8080
                      name: http
                  env:
                    - name: BACKEND_PORT
                      value: "8001"
                    - name: BACKEND_HOSTNAME
                      value: bobs-bookstore-cluster-cluster-1.bobs-books.svc.cluster.local
                    - name: COH_CLUSTER
                      value: bobbys-coherence
                    - name: COH_CACHE_CONFIG
                      value: coherence-cache-config.xml
                    - name: COH_POF_CONFIG
                      value: pof-config.xml
              imagePullSecrets:
                - name: bobs-books-repo-credentials
    

구성요소의 각 필드에 대한 간략한 설명:

*   **apiVersion** - 구성 요소 사용자 정의 리소스 정의의 버전
*   **kind** - 구성 요소 사용자 정의 리소스 정의의 표준 이름입니다.
*   **metadata.name** - 구성 요소의 사용자 정의 리소스를 만드는 데 사용되는 이름입니다.
*   **metadata.namespace** - 이 구성 요소의 사용자 정의 리소스를 만드는 데 사용되는 이름 공간입니다.
*   **spec.workload.kind** - VerrazzanoHelidonWorkload는 Kubernetes의 Stateless 워크로드를 정의합니다.
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - Kubernetes의 Stateless 작업 로드를 생성하는 데 사용되는 이름입니다.
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - 구현 컨테이너
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - 컨테이너에 의해 노출된 포트

Bobbys의 Books 응용 프로그램에 대한 전체 구성 요소 설명은 [bobs-books-comp.yaml](https://github.com/verrazzano/verrazzano/blob/master/examples/bobs-books/bobs-books-comp.yaml) 파일에서 찾을 수 있습니다.

### Verrazzano 애플리케이션 구성

Verrazzano 애플리케이션 구성은 환경별 사용자정의를 제공하는 Kubernetes 사용자정의 리소스입니다. 다음 코드는 이 실습에서 사용되는 Bob의 Books 예제에 대한 응용 프로그램 구성을 보여줍니다. 이 리소스는 bobs-books 네임스페이스에 대한 응용 프로그램 배치를 지정합니다.

추가 런타임 기능은 특성을 사용하여 지정되거나 워크로드를 보강하는 런타임 오버레이가 지정됩니다. 예를 들어 수신 특성은 수신 호스트 및 경로를 지정하고, 측정항목 특성은 애플리케이션 관련 측정항목을 가져오는 데 사용되는 Prometheus 스크레이퍼를 제공합니다.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: bobs-books
      namespace: bobs-books
      annotations:
        version: v1.0.0
        description: "Bob's Books"
    spec:
      components:
        - componentName: robert-helidon
          traits:
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
        - componentName: robert-coh
        - componentName: bobby-coh
        - componentName: bobby-helidon
        - componentName: bobby-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobbys-front-end"
                          pathType: Prefix
        - componentName: bobs-orders-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobs-bookstore-order-manager/orders"
                          pathType: Prefix
        - componentName: bobs-orders-configmap
        - componentName: bobs-mysql-deployment
        - componentName: bobs-mysql-service
        - componentName: bobs-mysql-configmap
    

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

이 실습에서는 Bobby's Books 샘플 애플리케이션을 배치하는 과정을 안내합니다.

예상 시간: 10분

### 목표

이 실습에서는 다음을 수행합니다.

*   라이센스를 수락하여 Oracle Container Registry의 저장소에서 이미지를 다운로드하십시오.
*   Bobby의 Books 샘플 응용 프로그램을 배치합니다.
*   Bobby's Books 샘플 응용 프로그램의 성공적인 배치를 확인합니다.

### 필요 조건

Lab 3을 실행하려면 다음이 있어야 합니다.

*   Oracle Cloud Infrastructure에 OKE 클러스터를 생성하는 Lab 1을 실행합니다.

\* Oracle Cloud Infrastructure의 OKE 클러스터에 액세스하도록 kubectl을 구성하는 Lab 1을 실행합니다.

*   Kubernetes 클러스터에 Verrazzano를 설치하는 Lab 2를 실행합니다.
*   환경에 따라 명령 및 URL을 붙여넣고 수정할 수 있는 텍스트 편집기입니다. 그런 다음 _Cloud Shell_에서 실행하기 위해 수정된 명령을 복사하여 붙여넣을 수 있습니다.

## 작업 1: 라이센스 계약에 동의하여 Oracle Container Registry의 저장소에서 이미지 다운로드

_Bobby's Books_ 샘플 응용 프로그램을 배치하기 위해 예제 이미지를 사용합니다. 이러한 이미지에는 Oracle 제품이 포함되어 있으므로 BOB 문서 파일 `bobs-books-comp.yaml`에서 해당 이미지를 사용하기 전에 라이센스 계약에 동의해야 합니다.

1.  Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/)에 대한 링크를 누르고 사인인합니다. 이를 위해서는 Oracle 계정이 필요합니다.
    
    ![로그인](images/registrysignin.png " ")
    
2.  Username(사용자 이름) 및 Password(암호) 필드에 _Oracle 계정 자격 증명_을 입력한 다음 _Sign In(사인인)_을 누릅니다. 나중에 이러한 인증서를 사용하여 Kubernetes에서 암호를 생성할 것입니다.
    
    ![Oracle SSO  ](images/oraclesso.png " ")
    
3.  홈 페이지에서 _Verrazzano_를 선택합니다.
    
    ![레지스트리 홈페이지](images/registryhomepage.png " ")
    
4.  예를 들어, example-bobbys-coherence, example-bobbys-front-end, example-bobs-books-order-manager, example-roberts-coherence 저장소의 경우 언어로 _영어_를 선택하고 _계속_을 누릅니다.
    
    ![계속](images/continue.png " ")
    
5.  _수락_을 눌러 라이센스 계약에 동의합니다.
    
    ![동의](images/acceptagreement.png " ")
    
6.  다음 그림과 같이 Verrazzano와 관련된 저장소에 대한 라이센스 계약을 수락했는지 확인합니다.
    
    ![라이센스 계약서 확인](images/verifyaggrement.png " ")
    
7.  Oracle Container Registry의 홈 페이지에서 _weblogic_을 검색합니다.
    
    ![검색 WebLogic](images/searchweblogic.png " ")
    
8.  표시된 대로 _weblogic_을 누르고 Verrazzano imagaes에 대한 라이센스를 수락합니다. ![WebLogic을 누릅니다.](images/clickweblogic.png) ![WebLogic 승인](images/acceptagreement.png " ")
    

## 작업 2: Bobby's Books 애플리케이션 배치

구성 파일 `bobs-books-app.yaml` 및 `bobs-books-comp.yaml`가 있는 소스 코드를 다운로드해야 합니다.

1.  Bobby's Book 예제의 Verrazzano OAM 구성 요소 yaml 파일 및 Verrazzano Application Configuration 파일을 다운로드하십시오. _복사_를 누르고 표시된 대로 Cloud Shell에 명령을 붙여넣습니다.
    
        <copy>
        curl -LSs  https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-app.yaml >~/bobs-books-app.yaml
        curl -LSs https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-comp.yaml >~/bobs-books-comp.yaml
        cd ~
        </copy>
        
2.  모든 Kubernetes 아티팩트는 별도의 네임스페이스에 보관됩니다. Bob의 Books 예제 응용 프로그램에 대한 네임스페이스를 생성합니다. 네임스페이스는 클러스터를 가상 하위 클러스터로 구성하는 방법입니다. 각 네임스페이스는 논리적으로 다른 네임스페이스와 구분되지만 서로 통신할 수 있는 기능을 사용하여 클러스터 내에 원하는 수의 네임스페이스를 가질 수 있습니다. 또한 Verrazzano가 해당 네임스페이스 Verrazzano 아티팩트에 저장함을 인식하도록 해야 합니다. 따라서 Verrazzano에서 관리하는 대로 bobs-books 네임스페이스를 식별하는 레이블을 추가해야 합니다. 레이블은 의미 있고 사용자와 관련된 객체의 식별 속성을 지정하는 데 사용됩니다. 여기서 bobs-book 네임스페이스의 경우 이 네임스페이스를 Verrazzano에서 관리되는 것으로 표시하는 레이블을 첨부하고 있습니다. _istio-injection=enabled_는 Istio "sidecar"를 사용으로 설정하므로 Istio 프록시를 설정하는 데 도움이 됩니다. Istio 프록시를 사용하면 Istio 게이트웨이와 같은 다른 Istio 서비스에 액세스할 수 있습니다. 앞에서 언급한 속성을 사용하여 봅시다. 네임스페이스에 레이블을 추가하려면 다음 명령을 복사하여 _Cloud Shell_에서 실행하십시오.
    
        <copy>
        kubectl create namespace bobs-books
        kubectl label namespace bobs-books verrazzano-managed=true istio-injection=enabled
        </copy>
        
    
    ![이름 공간 생성](images/createnamespace.png " ")
    
3.  다음 명령을 복사하여 스크립트를 다운로드합니다. 이 스크립트는 Oracle Container Registry에 대한 사용자를 인증합니다. 인증에 성공하면 도커 레지스트리 암호가 생성됩니다. Docker 레지스트리는 일반 코드의 경우 GitHub와 같은 이미지를 저장하고 버전 지정할 수 있는 방법이지만 컨테이너(Kubernetes가 가져올 수 있음)의 경우 이미지를 버전 지정할 수 있습니다. 여기서는 Oracle Container Registry에서 Bobby의 Books 예제 이미지를 가져올 수 있도록 도커 레지스트리 암호를 생성합니다. 다음 명령에서 _복사_를 누르고 선택한 텍스트 편집기에 붙여 넣은 다음 Oracle Container Registry에서 이미지를 다운로드하기 위한 라이센스 계약을 수락하기 위해 사용자 이름 및 비밀번호를 작업 1에서 사용한 전자메일 ID 및 비밀번호로 대체합니다. 그런 다음 Cloud Shell에서 다음과 같이 수정된 명령을 붙여넣습니다.
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/deploy-bobsbook/create_secret.sh >~/create_secret.sh
        chmod 777 create_secret.sh
        ./create_secret.sh username 'password'    
        </copy>
        
    
    ![암호 생성](images/createsecret.png " ")
    
    > 비밀번호를 작은 따옴표로 묶어 입력하십시오.
    
4.  인증서로 여러 개의 Kubernetes 암호를 만들어야 합니다. Bobby's Books 애플리케이션에는 두 개의 WebLogic 도메인인 _bobby-front-end_ 및 _bobs-bookstore_가 있습니다. WebLogic 도메인에 대한 인증서는 Kubernetes 암호에 유지됩니다. 여기서 암호 이름은 WebLogic 도메인 리소스의 _webLogicCredentialsSecret_를 사용하여 지정됩니다. 또한 도메인이 실행될 네임스페이스에 도메인 인증서 암호를 만들어야 합니다. WebLogic 서버 도메인에서 사용되는 _bobbys-front-end-weblogic-credentials_ 및 _bobs-bookstore-weblogic-credentials_라는 암호를 만들어야 합니다. 사용자 이름 값은 `weblogic`이고 암호는 봅시다. Bobby의 Books 애플리케이션은 _mysql_ 데이터베이스를 사용합니다. 따라서 사용자 이름 값 `weblogic`, 무작위로 생성된 비밀번호, JDBC URL _jdbc:mysql://mysql.bobs-books.svc.cluster.local:3306/books_를 사용하여 _mysql-credentials_라는 새 암호를 봅시다. 이 값은 WebLogic DataSource 객체의 JDBC 접속 문자열에 사용됩니다. 명령 블록을 복사하여 _Cloud Shell_에 붙여 넣으십시오.
    
        <copy>
        export WLS_USERNAME=weblogic
        export WLS_PASSWORD=$((< /dev/urandom tr -dc 'A-Za-z0-9"'\''/<=>?\^_`|~' | head -c10);(date +%S))
        echo $WLS_PASSWORD
        kubectl create secret generic bobbys-front-end-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic bobs-bookstore-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic mysql-credentials \
            --from-literal=username=$WLS_USERNAME \
            --from-literal=password=$WLS_PASSWORD \
            --from-literal=url=jdbc:mysql://mysql.bobs-books.svc.cluster.local:3306/books \
            -n bobs-books
        cd ~
        </copy>
        
    
    ![리소스 생성](images/createresource.png " ")
    
5.  노드 3개가 있는 Kuberneter 클러스터 _cluster1__verrazzano_가 있습니다. 이제 Bobby's Books 컨테이너화된 애플리케이션을 _cluster1__verrazzano_에 배포하려고 합니다. 이를 위해서는 Kubernetes 배포 구성이 필요합니다. 이 배치는 Kubernetes가 Bobby's Books 애플리케이션에 대한 인스턴스를 만들고 업데이트하도록 지시합니다. 여기서는 Kubernetes가 Bobby's Books 애플리케이션을 배치하도록 지시하는 `bobs-books-comp.yaml` 파일이 있습니다. 다음과 같이 다음 두 명령을 복사하여 붙여넣습니다. `bobs-books-comp.yaml` 파일에는 다양한 OAM 구성요소의 정의가 포함되어 있습니다. 여기서 OAM 구성요소는 애플리케이션의 일반 구성 및 환경 요구사항을 설명하는 Kubernetes 사용자정의 리소스입니다. `bobs-books-comp.yaml` 파일에 대한 자세한 내용은 이 실습 3의 소개 섹션에서 Verrazzano Components를 검토하십시오.
    
        <copy>kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh created
        component.core.oam.dev/robert-helidon created
        component.core.oam.dev/bobby-coh created
        component.core.oam.dev/bobby-helidon created
        component.core.oam.dev/bobby-wls created
        component.core.oam.dev/bobs-mysql-configmap created
        component.core.oam.dev/bobs-mysql-service created
        component.core.oam.dev/bobs-mysql-deployment created
        component.core.oam.dev/bobs-orders-configmap created
        component.core.oam.dev/bobs-orders-wls created
        $
        
6.  `bobs-books-app.yaml` 파일은 환경별 사용자정의를 제공하는 Verrazzano 애플리케이션 구성 파일입니다. `bobs-books-app.yaml` 파일에 대한 자세한 내용은 이 실습 3의 소개 섹션에서 Verrazzano 애플리케이션 구성을 검토하십시오.
    
        <copy>kubectl apply -f ~/bobs-books-app.yaml -n bobs-books</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $ kubectl apply -f ~/bobs-books-app.yaml -n bobs-books
        applicationconfiguration.core.oam.dev/bobs-books created
        $
        
7.  Bobby's Books 예제 응용 프로그램의 모든 POD가 _Running_ 상태가 될 때까지 기다립니다. 이 명령을 성공하기 전에 여러 번 반복해야 할 수도 있습니다. WebLogic Server 및 Coherence Pod를 생성하고 준비하려면 다소 시간이 걸릴 수 있습니다. 이 _kubectl_ 명령은 모든 Pod가 kubectl 네임스페이스 내에서 _Running_ 상태가 될 때까지 기다립니다. 4~5분 정도 걸립니다. 5분 이상 기다리면 명령을 다시 실행할 수 있습니다.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
          $ kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s
          pod/bobbys-coherence-0 condition met
          pod/bobbys-front-end-adminserver condition met
          pod/bobbys-front-end-managed-server1 condition met
          pod/bobbys-helidon-stock-application-5f74cbcc8b-cw4x4 condition met
          pod/bobs-bookstore-adminserver condition met
          pod/bobs-bookstore-managed-server1 condition met
          pod/mysql-6bc8f9f785-n4qjh condition met
          pod/robert-helidon-65b8874988-7x5vj condition met
          pod/robert-helidon-65b8874988-vnntp condition met
          pod/roberts-coherence-0 condition met
          pod/roberts-coherence-1 condition met
          $
        

## 작업 3: Bobby's Book 응용 프로그램의 성공적인 배치 확인

애플리케이션 구성, 도메인, Coherence 리소스 및 수신 특성이 모두 있는지 확인하십시오.

1.  _Bobby's Books_ 응용 프로그램이 봅시다 이름 공간에 성공적으로 배치되었는지 확인합니다.
    
        <copy>kubectl get ApplicationConfiguration -n bobs-books</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $ kubectl get ApplicationConfiguration -n bobs-books
          NAME         AGE
          bobs-books   72m
        $
        
2.  두 WebLogic 도메인이 모두 bobs-books 이름 공간 내에 성공적으로 만들어졌는지 확인합니다.
    
        <copy>kubectl get Domain -n bobs-books</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $ kubectl get Domain -n bobs-books
          NAME               AGE
          bobbys-front-end   73m
          bobs-orders-wls    73m
        $
        
3.  두 Coherence 클러스터가 모두 봅스-북 네임스페이스 내에 성공적으로 생성되었는지 확인합니다.
    
        <copy>kubectl get Coherence -n bobs-books</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $ kubectl get Coherence -n bobs-books
        NAME              CLUSTER           ROLE           REPLICAS  READY PHASE
        bobbys-coherence  bobbys-coherence  bobbys-coherence  1       1    Ready
        roberts-coherence roberts-coherence roberts-coherence 2       2    Ready
        $
        
4.  Bobby's Book 응용 프로그램의 IngressTrait를 가져오려면 _Cloud Shell_에서 다음 명령을 실행합니다.
    
        <copy>kubectl get IngressTrait -n bobs-books</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $ kubectl get IngressTrait -n bobs-books
          NAME                               AGE
          bobby-wls-trait-79b67d9d88         76m
          bobs-orders-wls-trait-57b4d8cb4b   76m
          robert-helidon-trait-54d7bcd54b    76m
        $
        
5.  서비스 Pod가 성공적으로 생성되고 _Running_ 상태로 전환되는지 확인합니다. 이 작업은 몇 분 정도 걸릴 수 있으며 일부 서비스가 종료되고 다시 시작될 수 있습니다. 마지막으로, bobs-books 네임스페이스와 연관된 모든 pod가 _Running_ 상태인지 확인합니다. _bobbys-helidon-stock-application_에 대한 포드 세부 정보를 복사하십시오.
    
        <copy>kubectl get pods -n bobs-books</copy>
        
    
        $ kubectl get pods -n bobs-books
        NAME                                          READY STATUS    RESTARTS   AGE
        bobbys-coherence-0                             2/2  Running   0          7m51s
        bobbys-front-end-adminserver                   4/4  Running   0          5m28s
        bobbys-front-end-managed-server1               4/4  Running   0          4m30s
        bobbys-helidon-stock-application-5f74cbc-cw4x4 2/2  Running   0          7m54s
        bobs-bookstore-adminserver                     4/4  Running   0          4m31s
        bobs-bookstore-managed-server1                 4/4  Running   0          3m41s
        mysql-6bc8f9f785-n4qjh                         2/2  Running   0          5m52s
        robert-helidon-65b8874988-7x5vj                2/2  Running   0          7m53s
        robert-helidon-65b8874988-vnntp                2/2  Running   0          7m54s
        roberts-coherence-0                            2/2  Running   0          7m52s
        roberts-coherence-1                            2/2  Running   0          7m51s
        $
        
    
    > **bobbys-helidon-stock-application**의 Pod 이름을 기록합니다. 이 구성요소를 재배치하면 이 POD가 _종료_ 상태로 전환되고 새 POD가 시작되고 Lab 7의 _실행 중_ 상태가 됩니다.
    
    _Cloud Shell_은 열어 둡니다. 다음 실습에도 사용됩니다.
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
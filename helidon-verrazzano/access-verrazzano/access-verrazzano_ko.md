# Verrazzano를 사용하여 애플리케이션 액세스 및 모니터링

## 소개

Helidon _quickstart-mp_ 응용 프로그램을 배치했습니다. 이 실습에서는 애플리케이션에 액세스하고 Verrazzano에서 제공하는 여러 관리 도구를 사용하여 확인합니다.

예상 시간: 15분

**Grafana**

Grafana는 데이터 분석을 실행하기 위한 오픈 소스 솔루션으로, 방대한 양의 데이터를 파악하는 측정지표를 가져오고, 멋진 커스터마이징 가능한 대시보드를 통해 앱을 모니터링합니다. 이 도구는 기술적으로 시계열 분석이라고 불리는 일정 시간 동안 데이터를 연구, 분석 및 모니터링하는 데 도움이 됩니다. 사용자 동작, 응용 프로그램 동작, 운용 또는 사전 운용 환경에서 팝업되는 오류 빈도, 팝업되는 오류 유형 및 상대적 데이터를 제공하여 상황별 시나리오를 추적하는 데 유용합니다.

[https://grafana.com/grafana/](https://grafana.com/grafana/)

**OpenSearch 대시보드**

OpenSearch 대시보드는 OpenSearch 클러스터에서 인덱스화된 콘텐츠에 대한 시각화 대시보드입니다. Verrazzano는 OpenSearch 대시보드 배포를 생성하여 OpenSearch에서 수집된 로그 데이터를 쿼리 및 시각화하기 위한 사용자 인터페이스를 제공합니다.

OpenSearch Dashboards 콘솔에 액세스하려면 [Access Verrazzano](https://verrazzano.io/latest/docs/access/)를 참조하십시오.

OpenSearch 대시보드를 통해 OpenSearch 인덱스의 레코드를 보려면 원하는 인덱스 아래에 레코드를 필터링할 인덱스 패턴을 생성합니다.

작업 3에서는 OpenSearch 대시보드를 살펴봅니다.

[https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)

**Prometheus**

Prometheus는 모니터링 및 경고 툴킷입니다. Prometheus는 척도를 시계열 데이터로 수집 및 저장합니다. 즉, 척도 정보는 기록된 시간 기록과 함께 레이블이라는 선택적 키-값 쌍과 함께 저장됩니다.

[https://prometheus.io/](https://prometheus.io/)

**랜셔**

Rancher는 Verrazzano가 운영 중인 여러 Kubernetes 클러스터에서 컨테이너를 실행할 수 있게 해주는 플랫폼입니다. 이를 통해 하이브리드가 가능하므로 온프레미스 클러스터를 단일 창에서 관리하고 클라우드 서비스에 호스팅할 수 있습니다.

[https://rancher.com/](https://rancher.com/)

**키알리**

Kiali는 Istio의 사용자 인터페이스이며 사용하기 매우 간단합니다. Verrazzano 콘솔에서 Kiali에 직접 액세스할 수 있습니다. 여기에서 트래픽 흐름과 핫 스팟 또는 문제 영역을 볼 수 있습니다. 그런 다음 드릴 다운하여 각 항목의 세부 정보를 볼 수 있습니다. Kiali는 Prometheus에 저장된 Envoy의 측정지표 데이터를 사용하여 그래픽 모델을 구축합니다.

### 목표

이 실습에서는 다음을 수행합니다.

*   Rancher 콘솔을 살펴봅니다.
*   Grafana 콘솔을 살펴보세요.
*   OpenSearch 대시보드를 살펴봅니다.
*   Prometheus 콘솔을 살펴봅니다.
*   Rancher 콘솔을 살펴봅니다.
*   Kiali 콘솔을 살펴보십시오.

### 필요 조건

*   Oracle Cloud Infrastructure에서 실행되는 OKE(Kubernetes) 클러스터입니다.
*   Kubernetes(OKE) 클러스터에 설치된 Verrazzano입니다.
*   Helidon _quickstart-mp_ 애플리케이션을 배치했습니다.

## 작업 1: Rancher Console 살펴보기

Verrazzano는 여러 콘솔을 설치합니다. 설치 끝점은 설치된 Verrazzano 사용자 정의 리소스의 `Status` 필드에 저장됩니다.

1.  다음 명령을 사용하여 이러한 콘솔의 끝점을 가져올 수 있습니다.
    
        <copy>kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $ kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .
        {
        "consoleUrl": "https://verrazzano.default.xx.xx.xx.xx.nip.io",
        "grafanaUrl": "https://grafana.vmi.system.default.xx.xx.xx.xx.nip.io",
        "keyCloakUrl": "https://keycloak.default.xx.xx.xx.xx.nip.io",
        "kialiUrl": "https://kiali.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchDashboardsUrl": "https://osd.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchUrl": "https://opensearch.vmi.system.default.xx.xx.xx.xx.nip.io",
        "prometheusUrl": "https://prometheus.vmi.system.default.xx.xx.xx.xx.nip.io",
        "rancherUrl": "https://rancher.default.xx.xx.xx.xx.nip.io"
        }
        $
        
2.  `https://rancher.default.YOUR_UNIQUE_IP.nip.io`를 사용하여 Rancher 콘솔을 엽니다.
    
3.  Verrazzano _dev_ 프로파일은 자체 서명된 인증서를 사용하므로 위험을 수락하고 경고를 건너뛰려면 **고급**을 눌러야 합니다.
    
    ![고급](images/rancher-advanced.png)
    
4.  **Proceed to rancher default XX.XX.XX.XX.nip.io(unsafe)**를 누릅니다. 계속 진행하기 위해 이 옵션을 사용하지 않는 경우, 이 크롬 브라우저 창 안의 아무 곳이나 공간 없이 _thisisunsafe_를 입력하면 됩니다. 크롬 브라우저 창에 입력할 때 볼 수 없지만 _thisisunsafe_ 입력을 완료하면 즉시 다음 페이지를 볼 수 있습니다. 자세한 내용은 [여기](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)에서 확인할 수 있습니다.
    
    ![진행](images/rancher-proceed.png)
    
5.  _Log in with Keycloak_을 누릅니다. ![로그인 취소](images/keycloak-login.png)
    
6.  인증을 위해 Keycloak 콘솔 URL로 재지정되므로 **Advanced**를 누릅니다.
    
    ![Keycloak 인증](images/keycloak-advanced.png)
    
7.  **Proceed to Keycloak default XX.XX.XX.XX.nip.io(unsafe)**를 누릅니다. 계속 진행하기 위해 이 옵션을 사용하지 않는 경우, 이 크롬 브라우저 창 안의 아무 곳이나 공간 없이 _thisisunsafe_를 입력하면 됩니다. 크롬 브라우저 창에 입력할 때 볼 수 없지만 _thisisunsafe_ 입력을 완료하면 즉시 다음 페이지를 볼 수 있습니다. 자세한 내용은 [여기](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)에서 확인할 수 있습니다.
    
    ![진행](images/keycloak-proceed.png)
    
8.  이제 Verrazzano 콘솔의 사용자 이름과 비밀번호가 필요합니다. _Username_은 _verrazzano_이며 암호를 찾으려면 _Cloud Shell_로 돌아가서 다음 명령을 붙여넣어 _Rancher Console_의 암호를 찾습니다.
    
        <copy>kubectl get secret --namespace verrazzano-system verrazzano -o jsonpath={.data.password} | base64 --decode; echo</copy>
        
9.  암호를 복사하고 _Rancher Console_이 열려 있는 브라우저로 돌아갑니다. _Password(암호)_ 필드에 암호를 붙여넣고 _verrazzano_를 _Username(사용자 이름)_으로 입력한 다음 **Sign In(사인인)**을 누릅니다.
    
    ![SignIn](images/verrazzano-sign-in.png)
    
10.  랜셔 콘솔의 홈 페이지에서 Verrazzano 링크를 볼 수 있습니다. console.Click _햄버거 메뉴_ -> _Verrazzano_에서 이러한 콘솔에 액세스할 수 있습니다.
    
    ![Rancher Home](images/rancher-home.png)
    
11.  _애플리케이션_을 누릅니다. 이 섹션은 네임스페이스가 있는 모든 애플리케이션을 보여주며 Verrazzano에서 관리합니다. _hello-helidon_ 이름 공간 내에서 _hello-helidon-appconf_ 응용 프로그램을 누릅니다. ![Helidon 애플리케이션](images/helidon-application.png)
    
12.  애플리케이션과 연관된 Pod를 볼 수 있습니다. Pod 이름은 해당 특정 복제본을 식별하기 위해 자동 생성된 고유 문자열을 포함합니다. Pod 로그를 보려면 _세 개의 점_ -> _로그 보기_를 누릅니다. ![Helidon 구성요소](images/view-pod-logs.png)
    
13.  애플리케이션에 대한 애플리케이션 로그를 볼 수 있습니다. 애플리케이션 로그가 표시되지 않는 경우 **설정**(기어 아이콘이 있는 파란색 버튼)을 누르고 시간 필터를 변경하여 컨테이너 시작의 모든 로그 항목을 표시합니다. 애플리케이션과 연관된 구성요소를 보려면 _구성요소_를 누릅니다. ![애플리케이션 로그](images/view-pod-log.png)
    
14.  이 애플리케이션에는 관련 리소스 보기가 component.To 하나만 있습니다. _관련 리소스_를 누릅니다. ![Helidon 리소스](images/helidon-resource.png) ![Helidon 리소스](images/helidon-resources.png)
    
15.  _Hamburgar 메뉴_ -> _로컬_을 눌러 _Cluster Explorer_를 엽니다. _클러스터 탐색기_를 사용하면 Rancher UI에서 Kubernetes 클러스터의 모든 사용자정의 리소스 및 CRD를 보고 조작할 수 있습니다. ![Verrazzano 클러스터](images/verrazzano-cluster.png)
    
16.  대시보드에서는 클러스터 및 배치된 애플리케이션의 개요를 제공합니다. 리소스 수는 시스템을 포함한 거의 모든 리소스인 _User Namespaces_에 속합니다. 대시보드 상단의 네임스페이스로 필터링할 수 있지만 지금은 필요하지 않습니다. 노드의 현재 로드에 대한 개요를 보려면 왼쪽 메뉴에서 **노드** 항목을 누르십시오.
    
    ![클러스터 탐색기](images/cluster-dashboard.png)
    
17.  전체 배포는 OKE 클러스터에 영향을 주지 않습니다. 이제 왼쪽 메뉴에서 **배치** 항목을 눌러 배치된 애플리케이션을 확인합니다.
    
    ![노드](images/cluster-nodes.png)
    
18.  여러 배치를 볼 수 있습니다.
    
    ![배포](images/deployment.png)
    

## 작업 2: Grafana 콘솔 살펴보기

1.  _햄버거 메뉴_ -> _홈_을 누릅니다. ![홈](images/ranchar-menu.png)
    
2.  홈 페이지에 _Grafana 콘솔_ 열기 링크가 표시됩니다. 다음과 같이 **Grafana 콘솔**에 대한 링크를 누릅니다.
    
    ![Grafana 홈](images/grafana-link.png)
    
3.  **고급**을 누릅니다.
    
    ![고급](images/grafana-advanced.png)
    
4.  **Proceed to grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)**를 누릅니다. 계속 진행하기 위해 이 옵션을 사용하지 않는 경우, 이 크롬 브라우저 창 안의 아무 곳이나 공간 없이 _thisisunsafe_를 입력하면 됩니다. 크롬 브라우저 창에 입력할 때 볼 수 없지만 _thisisunsafe_ 입력을 완료하면 즉시 다음 페이지를 볼 수 있습니다. 자세한 내용은 [여기](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)에서 확인할 수 있습니다.
    
    ![진행](images/grafana-proceed.png)
    
5.  Grafana 홈 페이지가 열립니다. 왼쪽 상단에서 **홈**을 누릅니다.
    
    ![홈](images/grafana-home.png)
    
6.  `Helidon`를 입력하면 **일반** 아래에 _Helidon Monitoring Dashboard_가 표시됩니다. **Helidon Monitoring Dashboard**를 누릅니다.
    
    ![Helidon 대시보드](images/search-helidon.png)
    
7.  Status, Heap Usage, Running Time, JVM Heap, Thread Count, HTTP Requests 등과 같은 Helidon _quickstart-mp_ 응용 프로그램의 JVM 세부 정보를 살펴봅니다. 이는 Helidon 워크로드를 위해 특별히 사전 구축된 대시보드입니다. 물론 필요에 따라 이 대시보드를 사용자 정의하고 사용자 정의 진단 정보를 추가할 수 있습니다.
    
    ![대시보드](images/helidon-dashboard.png)
    

## 작업 3: OpenSearch 대시보드 탐색

1.  Verrazzano 홈 페이지로 돌아가서 **OpenSearch Dashboards** 콘솔을 누릅니다.
    
    ![Kibana 링크](images/opensearch-link.png)
    
2.  필요한 경우 _Proceed to ... default XX.XX.XX.XX.nip.io(unsafe)_를 누릅니다. 처음 _OpenSearch Dashboards_에 시작 페이지가 표시됩니다. Verrazzano가 필요한 구성을 완료하고 애플리케이션 데이터를 이미 사용할 수 있기 때문에 OpenSearch을 시도하기 위해 내장된 샘플 데이터를 제공하지만 **자체 탐색** 옵션을 선택할 수 있습니다.
    
    ![Kibana 시작 페이지](images/opensearch-proceed.png)
    
3.  OpenSearch 홈페이지에서 **홈** -> **검색**을 누릅니다.
    
    ![Kibana 대시보드 누르기](images/discover-1.png)
    
4.  표시된 대로 _`verrazzano-application*`_ 이름 공간을 선택한 다음 Helidon 애플리케이션에서 생성한 사용자정의 로그 항목 값(`help` )을 필터 텍스트 상자에 입력합니다. **Enter** 키를 누르거나 **Refresh**를 누릅니다. 적어도 하나의 결과를 얻어야 합니다.
    
    > 애플리케이션 끝점에 도달하지 않았거나 오래 전에 발생한 경우 끝점에 대해 Cloud Shell에서 다음 HTTP 요청을 다시 호출하십시오. 요청을 여러 번 실행할 수 있습니다.
    
        <copy>curl -k https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings; echo</copy>
        
    
    ![로그 결과](images/log-result.png)
    

## 작업 4: Prometheus 콘솔 살펴보기

1.  Verrazzano 홈 페이지로 돌아가서 **Prometheus** 콘솔을 누릅니다.
    
    ![프로메테우스 링크](images/prometheus-link.png)
    
2.  메시지가 표시되면 **Proceed to ... default XX.XX.XX.XX.nip.io(unsafe)**를 누릅니다.
    
3.  Prometheus 대시보드 페이지에서 검색 필드에 _help_를 입력하고 사용자 정의 측정항목 _application \_me \_user \_mp \_quickstart \_GreetHelpResource \_helpCalled \_total_을 누릅니다.
    
    ![프로메테우스 실행](images/prometheus-query.png)
    
4.  **실행**을 누르고 아래 결과를 확인합니다. 측정항목의 현재 값이 끝점에 의해 완료된 요청 수를 의미합니다. _콘솔_ 모드 대신 _그래프_ 보기로 전환할 수도 있습니다.
    
    ![프로메테우스 값](images/execute-query.png) ![그래프 보기](images/graph-view.png)
    
    > 대시보드에 다른 측정지표를 추가할 수도 있습니다. 목록에서 사용 가능한 기본 측정항목을 검색합니다.
    

## 작업 5: Kiali 콘솔 살펴보기

1.  Verrazzano 홈 페이지로 돌아가서 **Kiali** 콘솔을 누릅니다.
    
    ![링크 취소](images/kiali-link.png)
    
2.  메시지가 표시되면 **Proceed to ... default XX.XX.XX.XX.nip.io(unsafe)**를 누릅니다.
    
3.  왼쪽에서 그래프를 클릭합니다.
    
    ![Kiali 대시보드](images/kiali-dashboard.png " ")
    
4.  Namespace 드롭다운에서 _verrazzano-system_의 확인란을 선택하고 커서가 드롭다운 밖으로 이동하도록 합니다. ![Bobs 네임스페이스](images/verrazzano-namespace.png " ")
    
5.  _verrazzano-system_ 이름 공간의 그래픽 보기를 확인할 수 있습니다. _범례_를 눌러 범례 뷰를 봅니다.
    
    ![그래픽 보기](images/graphical-view.png " ")
    
6.  여기서 각 모양이 나타내는 항목을 볼 수 있습니다. 예를 들어 원은 _작업 로드_를 나타냅니다.
    
    ![범례 뷰](images/legend-view.png " ")
    
7.  왼쪽에서 _애플리케이션_을 눌러 배치된 애플리케이션을 봅니다.
    
    ![애플리케이션](images/applications.png " ")
    

## 작업 6: Keycloak 콘솔 살펴보기

1.  Verrazzano 홈 페이지로 돌아가서 **Keycloak** 콘솔을 누릅니다.
    
    ![Keycloak 링크](images/keycloak-link.png)
    
2.  메시지가 표시되면 **Proceed to ... default XX.XX.XX.XX.nip.io(unsafe)**를 누릅니다.
    
3.  Welcome to Keycloak 페이지에서 _Administration Console_을 누릅니다. ![Keycloak 홈](images/keycloak-home.png)
    
4.  이제 Keycloak 콘솔에 대한 사용자 이름과 암호가 필요합니다. _Username_은 _keycloakadmin_이며 암호를 찾으려면 _Cloud Shell_로 돌아가서 다음 명령을 붙여넣어 _Keycloak Console_의 암호를 찾습니다.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  암호를 복사하고 _Keycloak Console_이 열려 있는 브라우저로 돌아갑니다. _Password(암호)_ 필드에 암호를 붙여넣고 _keycloakadmin_을 _Username(사용자 이름)_으로 입력한 다음 **Sign In(사인인)**을 누릅니다.
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  여기서 Verrazzano에서 수행된 기본 구성을 볼 수 있습니다. ![Keycloak 홈](images/keycloak-realms.png)
    

Verrazzano 랩에서 Helidon 애플리케이션 배포를 완료하신 것을 축하드립니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
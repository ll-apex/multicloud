# Bobby's Books 샘플 응용 프로그램 액세스 Verrazzano, Grafana 및 Kiali Console 살펴보기

## 소개

Lab 3에서는 Bobby's Book 애플리케이션을 구축했습니다. 이 실습에서는 애플리케이션에 액세스하여 작동 중인지 확인합니다. 나중에 Verrazzano 및 Grafana 콘솔을 살펴볼 것입니다.

예상 시간: 10분

### 목표

이 실습에서는 다음을 수행합니다.

*   Bobby's Book 응용 프로그램에 액세스합니다.
*   Verrazzano 콘솔을 살펴보세요.
*   Grafana 콘솔을 살펴보세요.

### 필요 조건

\* Oracle Cloud Infrastructure에 OKE 클러스터를 생성하는 Lab 1을 실행합니다. \* Oracle Cloud Infrastructure의 OKE 클러스터에 액세스하도록 kubectl을 구성하는 Lab 1을 실행합니다.

*   Kubernetes 클러스터에 Verrazzano를 설치하는 Lab 2를 실행합니다.
*   Bobby's Book 응용 프로그램을 배치하는 Lab 3을 실행합니다.
*   환경에 따라 명령 및 URL을 붙여넣고 수정할 수 있는 텍스트 편집기가 있어야 합니다. 그런 다음 _Cloud Shell_에서 실행하기 위해 수정된 명령을 복사하여 붙여넣을 수 있습니다.
*   Bobby's Books 애플리케이션인 Verrazano, Grafana 및 Kiali Console의 URL을 여는 데 Firefox를 사용하는 것이 좋습니다. 그러나 Chrome을 사용하려는 경우 문서화되지 않은 'thisisunsafe' 임시해결책을 사용하여 크롬에서 인증서를 수락해야 합니다.

## 작업 1: Bobby's Book 응용 프로그램 액세스

1.  Bobby's Book 애플리케이션에 액세스할 수 있는 `EXTERNAL_IP` 주소가 필요합니다. istio-ingressgateway 서비스의 `EXTERNAL_IP` 주소를 가져오려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣습니다.
    
        <copy> kubectl get service \
        -n "istio-system" "istio-ingressgateway" \
        -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
           $ kubectl get service \
           > -n "istio-system" "istio-ingressgateway" \
           > -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo
           XX.XX.XX.XX
        
2.  Robert의 Book Store 홈 페이지를 열려면 다음 URL을 복사하고 _XX.XX.XX.XX_를 다음 이미지와 같이 마지막 단계에서 얻은 _EXTERNAL\_IP_ 주소로 바꿉니다.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/</copy>
        
    
    ![Roberts Books 페이지](images/robertbooks.png " ")
    
3.  다음과 같이 _Advanced_를 누릅니다.
    
    ![로버트 어드밴스](images/robertadvanced.png " ")
    
4.  Select _Proceed to bobs-books.bobs-books. EXTERNAL\_IP .nip.io(unsafe)_ to access the application. If you are the not getting this option for proceed, just type _thisisunsafe_ without any space anywhere inside this chrome browser window. As you are typing in the chrome browser window, you can't see it, but as soon as you finish typing _thisisunsafe_, you can see application page immediately. You can find more details [here](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Robert Unsafe](images/robertunsafe.png " ") ![로버트 페이지](images/robertpage.png " ")
    
5.  Bobby의 Book Store 홈 페이지를 열려면 새 탭을 열고 다음 URL을 복사하고 다음 이미지와 같이 _XX.XX.XX.XX_를 `EXTERNAL_IP` 주소로 바꿉니다.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![Bobbys URL](images/bobbysurl.png " ")
    
    ![보비 서점](images/bobbysbookstore.png " ")
    
    > 이 페이지는 Lab 8에서 사용되므로 열어 둡니다.
    
6.  Bobby의 장부 주문 관리자 UI를 열려면 새 탭을 열고 다음 URL을 복사하고 다음 이미지와 같이 _XX.XX.XX.XX_를 _EXTERNAL\_IP_ 주소로 바꿉니다.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobs-bookstore-order-manager/orders</copy>
        
    
    ![주문 관리 책임자](images/ordermanager.png " ")
    
7.  _Bobby's Books_ 페이지로 돌아가서 장부를 구매합니다. 다음 이미지와 같이 _장부_를 누릅니다.
    
    ![주문 체크아웃](images/checkoutorder.png " ")
    
8.  다음 이미지와 같이 _Twilight_ 장부에 대한 이미지를 선택합니다.
    
    ![구매 장부](images/purchasebook.png " ")
    
9.  먼저 다음 이미지와 같이 _카트에 추가_, _확인_을 차례로 누릅니다.
    
    ![addcart를 누릅니다.](images/clickaddcart.png " ")
    
10.  장부 구매에 대한 세부정보를 입력하십시오. _시/도_에 2자리 시/도 코드를 입력한 다음 _주문 제출_을 누릅니다.
    

![주문 제출](images/submitorder.png " ") 11. _주문 관리자_ 페이지로 돌아가서 _새로고침_ 버튼을 선택하여 주문 관리자에 주문이 성공적으로 기록되었는지 확인합니다.

![주문 확인](images/verify-order.png " ")

## 작업 2: Rancher Console 살펴보기

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
    
10.  랜셔 콘솔의 홈 페이지에서 Verrazzano 링크를 볼 수 있습니다. _햄버거 메뉴_ -> _Verrazzano_를 누릅니다.
    
    ![Rancher Home](images/rancher-home.png)
    
11.  _애플리케이션_을 누릅니다. 이 섹션은 네임스페이스가 있는 모든 애플리케이션을 보여주며 Verrazzano에서 관리합니다. _bobs-books_ 네임스페이스 내에서 _bobs-books_ 애플리케이션을 누릅니다. ![Bobs 애플리케이션](images/bobs-application.png)
    
12.  애플리케이션과 연관된 Pod를 볼 수 있습니다. Pod 이름은 해당 특정 복제본을 식별하기 위해 자동 생성된 고유 문자열을 포함합니다. _bobbys-helidon-stok-application_ 포드의 로그를 보려면 _Three dots(점 3개)_ -> _View Logs(로그 보기)_를 누릅니다. ![Bobbys 로그](images/bobs-logs.png)
    
13.  애플리케이션에 대한 애플리케이션 로그를 볼 수 있습니다. 애플리케이션 로그가 표시되지 않는 경우 **설정**(기어 아이콘이 있는 파란색 버튼)을 누르고 시간 필터를 변경하여 컨테이너 시작의 모든 로그 항목을 표시합니다. 애플리케이션과 연관된 구성요소를 보려면 _구성요소_를 누릅니다. ![Bobbys 로그](images/bobs-log.png)
    
14.  여기서 _bobs-books_ 애플리케이션의 모든 구성요소를 볼 수 있습니다. 관련 리소스를 보려면 _관련 리소스_를 누릅니다. ![Bobs 리소스](images/bobs-resource.png) ![Bobs 리소스](images/bobs-resources.png)
    
15.  _Hamburgar 메뉴_ -> _로컬_을 눌러 _Cluster Explorer_를 엽니다. _클러스터 탐색기_를 사용하면 Rancher UI에서 Kubernetes 클러스터의 모든 사용자정의 리소스 및 CRD를 보고 조작할 수 있습니다. ![Verrazzano 클러스터](images/verrazzano-cluster.png)
    
16.  대시보드에서는 클러스터 및 배치된 애플리케이션의 개요를 제공합니다. 리소스 수는 시스템을 포함한 거의 모든 리소스인 _User Namespaces_에 속합니다. 대시보드 상단의 네임스페이스로 필터링할 수 있지만 지금은 필요하지 않습니다. 노드의 현재 로드에 대한 개요를 보려면 왼쪽 메뉴에서 **노드** 항목을 누르십시오.
    
    ![클러스터 탐색기](images/cluster-dashboard.png)
    
17.  전체 배포는 OKE 클러스터에 영향을 주지 않습니다. 이제 왼쪽 메뉴에서 **배치** 항목을 눌러 배치된 애플리케이션을 확인합니다.
    
    ![클러스터 노드](images/cluster-nodes.png)
    
18.  여러 배치를 볼 수 있습니다.
    
    ![배포](images/cluster-deployments.png)
    

## 작업 3: Grafana 콘솔 살펴보기

1.  _Hamburgar 메뉴_ -> _Home_을 눌러 Rancher 홈 페이지를 엽니다. ![홈](images/ranchar-menu.png)
    
2.  홈 페이지에 _Grafana 콘솔_ 열기 링크가 표시됩니다. 다음과 같이 **Grafana 콘솔**에 대한 링크를 누릅니다.
    
    ![Grafana 홈](images/grafana-link.png)
    
3.  _고급_을 누릅니다.
    
    ![Grafana 고급](images/grafanaadvanced.png " ")
    
4.  _grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)_를 선택합니다. 계속하기 위해 이 옵션을 사용하지 않는 경우 공간 없이 _thisisunsafe_를 입력하십시오.
    
    ![Grafana 진행](images/grafanaproceed.png " ")
    
5.  Grafana 홈 페이지가 열립니다. 왼쪽 상단에서 _Home_을 선택합니다.
    
    ![Grafana 홈페이지](images/grafana-homepage.png " ")
    
6.  _WebLogic_를 입력하면 _일반_ 아래에 _WebLogic 서버 대시보드_가 표시됩니다. _WebLogic Server Dashboard_를 선택합니다.
    
    !\[검색 WebLogic\](images/search-weblogic.png" ")
    
    여기서 _도메인_ 및 실행 중인 서버, 배치된 애플리케이션, 서버 이름 및 해당 상태, 힙 사용량, 실행 시간, JVM 힙의 두 도메인을 관찰할 수 있습니다. 애플리케이션에 JDBC 및 JMS와 같은 리소스가 있는 경우 여기에서 세부정보를 확인할 수도 있습니다.
    
    ![WebLogic 대시보드](images/weblogic-dashboard.png " ")
    
7.  이제 WebLogic Server Dashboard를 선택하고 _Helidon_을 입력하면 _Helidon Monitoring Dashboard_가 표시됩니다. _Helidon Monitoring Dashboard_를 선택합니다.
    
    ![Helidon ](images/search-helidon.png " ")
    
    여기에서 애플리케이션의 _상태_ 및 해당 _작동 시간_, 불필요한 정보 수집기, 스윕 합계 표시 및 해당 시간, 스레드 수 등의 다양한 세부정보를 볼 수 있습니다.
    
    ![Helidon 대시보드](images/helidon-dashboard.png " ")
    
8.  이제 Helidon Monitoring Dashboard를 선택하고 _Coherence_를 입력하면 _Coherence Dashboard Main_이 표시됩니다. _Coherence 대시보드 기본_을 선택합니다.
    
    ![Coherence입니다.](images/search-coherence.png " ")
    
9.  여기서 _Coherence 클러스터_의 세부정보를 확인할 수 있습니다. Bobby's Books 응용 프로그램의 경우 두 개의 Coherence 클러스터가 있습니다. 하나는 Bobby's Books용이고 다른 하나는 Robert's Books용입니다. 두 클러스터를 모두 보려면 _Cluster Name(클러스터 이름)_의 드롭다운 메뉴를 선택해야 합니다.
    
    ![보비 클러스터](images/bobby-cluster.png " ")
    
    ![로버트 클러스터](images/robert-cluster.png " ")
    

## 작업 4: Kiali 콘솔 살펴보기

1.  Verrazzano 홈 페이지로 돌아가서 **Kiali** 콘솔을 누릅니다.
    
    ![링크 취소](images/kiali-link.png)
    
2.  왼쪽에서 그래프를 클릭합니다.
    
    ![Kiali 대시보드](images/kiali-dashboard.png " ")
    
3.  Namespace 드롭다운에서 _bobs-books_의 확인란을 선택하고 커서를 드롭다운 밖으로 이동합니다. ![Bobs 네임스페이스](images/bobs-namespace.png " ")
    
4.  _bobs-books_ 응용 프로그램의 그래픽 뷰를 볼 수 있습니다. _범례_를 눌러 범례 뷰를 봅니다.
    
    ![그래픽 보기](images/graphical-view.png " ")
    
5.  여기서 각 모양이 나타내는 항목을 볼 수 있습니다. 예를 들어 원은 _작업 로드_를 나타냅니다.
    
    ![범례 뷰](images/legend-view.png " ")
    
6.  왼쪽에서 _애플리케이션_을 눌러 배치된 모든 애플리케이션을 봅니다.
    
    ![애플리케이션](images/kiali-applications.png " ")
    

## 작업 5: OpenSearch 대시보드 탐색

1.  Verrazzano 홈 페이지로 돌아가서 **OpenSearch Dashboards** 콘솔을 누릅니다.
    
    ![Kibana 링크](images/opensearch-link.png)
    
2.  필요한 경우 _Proceed to ... default XX.XX.XX.XX.nip.io(unsafe)_를 누릅니다.
    
3.  OpenSearch 홈페이지에서 **홈** -> **검색**을 누릅니다.
    
    ![Kibana 대시보드 누르기](images/discover-1.png)
    
4.  그림과 같이 _`verrazzano-applications*`_ 이름 공간을 선택한 다음 _book_을 검색하고 **Enter** 키를 누르거나 **Refresh**를 누릅니다. _book_이 포함된 로그를 가져와야 합니다. ![로그 결과](images/search-books.png)
    

## 작업 6: Prometheus 콘솔 살펴보기

1.  Verrazzano 홈 페이지로 돌아가서 **Prometheus** 콘솔을 누릅니다.
    
    ![프로메테우스 링크](images/prometheus-link.png)
    
2.  메시지가 표시되면 **Proceed to ... default XX.XX.XX.XX.nip.io(unsafe)**를 누릅니다.
    
3.  Prometheus 대시보드 페이지에서 검색 필드에 _get_을 입력하고 사용자정의 측정항목 _`application_org_books_bobby_BookResource_getBook_total`_을 누릅니다.
    
    ![프로메테우스 실행](images/prometheus-query.png)
    
4.  **실행**을 누르고 아래 결과를 확인합니다. 측정항목의 현재 값이 끝점에 의해 완료된 요청 수를 의미합니다. _콘솔_ 모드 대신 _그래프_ 보기로 전환할 수도 있습니다.
    
    ![프로메테우스 값](images/execute-query.png)
    
    > 대시보드에 다른 측정지표를 추가할 수도 있습니다. 목록에서 사용 가능한 기본 측정항목을 검색합니다.
    

## 작업 7: Keycloak 콘솔 살펴보기

1.  Verrazzano 홈 페이지로 돌아가서 **Keycloak** 콘솔을 누릅니다.
    
    ![Keycloak 링크](images/keycloak-link.png)
    
2.  메시지가 표시되면 **Proceed to ... default XX.XX.XX.XX.nip.io(unsafe)**를 누릅니다.
    
3.  Welcome to Keycloak 페이지에서 _Administration Console_을 누릅니다. ![Keycloak 홈](images/keycloak-home.png)
    
4.  이제 Keycloak 콘솔에 대한 사용자 이름과 암호가 필요합니다. _Username_은 _keycloakadmin_이며 암호를 찾으려면 _Cloud Shell_로 돌아가서 다음 명령을 붙여넣어 _Keycloak Console_의 암호를 찾습니다.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  암호를 복사하고 _Keycloak Console_이 열려 있는 브라우저로 돌아갑니다. _Password(암호)_ 필드에 암호를 붙여넣고 _keycloakadmin_을 _Username(사용자 이름)_으로 입력한 다음 **Sign In(사인인)**을 누릅니다.
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  여기서 Verrazzano에서 수행된 기본 구성을 볼 수 있습니다. ![Keycloak 홈](images/keycloak-realms.png)
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
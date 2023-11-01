# 로그 및 측정항목 탐색기 탐색

## 소개

이 실습에서는 Helidon 애플리케이션의 성공적인 배포를 확인합니다. 또한 로그 및 측정항목 탐색기 서비스를 탐색합니다.

추정시간: 5분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [로그 및 지표 탐색기 살펴보기](videohub:1_7a0qaaif)

### 목표

이 실습에서는 다음을 수행합니다.

*   Helidon 애플리케이션의 성공적인 배포를 확인합니다.
*   OCI Metric Explorer 살펴보기
*   OCI Logging 서비스 살펴보기
*   Helidon 애플리케이션의 실행 가능성 검증

### 필요 조건

*   Oracle Free Tier(체험판), 유료 또는 LiveLabs 클라우드 계정
*   OCI 콘솔에 대한 기본적인 이해

## 태스크 1: Helidon 애플리케이션에 접근

이 작업에서는 curl을 사용하여 GET 및 PUT HTTP 요청을 수행하는 애플리케이션에 액세스합니다.

1.  **코드 편집기**로 돌아가서 다음 명령을 터미널에 복사하여 붙여넣어 배치 노드 **`PUBLIC_IP`**를 환경 변수로 설정합니다.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  다음 명령을 복사하여 붙여넣어 **GET 요청**을 배치합니다. 아래와 유사한 출력이 나타납니다.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  안녕하십니까? 예: **Joe**. 아래와 유사한 출력이 나타납니다.
    
        <copy>curl http://$PUBLIC_IP:8080/greet/Joe</copy>
        {"message":"Hello Joe!","date":[2023,5,23]}
        
4.  Hello를 다른 인사말(예: **Hola**)로 바꿉니다. 아래와 유사한 출력이 나타납니다.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,23]}
        

## 작업 2: OCI Metrics Explorer 살펴보기

이 태스크에서는 **Helidon 측정항목**이 **OCI 모니터링 서비스**로 푸시되는지 검증합니다.

1.  In Cloud Console, Click _Hamburger menu_ -> _Observability & Management_ -> _Metrics Explorer_ under **Monitoring** as shown below. ![측정항목 탐색기](images/metrics-explorer.png)
    
2.  아래로 스크롤하여 **질의** 섹션으로 이동하고 올바른 구획을 선택한 다음 **`helidon_metrics`**을 측정항목 네임스페이스로, **`request.count_counter`**을 측정항목 이름으로 선택하여 질의를 작성합니다. 그런 다음 아래와 같이 _차트 업데이트_를 누릅니다. ![쿼리 생성](images/create-query.png)
    
3.  위쪽으로 스크롤하면 아래와 유사한 출력이 표시됩니다. 그러면 요청 카운터에 대한 그래프 형식의 데이터가 제공됩니다. ![질의 편집기](images/query-editor.png)
    
4.  데이터 테이블에 데이터를 표시하려면 아래와 같이 **데이터 테이블 표시**에 대한 단추를 토글합니다. 버튼을 토글할 때 _Show Data table_이 **Show Graph**로 바뀝니다. ![데이터 테이블](images/data-table.png)
    

## 작업 3: OCI 로깅 서비스 살펴보기

이 작업에서는 로그 그룹 **app-log-group-helidon-ocw-hol**에서 로그 **app-log-helidon-ocw-hol**을 탐색하여 로깅 서비스에 메시지를 푸시하는 **OCI 로깅 SDK** 통합을 검증합니다.

1.  클라우드 콘솔에서 _햄버거 메뉴_ -> _관찰 가능성 및 관리_ -> _로그_를 누릅니다. ![로그 메뉴](images/logs-menu.png)
    
2.  올바른 구획을 선택한 다음 _`app-log-group-helidon-ocw-hol`_를 **Log Group**으로 선택하고 아래와 같이 **app-log-helidon-ocw-hol**을 누릅니다. ![구획 선택](images/select-compartment.png)
    
3.  **시간별 필터링**의 드롭다운에서 **오늘**을 선택하면 애플리케이션 로그를 볼 수 있습니다. ![로그 보기](images/view-logs.png)
    

## 작업 4: Helidon 애플리케이션의 건전성 검사를 통한 효율성 및 준비성 검증

Helidon **oci-mp** 애플리케이션은 **건전성 검사** 기능을 추가하여 **liveness** 및/또는 **준비**를 검증합니다. 프로젝트에서 각각 _GreetLivenessCheck_ 및 _GreetReadinessCheck_ 클래스 파일을 확인하여 수행 방법을 확인할 수 있습니다. 이 기능은 특히 Kubernetes와 같은 오케스트레이터 환경에서 마이크로서비스로 앱을 실행하여 마이크로서비스가 정상이 아닌 경우 다시 시작해야 하는지 확인할 때 유용합니다. 이 실습과 관련하여 준비 검사는 _Helidon 애플리케이션이 성공적으로 시작되었는지 확인_하기 위해 앱이 시작된 후 DevOps 배치 파이프라인 사양에서 활용됩니다. **deployment\_spec.yaml**의 _line #79_에서 코드를 확인하여 동작하는지 확인합니다.

1.  **코드 편집기**로 돌아가서 다음 명령을 터미널에 복사하여 붙여넣어 배치 노드 **`PUBLIC_IP`**를 환경 변수로 설정합니다.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  다음 명령을 복사하여 붙여넣어 **liveness**를 확인합니다. 아래와 유사한 출력이 나타납니다.
    
        <copy>curl http://$PUBLIC_IP:8080/health/live</copy>
        {"status":"UP","checks":[{"name":"CustomLivenessCheck","status":"UP","data":{"time":1684391639448}}]}
        
3.  다음 명령을 복사하여 붙여넣어 **readiness**를 확인합니다. 아래와 유사한 출력이 나타납니다.
    
        <copy>curl http://$PUBLIC_IP:8080/health/ready</copy>
        {"status":"UP","checks":[{"name":"CustomReadinessCheck","status":"UP","data":{"time":1684391438298}}]}
        

이제 **다음 실습으로 진행할 수 있습니다.**

## 자세히 알아보기

*   [Helidon을 사용하는 OCI 애플리케이션](https://medium.com/helidon/oci-application-with-helidon-caa78cacaee5)
*   [Helidon MP 측정항목](https://helidon.io/docs/v3/#/mp/metrics/metrics)
*   [Helidon MP 메트릭 가이드](https://helidon.io/docs/v3/#/mp/guides/metrics)
*   [Helidon MP 건전성](https://helidon.io/docs/v3/#/mp/health)
*   [Helidon MP 상태 점검 가이드](https://helidon.io/docs/v3/#/mp/guides/health)

## 확인

*   **작성자** - Keith Lustria
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
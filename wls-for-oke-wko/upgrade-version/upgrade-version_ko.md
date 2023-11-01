# WebLogic 서버 버전에서 업그레이드(선택사항)

## 소개

이 실습에서는 기본 이미지를 수정하고 _12.2.1.4-slim-ol8_ 태그와 함께 WebLogic Server Image를 사용합니다. 그런 다음 WebLogic Kubernetes 툴킷 UI를 사용하여 도메인을 재배치합니다. 마지막으로 새로 관리되는 서버 Pod가 WebLogic Remote Console을 통해 업데이트된 WebLogic 서버 이미지를 사용하고 있는지 확인합니다.

예상 시간: 10분

### 목표

이 실습에서는 다음을 수행합니다.

*   WebLogic 서버 이미지(12.2.1.4)를 기본 이미지로 사용합니다.
*   WebLogic 도메인을 재배치합니다.

### 필요 조건

*   랩 1에서 만든 noVNC 원격 데스크탑에 액세스합니다.

## 작업 1: 새 WebLogic 서버 이미지의 세부정보를 기본 이미지로 입력

이 작업에서는 업그레이드된 WebLogic Server 12.2.1.4.0 이미지를 사용하도록 기본 이미지를 업데이트합니다.

1.  WebLogic Kubernetes Toolkit UI로 돌아가서 _WebLogic Domain_을 누릅니다. WebLogic 서버 태그를 _12.2.1.4-slim-ol8_로 변경했습니다. ![기본 이미지 태그 업데이트](images/update-primary-image-tag.png)

## 작업 2: 서버 Pod의 롤링 재시작을 통해 배치된 애플리케이션 업데이트

이 작업에서는 WebLogic 도메인을 재배치합니다. 나중에 WebLogic Remote Console을 사용하여 서버 Pod에서 업데이트된 WebLogic Server 12.2.1.4.0 이미지를 사용하고 있는지 확인합니다.

1.  _Deploy Domain(도메인 배치)_을 누릅니다. 그러면 도메인이 다시 배치됩니다. ![도메인 재배치](images/redeploy-domain.png)
    
    > **참고용:**  
    > 기본 이미지를 변경했으므로 서버를 하나씩 롤링 재시작할 수 있습니다. _도메인 배치_를 누르면 실행 중인 관리 서버 포드를 종료하고 WebLogic 서버 12.2.1.4.0 이미지를 사용하는 관리 서버에 대한 새 포드를 생성하는 _통합기 작업_이 시작됩니다. 검토자는 관리 대상 서버와 동일한 프로세스를 수행합니다.
    
2.  _WebLogic Domain Deployment to Kubernetes Complete_ 창이 표시되면 _Ok_를 누릅니다. ![배포 완료](images/deployment-complete.png)
    
3.  _Terminal_으로 돌아가서 아래 명령을 복사하여 터미널에 붙여 넣습니다. 서버 롤링 재시작이 하나씩 발생합니다. 먼저 관리 서버 Pod가 종료되고 _실행 중_ 상태가 됩니다.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![POD 보기](images/view-pods.png)
    
4.  관리 서버 및 관리 서버 Pod가 업데이트된 WebLogic 서버 이미지를 사용 중인지 확인하려면 Monitoring Tree(모니터링 트리) 아이콘을 누른 다음 _Environment(환경)_ -> _Servers(서버)_ -> _admin-server(관리 서버)_를 선택합니다. 12.2.1.4.0을 사용하고 있습니다. ![WLS 버전](images/wls-version.png)
    

Congratulation !!!

이 워크샵의 끝입니다.

이 워크샵이 도움이 되셨기를 바랍니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 10월
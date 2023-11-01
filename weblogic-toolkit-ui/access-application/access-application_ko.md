# 애플리케이션 배치 테스트

## 소개

### WebLogic Remote Console 정보

WebLogic 원격 콘솔은 물리적 또는 가상 머신, 컨테이너, Kubernetes 또는 Oracle Cloud와 같이 어디서나 실행되는 WebLogic 서버 도메인을 관리하는 데 사용할 수 있는 경량 오픈 소스 콘솔입니다. WebLogic 원격 콘솔은 WebLogic 서버 도메인과 함께 배치할 필요가 없습니다.

어디서나 WebLogic Remote Console을 설치 및 실행하고 WebLogic REST API를 사용하여 도메인에 연결할 수 있습니다. 데스크톱 응용 프로그램을 실행하고 도메인의 관리 서버에 연결하기만 하면 됩니다. 또는 콘솔 서버를 시작하고 브라우저에서 콘솔을 실행한 다음 관리 서버에 연결할 수 있습니다.

WebLogic 원격 콘솔은 WebLogic 서버 12.2.1.3, 12.2.1.4 및 14.1.1.0에서 완전히 지원됩니다.

**WebLogic Remote Console의 주요 기능**

*   WebLogic 서버 인스턴스 및 클러스터 구성
*   WDT 메타데이터 모델 생성 또는 수정
*   데이터베이스 접속(JDBC) 및 메시지(JMS)와 같은 WebLogic 서버 서비스 구성
*   애플리케이션 배치 및 배치 해제
*   서버 및 애플리케이션 시작 및 정지
*   서버 및 응용 프로그램 성능 모니터

이 실습에서는 _opdemo_ 애플리케이션에 액세스하고 오프라인 온프레미스 도메인의 성공적인 마이그레이션을 확인합니다. 또한 관리 대상 서버 Pod 간의 로드 밸런싱도 확인합니다. 나중에 WebLogic Remote Console을 사용하여 kubernetes 환경에서 test-domain 리소스의 성공적인 배치를 확인합니다.

예상 시간: 10분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [애플리케이션 배치 테스트](videohub:1_1khcsrbq)

### 목표

이 실습에서는 다음을 수행합니다.

*   브라우저를 통해 애플리케이션에 액세스합니다.
*   WebLogic Remote Console을 사용하여 WebLogic 도메인을 탐색합니다.

## 작업 1: 브라우저를 통해 응용 프로그램 액세스

이 작업에서는 _opdemo_ 응용 프로그램에 액세스합니다. 두 관리 서버 Pod 간의 로드 밸런싱을 확인하려면 새로고침 아이콘을 눌러 애플리케이션에 여러 요청을 수행합니다.

1.  아래 URL을 복사하고 _XX.XX.XX.XX_를 마지막 실습에서 기록한 IP로 바꿉니다. 아래 출력이 표시됩니다.
    
        <copy>http://XX.XX.XX.XX/opdemo/?dsname=testDatasource</copy>
        
    
    ![신청 열기](images/open-application.png)
    
2.  Refresh(새로고침) 아이콘을 누르면 두 관리 서버 Pod 간의 로드 균형 조정을 볼 수 있습니다. ![로드 밸런싱 표시](images/show-load-balancing.png)
    

## 작업 2: WebLogic Remote Console을 사용하여 Kubernetes 클러스터의 WebLogic 도메인 탐색

이 작업에서는 WebLogic Remote Console을 살펴봅니다. 원격 콘솔에서 _Admin Server_에 대한 연결을 만들고 WebLogic 도메인에서 리소스를 확인합니다. 이는 온프레미스 도메인을 Oracle Kubernetes 클러스터로 성공적으로 마이그레이션했는지 확인합니다.

1.  WebLogic Remote Console을 열려면 _Activities_를 누르고, 검색 상자에 _WebLogic_를 입력하고, _WebLogic Remote Console_ 아이콘을 누릅니다.
    
2.  _키오스크_ 아래의 `Three dots`을 누르고 _관리 서버 접속 제공자 추가_를 선택하고 _선택_을 누릅니다. ![관리 서버 접속](images/adminserver-connection.png)
    
3.  다음 데이터를 입력하고 _확인_을 누릅니다.  
    접속 제공자 이름: AdminServer  
    사용자 이름: weblogic  
    비밀번호: welcome1  
    URL: `Copy_IP_From_TextFile`  
    ![접속 세부정보](images/connection-details.png)
    
4.  _트리 편집_ 아이콘을 누른 다음 _서비스_ -> _데이터 소스_를 선택합니다. 온프레미스 도메인에서 확인한 것과 동일한 데이터 소스를 확인할 수 있습니다. ![데이터 소스 확인](images/verify-datasources.png)
    
5.  도메인에서 실행 중인 서버를 표시합니다. 그림과 같이 **Monitoring Tree** 아이콘을 누른 다음 **Environment(환경)** -> **Servers(서버)**를 선택합니다. **Admin Server** 및 2개의 Managed Server 포드가 실행 중인 것을 확인할 수 있습니다. ![실행 중인 서버](images/running-server-status.png)
    
6.  **admin-server**를 누르면 WebLogic 버전 **12.2.1.3.0**이 표시됩니다. ![서버 상태](images/wls-version.png)
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
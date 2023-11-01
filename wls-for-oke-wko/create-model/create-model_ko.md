# WKT UI 프로젝트 수정 및 모델 파일 생성

## 소개

이 실습에서는 온프레미스 WebLogic 도메인을 살펴봅니다. 관리 콘솔을 탐색하여 _test-domain_에서 배치된 애플리케이션, 데이터 소스 및 서버를 확인합니다. 또한 _Project Settings_ 섹션에 대해 미리 채워진 값이 이미 있는 미리 생성된 _`base_project.wktproj`_를 엽니다. 그런 다음 오프라인 온프레미스 도메인을 검토하여 모델 파일을 생성합니다. 마지막으로 모델을 검증하고 Oracle Kubernetes 클러스터(OKE)에 배포할 모델을 준비합니다.

예상 시간: 15분

### 목표

이 실습에서는 다음을 수행합니다.

*   온프레미스 WebLogic 도메인 _test-domain_을 살펴봅니다.
*   기본 WKT 프로젝트를 엽니다.
*   오프라인 온프레미스 도메인의 검토
*   모델을 검증하고 준비합니다.

### 필요 조건

이 실습을 실행하려면 다음이 있어야 합니다.

*   랩 2에서 만든 noVNC 원격 데스크탑에 액세스합니다.

## 작업 1: 온-프레미스 도메인 탐색

이 작업에서는 WebLogic 관리 콘솔을 사용하여 온프레미스 _test-domain_의 리소스를 탐색합니다.

1.  왼쪽에서 _화살표 아이콘_을 누릅니다. ![클립보드](images/clipboard.png)

> **중요** - _클립보드_에서 호스트 시스템과 원격 데스크탑 간 복사 및 붙여넣기를 확인할 수 있으며 _클립보드_를 사용합니다. 예를 들어, 호스트 시스템에서 복사하여 원격 데스크탑에 붙여 넣으려면 먼저 클립보드에 붙여 넣은 다음 원격 데스크탑에 붙여 넣을 수 있습니다. 다시 _화살표 아이콘_을 눌러 _설정_ 옵션을 숨깁니다.

2.  _Oracle WebLogic Server_ 탭을 누르고 _weblogic/Welcome1%_를 `Username/Password`로 입력한 다음 _로그인_을 누릅니다. WebLogic Server 버전 _12.2.1.3.0_이 표시됩니다.  
    ![로그인 관리 콘솔](images/login-admin-console.png)
    
3.  사용 가능한 서버를 보려면 _Environment(환경)_를 확장하고 _Servers(서버)_를 누릅니다. 5개의 관리 서버가 있는 동적 클러스터가 하나 있음을 알 수 있습니다. ![서버 보기](images/view-servers.png)
    
4.  데이터 소스를 보려면 _서비스_를 확장하고 _데이터 소스_를 누릅니다. ![데이터 출처 보기](images/view-datasources.png)
    
5.  배치된 애플리케이션을 보려면 _배치_를 누릅니다. _opdemo_가 배치된 애플리케이션임을 확인할 수 있습니다. ![배치 보기](images/view-deployments.png)
    

## 태스크 2: 기본 WKT UI 프로젝트 열기

간편한 실습을 위해 도커, Java, Oracle 홈, 기본 이미지 태그의 위치를 미리 설정하는 _`base_project.wktproj`_을 만들었습니다. 이 작업에서는 _`base_project.wktproj`_ 프로젝트를 엽니다.

1.  _Activities(작업)_를 누른 다음 검색 상자에 **WebLogic**를 입력합니다. _WebLogic Kubernetes 툴킷 UI_ 아이콘을 누릅니다. ![WKTUI 열기](images/open-wktui.png)
    
2.  _base\_project.wktproj_ 프로젝트를 열려면 _File(파일)_ -> _Open Project(프로젝트 열기)_를 누릅니다. ![프로젝트 열기](images/open-project.png)
    
3.  왼쪽에서 _다운로드_를 누른 다음 _base\_project.wktproj_을 선택하고 _프로젝트 열기_를 누릅니다. ![프로젝트 위치](images/project-location.png)
    
    > **For your information only:**  
    > As _Credential Story Policy_, we select **Store in Native OS Credentials Store**. It means the credentials (like for WebLogic Server and datasources) are only stored on the local machine.  
    > For _Where would you like the target Oracle Fusion Middleware domain to live?_, we select **Created in the container from the model in the image**. In this case, the set of model-related files are added to the image. So when the WebLogic Kubernetes Operator domain object is deployed, its inspector process runs and creates the WebLogic Server domain inside a running container on-the-fly.  
    > ![프로젝트 설정](images/project-settings.png) As _Kubernetes Environment Target Type_, we select **WebLogic Kubernetes Operator**. This means, you want this domain to be deployed in Kubernetes managed by the WebLogic Kubernetes Operator. This settings also determine what sections and their associated actions within the application, to display.  
    > we also specify the location for _JAVA HOME_ and _ORACLE\_HOME_. WebLogic Kubernetes Toolkit UI uses this directory when invoking the WebLogic Deployer Tooling and WebLogic Image Tool.  
    > To build new images, inspect images and interact with image repositories, the WKT UI application uses an image build tool, which defaults to docker.  
    > ![Kubernetes 클러스터 유형](images/kubernetes-cluster-type.png)
    
4.  _welcome1_에 **Password**를 입력한 다음 _Unlock_을 누릅니다. ![잠금 해제](images/unlock.png)
    

## 작업 3: 오프라인 온-프레미스 도메인 검사

이 작업에서는 도메인 구성으로 구성된 모델 파일을 생성하는 온-프레미스 도메인 검사를 수행합니다.

1.  WebLogic Kubernetes 툴킷 UI에서 _모델_을 누릅니다. ![모델](images/click-model.png)
    
2.  _파일_ -> _모델 추가_ -> _모델 검색(오프라인)_을 누릅니다. ![모델 검색](images/discover-model.png)
    
3.  폴더 열기 _아이콘_을 눌러 _도메인 홈_을 엽니다. ![도메인 홈 열기](images/open-domain-home.png)
    
4.  홈 폴더에서 _`/home/opc/Oracle/Middleware/Oracle_Home/user_projects/domains/`_ 디렉토리로 이동하고 _test-domain_ 폴더를 선택한 다음 _Select_를 누릅니다. _확인_을 누릅니다. ![탐색 위치](images/navigate-location.png) ![위치 지정](images/specify-location.png)
    
    > 콘솔을 살펴보면 WebLogic Deployer Tool을 호출하여 도메인 구성을 오프라인 모드로 확인하는 것을 확인할 수 있습니다.
    
5.  아래와 같이 창을 볼 수 있습니다. 결국 모델을 사용할 수 있습니다. ![모델 보기](images/view-model.png)
    
    > 이 WDT 검사 결과는 모델(도메인 구성의 메타데이터 표현), 위치 표시자로, 데이터 소스의 비밀번호와 같은 값과 애플리케이션 아카이브의 애플리케이션을 지정할 수 있습니다.
    

## 태스크 4: 모델 검증 및 준비

이 작업에서는 모델을 검증하고 Oracle Kubernetes 클러스터(OKE)에 배포할 모델을 준비합니다.

1.  _코드 뷰_를 누르고 모델을 검증하려면 _모델 검증_을 누릅니다. ![모델 검증](images/validate-model.png)
    
    > **참고용:**  
    > 모델을 검증하면 WDT [모델 검증 툴](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/validate/)이 호출됩니다. 이 툴은 모델 및 관련 아티팩트의 형식이 올바른지 검증하고 특정 모델 위치의 적합한 속성 및 하위 폴더에 대한 도움말을 제공합니다.
    
2.  _모델 완료 검증_ 창이 표시되면 _확인_을 누릅니다. ![검증 완료](images/validate-complete.png)
    
3.  모델을 준비하려면 Kubernetes 클러스터에 배치하려면 _모델 준비_를 누릅니다. ![모델 준비](images/prepare-model.png)
    
    > **참고용:**  
    > Prepare 모델은 WDT [Prepare Model Tool](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/prepare/)을 호출하여 WebLogic Kubernetes Operator 또는 Verrazzano가 설치된 Kubernetes 클러스터에서 작동하도록 모델을 수정합니다.  
    > 모델 준비는 다음을 수행합니다.
    
    *   대상 환경과 호환되지 않는 모델 섹션 및 필드를 제거합니다.
    *   끝점 값을 변수를 참조하는 모델 토큰으로 바꿉니다.
    *   인증서 값을 Kubernetes 암호 또는 변수의 필드를 참조하는 모델 토큰으로 바꿉니다.
    *   애플리케이션의 변수, 변수 대체 및 암호 편집기에 표시되는 필드에 대한 기본값을 제공합니다.
    *   도메인 배치에 사용되는 리소스 파일을 생성하는 데 사용하는 응용 프로그램에 토폴로지 정보를 추출합니다.
4.  _모델 완료 준비_ 창이 나타나면 _확인_을 누릅니다. ![준비 완료](images/prepare-complete.png)
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 10월
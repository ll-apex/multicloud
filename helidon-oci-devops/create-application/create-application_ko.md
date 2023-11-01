# Helidon MP 애플리케이션 생성

## 소개

이 실습에서는 **Helidon CLI**를 사용하여 **Helidon MP** 애플리케이션을 생성하는 단계를 안내합니다. 또한 **OCI Logging and Monitoring** 서비스와의 통합을 표시하도록 Helidon 애플리케이션을 수정합니다.

예상 시간: 15분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [Helidon MP 애플리케이션 생성](videohub:1_i4j33ed4)

### 목표

이 실습에서는 다음을 수행합니다.

*   Helidon CLI 다운로드
*   Helidon CLI를 사용하여 Helidon MP 애플리케이션 생성
*   OCI 로깅 및 측정항목 서비스와의 통합 수행
*   응용 프로그램 코드를 OCI Code Repository로 푸시
*   DevOps 파이프라인을 트리거합니다.

### 필요 조건

*   Oracle Free Tier(체험판), 유료 또는 LiveLabs 클라우드 계정
*   git 명령에 대한 기본적인 이해

## 작업 1: Helidon CLI 다운로드

1.  **OCI 코드 편집기**에서 새 터미널을 열고 다음 명령을 붙여넣어 홈 폴더로 이동합니다.
    
        <copy>cd ~</copy>
        
2.  다음 명령을 복사하여 붙여넣어 **Helidon CLI**를 다운로드하고 실행 가능하도록 **permissions**를 변경합니다.
    
        <copy>curl -L -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon</copy>
        

## 작업 2: Helidon CLI를 사용하여 Helidon Microprofile 애플리케이션 생성

1.  CLI를 실행하여 **generate a Helidon Microprofile application** 프로젝트를 생성합니다.
    
        <copy>./helidon init</copy>
        
2.  _Helidon 버전_을 묻는 메시지가 표시되면 _4_를 입력하여 모든 버전을 확인한 다음 _35_를 입력하여 아래와 같이 _4.0.0-ALPHA6_ 버전을 선택합니다.
    
        Helidon versions
        (1) 3.2.2 
        (2) 2.6.2 
        (3) 4.0.0-M1 
        (4) Show all versions 
        Enter selection (default: 1): 4
        -----------------------------
        -----------------------------
        (33) 2.0.1 
        (34) 2.0.0 
        (35) 4.0.0-ALPHA6 
        (36) 4.0.0-M1 
        Enter selection (default: 1): 35 
        
3.  _Select a Flavor_ 프롬프트가 표시되면 아래 값을 복사하여 터미널에 붙여 넣습니다.
    
            | Helidon Flavor
        
            Select a Flavor
            (1) se   | Helidon SE
            (2) mp   | Helidon MP
            (3) nima | Helidon Níma
            Enter selection (default: 1): <copy>2</copy>
        
4.  _응용 프로그램 유형 선택_ 프롬프트가 표시되면 아래 값을 복사하여 터미널에 붙여 넣습니다.
    
            Select an Application Type
        (1) quickstart | Quickstart
        (2) database   | Database
        (3) custom     | Custom
        (4) oci        | OCI
        Enter selection (default:1):<copy>4</copy>
        
5.  _프로젝트 groupId_, _프로젝트 artifactId_ 및 _프로젝트 버전_에 대한 프롬프트가 표시되면 **기본값을 수락**하기만 하면 됩니다.
    
6.  _Java 패키지 이름_을 묻는 메시지가 표시되면 아래 값을 복사하여 터미널에 붙여 넣습니다.
    
        Java package name (default: me.username.mp.oci): <copy>ocw.hol.mp.oci</copy>
        
7.  _개발 루프 시작?(기본값: n):_이 표시되면 _Enter_ 키를 눌러 기본값을 선택합니다.
    
    > 완료되면 **oci-mp** 프로젝트가 생성됩니다.
    

## 태스크 3: 로깅 및 척도 탐색기에 대한 Helidon 애플리케이션 수정

1.  _Code Editor_에서 **oci-mp** 프로젝트를 열려면 _File_ -> _Open_을 누릅니다. ![프로젝트 열기](images/open-project.png)
    
2.  _위쪽 화살표_를 눌러 상위 폴더로 이동한 다음 _oci-mp_ 폴더를 선택하고 _열기_를 누릅니다. ![오픈 oci mp](images/open-ocimp.png)
    
    > 그러면 탐색기에서 _oci-mp_ 애플리케이션이 열립니다.
    
3.  새 터미널을 열고 다음 명령을 복사하여 붙여넣어 **`devops_helidon_to_instance_ocw_hol`** 폴더에서 _빌드 및 배치 파이프라인 사양을 복사_합니다.
    
        <copy>cd ~/oci-mp/
        cp ~/devops_helidon_to_instance_ocw_hol/pipeline_specs/* .</copy>
        
4.  저장소에 속할 필요가 없는 파일 및 디렉토리가 git에서 무시되도록 **.gitignore**를 추가합니다.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/.gitignore .</copy>
        
5.  다음 명령을 복사하여 기본 폴더 _`devops_helidon_to_instance_ocw_hol`_에서 유틸리티 스크립트를 실행하여 **config** 매개변수를 업데이트합니다. **Helidon MP 프로젝트의 루트 디렉토리 입력**을 묻는 경우 Enter 키를 눌러 **default** 값을 선택합니다.
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/update_config_values.sh</copy>
        
    
    다음과 유사한 출력이 표시됩니다. ![업데이트 구성](images/update-config.png)
    
    > **참고:-**
    
    *   이 스크립트를 호출하면 다음이 수행됩니다.
    *   _~/oci-mp/server/src/main/resources/application.yaml_ 구성 파일의 업데이트로 Helidon 생성 측정항목을 OCI 모니터링 서비스로 전송하는 Helidon 기능을 설정합니다.
        *   **compartmentId** - 이 데모에 사용되는 구획 OCID
        *   **namespace** - 모든 문자열일 수 있지만 이 데모의 경우 helidon\_metrics로 설정됩니다.
    *   OCI 로깅 및 측정항목 서비스와의 통합을 수행하기 위해 Helidon 앱 코드에 사용되는 구성 매개변수를 설정하기 위해 _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ 구성 파일의 업데이트입니다.
        *   **oci.monitoring.compartmentId** - 이 데모에 사용되는 구획 OCID
        *   **oci.monitoring.namespace** - 모든 문자열일 수 있지만 이 데모의 경우 helidon\_application로 설정됩니다.
        *   **oci.logging.id** - Terraform 스크립트로 프로비전된 애플리케이션 로그 ID입니다.
    *   _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ 구성 파일에서 _oci.bucket.name_ 등록 정보를 업데이트하여 이후 연습에서 Helidon 애플리케이션에서 오브젝트 스토리지 지원을 시연하는 데 사용할 terraform 스크립트로 프로비전된 오브젝트 스토리지 버킷 이름을 포함하도록 합니다.
6.  Code Editor에서 _~/oci-mp/server/src/main/resources/application.yaml_ 파일을 열어 **compartmentId** 및 **namespace** 값이 업데이트되었는지 확인합니다. ![애플리케이션 YAML](images/application-yaml.png)
    
7.  Code Editor에서 _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ 파일을 열어 **oci.monitoring.compartmentId**, **oci.monitoring.namespace**, **oci.logging.id** 및 **oci.bucket.name** 값이 업데이트되었는지 확인합니다. _oci.bucket.name_는 나중에 Lab 5에서 사용됩니다.
    

## 작업 4: 코드를 OCI 코드 저장소에 푸시하는 인증 토큰 생성

이 단계에서는 Helidon 애플리케이션 코드를 _OCI Code 저장소_로 푸시하는 데 사용할 _인증 토큰_을 생성합니다.

1.  오른쪽 맨 위에 있는 _사용자 아이콘_을 선택한 다음 _내 프로파일_을 선택합니다. ![사용자 아이콘](images/user-icon.png)
    
2.  아래로 스크롤하여 _Auth Tokens_을 선택합니다. ![인증 토큰](images/auth-tokens.png)
    
3.  _토큰 생성_을 누릅니다. ![토큰 생성](images/generate-token.png)
    
4.  _oci-mp_를 복사하여 \[설명\] 상자에 붙여 넣고 _토큰 생성_을 누릅니다. ![토큰 설명](images/token-description.png)
    
5.  생성된 토큰에서 **복사**를 선택하고 선택한 편집기를 사용하여 텍스트 파일에 붙여넣거나 저장합니다. 이 토큰은 나중에 검색할 수 없으므로 지금 이 복사본을 보관하는 것이 중요합니다. 완료되면 **닫기**를 누릅니다. ![토큰 복사](images/copy-token.png)
    

## 작업 5: DevOps 프로젝트의 OCI 코드 저장소에 Helidon 애플리케이션 프로젝트 동기화

1.  새 터미널을 열고 다음 명령을 복사하여 붙여넣어 _oci-mp_ 디렉토리로 이동합니다.
    
        <copy>cd ~/oci-mp</copy>
        
2.  _oci-mp_ 프로젝트 디렉토리를 **git 저장소**로 초기화합니다.
    
        <copy>git init</copy>
        
3.  해당하는 원격 분기와 일치하도록 분기를 **main**으로 설정합니다.
    
        <copy>git checkout -b main</copy>
        
4.  **원격 저장소**를 설정합니다. 마지막 Terraform 출력에서 표시된 OCI 코드 저장소의 https URL을 사용하거나 `devops_helidon_to_instance_ocw_hol`의 get.sh 툴을 사용하여 해당 값을 검색합니다.
    
        <copy>git remote add origin $(~/devops_helidon_to_instance_ocw_hol/main/get.sh code_repo_https_url)
        git remote -v</copy>
        
    
    > **git remote -v**는 원본이 설정되었는지 확인하는 것입니다.
    
5.  OCI 저장소의 사용자 이름과 비밀번호가 필요한 git 명령에 한 번만 입력되도록 **자격 증명 도우미** 저장소를 사용하도록 git를 구성합니다. 또한 git 커밋에 필요한 **user.name** 및 **user.email**을 설정합니다.
    
        <copy>git config credential.helper store
        git config --global user.email "my.name@example.com"
        git config --global user.name "FIRST_NAME LAST_NAME"</copy>
        
6.  **git pull**을 사용해서 oci 저장소의 git 로그를 로컬 저장소와 동기화합니다.
    
        <copy>git pull origin main</copy>
        
7.  그러면 사용자 이름과 암호를 묻는 메시지가 표시됩니다. 사용자 이름에 **tenancy name**/**username**을, 암호에 대해 **task 4**에서 생성된 OCI 사용자 **auth token**을 사용합니다. ![git 사용자 이름](images/git-username.png) ![git 비밀번호](images/git-password.png)
    
8.  다음과 유사한 출력이 나타납니다. 그렇지 않은 경우 모든 git 명령을 올바르게 실행했는지 확인하십시오. ![git pull sync  ](images/git-pull-sync.png)
    

## 작업 6: Helidon 애플리케이션 코드를 푸시하고 DevOps 파이프라인을 트리거합니다.

1.  git 커밋에 대한 모든 파일을 **스테이지**합니다.
    
        <copy>git add .
        git status</copy>
        
    
    > git 상태는 저장소의 모든 파일을 출력합니다.
    
2.  첫번째 **commit**을 수행합니다.
    
        <copy>git commit -m "Helidon oci-mp first commit"</copy>
        
3.  원격 저장소에 변경사항을 **푸시**합니다.
    
        <copy>git push -u origin main</copy>
        
    
    > 그러면 DevOps가 트리거되어 빌드 파이프라인이 시작됩니다.
    
4.  새 탭에서 [Cloud Console](https://cloud.oracle.com/)을 열고 _햄버거 메뉴_ -> _개발자 서비스_ -> _프로젝트_ 아래의 **DevOps**를 누릅니다. ![devops 프로젝트](images/devops-project.png)
    
5.  **Lab 1**에서 만든 구획을 선택한 다음 _devops-project-helidon-ocw-hol-string_을 눌러 **DevOps Project**를 엽니다. ![구획 선택](images/select-compartment.png)
    
    > 새 구획이 표시되지 않으면 브라우저를 새로고침합니다.
    
6.  _최신 빌드 내역_에서 _실행_ 및 상태가 _수락됨/진행 중_으로 표시됩니다. 아래와 같이 최신 실행을 누릅니다. ![빌드 내역](images/build-history.png)
    
7.  빌드 파이프라인이 세 단계를 모두 완료하면 아래와 같이 출력이 표시됩니다. 단계 바로 앞에 있는 화살표를 클릭하여 수행 중인 작업을 볼 수 있습니다. 이 작업을 수행하면 _oci-mp_ 폴더의 _`build_spec.yaml`_ 파일에서 defind가 수행됩니다. ![먼저 실행된 빌드](images/build-run-first.png)
    
8.  빌드 실행 진행 중 세번째 단계에서 **세 개의 점**을 누른 후 아래와 같이 **배치 보기**를 누릅니다. 그러면 **배포 파이프라인**이 열립니다. ![배치 보기](images/view-deployment.png)
    
9.  여기에서 _배포 진행률_을 확인할 수 있습니다. 배치 파이프라인이 완료되면 아래와 같이 출력이 표시됩니다. 단계 바로 앞에 있는 화살표를 클릭하여 수행 중인 작업을 볼 수 있습니다. 이 작업을 수행하면 _oci-mp_ 폴더의 _`deployment_spec.yaml`_ 파일에서 defind가 수행됩니다. ![배치 실행](images/deployment-run.png)
    
    > 그러면 OCI의 **컴퓨트 인스턴스**에 Helidon 애플리케이션이 성공적으로 배치됩니다.
    
10.  배치 파이프라인의 로그를 보려면 배치 단계 근처의 **세 개의 점**을 누르고 아래와 같이 **세부정보 보기**를 누릅니다. ![로그 보기](images/view-logs.png)
    
11.  로그를 아래로 스크롤하고 JDK flavor가 **Open JDK**인지 확인합니다. 또한 Helidon 애플리케이션이 아래와 같이 Java의 새 **가상 스레드** 기능을 활용하는 로그도 참조하십시오. ![jdk 열기](images/open-jdk.png)
    
    > **Lab 4**의 일부로 **Open JDK**를 **Oracle JDK**로 바꿉니다.
    

이제 **다음 실습으로 진행할 수 있습니다.**

## 자세히 알아보기

*   [Helidon CLI](https://helidon.io/docs/v3/#/about/cli)
*   [Helidon MP 빠른 시작 가이드](https://helidon.io/docs/v3/#/mp/guides/quickstart)
*   [Helidon MP 구성 소스](https://helidon.io/docs/v3/#/mp/config/advanced-configuration)
*   [Helidon MP 구성 소스](https://helidon.io/docs/v3/#/mp/guides/config)

## 확인

*   **작성자** - Keith Lustria
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
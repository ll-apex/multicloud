# Helidon MP 프로젝트 생성 및 코드 편집기에서 프로젝트 실행

## 소개

이 실습에서는 Helidon MP 애플리케이션 생성 단계를 안내합니다.

예상 시간: 15분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [코드 편집기에서 Helidon MP 프로젝트 생성 및 프로젝트 실행](videohub:1_22nv8v4q)

### 제품/기술 정보

Helidon은 쉽게 사용할 수 있도록 설계되었으며, 도구와 예제를 통해 빠르게 시작할 수 있습니다. Helidon은 빠른 Netty 코어에서 실행되는 라이브러리의 모음일 뿐이므로 추가 오버헤드나 피트가 없습니다. Helidon은 MicroProfile를 지원하고 JAX-RS, CDI 및 JSON-P/B와 같은 친숙한 API를 제공합니다. MicroProfile 구현은 빠른 Helidon Reactive WebServer에서 실행됩니다. 반응적 WebServer는 최신의 기능적 프로그래밍 모델을 제공하며 Netty를 기반으로 실행됩니다. 가볍고 유연하며 반응성이 뛰어난 Helidon WebServer은 마이크로서비스를 위한 사용하기 쉽고 빠른 기반을 제공합니다.

상태 검사, 측정 지표, 추적 및 내결함성을 지원하는 Helidon은 Prometheus, Jaeger/Zipkin 및 Kubernetes와 통합된 클라우드 지원 애플리케이션을 작성하는 데 필요한 기능을 제공합니다.

### Helidon Project Starter 정보

Project Starter는 Helidon 프로젝트를 생성하기 위한 새로운 웹 UI입니다. 사용자가 프로젝트에 추가할 Helidon 기능을 선택할 수 있는 다양한 옵션을 제공하는 것은 매우 사용자 정의가 가능합니다. 최종 사용자는 특정 요구에 맞게 프로젝트를 생성할 수 있습니다. 자세한 내용을 보려면 [Helidon Starter](https://helidon.io/starter)를 누릅니다.

### 코드 편집기 정보

코드 편집기를 사용하면 OCI 콘솔에서 직접 다양한 OCI 서비스에 대한 코드를 편집하고 배치할 수 있습니다. 이제 콘솔과 로컬 개발 환경 간에 전환할 필요 없이 서비스 워크플로우와 스크립트를 업데이트할 수 있습니다. 이를 통해 클라우드 솔루션을 신속하게 프로토타입화하고, 새로운 서비스를 탐색하고, 빠른 코딩 작업을 수행할 수 있습니다.

Code Editor와 Cloud Shell의 직접 통합을 통해 Cloud Shell에 사전 설치된 GraalVM Enterprise Native Image 및 JDK 17(Java Development Kit)에 액세스할 수 있습니다.

### OCI Cloud Shell 정보

[OCI Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm)은 Oracle Cloud 콘솔에서 액세스할 수 있는 브라우저 기반 터미널입니다. 사전 인증된 OCI CLI(명령행 인터페이스) 및 사전 설치된 개발자 도구를 통해 Linux 셸에 대한 액세스를 제공하며 5GB의 스토리지가 제공됩니다.

버전 22.2.0부터 GraalVM Enterprise JDK 17 및 Native Image는 Cloud Shell에 사전 설치됩니다.

> GraalVM 추가 비용 없이 Oracle Cloud Infrastructure에서 엔터프라이즈를 사용할 수 있습니다.

### 목표

*   Helidon Greeting 애플리케이션이라는 MicroProfile 지원 마이크로서비스 생성
*   코드 편집기에서 Helidon 애플리케이션 열기
*   클라우드 셸에서 기본 JDK 변경
*   필요한 Maven 구성
*   Helidon 인사말 앱 실행 및 실행

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.

## 태스크 1: 프로젝트 시작 프로그램을 사용하여 Helidon 프로젝트 생성

1.  아래 URL을 복사하여 브라우저에 붙여 넣어 Helidon Project 페이지를 엽니다.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  프로젝트 생성에서 _Helidon MP_를 Helidon Flavor로 선택한 후 _다음_을 누릅니다.
    
3.  애플리케이션 유형에 대해 _빠른 시작_을 선택한 후 _다음_을 누릅니다.
    
4.  Media Support의 경우 _JSON-B_를 선택한 다음 _Next_를 누릅니다.
    
5.  프로젝트 사용자정의의 경우 기본값을 선택하고 _다운로드_를 누릅니다. 창에 팝업이 표시되면 이 _myproject.zip_을 선택한 위치에 저장합니다. 이 워크샵의 나머지 부분에서는 myproject 이름이 사용됩니다. 다른 이름을 선택하면 각각 변경하십시오.
    

## 작업 2: 로컬에서 helidon 프로젝트 작성 및 실행

1.  클라우드 콘솔에서 표시된 대로 _개발자 툴_ 아이콘을 누른 다음 _코드 편집기_를 누릅니다. ![코드 편집기](images/code-editor.png)
    
2.  Code Editor에서 _Terminal_ -> _New Terminal_을 누릅니다. ![터미널 열기](images/open-terminal.png)
    
3.  아래 명령을 복사하여 터미널에 붙여넣어 _myproject_ 폴더를 만듭니다. 여기서 기본 myproject.zip 파일을 다운로드합니다.
    
        <copy>mkdir -p ~/helidon-project/myproject</copy>
        
4.  Code Editor에서 _File_ -> _Open_을 누릅니다. ![열린 폴더](images/open-folder.png)
    
5.  _helidon-project_ 폴더를 선택하고 _Open_을 누릅니다. ![Helidon 열기](images/open-helidon.png)
    
6.  코드 편집기의 EXPLORER 창에서 _HELIDON-PROJECT_를 볼 수 있습니다. 여기서 _myproject_ 폴더를 보고 누릅니다. 이제 _파일_ -> _파일 업로드.._를 누른 다음 프로젝트를 다운로드한 위치를 지정하고 zip 파일을 선택하고 _열기_를 누릅니다. ![프로젝트](images/myproject.png) ![파일 업로드](images/upload-files.png)
    
7.  다음 명령을 복사하여 붙여넣어 _myproject.zip_ 파일의 압축을 풉니다.
    
        <copy>cd ~/helidon-project/myproject
        unzip myproject.zip
        </copy>
        
8.  프로젝트 구조를 보려면 _myproject_ 폴더를 확장합니다. ![프로젝트 확장](images/expand-project.png)
    
9.  이 프로젝트를 실행하기 위해 Maven 3.8+ 및 JDK 17+를 사용합니다. Oracle 클라우드에서는 다양한 JDK가 제공됩니다. 여기서는 GraalVM JDK를 선택합니다. 다음 명령을 복사하여 터미널에 붙여넣어 기본 JDK를 확인합니다.
    
        <copy>csruntimectl java list</copy>
        
    
    ![JDK 나열](images/list-jdk.png)
    
    > \* _asterisk_를 처음 사용하는 JDK는 기본 JDK입니다. 다른 JDK와 graalvmeejdk가 있는 경우 아래 명령을 실행하여 기본 JDK 버전을 변경합니다. 표시된 버전의 graalvmeejdk는 명령에 표시된 것과 다를 수 있으므로 사용하십시오.
    
        <copy>csruntimectl java set graalvmeejdk-17</copy>
        
10.  필요한 maven을 구성하려면 다음 명령을 복사하여 터미널에 붙여 넣습니다.
    
        <copy>cd ~/helidon-project/myproject/
        wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
        tar -xzvf apache-maven-3.8.8-bin.tar.gz
        PATH=~/helidon-project/myproject/apache-maven-3.8.8/bin:$PATH</copy>
        
    
    ![Maven 구성](images/configure-maven.png)
    
11.  아래와 같이 올바른 버전의 JDK 및 Maven이 있는지 확인하려면 터미널에서 다음 명령을 실행하십시오.
    
        <copy>mvn -v</copy>
        
    
    ![필요 조건 확인](images/verify-prerequisite.png)
    
12.  _myproject_ 폴더에서 다음 명령을 실행하여 프로젝트를 빌드합니다.
    
        <copy>cd ~/helidon-project/myproject/myproject
        mvn clean package</copy>
        
    
    ![프로젝트 작성](images/build-project.png)
    
    > 이 명령을 실행하면 _BUILD SUCCESS_가 표시됩니다.
    
13.  이 응용 프로그램을 실행하려면 다음 명령을 복사하여 터미널에 붙여 넣습니다. 아래 스크린샷에 표시된 것과 유사한 출력이 표시됩니다.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    ![프로젝트 실행](images/run-project.png)
    

> 시작 시간은 4822밀리초입니다. 이 시간을 나중에 원시 이미지 실행 파일과 비교합니다.

14.  Code Editor에서 새 터미널/콘솔을 열고 다음 명령을 실행하여 응용 프로그램을 확인합니다.
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        
15.  _"java -jar target/myproject.jar" 명령이 실행되고 있는 터미널에 `Ctrl + C`를 입력하여 **myproject** 응용 프로그램을 정지합니다_. 그것은 매우 중요하다, 다른 경우 당신은 실험실에서 문제를 직면하게됩니다.
    

## 확인

*   **작성자** - Dmitry Aleksandrov
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 4월
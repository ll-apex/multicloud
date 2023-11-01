# Helidon 애플리케이션 개발

## 소개

이 실습에서는 Helidon MP 애플리케이션 생성 단계를 안내합니다.

예상 시간: 15분

### 제품/기술 정보

Helidon은 쉽게 사용할 수 있도록 설계되었으며, 도구와 예제를 통해 빠르게 시작할 수 있습니다. Helidon은 빠른 Netty 코어에서 실행되는 라이브러리의 모음일 뿐이므로 추가 오버헤드나 피트가 없습니다. Helidon은 MicroProfile를 지원하고 JAX-RS, CDI 및 JSON-P/B와 같은 친숙한 API를 제공합니다. MicroProfile 구현은 빠른 Helidon Reactive WebServer에서 실행됩니다. 반응적 WebServer는 최신의 기능적 프로그래밍 모델을 제공하며 Netty를 기반으로 실행됩니다. 가볍고 유연하며 반응성이 뛰어난 Helidon WebServer은 마이크로서비스를 위한 사용하기 쉽고 빠른 기반을 제공합니다.

상태 검사, 측정 지표, 추적 및 내결함성을 지원하는 Helidon은 Prometheus, Jaeger/Zipkin 및 Kubernetes와 통합된 클라우드 지원 애플리케이션을 작성하는 데 필요한 기능을 제공합니다.

### Helidon CLI 정보

Helidon CLI를 사용하면 일련의 조정에서 선택하여 Helidon 프로젝트를 쉽게 만들 수 있습니다. 또한 지속적인 컴파일 및 응용 프로그램 재시작을 수행하는 개발자 루프를 지원하므로 소스 코드 변경에 대해 쉽게 반복할 수 있습니다.

CLI는 설치하기 쉽도록 GraalVM를 사용하여 컴파일된 독립형 실행 파일로 배포됩니다. 현재 Linux, Mac 및 Windows용 다운로드로 제공됩니다. 바이너리를 다운로드하고 PATH에서 액세스할 수 있는 위치에 설치하면 바로 사용할 수 있습니다.

### 목표

*   Helidon CLI 설치
*   Helidon Greeting이라는 MicroProfile 지원 마이크로서비스 생성
*   Helidon 인사말 앱 실행 및 실행
*   건전성 및 측정항목 데이터 보기
*   애플리케이션에 새 기능 추가

### 필요 조건

*   Helidon에 Java 11+ 필요
*   Maven 3.6.x

> **주의: 애플리케이션 빌드의 알려진 문제로 인해 3.8.x 버전을 사용하지 마십시오!**

*   Java 및 `mvn`가 사용자 경로에 있습니다.
*   Windows 사용자는 Visual C++ 재배포 가능 런타임도 필요합니다.  
    자세한 내용은 [Windows의 Helidon](https://helidon.io/docs/v2/#/about/04_windows)을 참조하십시오.

## 작업 1: Helidon CLI 설치

1.  Helidon CLI 설치
    
    macOS의 경우:
    
        <copy>
        curl -O https://helidon.io/cli/latest/darwin/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Linux:
    
        <copy>
        curl -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Windows의 경우:
    
        <copy>
        PowerShell -Command Invoke-WebRequest -Uri "https://helidon.io/cli/latest/windows/helidon.exe" -OutFile "C:\Windows\system32\helidon.exe"
        </copy>
        

## 태스크 2: Helidon 인사말 애플리케이션 생성

1.  콘솔에서 다음을 입력합니다.
    
        <copy>helidon init --version 2.4.0 </copy>
        
    
    > 잠재적인 문제를 방지하려면 이 실습 환경에 대해 테스트된 특정 Helidon 버전을 정의합니다.
    
2.  이 데모에서는 MicroProfile 지원되는 마이크로서비스를 생성하므로 **Helidon MP Flavor**에 대해 **(2)** 옵션을 선택합니다.
    
        Helidon flavor
        (1) SE 
        (2) MP 
        Enter selection (Default: 1): 2
        
3.  대부분의 기능을 보려면 기본 답변을 보려면 **(2) 빠른 시작** 옵션과 **입력** 옵션을 선택합니다. OS 유저 이름을 사용하기 때문에 기본 패키지 이름과 프로젝트 그룹 이름이 다를 수 있습니다. 패키지 이름을 적어 두십시오. 이 이름은 새 Java 클래스를 생성할 때 사용해야 합니다.
    
        Select archetype
        (1) bare | Minimal Helidon MP project suitable to start from scratch 
        (2) quickstart | Sample Helidon MP project that includes multiple REST operations 
        (3) database | Helidon MP application that uses JPA with an in-memory H2 database 
        Enter selection (Default: 1): 2
        Project name (Default: quickstart-mp): 
        Project groupId (Default: me.user-helidon): 
        Project artifactId (Default: quickstart-mp): 
        Project version (Default: 1.0-SNAPSHOT): 
        Java package name (Default: me.user_name.mp.quickstart): 
        Switch directory to /home/user/quickstart-mp to use CLI
        
        Start development loop? (Default: n):
        $
        
    
    > **development loop**에 대해 기본값(**n**)을 적용합니다. 이 연습의 뒷부분에서 개발 루프를 시작합니다.
    
    이제 완전한 기능을 갖춘 마이크로서비스 Maven 프로젝트:
    

        quickstart-mp
        ├── Dockerfile
        ├── Dockerfile.jlink
        ├── Dockerfile.native
        ├── README.md
        ├── app.yaml
        ├── pom.xml
        └── src
            ├── main
            │   ├── java
            │   │   └── me
            │   │       └── buzz
            │   │           └── mp
            │   │               └── quickstart
            │   │                   ├── GreetResource.java
            │   │                   ├── GreetingProvider.java
            │   │                   └── package-info.java
            │   └── resources
            │       ├── META-INF
            │       │   ├── beans.xml
            │       │   ├── microprofile-config.properties
            │       │   └── native-image
            │       │       └── reflect-config.json
            │       └── logging.properties
            └── test
                └── java
                    └── me
                        └── buzz
                            └── mp
                                └── quickstart
                                    └── MainTest.java
    
    

## 태스크 3: Helidon 인사말 애플리케이션 실행

1.  동일한 콘솔/터미널에서 quickstart-mp 디렉토리로 이동하고 다음 명령을 실행합니다.
    
        <copy> cd quickstart-mp
        </copy>
        
2.  JDK11+ 사용
    
        <copy>
        mvn package
        java -jar target/quickstart-mp.jar
        </copy>
        

### 애플리케이션 실행

1.  새 터미널/콘솔을 열고 다음 명령을 실행하여 응용 프로그램을 확인합니다.
    
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
        

### 건전성 및 측정항목 데이터 검토

1.  동일한 터미널/콘솔에서 다음 명령을 실행하여 건전성 및 측정항목을 확인합니다.
    
        <copy>
        curl -s -X GET http://localhost:8080/health
        </copy>
        {"outcome":"UP",...
        . . .
        
    
        # Prometheus Format
        <copy>
        curl -s -X GET http://localhost:8080/metrics
        </copy>
        # TYPE base:gc_g1_young_generation_count gauge
        . . .
        
    
        # JSON Format
        <copy>
        curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
        </copy>
        {"base":...
        . . .
        
2.  "java -jar target/quickstart-mp.jar" 명령이 실행되고 있는 터미널에 `Ctrl + C`을 입력하여 _quickstart-mp_ 응용 프로그램을 정지합니다.
    

## 작업 4: 응용 프로그램 수정

1.  원하는 IDE를 열고 **microprofile-config.properties** 파일로 이동합니다.
    
    ![구성 파일](images/config.jpg)
    
2.  콘솔/터미널에서 프로젝트 폴더로 이동하고 다음을 입력합니다.
    
        <copy>helidon dev</copy>
        
    
    > 그러면 이전 작업에서 언급된 **개발 루프**가 시작됩니다.
    
3.  _app.greeting_ 등록 정보를 "Hello Oracle"로 변경하고 파일을 저장합니다.
    
        <copy>app.greeting=Hello Oracle</copy>
        
    
    ![HelidonDev](images/properties.jpg)
    
    > 파일을 변경할 때마다 **Helidon CLI**에서 변경 사항이 있음을 인식하고, 앱을 다시 컴파일하고, 다시 실행합니다. Helidon은 작기 때문에 모든 일이 빠르게 이루어집니다.
    
4.  콘솔/터미널에 다음을 입력합니다.
    
        <copy>curl -X GET http://localhost:8080/greet</copy>
        
    
    결과는 다음과 같아야 합니다.
    
        {"message":"Hello Oracle World!"}
        
    
    > `CTRL+C`로 개발 루프를 정지해야 합니다.
    
5.  프로젝트 폴더로 돌아가서 **GreetResource.java** 파일을 엽니다.
    
    > 순수 MicroProfile 호환 코드임을 확인할 수 있습니다.
    
    ![ModifyJava](images/greet-resource.jpg)
    
6.  서로 다른 언어의 인사말에 대한 도움말을 제공하는 새 끝점을 생성합니다. 이 새 기능을 만들려면 다음 코드를 사용하여 **GreetHelpResource**라는 새 클래스를 만듭니다.
    
        <copy>
        package me.user_name.me.quickstart;
        import java.util.Arrays;
        import java.util.List;
        import java.util.logging.Logger;
        
        import javax.enterprise.context.ApplicationScoped;
        import javax.ws.rs.GET;
        import javax.ws.rs.Path;
        
        import org.eclipse.microprofile.metrics.annotation.Counted;
        
        @ApplicationScoped
        @Path("/help")
        public class GreetHelpResource {
        
            Logger LOGGER = Logger.getLogger(GreetHelpResource.class.getName());
        
            @GET
            @Path("/allGreetings")
            @Counted(name = "helpCalled", description = "How many time help was called")
            public String getAllGreetings(){
                LOGGER.info("Help requested!");
                return Arrays.toString(List.of("Hello","Привет","Hola","Hallo","Ciao","Nǐ hǎo", "Marhaba","Olá").toArray());
            }
        }
        </copy>
        
    
    > 클래스에는 다른 언어의 인사말 목록을 반환하는 하나의 메소드 _getAllGreetings_만 있습니다. 코드를 복사하는 동안 클래스 상단에 필요한 패키지 이름을 추가해야 합니다.
    
7.  응용 프로그램을 빌드하고 실행합니다.
    
        <copy>
        mvn package -DskipTests
        java -jar target/quickstart-mp.jar
        </copy>
        
8.  다음 명령을 실행하고 결과를 살펴봅니다.
    
        <copy>curl http://localhost:8080/help/allGreetings</copy>
        
    
    예상 결과:
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
9.  지표를 살펴보면 새 카운터가 나타났음을 알 수 있습니다. 이 끝점이 호출될 때마다 값이 증분됩니다.
    
        curl http://localhost:8080/metrics
        
        ...
        # TYPE application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total counter
        # HELP application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total How many time help was called
        application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total 1
        ...
        
10.  이제 콘솔에서 이 호출에 대한 INFO 로그 행이 표시됩니다.
    
        INFO me.buzz.mp.quickstart.GreetHelpResource Thread[helidon-4,5,server]: Help requested!
        
    
    새 끝점이 추가되었습니다.
    
    ![NewEndpoint](images/logs-output.jpg)
    
    > Helidon과 함께 작업하면 도구는 쉽고 빠릅니다!
    
11.  터미널/콘솔을 열어 두고 Verrazzano 설치 랩을 계속 진행합니다.
    

## 확인

*   **작성자** - Dmitry Aleksandrov
*   **기여자** - Maciej Gruszka, Ankit Pandey
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 3월
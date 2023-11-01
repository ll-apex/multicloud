# Helidon 3 애플리케이션을 생성한 다음 Helidon 4로 마이그레이션합니다.

## 소개

이 실습에서는 Netty 기반의 원래 반응적 웹 서버에서 실행되는 Helidon 3 응용 프로그램으로 시작합니다. 그런 다음 가상 스레드를 사용하여 새 Helidon Nima WebServer에서 실행 중인 Helidon 4로 애플리케이션을 마이그레이션합니다.

[Lab4 연습](videohub:1_zr1m00ba)

예상 시간: 20분

### 목표

*   Helidon Starter를 사용하여 Helidon MicroProfile 애플리케이션을 생성, 구축 및 실행합니다.
*   Helidon 3 Microprofile 애플리케이션을 Helidon 4로 마이그레이션

### 필요 조건

*   Oracle Cloud 계정

## 작업 1: Helidon 3 애플리케이션 생성 및 애플리케이션 빌드

1.  아래 URL을 복사하여 브라우저에 붙여 넣어 Helidon Project 페이지를 엽니다.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  프로젝트 생성에서 _Helidon MP_를 Helidon Flavor로 선택한 후 _다음_을 누릅니다.
    
3.  애플리케이션 유형에 대해 _빠른 시작_을 선택한 후 _다음_을 누릅니다.
    
4.  Media Support의 경우 _Jackson_을 선택한 다음 _Next_를 누릅니다.
    
5.  프로젝트 사용자정의의 경우 기본값을 선택하고 _다운로드_를 누릅니다. 창에 팝업이 표시되면 이 _myproject.zip_을 선택한 위치에 저장합니다. 이 워크샵의 나머지 부분에서는 _myproject_ 이름이 사용됩니다. 다른 이름을 선택하면 각각 변경하십시오.
    
6.  Code Editor로 돌아가서 HELIDON-LEVELUP-2023-MAIN으로 이동하고 _docs_를 누릅니다. ![문서 선택](images/select-docs.png)
    
7.  _File_ -> _Upload Files_를 누르고 이전에 이 파일을 저장한 위치에서 _myproject.zip_를 선택한 다음 _Open_을 누릅니다. ![파일 업로드](images/upload-files.png)
    
8.  _docs_ 폴더 아래에 _myproject.zip_ 파일이 표시됩니다. ![Zip 파일](images/zip-file.png)
    
9.  다음 명령을 복사하여 붙여넣어 파일 압축을 해제합니다.
    
        <copy>cd ~/helidon-levelup-2023-main/docs/
        unzip myproject.zip</copy>
        
10.  myproject 폴더에서 다음 명령을 실행하여 프로젝트를 작성합니다. PATH 및 JAVA\_HOME 변수를 설정한 터미널을 사용하십시오.
    
        <copy>cd myproject
        mvn clean package</copy>
        
    
    > 이 명령을 실행하면 _BUILD SUCCESS_가 표시됩니다.
    
11.  이 응용 프로그램을 실행하려면 다음 명령을 복사하여 터미널에 붙여 넣습니다. 아래 스크린샷에 표시된 것과 유사한 출력이 표시됩니다.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    다음과 유사한 출력이 표시됩니다.
    
        $ java -jar target/myproject.jar
        2023.02.28 03:48:36 INFO io.helidon.common.LogConfig Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:48:39 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:48:40 INFO io.helidon.webserver.NettyWebServer Thread[#31,nioEventLoopGroup-2-1,10,main]: Channel '@default' started: [id: 0x5b66bd2a, L:/0.0.0.0:8080]
        2023.02.28 03:48:41 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 4816 milliseconds (since JVM startup).
        2023.02.28 03:48:41 INFO io.helidon.common.HelidonFeatures Thread[#32,features-thread,5,main]: Helidon MP 3.1.2 features: [CDI, Config, Health, JAX-RS, Metrics, Open API, Server]
        
12.  curl 명령을 실행하는 터미널로 돌아가서 다음 명령을 실행하여 응용 프로그램을 확인합니다.
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
13.  "java -jar target/myproject.jar" 명령이 실행되고 있는 터미널에 `Ctrl + C`를 입력하여 _myproject_ 응용 프로그램을 정지합니다.
    
14.  _src/main/java/com/example/myproject/GreetResource.java_ 파일을 편집하고 _createResponse(String who)_ 메소드를 찾아 스크린샷에 표시된 대로 다음 행을 추가합니다.
    
        <copy>System.out.println("Running on thread " + Thread.currentThread());</copy>
        
    
    ![애플리케이션 수정](images/modify-application.png)
    
15.  10, 11 및 12단계에 설명된 대로 응용 프로그램을 재구축, 실행 및 실행합니다.
    
16.  서버를 시작한 터미널에서 서버 출력을 확인합니다. 스레드 이름은 helidon-server-n입니다. JAX-RS 요청을 처리하기 위해 Helidon에서 생성된 스레드 풀의 기존 플랫폼 스레드입니다. 다음과 유사한 서버 출력이 제공됩니다.
    
        Running on thread Thread[#25,helidon-server-1,5,server]
        
17.  "java -jar target/myproject.jar" 명령이 실행되고 있는 터미널에 `Ctrl + C`를 입력하여 _myproject_ 응용 프로그램을 정지합니다.
    

## 작업 2: Helidom MicroProfile 애플리케이션을 Helidon 4로 마이그레이션

1.  myproject의 경우 _pom.xml_ 파일을 열고 상위 pom을 _3.1.1_에서 _4.0.0-ALPHA5_로 변경합니다.
    
        <parent>
            <groupId>io.helidon.applications</groupId>
            <artifactId>helidon-mp</artifactId>
            <version>4.0.0-ALPHA5</version>
            <relativePath/>
        </parent>
        
    
    ![폼 버전](images/pom-version.png)
    
2.  src/main/resources/logging.properties를 편집하고 _io.helidon.common.HelidonConsoleHandler_를 _io.helidon.logging.jul.HelidonConsoleHandler_로 변경합니다. ![로깅 편집](images/edit-logging.png)
    
3.  이제 애플리케이션이 Helidon 4로 이전되었습니다! 다음 명령을 복사하여 붙여넣어 응용 프로그램을 작성합니다.
    
        <copy>mvn clean package -DskipTests</copy>
        
4.  다음 명령을 복사하여 붙여넣어 응용 프로그램을 실행합니다.
    
        <copy>java --enable-preview  -jar target/myproject.jar</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ java --enable-preview  -jar target/myproject.jar
        2023.02.28 03:56:17 INFO io.helidon.logging.jul.JulProvider Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:56:21 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] http://0.0.0.0:8080 bound for socket '@default'
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] direct writes
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Started all channels in 19 milliseconds. 5131 milliseconds since JVM startup. Java 19.0.2+7-44
        2023.02.28 03:56:22 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 5144 milliseconds (since JVM startup).
        2023.02.28 03:56:22 INFO io.helidon.common.features.HelidonFeatures Thread[#28,features-thread,5,main]: Helidon MP 4.0.0-ALPHA5 features: [CDI, Config, Health, Metrics, Open API, Server, WebServer]
        
5.  서버 출력을 살펴보고 스레드가 이제 VirtualThread인지 확인합니다.
    
6.  "java --enable-preview -jar target/myproject.jar" 명령이 실행되고 있는 터미널에 `Ctrl + C`를 입력하여 _myproject_ 응용 프로그램을 정지합니다.
    

축하합니다. Helidon 가상 스레드 워크샵을 완료했습니다.

## 확인

*   **작성자** - Joe DiPol
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **Last Updated By/Date** - Ankit Pandey, 2023년 2월
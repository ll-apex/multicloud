# Helidon Nima 및 반응적 애플리케이션 구축 및 실행

## 소개

이 실습에서는 Oracle Cloud Infrastructure 내부의 Oracle Code Editor에서 Nima 및 Reactive Helidon 애플리케이션을 구축하고 실행하는 프로세스를 안내합니다.

[Lab2 연습](videohub:1_e88ydqwt)

예상 시간: 15분

### 목표

이 실습에서는 다음을 수행합니다.

*   Helidon Nima 애플리케이션 구축, 실행 및 테스트
*   Helidon Reactive 애플리케이션 구축, 실행 및 테스트
*   반응적 애플리케이션과 비교하여 Nima 애플리케이션의 단순성 분석

### 필요 조건

이 실습을 실행하려면 다음이 있어야 합니다.

*   Oracle Cloud 계정
*   필요한 JDK 및 Maven을 설치하는 Lab 1을 완료했습니다.

## 작업 1: Nima 응용 프로그램 빌드 및 실행

1.  Code Editor에서 _File_ -> _Open_을 누릅니다. ![프로젝트 열기](images/open-project.png)
    
2.  _helidon-levelup-2023-main_ 폴더를 선택하고 _Open_을 누릅니다. ![Helidon 프로젝트](images/helidon-project.png)
    
    > 이 폴더를 여는 동안 코드 편집기에서 발생한 경고/문제를 무시하십시오.
    
3.  새 터미널을 열고 다음 명령을 복사하여 붙여넣어 PATH 및 JAVA\_HOME 변수를 설정합니다.
    
        <copy>PATH=~/jdk-19.0.2/bin:~/apache-maven-3.8.3/bin:$PATH
        JAVA_HOME=~/jdk-19.0.2</copy>
        
4.  다음 명령을 복사하고 터미널에서 실행하여 필요한 JDK 및 Maven 버전이 제대로 구성되었는지 확인합니다.
    
        <copy>mvn -v</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ mvn -v 
        Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
        Maven home: /home/ankit_x_pa/apache-maven-3.8.3
        Java version: 19.0.2, vendor: Oracle Corporation, runtime: /home/ankit_x_pa/jdk-19.0.2
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "4.14.35-2047.520.3.1.el7uek.x86_64", arch: "amd64", family: "unix"
        $ 
        
    
    > _이 터미널은 필요한 JDK 및 Maven 버전을 사용하므로 애플리케이션을 빌드하고 실행하는 데만 사용됩니다._
    
5.  다음 명령을 복사하여 붙여넣어 nima 애플리케이션을 작성합니다.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    다음과 같이 출력됩니다.
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-blocking ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/nima/target/example-nima-blocking.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  7.562 s
        [INFO] Finished at: 2023-02-27T11:42:52Z
        [INFO] ------------------------------------------------------------------------
        
6.  터미널에서 다음 명령을 복사하여 붙여넣어 애플리케이션의 차단 nima 버전을 실행합니다.
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.27 11:43:18.589 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:43:18.902 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:43:19.700 [0x314d2373] http://127.0.0.1:33193 bound for socket '@default'
        2023.02.27 11:43:19.703 [0x314d2373] direct writes
        2023.02.27 11:43:19.722 Helidon Níma 4.0.0-ALPHA5
        
7.  서버가 실행되는 포트 번호를 기록해 둡니다(@default의 로그 항목 참조). 예를 들어 출력에서 포트 번호는 33193입니다. 마찬가지로 서버 포트 번호를 찾습니다.
    
8.  새 터미널을 열려면 _터미널_ -> _새 터미널_을 누릅니다. 이 터미널을 사용하여 _curl_ 명령을 실행합니다. ![새 터미널](images/new-terminal.png)
    
9.  다음 명령을 복사하여 새 터미널에 붙여 넣습니다. _`<port>`_를 서버 포트로 교체해야 합니다.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    다음과 같이 출력됩니다.
    
        curl http://localhost:33193/one
        remote_1
        $
        
10.  다음 명령을 복사하여 새 터미널에 붙여 넣습니다. _`<port>`_를 서버 포트로 교체해야 합니다.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ curl http://localhost:33193/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > 결과 순서를 살펴봅니다. 이름에서 제안한 대로 이 요청은 원격 리소스를 순서대로 여러 번 호출합니다.
    
11.  다음 명령을 복사하여 새 터미널에 붙여 넣습니다. _`<port>`_를 서버 포트로 교체해야 합니다.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ curl http://localhost:33193/parallel
        Combined results: [remote_21, remote_18, remote_12, remote_13, remote_14, remote_15, remote_16, remote_17, remote_19, remote_20]
        $
        
    
    > 결과 순서를 살펴봅니다. 이름에서 제안한 대로 이 요청은 원격 리소스를 병렬로 여러 번 호출합니다.
    
12.  \*java -jar \* 명령이 실행 중인 터미널에서 _Ctrl + C_를 눌러 서버를 정지합니다.
    

## 작업 2: 반응적 응용 프로그램 빌드 및 실행

1.  다음 명령을 복사하여 붙여넣어 nima 애플리케이션을 작성합니다.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    다음과 같이 출력됩니다.
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-reactive ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/reactive/target/example-nima-reactive.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  6.196 s
        [INFO] Finished at: 2023-02-27T11:51:17Z
        [INFO] ------------------------------------------------------------------------
        $
        
2.  다음 명령을 복사하여 터미널에 붙여넣어 응용 프로그램의 반응적 버전을 실행합니다.
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.27 11:51:26.227 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:51:26.723 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:51:27.286 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.27 11:51:27.618 Channel '@default' started: [id: 0x53fadd43, L:/0.0.0.0:45765]
        
3.  서버가 실행되는 포트 번호를 기록해 둡니다(@default의 로그 항목 참조). 예를 들어 출력에서 포트 번호는 45765입니다. 마찬가지로 서버 포트 번호를 찾습니다.
    
4.  curl 명령을 실행하기 위해 작업 1에서 연 터미널로 돌아갑니다.
    
5.  다음 명령을 복사하여 새 터미널에 붙여 넣습니다. _`<port>`_를 서버 포트로 교체해야 합니다.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ curl http://localhost:45765/one
        remote_1
        $
        
6.  다음 명령을 복사하여 새 터미널에 붙여 넣습니다. _`<port>`_를 서버 포트로 교체해야 합니다.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ curl http://localhost:45765/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > 결과 순서를 살펴봅니다. 이름에서 제안한 대로 이 요청은 원격 리소스를 순서대로 여러 번 호출합니다.
    
7.  다음 명령을 복사하여 새 터미널에 붙여 넣습니다. _`<port>`_를 서버 포트로 교체해야 합니다.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    다음과 같이 출력됩니다.
    
        $ curl http://localhost:45765/parallel
        Combined results: [remote_17, remote_16, remote_13, remote_20, remote_12, remote_19, remote_18, remote_14, remote_21, remote_15]
        $
        
    
    > 결과 순서를 살펴봅니다. 이름에서 제안한 대로 이 요청은 원격 리소스를 병렬로 여러 번 호출합니다.
    
8.  \*java -jar \* 명령이 실행 중인 터미널에서 _Ctrl + C_를 눌러 서버를 정지합니다.
    

## 작업 3: Nima 애플리케이션의 단순성 분석

**차단과 반응성 비교**

동일한 작업에 대해 Níma(차단)와 Helidon SE(반응적) 간의 구현을 비교해 보겠습니다.

*   두 구현 모두 Helidon WebClient을 사용하여 REST 호출을 실행합니다.
*   차단 구현은 다음 사항을 따르기 쉽습니다.
    *   실행 흐름은 코드의 명령문 순서에 따라 반영됩니다.
    *   복잡하거나 의미가 풍부한 라이브러리 호출이 없습니다.
    *   디버깅은 간단합니다.
*   반응적 버전을 사용하려면 반응적 라이브러리를 제대로 이해해야 합니다.
    *   멀티, flatMap, 오류 처리 등 포함
    *   사후 처리기 사용으로 인해 제어 흐름이 더 이상 명확하지 않습니다.
    *   FlatMap의 동시성 활성화/비활성화 기능에 대한 이해가 필요합니다.
    *   디버깅이 더 어렵습니다.

1.  _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ 파일을 열어 애플리케이션의 nima 버전에서 엔드포인트가 구현되는 방법을 확인합니다. ![니마 블록](images/nima-block.png)
    
2.  _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ 파일을 열어 애플리케이션의 반응적 버전에서 엔드포인트가 구현되는 방법을 확인합니다. ![반응적 서비스](images/reactive-service.png)
    
3.  _OPEN EDITORS_ 섹션에서 _BlockingService.java_ 파일을 마우스 오른쪽 버튼으로 누르고 _Select for Compare_를 선택합니다. ![비교 선택](images/select-compare.png)
    
4.  _ReactiveService.java_ 파일을 마우스 오른쪽 버튼으로 누르고 _Compare with Selected_를 선택합니다. ![선택한 항목 비교](images/compare-selected.png)
    
5.  반응적 코드는 차단(가상 스레드)보다 더 복잡합니다. ![코드 비교](images/compare-code.png)
    
    > BlockingService 및 ReactiveService에서 각각 메소드 1, 시퀀스 및 병렬을 확인합니다. 그들이 어떻게 작동하는지 알고 있는지 확인하십시오!
    

이제 _다음 실습으로 진행_할 수 있습니다.

## 확인

*   **작성자** - Joe DiPol
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **Last Updated By/Date** - Ankit Pandey, 2023년 2월
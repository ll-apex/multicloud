# Bobby의 장부 애플리케이션 수정 및 새 애플리케이션 구성요소 이미지 생성

## 소개

이 실습에서는 _Cloud Shell_을 통해 bobbys-helidon-stock-application을 수정합니다. 나중에 bobbys-helidon-stock-application용 새 Docker 이미지를 생성할 것입니다. 이 bobbys-helidon-stock-application 이미지는 Bobby's Books 애플리케이션의 구성요소입니다.

예상 시간: 10분

### 목표

이 실습에서는 다음을 수행합니다.

*   bobbys-helidon-stock-application을 수정합니다.
*   bobbys-helidon-stock-application에 대한 새 Docker 이미지를 생성합니다.

### 필요 조건

환경에 따라 명령 및 URL을 붙여넣고 수정할 수 있는 텍스트 편집기가 있어야 합니다. 그런 다음 _Cloud Shell_에서 실행하기 위해 수정된 명령을 복사하여 붙여넣을 수 있습니다.

## 작업 1: bobbys-helidon-stock-application 수정

1.  Bobby의 Book 탭을 선택한 다음 _Books_를 누른 다음 그림과 같이 _The Hobbit_ 설명서의 이미지를 누릅니다.
    
    ![장부 클릭](images/clickbooks.png " ")
    
    그림과 같이 장부 이름을 _The Hobbit_ 형식으로 표시합니다.
    
    ![호빗](images/thehobbit.png " ")
    
2.  장부 이름을 대문자로 변환하려고 합니다(THE HOBBIT). Bobby's Books 애플리케이션의 소스 코드를 다운로드해야 합니다. 홈 폴더에 있는지 확인합니다. 다음 명령을 복사하여 _Cloud Shell_에 붙여넣습니다.
    
        <copy>cd ~
        git clone -b v0.16.0 https://github.com/verrazzano/examples.git</copy>
        
    
    ![저장소 복제](images/clonerepository.png " ")
    
3.  Bobby's Book 애플리케이션 내의 파일을 보려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣습니다.
    
        <copy>ls -la ~/examples/bobs-books/</copy>
        
    
    출력은 다음과 유사해야 합니다. `bash $ ls -la ~/examples/bobs-books/ total 16 drwxr-xr-x. 5 ankit_x_pa oci 100 Mar 21 12:14 . drwxr-xr-x. 7 ankit_x_pa oci 4096 Mar 21 12:14 .. drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobbys-books drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobs-bookstore-order-manager -rw-r--r--. 1 ankit_x_pa oci 942 Mar 21 12:14 README.md drwxr-xr-x. 4 ankit_x_pa oci 72 Mar 21 12:14 roberts-books $`
    
4.  이제 관련 JAVA\_FILE를 변경하려고 합니다. 파일을 열려면 다음 명령을 복사하여 _Cloud Shell_에 붙여 넣으십시오.
    
        <copy>vi ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/src/main/java/org/books/bobby/BookResource.java</copy>
        
    
    ![파일 열기](images/openfile.png " ")
    
5.  코드를 수정할 수 있도록 _i_를 누릅니다. 행 번호 84에서 새 행을 추가하려면 다음 행을 복사하여 그림과 같이 행 번호 84에 붙여넣습니다.
    
        <copy>optional.get().setTitle(optional.get().getTitle().toUpperCase());</copy>
        
    
    ![선 삽입](images/insertline.png " ")
    
6.  _Esc_, _:wq_를 차례로 눌러 변경 사항을 저장합니다.
    
    ![변경사항 저장](images/savechanges.png " ")
    

## 작업 2: bobbys-helidon-stock-application에 대한 새 Docker 이미지 생성

1.  _bobbys-coherence-application_에 대한 종속성이 있는 _bobbys-helidon-stock-application_에 대한 새 Docker 이미지를 빌드하려고 하므로 _Maven_ 명령을 실행하여 기존 bobbys-coherence-application 아카이브를 정리하고 로컬 Maven 저장소에 새 bobby-coherence-application 아카이브를 컴파일, 빌드, 패키지화 및 설치합니다. _bobbys-coherence_ 디렉토리 폴더로 변경하고 _bobbys-coherence_ 애플리케이션 아카이브를 컴파일, 빌드 및 로컬 Maven 저장소에 설치하려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣으십시오.
    
        <copy>
        cd ~/examples/bobs-books/bobbys-books/bobbys-coherence/
        mvn clean install
        </copy>
        
    
    결과 출력은 다음과 유사합니다(출력의 시작과 끝만 표시하도록 축약). `bash $ mvn clean install [INFO] Scanning for projects... [INFO] [INFO] ------------< io.verrazzano.example.books:bobbys-coherence >------------ [INFO] Building bobbys-coherence 1.0-SNAPSHOT [INFO] --------------------------------[ jar ]--------------------------------- [INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ bobbys-coherence --- [[INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/target/bobbys-coherence.jar to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.jar [INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/pom.xml to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.pom [INFO] ------------------------------------------------------------------------ [INFO] BUILD SUCCESS [INFO] ------------------------------------------------------------------------ [INFO] Total time: 8.887 s [INFO] Finished at: 2023-03-22T04:09:11Z [INFO] ------------------------------------------------------------------------ $`
    
2.  _bobbys-helidon-stock-application_을 수정했으므로 이 애플리케이션을 컴파일, 빌드 및 패키지화해야 합니다. _bobbys-helidon-stock-application_ 디렉토리로 변경하고 _bobbys-helidon-stock-application_을 JAR 파일로 패키지화하려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣으십시오. 두번째 이미지에서는 `~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target`에 `bobbys-helidon-stock-application.jar` 파일이 생성되는 것을 볼 수 있습니다.
    
        <copy>cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/
        mvn clean package
        </copy>
        
    
    The resulting output is similar to the following (abbreviated to show only the start and end of output): \`\`\`bash $ cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/ $ mvn clean package \[INFO\] Scanning for projects... \[INFO\] ------------------------------------------------------------------------ \[INFO\] Detecting the operating system and CPU architecture \[INFO\] ------------------------------------------------------------------------
    
         [INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ bobbys-helidon-stock-application ---
         [INFO] Building jar: /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target/bobbys-helidon-stock-application.jar
         [INFO] ------------------------------------------------------------------------
         [INFO] BUILD SUCCESS
         [INFO] ------------------------------------------------------------------------
         [INFO] Total time:  10.106 s
         [INFO] Finished at: 2023-03-22T04:11:38Z
         [INFO] ------------------------------------------------------------------------
         $
         ```
        
3.  bobby-stock-helidon-application용 Docker 이미지를 생성할 예정이지만 이 애플리케이션은 특정 버전의 JDK를 사용하며 새 이미지를 빌드하는 Docker 파일을 변경하고 싶지 않습니다. 따라서 필요한 JDK를 다운로드합니다. 필요한 JDK 버전을 다운로드하려면 아래 명령을 복사하여 _Cloud Shell_에 붙여넣으십시오.
    
        <copy>wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2_linux-x64_bin.tar.gz</copy>
        
    
    출력은 다음과 유사해야 합니다. \`\`bash $ wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz --2023-03-22 04:20:16-- https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz Resolving download.java.net (download.java.net)... 2.23.220.73 Connecting to download.java.net (download.java.net)|2.23.220.73|:443... connected. HTTP 요청이 전송됨, 응답 대기 중... 200 OK 길이: 198606200 (189M) \[application/x-gzip\] 저장 위치: 'openjdk-14.0.2\_linux-x64\_bin.tar.gz'
    
         100%[==============================================================================================================================>] 198,606,200  194MB/s   in 1.0s   
        
         2023-03-22 04:20:17 (194 MB/s) - ‘openjdk-14.0.2_linux-x64_bin.tar.gz’ saved [198606200/198606200]
         $
         ```
        
4.  실습 6의 Oracle Cloud Container Registry에 업로드할 Docker 이미지를 생성하고 있습니다. 파일 이름을 만들려면 다음 정보가 필요합니다.
    
    *   테넌시 네임스페이스
    *   영역의 끝점
5.  테넌시의 네임스페이스를 찾으려면 다음과 같이 _User_ Icon(사용자) -> _Tenancy(테넌시)_를 누릅니다. **오브젝트 스토리지 설정**에서 네임스페이스를 찾습니다. Lab 6에서도 사용할 수 있으므로 텍스트 파일의 어딘가에 복사하여 저장합니다.
    
    ![테넌시 네임스페이스 복사](images/copy-tenancynamespace.png " ")
    
6.  _지역의 끝점_을 찾습니다. 이 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)에 설명된 표를 참조하십시오. 표시된 예제에서 영역의 끝점은 _UK South (London)_(영역 이름)이고 끝점은 _lhr.ocir.io_입니다. 고유의 _영역 이름_에 대한 끝점을 찾아 텍스트 파일에 저장합니다. 다음 연습에도 필요합니다.
    
    ![끝점](images/end-point.png " ")
    
    > 이제 해당 지역에 대한 테넌시 네임스페이스와 엔드포인트가 모두 제공됩니다.
    
7.  이제 해당 지역의 테넌시 네임스페이스와 끝점이 모두 있습니다. 다음 명령을 복사하여 텍스트 편집기에 붙여넣습니다. 그런 다음 **`END_POINT_OF_YOUR_REGION`**를 영역 이름의 끝점으로 바꾸고, **`NAMESPACE_OF_YOUR_TENANCY`**을 테넌시의 네임스페이스로 바꾸고, **`your_first_name`**을 소문자의 이름으로 바꿉니다.
    
        <copy>docker build --force-rm=true -f Dockerfile -t END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0 .</copy>
        
    
    ![Docker 빌드](images/docker-build.png " ")
    
    ![만들어진 이미지](images/image-created.png " ")
    
    > 예를 들어, 내 경우 명령은 `docker build --force-rm=true -f Dockerfile -t lhr.ocir.io/tenancynamespace/helidon-stock-application-ankit`입니다.
    
    그러면 실습 6의 Oracle Cloud Container Registry 저장소로 푸시할 Docker 이미지가 생성됩니다. editor.In Lab 6 텍스트에서 대체된 전체 이미지 이름 **`END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0`**을 복사해야 합니다. 저장소를 만들어야 하는 경우 이름을 **`helidon-stock-application-your_first_name`**로 지정해야 합니다.
    
    _Cloud Shell_은 열어 둡니다. 다음 실습을 위해 필요합니다.
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
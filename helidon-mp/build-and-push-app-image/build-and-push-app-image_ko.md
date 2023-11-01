# Helidon 애플리케이션 이미지를 빌드하여 Oracle Cloud Container Registry로 푸시

## 소개

이 실습에서는 Helidon 애플리케이션을 사용하여 _고유_ Docker 이미지를 빌드하고 해당 이미지를 Oracle Cloud Container Registry 내의 저장소에 푸시합니다.

예상 시간: 15분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [Helidon 애플리케이션 이미지를 빌드하여 Oracle Cloud Container Registry로 푸시](videohub:1_mh1brw5t)

### 목표

*   Docker를 사용하여 애플리케이션을 구축하고 패키지화합니다.
*   인증 토큰을 생성하여 Oracle Cloud Container Registry에 로그인합니다.
*   Helidon 애플리케이션 Docker 이미지를 Oracle Cloud Container Registry 리포지토리로 푸시합니다.

### 필요 조건

*   이전 연습에서 생성한 Helidon 응용 프로그램
*   Docker
*   Oracle Cloud 계정

## 작업 1: Helidon Application Docker 이미지 빌드

OCI 계정에 속한 Oracle Cloud Container Registry에 업로드할 Docker 이미지를 생성하는 중입니다. 이렇게 하려면 레지스트리 좌표를 반영하는 이미지 이름을 생성해야 합니다.

다음 정보가 필요합니다.

*   지역 이름
*   테넌시 네임스페이스
*   영역에 대한 끝점
    
    > 이 정보를 텍스트 파일에 복사하여 실습 과정에서 참조할 수 있습니다.
    

1.  _지역 이름_을 찾습니다.  
    _영역 이름_은 Oracle Cloud 콘솔의 오른쪽 맨 위에 있으며 이 예에서는 _영국 남부(런던)_로 표시됩니다. 너는 다를지도 모른다.
    
    ![컨테이너 레지스트리](images/region-name.png)
    
2.  _Tenancy Namespace_를 찾습니다.  
    콘솔에서 탐색 메뉴를 열고 **개발자 서비스**를 누릅니다. **컨테이너 및 아티팩트**에서 **컨테이너 레지스트리**를 누릅니다.
    
    ![테넌시 네임스페이스](images/container-registry.png)
    
    > 테넌시 네임스페이스가 구획에 나열됩니다. 복사하여 텍스트 파일에 저장합니다. 이 정보는 다음 연습에서도 사용할 수 있습니다. ![테넌시 네임스페이스](images/name-space.png)
    
3.  _지역의 끝점_을 찾습니다.  
    이 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)에 설명된 표를 참조하십시오. 표시된 예제에서 영역의 끝점은 _UK South (London)_(영역 이름)이고 끝점은 _lhr.ocir.io_입니다. 고유의 _영역 이름_에 대한 끝점을 찾아 텍스트 파일에 저장합니다. 다음 연습에도 필요합니다.
    
    ![끝점](images/end-points.png)
    
    > 이제 해당 지역에 대한 테넌시 네임스페이스와 엔드포인트가 모두 제공됩니다.
    
4.  다음 명령을 복사하여 텍스트 파일에 붙여 넣습니다. 그런 다음 _`ENDPOINT_OF_YOUR_REGION`_를 영역 이름의 끝점으로 바꾸고, _`NAMESPACE_OF_YOUR_TENANCY`_을 테넌시의 네임스페이스로 바꾸고, _`your_first_name`_을 사용자 이름으로 바꿉니다.
    
    > Docker 컨테이너 내에서 전체 빌드를 수행합니다. 처음 실행하면 모든 Maven 종속성을 다운로드하고 Docker 계층에 캐시하기 때문에 다소 시간이 걸립니다. 이후 빌드는 pom.xml 파일을 변경하지 않는 한 훨씬 더 빠릅니다. POM이 수정된 경우 종속성이 다시 다운로드됩니다.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0 -f Dockerfile.native .</copy>
        
    
    명령이 준비되면 _`~/helidon-project/myproject/myproject`_ 디렉토리의 Code Editor 내부 터미널에서 명령을 실행합니다. 빌드가 종료 시 다음 결과를 생성합니다.
    
        $ docker build -t lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 -f Dockerfile.native .
        
        [1/7] Initializing...                                                                                   (15.7s @ 0.14GB)
        Version info: 'GraalVM 22.3.0 Java 17 CE'
        Java version info: '17.0.5+8-jvmci-22.3-b08'
        C compiler: gcc (redhat, x86_64, 11.3.1)
        Garbage collector: Serial GC
        2 user-specific feature(s)
        - io.helidon.integrations.graal.mp.nativeimage.extension.WeldFeature
        - io.helidon.integrations.graal.nativeimage.extension.HelidonReflectionFeature
        2023.04.06 05:41:01 INFO io.helidon.common.LogConfig Thread[main,5,main]: Logging at initialization configured using classpath: /logging.properties
        [2/7] Performing analysis...  [**********]                                                             (202.8s @ 1.92GB)
        18,812 (92.77%) of 20,278 classes reachable
        27,564 (63.52%) of 43,392 fields reachable
        87,900 (62.22%) of 141,268 methods reachable
        1,068 classes,   565 fields, and 6,864 methods registered for reflection
            65 classes,    70 fields, and    58 methods registered for JNI access
            6 native libraries: dl, m, pthread, rt, stdc++, z
        [3/7] Building universe...                                                                              (27.6s @ 3.15GB)
        [4/7] Parsing methods...      [*****]                                                                   (22.5s @ 3.00GB)
        [5/7] Inlining methods...     [***]                                                                     (11.9s @ 1.84GB)
        [6/7] Compiling methods...    [************]                                                           (156.5s @ 3.05GB)
        [7/7] Creating image...                                                                                 (15.6s @ 2.44GB)
        35.03MB (45.80%) for code area:    57,947 compilation units
        39.02MB (51.01%) for image heap:  477,987 objects and 128 resources
        2.44MB ( 3.19%) for other data
        76.49MB in total
        ------------------------------------------------------------------------------------------------------------------------
        Top 10 packages in code area:                               Top 10 object types in image heap:
        1.63MB sun.security.ssl                                     7.71MB byte[] for code metadata
        1.20MB com.sun.media.sound                                  4.60MB java.lang.Class
        1.17MB java.util                                            3.93MB java.lang.String
        822.87KB java.lang.invoke                                     3.41MB byte[] for java.lang.String
        717.54KB com.sun.crypto.provider                              3.22MB byte[] for general heap data
        517.57KB io.helidon.config                                    1.58MB com.oracle.svm.core.hub.DynamicHubCompanion
        510.02KB java.util.concurrent                                 1.13MB byte[] for reflection metadata
        481.49KB jdk.proxy4                                           1.03MB byte[] for embedded resources
        474.98KB java.lang                                          915.61KB java.util.HashMap$Node
        468.42KB com.sun.org.apache.xerces.internal.impl            781.21KB java.lang.String[]
        26.70MB for 671 more packages                                9.83MB for 4584 more object types
        ------------------------------------------------------------------------------------------------------------------------
                                31.0s (6.6% of total time) in 59 GCs | Peak RSS: 4.80GB | CPU load: 1.60
        ------------------------------------------------------------------------------------------------------------------------
        Produced artifacts:
        /helidon/target/myproject (executable)
        /helidon/target/myproject.build_artifacts.txt (txt)
        ========================================================================================================================
        Finished generating 'myproject' in 7m 48s.
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  08:04 min
        [INFO] Finished at: 2023-04-06T05:48:33Z
        [INFO] ------------------------------------------------------------------------
        Removing intermediate container e400c5c6897b
        ---> 20099e4619d6
        Step 10/15 : RUN echo "done!"
        ---> Running in a8eddd448e48
        done!
        Removing intermediate container a8eddd448e48
        ---> ebfd3064dc68
        Step 11/15 : FROM scratch
        ---> 
        Step 12/15 : WORKDIR /helidon
        ---> Running in 46be56a98462
        Removing intermediate container 46be56a98462
        ---> eaf15b746a1c
        Step 13/15 : COPY --from=build /helidon/target/myproject .
        ---> a69ac5933048
        Step 14/15 : ENTRYPOINT ["./myproject"]
        ---> Running in 71633a601e7f
        Removing intermediate container 71633a601e7f
        ---> cd9f22bfa4b3
        Step 15/15 : EXPOSE 8080
        ---> Running in 4b9763eb49fa
        Removing intermediate container 4b9763eb49fa
        ---> aa8b6e7b04c0
        Successfully built aa8b6e7b04c0
        Successfully tagged lhr.ocir.io/lrv4zdykjqrj/myproject-ankit:1.0
        
5.  그러면 로컬 저장소에서 확인할 수 있는 Docker 이미지가 생성됩니다.
    
        $ docker images
        REPOSITORY                     TAG IMAGE ID        CREATED           SIZE
        lhr.ocir.io/tn/myproject-ankit 1.0 aa8b6e7b04c0 About a minute ago   80.2MB
        
    
    나중에 필요하므로 텍스트 편집기에 대체된 전체 이미지 이름 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0`를 복사합니다.
    
6.  다음 명령을 복사하여 터미널에 붙여넣어 Cloud Shell of Code Editor에서 도커 이미지를 실행합니다.
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    ![Docker 실행](images/docker-run.png)
    
7.  새 터미널/콘솔을 열고 다음 명령을 실행하여 응용 프로그램을 확인합니다.
    
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
        

## 작업 2: Oracle Cloud Container Registry에 로그인하기 위한 인증 토큰 생성

이 단계에서는 Oracle Cloud Container Registry 로그인에 사용할 _인증 토큰_을 생성합니다.

1.  오른쪽 맨 위에 있는 사용자 아이콘을 선택한 다음 _내 프로파일_을 선택합니다.
    
    ![내 프로파일](images/my-profile.png " ")
    
2.  아래로 스크롤하여 _Auth Tokens_을 선택합니다.
    
    ![인증 토큰](images/auth-token.png " ")
    
3.  _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/generate-token.png " ")
    
4.  _`myproject-your_first_name`_을 복사하여 _설명_ 상자에 붙여넣고 _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/token-create.png " ")
    
5.  생성된 토큰에서 _복사_를 선택하고 텍스트 편집기에 붙여넣습니다. 나중에 복사할 수 없습니다. 그런 다음 _닫기_를 누릅니다.
    
    ![토큰 복사](images/copy-token.png " ")
    

## 작업 3: Helidon 애플리케이션(myproject) Docker 이미지를 컨테이너 레지스트리 저장소로 푸시

1.  이 연습의 작업 1에서 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)을 열고 영역 이름의 끝점을 확인하여 텍스트 파일로 복사했습니다. 예제에서 Region Name은 UK South(런던)입니다. 이 태스크에 대해 이 정보가 필요합니다. ![엔드포인트](images/end-points.png)
    
2.  다음 명령을 복사하여 텍스트 파일에 붙여 넣은 다음 `ENDPOINT_OF_REGION_NAME`를 해당 영역의 끝점으로 바꿉니다.
    
    > 예제에서 Region Name은 _UK South (London)_이고 끝점은 _lhr.ocir.io_입니다. 이 태스크에 대한 특정 정보가 필요합니다.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  이전 단계에서는 테넌시 네임스페이스도 확인했습니다. Username에 `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`을 입력합니다.  
    
    *   `NAMESPACE_OF_YOUR_TENANCY`를 테넌시의 네임스페이스로 바꾸기
    *   `YOUR_ORACLE_CLOUD_USERNAME`을 Oracle Cloud 계정 사용자 이름으로 바꾼 다음 대체된 사용자 이름을 텍스트 파일에서 복사하여 _Cloud Shell_에 붙여넣습니다.
    *   비밀번호의 경우 텍스트 파일(또는 저장 위치)에서 인증 토큰을 복사하여 붙여 넣습니다.
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Container Registry로 다시 이동합니다. 콘솔에서 탐색 메뉴를 열고 **개발자 서비스**를 누릅니다. **컨테이너 및 아티팩트**에서 **컨테이너 레지스트리**를 누릅니다. ![컨테이너 레지스트리](images/container-registry.png)
    
5.  구획을 선택한 다음 **저장소 생성**을 누릅니다. ![저장소 생성](images/repository-create.png)
    
6.  구획을 선택하고 Repository Name으로 _`myproject-your_first_name`_를 입력한 다음 Access를 **Public**으로 선택하고 **Create Repository**를 누릅니다.
    
    ![저장소 설명](images/describe-repository.png)
    
7.  저장소 _`myproject-your_first_name`_를 만든 후 저장소 목록 및 해당 설정에서 확인할 수 있습니다.
    
    ![네임스페이스 확인](images/verify-namespace.png)
    
8.  Docker 이미지를 Oracle Cloud Container Registry 내의 저장소에 푸시하려면 텍스트 파일에서 다음 명령을 복사하여 붙여 넣은 다음 `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/myproject-your\_first\_name:1.0을 이전에 저장한 Docker 이미지 전체 이름으로 바꾸십시오.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    결과는 다음과 같습니다.
    
        $ docker push lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 The push refers to repository [lhr.ocir.io/tenancynamespace/myproject-ankit]
        0bf9e88b7284: Pushed 
        0acc08535a89: Pushed 
        1.0: digest: sha256:3e8cc0ff23d256499dff8d907150b639925687aed0c41008cbe1794bc02433e2 size: 735
        $
        
9.  _docker push_ 명령이 성공적으로 실행된 후 _`myproject-your_first_name`_ 저장소를 확장하면 새 이미지가 이 저장소에 업로드되었음을 알 수 있습니다.
    
    ![이미지 업로드됨](images/verify-push.png)
    

## 확인

*   **작성자** - Dmitry Aleksandrov
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 4월
# Springboot 애플리케이션 이미지를 Oracle Cloud Container Registry로 푸시

## 소개

이 실습에서는 springboot 애플리케이션을 사용하여 Docker 이미지를 빌드하고 해당 이미지를 Oracle Cloud Container Registry 내의 저장소에 푸시합니다.

예상 시간: 10분

### 목표

*   Docker를 사용하여 애플리케이션을 구축하고 패키지화합니다.
*   인증 토큰을 생성하여 Oracle Cloud Container Registry에 로그인합니다.
*   Springboot 애플리케이션 Docker 이미지를 Oracle Cloud Container Registry 저장소에 푸시합니다.

### 필요 조건

*   Docker
*   Oracle Cloud 계정

## 작업 1: 응용 프로그램 소스 코드 및 필요한 JDK 다운로드

1.  다음 명령을 복사하여 붙여넣어 이 워크샵의 소스 코드를 다운로드합니다.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/EeNNvCVLObhXjHIwu5M-J_zbSXJsbLop6wcsFFnDfneY3zqbAFfdKikZe-Q0GT3I/n/lrv4zdykjqrj/b/ankit-bucket/o/springboot-app.zip >~/springboot-app.zip</copy>
        
2.  다음 명령을 복사하여 붙여넣어 소스 코드의 압축을 풀고 현재 디렉토리를 응용 프로그램 폴더로 변경합니다.
    
        <copy>unzip ~/springboot-app.zip
        cd ~/springboot-app/</copy>
        
3.  Springboot 애플리케이션에 대한 Docker 이미지를 생성할 예정이지만, 이 애플리케이션은 특정 버전의 JDK를 사용하며, 새 이미지를 빌드하는 Docker 파일을 변경하고 싶지 않습니다. 따라서 필요한 JDK를 다운로드합니다. 필요한 JDK 버전을 다운로드하려면 아래 명령을 복사하여 Cloud Shell에 붙여넣으십시오.
    
        <copy>wget https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz</copy>
        
4.  다음 명령을 복사하여 붙여넣어 springboot 응용 프로그램을 작성합니다.
    
        <copy>mvn clean package</copy>
        

## 작업 2: Springboot 응용 프로그램 Docker 이미지 빌드

먼저 Verrazzano에 배포하는 데 사용할 Docker 이미지를 준비합니다.

OCI 계정에 속한 Oracle Cloud Container Registry에 업로드할 Docker 이미지를 생성하는 중입니다. 이렇게 하려면 레지스트리 좌표를 반영하는 이미지 이름을 생성해야 합니다.

다음 정보가 필요합니다.

*   테넌시 네임스페이스
    
*   영역에 대한 끝점
    
    > 이 정보를 텍스트 파일에 복사하여 실습 과정에서 참조할 수 있습니다.
    

1.  테넌시의 네임스페이스를 찾으려면 다음과 같이 _User_ Icon(사용자) -> _Tenancy(테넌시)_를 누릅니다. **오브젝트 스토리지 설정**에서 네임스페이스를 찾습니다. 나중에 사용할 수도 있으므로 텍스트 파일에 복사하여 저장합니다.
    
    ![테넌시 네임스페이스 복사](images/copy-tenancynamespace.png " ")
    
2.  _지역의 끝점_을 찾습니다.  
    이 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)에 설명된 표를 참조하십시오. 표시된 예제에서 영역의 끝점은 _UK South (London)_(영역 이름)이고 끝점은 _lhr.ocir.io_입니다. 고유의 _영역 이름_에 대한 끝점을 찾아 텍스트 파일에 저장합니다.
    
    ![끝점](images/end-points.png)
    
    > 이제 해당 지역에 대한 테넌시 네임스페이스와 엔드포인트가 모두 제공됩니다.
    
3.  다음 명령을 복사하여 텍스트 파일에 붙여 넣습니다. 그런 다음 _`ENDPOINT_OF_YOUR_REGION`_를 영역 이름의 끝점으로 바꾸고, _`NAMESPACE_OF_YOUR_TENANCY`_을 테넌시의 네임스페이스로 바꾸고, _`your_first_name`_을 사용자 이름으로 바꿉니다.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-your_first_name:v1 .</copy>
        
    
    명령이 준비되면 `~/springboot-app/` 디렉토리의 Cloud Shell에서 실행합니다. 빌드는 다음과 같은 결과를 생성합니다.
    
        $ cd ~/springboot-app/
        $ docker build -t lhr.ocir.io/tenancy-namespace/springboot-ankit:v1 .
        Sending build context to Docker daemon  206.7MB
        Step 1/14 : FROM ghcr.io/oracle/oraclelinux:7-slim AS build_base
        Trying to pull repository ghcr.io/oracle/oraclelinux ... 
        7-slim: Pulling from ghcr.io/oracle/oraclelinux
        6cb086706000: Pull complete 
        Digest: sha256:4353fdc8664c386c0a443eb40b10a7662b4eb8d6eb5d6dcefe218e9783132c71
        Status: Downloaded newer image for ghcr.io/oracle/oraclelinux:7-slim
        ---> 1d56b1a0fd84
        Step 2/14 : RUN yum update -y && yum-config-manager --save --setopt=ol7_ociyum_config.skip_if_unavailable=true     && yum install -y tar unzip gzip     && yum clean all; rm -rf /var/cache/yum     && mkdir -p /license
        ---> Running in 92c013a8e84f
        Loaded plugins: ovl
        No packages marked for update
        Loaded plugins: ovl
        ===================================== main =====================================
        [main]
        alwaysprompt = True
        assumeno = False
        assumeyes = False
        autocheck_running_kernel = True
        autosavets = True
        bandwidth = 0
        bugtracker_url = https://linux.oracle.com
        cache = 0
        cachedir = /var/cache/yum/x86_64/7Server
        check_config_file_age = True
        clean_requirements_on_remove = False
        ----------------------------------------------------------------------
        Step 3/14 : ENV JAVA_HOME=/usr/java
        ---> Running in 96b99fba7e50
        Removing intermediate container 96b99fba7e50
        ---> 1cc6c7a63b89
        Step 4/14 : ENV PATH $JAVA_HOME/bin:$PATH
        ---> Running in 4a88eb052547
        Removing intermediate container 4a88eb052547
        ---> 48e7fa9b7b0c
        Step 5/14 : ARG JDK_BINARY="${JDK_BINARY:-openjdk-11_linux-x64_bin.tar.gz}"
        ---> Running in e922e8b35bfd
        Removing intermediate container e922e8b35bfd
        ---> 6888b690a4b0
        Step 6/14 : COPY ${JDK_BINARY} jdk.tar.gz
        ---> e9c5ffd0f2a5
        Step 7/14 : ENV JDK_DOWNLOAD_SHA256=3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e
        ---> Running in a7a89b057f25
        Removing intermediate container a7a89b057f25
        ---> 12ecfaa002bf
        Step 8/14 : RUN set -eux     echo "Checking JDK hash";     echo "${JDK_DOWNLOAD_SHA256} *jdk.tar.gz" | sha256sum --check -;     echo "Installing JDK";     mkdir -p "$JAVA_HOME";     tar xzf jdk.tar.gz --directory "${JAVA_HOME}" --strip-components=1;     rm -f jdk.tar.gz;
        ---> Running in a6a590adeee6
        + echo '3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e *jdk.tar.gz'
        + sha256sum --check -
        jdk.tar.gz: OK
        + echo 'Installing JDK'
        + mkdir -p /usr/java
        Installing JDK
        + tar xzf jdk.tar.gz --directory /usr/java --strip-components=1
        + rm -f jdk.tar.gz
        Removing intermediate container a6a590adeee6
        ---> 1f37a6cb044d
        Step 9/14 : COPY LICENSE.txt /license/
        ---> dc69f24f5be6
        Step 10/14 : COPY THIRD_PARTY_LICENSES.txt /license/
        ---> 5ef683dbda22
        Step 11/14 : ARG JAR_FILE=target/*.jar
        ---> Running in 3b80032f8310
        Removing intermediate container 3b80032f8310
        ---> 8eca16289bd7
        Step 12/14 : COPY ${JAR_FILE} app.jar
        ---> dcb7e3ed0871
        Step 13/14 : ENTRYPOINT ["java","-jar","/app.jar"]
        ---> Running in 2191623bf524
        Removing intermediate container 2191623bf524
        ---> 11e59e19cfb4
        Step 14/14 : USER 1000
        ---> Running in 16446779b92b
        Removing intermediate container 16446779b92b
        ---> a701fa912f2e
        Successfully built a701fa912f2e
        Successfully tagged lhr.ocir.io/tenancynamespace/springboot-ankit:v1
        $
        
4.  그러면 로컬 저장소에서 확인할 수 있는 Docker 이미지가 생성됩니다.
    
        $ docker images
        REPOSITORY                            TAG     IMAGE ID    CREATED      SIZE
        lhr.ocir.io/namespace/springboot-ankit v1    a701fa912f2 3 minutes ago 664MB
        ghcr.io/oracle/oraclelinux            7-slim 1d56b1a0fd8 3 weeks ago   138MB
        $ 
        
    
    나중에 필요하므로 텍스트 파일에 대체된 전체 이미지 이름 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1`를 복사합니다.
    

## 작업 3: 인증 토큰을 생성하여 Oracle Cloud Container Registry에 로그인

이 단계에서는 Oracle Cloud Container Registry 로그인에 사용할 _인증 토큰_을 생성합니다.

1.  오른쪽 맨 위에 있는 사용자 아이콘을 선택한 다음 _내 프로파일_을 선택합니다.
    
    ![내 프로파일](images/my-profile.png " ")
    
2.  아래로 스크롤하여 _Auth Tokens_을 선택합니다.
    
    ![인증 토큰](images/auth-token.png " ")
    
3.  _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/generate-token.png " ")
    
4.  _`springboot-your_first_name`_을 복사하여 _설명_ 상자에 붙여넣고 _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/token-create.png " ")
    
5.  생성된 토큰에서 _복사_를 선택하고 텍스트 파일에 붙여넣습니다. 나중에 복사할 수 없습니다. 그런 다음 _닫기_를 누릅니다.
    
    ![토큰 복사](images/copy-token.png " ")
    

## 작업 4: Springboot 애플리케이션 Docker 이미지를 컨테이너 레지스트리 저장소로 푸시

1.  이 연습의 작업 1에서 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)을 열고 영역 이름의 끝점을 확인하여 텍스트 파일로 복사했습니다. 예제에서 Region Name은 UK South(런던)입니다. 이 태스크에 대해 이 정보가 필요합니다. ![엔드포인트](images/end-points.png)
    
2.  다음 명령을 복사하여 텍스트 편집기에 붙여 넣은 다음 `ENDPOINT_OF_REGION_NAME`를 해당 영역의 끝점으로 바꿉니다.
    
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
    
5.  구획을 선택한 다음 **생성**을 누릅니다. ![저장소 생성](images/repository-create.png)
    
6.  구획을 선택하고 Repository Name으로 _`springboot-your_first_name`_를 입력한 다음 Access를 **Public**으로 선택하고 **Create Repository**를 누릅니다.
    
    ![저장소 설명](images/describe-repository.png)
    
7.  Docker 이미지를 Oracle Cloud Container Registry 내의 저장소에 푸시하려면 텍스트 파일에서 다음 명령을 복사하여 붙여 넣은 다음 _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/springboot-your\_firstname:1.0_을 이전에 저장한 Docker 이미지 전체 이름으로 바꿉니다.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1</copy>
        
    
    결과는 다음과 같습니다.
    
        $ docker push lhr.ocir.io/namespace/springboot-ankit:v1
        The push refers to repository [lhr.ocir.io/namespace/springboot-ankit]
        31118271414e: Pushed 
        e6144652ec48: Pushed 
        a5ac4d4576aa: Pushed 
        2f93ab3a0c42: Pushed 
        3ed60ad88e51: Pushed 
        f47db30f116a: Pushed 
        f50ba2e0b2f9: Pushed 
        v1: digest: sha256:96aacff31cb255ea815213aba837f16f40d73b14d67449d4744ed811c7a864c8 size: 1795
        $ 
        
8.  _docker push_ 명령이 성공적으로 실행된 후 _`springboot-ankit:v1`_ 저장소를 확장하면 새 이미지가 이 저장소에 업로드되었음을 알 수 있습니다.
    

이제 **다음 실습으로 진행**할 수 있습니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
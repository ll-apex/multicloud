# Tomcat 애플리케이션 이미지를 Oracle Cloud Container Registry로 푸시

## 소개

이 실습에서는 tomcat 애플리케이션으로 Docker 이미지를 구축하여 해당 이미지를 Oracle Cloud Container Registry 내의 저장소에 푸시합니다.

예상 시간: 10분

### 목표

*   Docker를 사용하여 애플리케이션을 구축하고 패키지화합니다.
*   인증 토큰을 생성하여 Oracle Cloud Container Registry에 로그인합니다.
*   Tomcat 애플리케이션 Docker 이미지를 Oracle Cloud Container Registry 리포지토리로 푸시합니다.

### 필요 조건

*   Docker
*   Oracle Cloud 계정

## 작업 1: 응용 프로그램 소스 코드 다운로드

1.  다음 명령을 복사하여 붙여넣어 이 워크샵의 소스 코드를 다운로드합니다.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/5DhoxZ22MseWaoSjG3EoBRf6DF5JbQ1QyR7tHa2qem-7IHb7nNEymF1ZSm2eA1ix/n/lrv4zdykjqrj/b/ankit-bucket/o/tomcat-examples.zip >~/tomcat-examples.zip</copy>
        
2.  다음 명령을 복사하여 붙여넣어 소스 코드의 압축을 풀고 현재 디렉토리를 응용 프로그램 폴더로 변경합니다.
    
        <copy>unzip ~/tomcat-examples.zip
        cd ~/tomcat-examples/</copy>
        

## 작업 2: Tomcat 애플리케이션 Docker 이미지 빌드

먼저 Verrazzano에 배포하는 데 사용할 Docker 이미지를 준비합니다.

OCI 계정에 속한 Oracle Cloud Container Registry에 업로드할 Docker 이미지를 생성하는 중입니다. 이렇게 하려면 레지스트리 좌표를 반영하는 이미지 이름을 생성해야 합니다.

다음 정보가 필요합니다.

*   테넌시 네임스페이스
    
*   영역에 대한 끝점
    
    > 이 정보를 텍스트 파일에 복사하여 실습 과정에서 참조할 수 있습니다.
    

1.  테넌시의 네임스페이스를 찾으려면 다음과 같이 _User_ Icon(사용자) -> _Tenancy(테넌시)_를 누릅니다. **오브젝트 스토리지 설정**에서 네임스페이스를 찾습니다. 나중에 사용할 수도 있으므로 텍스트 파일에 복사하여 저장합니다.
    
    ![테넌시 네임스페이스 복사](images/copy-tenancynamespace.png " ")
    
2.  URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)를 열고 영역 이름의 끝점을 확인하여 텍스트 파일에 복사합니다. 예제에서 Region Name은 UK South(런던)입니다. 이 태스크에 대해 이 정보가 필요합니다. ![엔드포인트](images/end-point.png)
    
3.  다음 명령을 복사하여 텍스트 편집기에 붙여 넣습니다. 그런 다음 _`ENDPOINT_OF_YOUR_REGION`_를 영역 이름의 끝점으로 바꾸고, _`NAMESPACE_OF_YOUR_TENANCY`_을 테넌시의 네임스페이스로 바꾸고, _`your_first_name`_을 사용자 이름으로 바꿉니다.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-your_first_name:v1 .</copy>
        
    
    명령이 준비되면 `~/tomcat-examples/` 디렉토리의 Cloud Shell에서 실행합니다. 빌드는 다음과 같은 결과를 생성합니다.
    
        $ cd ~/tomcat-examples/
        $ $ docker build -t lhr.ocir.io/tenancy-namespace/tomcat-example-ankit:v1 .
        Sending build context to Docker daemon  1.097MB
        Step 1/10 : FROM tomcat:8.0-alpine
        Trying to pull repository docker.io/library/tomcat ... 
        8.0-alpine: Pulling from docker.io/library/tomcat
        4fe2ade4980c: Pull complete 
        6fc58a8d4ae4: Pull complete 
        7d9bd64c803b: Pull complete 
        a22aedc5ac11: Pull complete 
        5bde63ae3587: Pull complete 
        69cb0c9b940a: Pull complete 
        Digest: sha256:d02a16c0147fcae13d812fa670a4b3c9944f5328b10a5a463ad697d2aa5bb063
        Status: Downloaded newer image for tomcat:8.0-alpine
        ---> 624fb61775c3
        Step 2/10 : LABEL maintainer="ankit.x.pandey@oracle.com"
        ---> Running in 20cc23726499
        Removing intermediate container 20cc23726499
        ---> 50245c696fb6
        Step 3/10 : ADD sample-webapp.war /usr/local/tomcat/webapps/
        ---> 727c55f91bb5
        Step 4/10 : RUN mkdir /data
        ---> Running in f3129a859e11
        Removing intermediate container f3129a859e11
        ---> 9ce0f5674f51
        Step 5/10 : ADD jmx_prometheus_javaagent-0.17.0.jar /data/jmx_prometheus_javaagent-0.17.0.jar
        ---> f03cc9ee1bee
        Step 6/10 : ADD prometheus-jmx-config.yaml /data/prometheus-jmx-config.yaml
        ---> 50c51ae6a148
        Step 7/10 : ENV JAVA_OPTS="-javaagent:/data/jmx_prometheus_javaagent-0.17.0.jar=8088:/data/prometheus-jmx-config.yaml"
        ---> Running in 5e9effd5d494
        Removing intermediate container 5e9effd5d494
        ---> 85ca06fcd965
        Step 8/10 : EXPOSE 8088
        ---> Running in 795325f82526
        Removing intermediate container 795325f82526
        ---> 19dfc6fd903c
        Step 9/10 : EXPOSE 8080
        ---> Running in 43be96f20275
        Removing intermediate container 43be96f20275
        ---> 7d9bcaa7a271
        Step 10/10 : CMD ["catalina.sh", "run"]
        ---> Running in 3e25cd78ab88
        Removing intermediate container 3e25cd78ab88
        ---> 516065fe1bf5
        Successfully built 516065fe1bf5
        Successfully tagged lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        $
        
4.  그러면 로컬 저장소에서 확인할 수 있는 Docker 이미지가 생성됩니다.
    
        $ docker images
        REPOSITORY                           TAG IMAGE ID      CREATED       SIZE
        lhr.ocir.io/name/tomcat-example-ankit v1 516065fe1bf5 2 minutes ago  147MB
        tomcat                       8.0-alpine  624fb61775c3 4 years ago    147MB
        
    
    나중에 필요하므로 텍스트 편집기에 대체된 전체 이미지 이름 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1`를 복사합니다.
    

## 작업 3: 인증 토큰을 생성하여 Oracle Cloud Container Registry에 로그인

이 단계에서는 Oracle Cloud Container Registry 로그인에 사용할 _인증 토큰_을 생성합니다.

1.  오른쪽 맨 위에 있는 사용자 아이콘을 선택한 다음 _내 프로파일_을 선택합니다.
    
    ![내 프로파일](images/my-profile.png " ")
    
2.  아래로 스크롤하여 _Auth Tokens_을 선택합니다.
    
    ![인증 토큰](images/auth-token.png " ")
    
3.  _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/generate-token.png " ")
    
4.  _`tomcat-example-your_first_name`_을 복사하여 _설명_ 상자에 붙여넣고 _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/token-create.png " ")
    
5.  생성된 토큰에서 _복사_를 선택하고 텍스트 편집기에 붙여넣습니다. 나중에 복사할 수 없습니다. 그런 다음 _닫기_를 누릅니다.
    
    ![토큰 복사](images/copy-token.png " ")
    

## 작업 4: Tomcat 애플리케이션 Docker 이미지를 컨테이너 레지스트리 저장소로 푸시

1.  이 연습의 작업 2에서 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)을 열고 영역 이름의 끝점을 확인하여 텍스트 파일로 복사했습니다. 예제에서 Region Name은 UK South(런던)입니다. 이 태스크에 대해 이 정보가 필요합니다. ![엔드포인트](images/end-point.png)
    
2.  다음 명령을 복사하여 텍스트 편집기에 붙여 넣은 다음 `ENDPOINT_OF_REGION_NAME`를 해당 영역의 끝점으로 바꿉니다.
    
    > 예제에서 Region Name은 _UK South (London)_이고 끝점은 _lhr.ocir.io_입니다. 이 태스크에 대한 특정 정보가 필요합니다.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  이전 단계에서는 테넌시 네임스페이스도 확인했습니다. Username에 `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`을 입력합니다.  
    
    *   `NAMESPACE_OF_YOUR_TENANCY`를 테넌시의 네임스페이스로 바꾸기
    *   `YOUR_ORACLE_CLOUD_USERNAME`을 Oracle Cloud 계정 사용자 이름으로 바꾼 다음 대체된 사용자 이름을 텍스트 편집기에서 복사하여 _Cloud Shell_에 붙여넣습니다.
    *   비밀번호의 경우 텍스트 편집기(또는 저장 위치)에서 인증 토큰을 복사하여 붙여 넣습니다.
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Container Registry로 다시 이동합니다. 콘솔에서 탐색 메뉴를 열고 **개발자 서비스**를 누릅니다. **컨테이너 및 아티팩트**에서 **컨테이너 레지스트리**를 누릅니다. ![컨테이너 레지스트리](images/container-registry.png)
    
5.  구획을 선택한 다음 **저장소 생성**을 누릅니다. ![저장소 생성](images/repository-create.png)
    
6.  구획을 선택하고 저장소 이름으로 _`tomcat-example-your_first_name`_을 입력한 다음 Access를 **Public**으로 선택하고 **Create**를 누릅니다.
    
    ![저장소 설명](images/describe-repository.png)
    
7.  Docker 이미지를 Oracle Cloud Container Registry 내의 저장소에 푸시하려면 텍스트 편집기에서 다음 명령을 복사하여 붙여 넣은 다음 이전에 저장한 `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`tomcat-example-your_first_name`:1.0을 Docker 이미지 전체 이름으로 바꾸십시오.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1</copy>
        
    
    결과는 다음과 같습니다.
    
        $ docker push lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        The push refers to repository [lhr.ocir.io/tenancynamespace/tomcat-example-ankit]
        4b193f4c616d: Pushed 
        0469528628db: Pushed 
        cce8193c4190: Pushed 
        ca36c0db4673: Pushed 
        0136a6a85859: Pushed 
        98a0db77a14c: Pushed 
        9072514c7af0: Pushed 
        f6146a44a7d3: Pushed 
        0c3170905795: Pushed 
        df64d3292fd6: Pushed 
        v1: digest: sha256:65b562a7117870540f1807e0d796fe964e6428bda0ae290b8a6389bf637d1aba size: 2405
        $ 
        
8.  _docker push_ 명령이 성공적으로 실행된 후 _`tomcat-example-ankit:v1`_ 저장소를 확장하면 새 이미지가 이 저장소에 업로드되었음을 알 수 있습니다.
    

이제 **다음 실습으로 진행**할 수 있습니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
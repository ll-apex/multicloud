# Helidon 애플리케이션 이미지를 Oracle Cloud Container Registry로 푸시

## 소개

이 실습에서는 Helidon 애플리케이션으로 Docker 이미지를 빌드하고 Oracle Cloud Container Registry 내의 저장소에 해당 이미지를 푸시합니다.

예상 시간: 10분

### 목표

*   Docker를 사용하여 애플리케이션을 구축하고 패키지화합니다.
*   인증 토큰을 생성하여 Oracle Cloud Container Registry에 로그인합니다.
*   Helidon 애플리케이션 Docker 이미지를 Oracle Cloud Container Registry 리포지토리로 푸시합니다.

### 필요 조건

*   이전 연습에서 생성한 Helidon 응용 프로그램
*   Docker
*   Oracle Cloud 계정

## 작업 1: Helidon Application Docker 이미지 빌드

먼저 Verrazzano에 배포하는 데 사용할 Docker 이미지를 준비합니다.

OCI 계정에 속한 Oracle Cloud Container Registry에 업로드할 Docker 이미지를 생성하는 중입니다. 이렇게 하려면 레지스트리 좌표를 반영하는 이미지 이름을 생성해야 합니다.

다음 정보가 필요합니다.

*   테넌시 네임스페이스
*   영역에 대한 끝점

1.  테넌시의 네임스페이스를 찾으려면 다음과 같이 _User_ Icon(사용자) -> _Tenancy(테넌시)_를 누릅니다. **오브젝트 스토리지 설정**에서 네임스페이스를 찾습니다. 나중에 사용할 수도 있으므로 텍스트 파일에 복사하여 저장합니다.
    
    ![테넌시 네임스페이스 복사](images/copy-tenancynamespace.png " ")
    
2.  _지역의 끝점_을 찾습니다. 이 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)에 설명된 표를 참조하십시오. 표시된 예제에서 영역의 끝점은 _UK South (London)_(영역 이름)이고 끝점은 _lhr.ocir.io_입니다. 고유의 _영역 이름_에 대한 끝점을 찾아 텍스트 파일에 저장합니다. 다음 연습에도 필요합니다.
    
    ![끝점](images/end-point.png " ")
    
    > 이제 해당 지역에 대한 테넌시 네임스페이스와 엔드포인트가 모두 제공됩니다.
    
3.  다음 명령을 복사하여 텍스트 편집기에 붙여 넣습니다. 그런 다음 _`ENDPOINT_OF_YOUR_REGION`_를 영역 이름의 끝점으로 바꾸고, _`NAMESPACE_OF_YOUR_TENANCY`_을 테넌시의 네임스페이스로 바꾸고, _`your_first_name`_을 사용자 이름으로 바꿉니다.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0 .</copy>
        
    
    명령이 준비되면 _`~/quickstart-mp/`_ 디렉토리의 Cloud Shell에서 실행합니다. 빌드는 다음과 같은 결과를 생성합니다.
    
        $ cd ~/quickstart-mp/
        $ docker build lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0 .
        > docker pull lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        [+] Building 107.5s (19/19) FINISHED                                                                                                            
        => [internal] load build definition from Dockerfile                                                                                       0.1s
        => => transferring dockerfile: 785B                                                                                                       0.1s
        => [internal] load .dockerignore                                                                                                          0.1s
        => => transferring context: 48B                                                                                                           0.0s
        => [internal] load metadata for docker.io/library/openjdk:11-jre-slim                                                                     3.7s
        => [internal] load metadata for docker.io/library/maven:3.6-jdk-11                                                                        2.8s
        => [auth] library/openjdk:pull token for registry-1.docker.io                                                                             0.0s
        => [auth] library/maven:pull token for registry-1.docker.io                                                                               0.0s
        => [stage-1 1/4] FROM docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527      33.3s
        => => resolve docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527               0.0s
        => => sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527 549B / 549B                                                 0.0s
        => => sha256:f3cdb8fd164057f4ef3e60674fca986f3cd7b3081d55875c7ce75b7a214fca6d 1.16kB / 1.16kB                                             0.0s
        => => sha256:9c9e40a31d4fa290f933d76f3b0a4183ba02a7298a309f6bfa892d618e7196ef 7.56kB / 7.56kB                                             0.0s
        => => sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717 31.36MB / 31.36MB                                          18.6s
        => => sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251 1.58MB / 1.58MB                                             1.6s
        => => sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c 211B / 211B                                                 0.7s
        => => sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358 47.13MB / 47.13MB                                          24.4s
        => => extracting sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717                                                  7.7s
        => => extracting sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251                                                  0.3s
        => => extracting sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c                                                  0.0s
        => => extracting sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358                                                  5.7s
        => [build 1/7] FROM docker.io/library/maven:3.6-jdk-11@sha256:1d29ccf46ef2a5e64f7de3d79a63f9bcffb4dc56be0ae3daed5ca5542b38aa2d            0.0s
        => [internal] load build context                                                                                                          0.1s
        => => transferring context: 13.99kB                                                                                                       0.1s
        => CACHED [build 2/7] WORKDIR /helidon                                                                                                    0.0s
        => [build 3/7] ADD pom.xml .                                                                                                              0.1s
        => [build 4/7] RUN mvn package -Dmaven.test.skip -Declipselink.weave.skip                                                                91.7s
        => [stage-1 2/4] WORKDIR /helidon                                                                                                         0.8s
        => [build 5/7] ADD src src                                                                                                                0.1s
        => [build 6/7] RUN mvn package -DskipTests                                                                                               10.8s
        => [build 7/7] RUN echo "done!"                                                                                                           0.5s
        => [stage-1 3/4] COPY --from=build /helidon/target/quickstart-mp-ankit.jar ./                                                                   0.1s
        => [stage-1 4/4] COPY --from=build /helidon/target/libs ./libs                                                                            0.1s
        => exporting to image                                                                                                                     0.1s
        => => exporting layers                                                                                                                    0.1s
        => => writing image sha256:587a079ad854fc79e768acda11fc05dd87d37013261249e778e80749798c2837                                               0.0s
        => => naming to lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0                                                                           0.0s
        
4.  그러면 로컬 저장소에서 확인할 수 있는 Docker 이미지가 생성됩니다.
    
        $ docker images
        
        REPOSITORY                                                                           TAG                               IMAGE ID       CREATED         SIZE
        lhr.ocir.io/tenancynamespace/quickstart-mp-ankit                                               1.0                               587a079ad854   5 minutes ago   243MB
        
    
    나중에 필요하므로 텍스트 파일에 대체된 전체 이미지 이름 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-ankit:1.0`를 복사합니다.
    

## 작업 2: Oracle Cloud Container Registry에 로그인하기 위한 인증 토큰 생성

이 단계에서는 Oracle Cloud Container Registry 로그인에 사용할 _인증 토큰_을 생성합니다.

1.  오른쪽 맨 위에 있는 사용자 아이콘을 선택한 다음 _내 프로파일_을 선택합니다.
    
    ![내 프로파일](images/my-profile.png)
    
2.  아래로 스크롤하여 _Auth Tokens_을 선택합니다.
    
    ![인증 토큰](images/auth-token.png)
    
3.  _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/generate-token.png)
    
4.  _`quickstart-mp-your_first_name`_을 복사하여 \[설명\] 상자에 붙여 넣고 _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/create-token.png)
    
5.  생성된 토큰에서 _복사_를 눌러 텍스트 편집기에 붙여넣습니다. 나중에 복사할 수 없으므로 이 토큰의 복사본이 저장되었는지 확인하십시오. 그런 다음 _닫기_를 누릅니다.
    
    ![토큰 복사](images/copy-token.png)
    

## 태스크 3: Helidon 애플리케이션(quickstart-mp) Docker 이미지를 컨테이너 레지스트리 저장소로 푸시

1.  이 연습의 작업 1에서 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)을 열고 영역 이름의 끝점을 확인하여 텍스트 편집기로 복사했습니다. 예제에서 Region Name은 UK South(런던)입니다. 이 태스크에 대해 이 정보가 필요합니다. ![엔드포인트](images/end-point.png)
    
2.  다음 명령을 복사하여 텍스트 편집기에 붙여 넣은 다음 `ENDPOINT_OF_REGION_NAME`를 해당 영역의 끝점으로 바꿉니다.
    
    > 예제에서 Region Name은 _UK South (London)_이고 끝점은 _lhr.ocir.io_입니다. 이 태스크에 대한 특정 정보가 필요합니다.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  이전 단계에서는 테넌시 네임스페이스도 확인했습니다. Username에 `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`을 입력합니다.  
    
    *   _`NAMESPACE_OF_YOUR_TENANCY`_를 테넌시의 네임스페이스로 바꿉니다.
    *   _`YOUR_ORACLE_CLOUD_USERNAME`_을 Oracle Cloud 계정 사용자 이름으로 바꾼 다음 텍스트 편집기에서 대체된 사용자 이름을 복사하여 _Cloud Shell_에 붙여 넣습니다.
    *   비밀번호의 경우 텍스트 편집기(또는 저장 위치)에서 인증 토큰을 복사하여 붙여 넣습니다.
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Container Registry로 다시 이동합니다. 콘솔에서 탐색 메뉴를 열고 **개발자 서비스**를 누릅니다. **컨테이너 및 아티팩트**에서 **컨테이너 레지스트리**를 누릅니다. ![컨테이너 레지스트리](images/container-registry.png)
    
5.  구획을 선택한 다음 **생성**을 누릅니다. ![저장소 생성](images/repository-create.png)
    
6.  구획을 선택하고 Repository Name으로 _`quickstart-mp-your_first_name`_를 입력한 다음 Access를 **Public**으로 선택하고 **Create Repository**를 누릅니다.
    
    ![저장소 설명](images/describe-repository.png)
    
7.  저장소 _`quickstart-mp-your_first_name`_를 만든 후 저장소 목록 및 해당 설정에서 확인할 수 있습니다.
    
    ![네임스페이스 확인](images/verify-namespace.png)
    
8.  Docker 이미지를 Oracle Cloud Container Registry 내의 저장소에 푸시하려면 텍스트 편집기에서 다음 명령을 복사하여 붙여 넣은 다음 이전에 저장한 `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`quickstar-mp-your_first_name`:1.0을 Docker 이미지 전체 이름으로 바꾸십시오.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0</copy>
        
    
    결과는 다음과 같습니다.
    
        $ docker push lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        The push refers to a repository [lhr.ocir.io/tenancynamespace/quickstart-mp-ankit]
        0795b8384c47: Pushed
        131452972f9d: Pushed
        93c53f2e9519: Pushed
        3b78b65a4be9: Pushed
        e1434e7d0308: Pushed
        17679d5f39bd: Pushed
        300b011056d9: Pushed
        1.0: digest: sha256:355fa56eab185535a58c5038186381b6d02fd8e0bcb534872107fc249f98256a size: 1786
        
9.  _docker push_ 명령이 성공적으로 실행된 후 _`quickstart-mp-your_first_name`_ 저장소를 확장하면 새 이미지가 이 저장소에 업로드되었음을 알 수 있습니다.
    
10.  _Cloud Shell_ 및 컨테이너 레지스트리 저장소 페이지를 열어 둔 채 다음 실습을 위해 필요합니다.
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
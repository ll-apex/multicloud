# 오브젝트 스토리지 지원을 추가하도록 Helidon 애플리케이션 수정

## 소개

이 실습의 목표는 Helidon 애플리케이션에서 **Object Storage 접근 권한을 추가하는 방법**을 시연하는 것입니다. 인사말을 저장하는 데 사용되는 변수를 새 인사말 컨테이너가 되어 오브젝트 스토리지 버킷에서 저장 및 **검색**되는 오브젝트로 바꾸면 됩니다. 객체가 지속되므로 마지막 인사말 값은 애플리케이션 재시작 후에도 유지됩니다. 이렇게 변경하지 않고 변수를 통해 메모리에 인사말을 사용하면 응용 프로그램을 다시 시작할 때 인사말이 기본값으로 재설정됩니다.

예상 시간: 15분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [오브젝트 스토리지 지원](videohub:1_p5v2wehm)

### 목표

이 실습에서는 다음을 수행합니다.

*   Object Storage와 같은 OCI 서비스와의 통합을 표시하도록 Helidon 애플리케이션을 수정합니다.
*   성공적인 오브젝트 스토리지 통합 확인

### 필요 조건

*   Oracle Free Tier(체험판), 유료 또는 LiveLabs 클라우드 계정

## 작업 1: 오브젝트 스토리지 통합에 대한 Helidon 애플리케이션 수정

1.  **oci.bucket.name**가 Lab 2/Task 3의 5단계에서 이미 설정되었어야 하는 **~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties**에 제대로 구성되었는지 확인합니다.
    
2.  **코드 편집기**에서 **~/oci-mp/server/** 아래의 파일 이름 _`pom.xml`_을 눌러 열고 아래와 같이 **종속성** 절 내에 **오브젝트 스토리지 OCI SDK** 종속성을 추가합니다.
    
        <copy><dependency>
                    <groupId>com.oracle.oci.sdk</groupId>
                    <artifactId>oci-java-sdk-objectstorage</artifactId>
        </dependency></copy>
        
    
    ![종속성 추가](images/add-dependency.png)
    
    > 들여쓰기를 제대로 유지해야 합니다.
    
3.  _~/oci-mp/server/src/main/java/ocw/hol/mp/oci/server/GreetingProvider.java_ 파일을 수정하여 **오브젝트 스토리지** 액세스를 추가합니다. 이 변경에 대한 소스 코드가 있습니다. 다음 코드를 복사하여 붙여넣어 필요한 변경사항으로 이 파일을 업데이트하십시오. 아래 _참고_에서 이 파일의 변경사항에 대해 설명합니다.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/source/GreetingProvider.java server/src/main/java/ocw/hol/mp/oci/server/</copy>
        
    
    ![객체 스토리지 추가됨](images/os-added.png)
    
    > **참고:-**
    
    *   생성자의 인수 섹션에서 _ObjectStorage objectStorageClient_ 매개변수를 추가했습니다. 이 매개변수는 _@Injected_ 주석의 일부이므로 Helidon에서 자동으로 처리되고 해당 용도로 여러 줄의 **OCI SDK** 코드를 추가하지 않고도 오브젝트 스토리지 서비스와 통신하는 데 사용할 수 있는 클라이언트를 포함하도록 설정됩니다.
    *   동일한 생성자의 인수 섹션에서 **ConfigProperty**를 추가하여 구성의 **oci.bucket.name** 등록 정보에서 값을 추출합니다. **`update_config_values.sh`**이라는 유틸리티 스크립트가 **`devops_helidon_to_instance_ocw_hol`** 저장소 디렉토리에서 실행되었을 때 초기 응용 프로그램 설정 중 이전에 **microprofile-config.properties**에 채워졌습니다.
    *   **getNamespace() Object Storage SDK** 메소드를 사용하여 나중에 객체를 검색하거나 저장하는 데 사용될 오브젝트 스토리지의 네임스페이스를 검색합니다.
    
        <copy>public GreetingProvider(@ConfigProperty(name = "app.greeting") String message,
                            ObjectStorage objectStorageClient,
                            @ConfigProperty(name = "oci.bucket.name") String bucketName) {
        try {
            this.bucketName = bucketName;
            GetNamespaceResponse namespaceResponse =
                    objectStorageClient.getNamespace(GetNamespaceRequest.builder().build());
            this.objectStorageClient = objectStorageClient;
            this.namespaceName = namespaceResponse.getValue();
            LOGGER.info("Object storage namespace: " + namespaceName);
            setMessage(message);
        } catch (Exception e) {
            LOGGER.warning("Error invoking getNamespace from Object Storage: " + e);
        }
        }     </copy>
        
    
    *   GreetingProvider 클래스 바로 아래에서 변수 선언을 제거했습니다.
    
        <copy>private final AtomicReference<String> message = new AtomicReference<>();</copy>
        
    
    *   이 변수를 아래와 같은 로컬 전역 변수로 바꿨습니다. _LOGGER_는 _objectStorageClient_, _namespaceName_, _bucketName_ 및 _objectName_가 _Object Storage SDK_ 호출에 사용되는 동안 로깅에 사용됩니다.
    
        <copy>private static final Logger LOGGER = Logger.getLogger(GreetingProvider.class.getName());
        private ObjectStorage objectStorageClient;
        private String namespaceName;
        private String bucketName;
        private final String objectName = "hello.txt";</copy>
        
    
    *   _getMessage()_의 내용을 아래 코드로 바꿨습니다. 그러면 **getObject() SDK** 메소드를 사용하여 버킷에서 패치(fetch)된 **hello.txt** 객체에서 인사말을 검색합니다.
    
        <copy>try {
        GetObjectResponse getResponse =
                objectStorageClient.getObject(
                        GetObjectRequest.builder()
                                .namespaceName(namespaceName)
                                .bucketName(bucketName)
                                .objectName(objectName)
                                .build());
        return new String(getResponse.getInputStream().readAllBytes());
        } catch (Exception e) {
            LOGGER.warning("Error invoking getObject from Object Storage: " + e);
            return "Hello-Error";
        }</copy>
        
    
    *   void **setMessage(String message)** 메소드의 내용을 아래 코드로 바꿨습니다. 이렇게 하면 **putObject() SDK** 메소드를 사용하여 인사말이 포함된 **hello.txt** 객체를 버킷에 저장합니다.
    
        <copy>try {
        byte[] contents = message.getBytes();
        PutObjectRequest putObjectRequest =
                PutObjectRequest.builder()
                        .namespaceName(namespaceName)
                        .bucketName(bucketName)
                        .objectName(objectName)
                        .putObjectBody(new ByteArrayInputStream(message.getBytes()))
                        .contentLength(Long.valueOf(contents.length))
                        .build();
        objectStorageClient.putObject(putObjectRequest);
        } catch (Exception e) {
            LOGGER.warning("Error invoking putObject from Object Storage: " + e);
        }</copy>
        
    
    *   추가된 모든 새 코드를 지원하기 위해 임포트(**import java.util.concurrent.atomic.AtomicReference;**)를 제거하고 새 임포트로 바꿨습니다.
    
        <copy>import java.io.ByteArrayInputStream;
        import java.util.logging.Logger;
        
        import com.oracle.bmc.objectstorage.ObjectStorage;
        import com.oracle.bmc.objectstorage.requests.GetNamespaceRequest;
        import com.oracle.bmc.objectstorage.requests.GetObjectRequest;
        import com.oracle.bmc.objectstorage.requests.PutObjectRequest;
        import com.oracle.bmc.objectstorage.responses.GetNamespaceResponse;
        import com.oracle.bmc.objectstorage.responses.GetObjectResponse;</copy>
        

## 작업 2: Helidon 애플리케이션 코드 변경을 푸시하고 DevOps 파이프라인을 트리거합니다.

1.  **변경 사항을 커밋하고 푸시**하려면 터미널에서 다음 명령을 복사하여 붙여 넣습니다.
    
        <copy>git add .
        git status
        git commit -m "Add support Object Storage integration"
        git push</copy>
        
    
    > 이 Git 푸시로 파이프라인이 트리거됩니다.
    
2.  빌드 및 배치 파이프라인 로그를 모니터하여 **DevOps 수명 주기**가 완료될 때까지 기다립니다. ![배치 실행](images/deployment-run.png)
    

## 작업 3: 성공적인 오브젝트 스토리지 통합 확인

curl을 사용하여 테스트하고 새 **hello.txt** 객체가 **bucket**에 추가되었는지 확인합니다. 객체의 크기가 인사말의 크기와 동일한지 검증합니다. 예를 들어 인사말이 _Hello_인 경우 크기는 **5**여야 합니다. 인사말이 _Hola_인 경우 크기는 **4**여야 합니다.

1.  배치 노드 **`PUBLIC_IP`**를 환경 변수로 설정합니다.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  다음 명령을 복사하여 붙여넣어 **GET** 요청을 배치합니다. 아래와 유사한 출력이 나타납니다.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  표시된 대로 **Cloud Console**에서 _Hamburger 메뉴_ -> _Storage_ -> _Buckets_를 누릅니다. ![버킷 메뉴](images/bucket-menu.png)
    
4.  올바른 구획을 선택하고 **app-bucket-helidonocw-hol-string**을 눌러 **bucket**을 엽니다. ![구획 선택](images/select-compartment.png)
    
5.  인사말이 **Hello**이므로 이제 버킷에 **hello.txt** 객체가 포함되고 크기가 **5바이트**인지 확인합니다. 객체를 다운로드하고 콘텐츠가 실제로 _안녕하세요_인지 확인할 수도 있습니다. ![크기 확인](images/verify-size.png)
    
    > 다음 단계에서 인사말을 변경하면 이 페이지를 새로 고치므로 이 페이지를 열어 둡니다.
    
6.  다음 명령을 복사하여 붙여넣어 **Hello** 인사말을 **Hola**로 바꿉니다.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
7.  인사말이 **Hola**로 대체되므로 버킷의 **hello.txt** 객체 크기가 **4바이트**인지 확인합니다. 객체를 다운로드하고 콘텐츠가 **Hola**로 변경되었는지 확인할 수도 있습니다. ![hola 크기](images/hola-size.png)
    
8.  **restart.sh** 툴을 사용하여 인사말의 값이 **오브젝트 스토리지**에서 지속됨에 따라 유지된다는 것을 보여주기 위해 애플리케이션을 재시작합니다.
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/restart.sh</copy>
        Created private.key and can be used to ssh to the deployment instance by running this command: "ssh -i private.key opc@xx.xx.xx.xx"
        FIPS mode initialized
        The authenticity of host 'xx.xx.xx.xx (xx.xx.xx.xx)' can't be established.
        ECDSA key fingerprint is SHA256:hJl8axCNhFcILDo+AwxMkodxhY+UxRD40d1ans83GTg.
        ECDSA key fingerprint is SHA1:IBUhyn05DaIs60GAQsruVXajhym.
        Are you sure you want to continue connecting (yes/no)? yes
        Warning: Permanently added 'xx.xx.xx.xx' (ECDSA) to the list of known hosts.
        Starting oci-mp-server.jar
        Helidon app is now running with pid 264792!
        Cleaning up ssh private.key
        
    
    ![애플리케이션 재시작](images/restart-application.png)
    
    > **Are you sure you want to continue connections (yes/no)?**를 묻는 메시지가 표시되면 _yes_를 입력합니다.
    
9.  기본 Hello World 요청을 호출하고 **채팅 단어가 여전히 Hola임을 관찰합니다**.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
10.  **Cloud Console**에서 Bucket 페이지를 새로 고치면 인사말이 여전히 **Hola**임을 확인하는 크기가 여전히 **4**임을 알게 됩니다. ![지속성 확인](images/verify-persistence.png)
    

**축하합니다!** 워크샵이 완료되었습니다. 이 워크샵 중 생성된 **모든 리소스를 삭제**하는 **Lab 6**을 진행할 수 있습니다.

## 자세히 알아보기

*   [Helidon OCI 통합](https://helidon.io/docs/v3/#/mp/integrations/oci)

## 확인

*   **작성자** - Keith Lustria
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
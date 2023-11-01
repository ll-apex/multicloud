# Oracle Kubernetes 클러스터(OKE)에 WebLogic Operator 배포

## 소개

이 실습에서는 _.OCI/config_ 파일을 만드는 브라우저를 사용하여 OCI CLI를 인증합니다. kubectl을 사용하여 _Local Access_를 통해 원격으로 클러스터를 관리할 것입니다. _kubeconfig_ 파일이 필요합니다. 이 kubeconfig 파일은 OCI CLI를 사용하여 생성됩니다. 그런 다음 WebLogic Kubernetes 툴킷 UI에서 Kubernetes 클러스터에 대한 연결을 확인합니다. 마지막으로 Kubernetes 클러스터(OKE)에 WebLogic Kubernetes Operator를 설치합니다.

예상 시간: 10분

### 목표

이 실습에서는 다음을 수행합니다.

*   Kubernetes 클러스터에 접속하도록 kubectl(Kubernetes Cluster CLI)을 구성합니다.
*   WebLogic Kubernetes 툴킷 UI와 Kubernetes 클러스터의 연결을 확인합니다.
*   Kubernetes 클러스터에 WebLogic Kubernetes Operator를 설치합니다.

## 작업 1: Oracle Kubernetes 클러스터에 연결하도록 kubectl(Kubernetes 클러스터 CLI) 구성

이 작업에서는 _/home/opc_ 디렉토리에 구성 파일 _.oci/config_ 및 _.kube/config_를 만듭니다. 이 구성 파일을 사용하면 이 가상 머신에서 Oracle Kubernetes 클러스터(OKE)에 액세스할 수 있습니다.

1.  Firefox에서 클라우드 콘솔을 열고 표시된 것처럼 **햄버거 메뉴** -> **개발자 서비스** -> **Kubernetes 클러스터(OKE)**를 선택합니다. ![OKE 아이콘](images/oke-icon.png)
    
2.  lab 3에서 생성한 클러스터 이름을 누른 다음 **Access Cluster**를 누릅니다. ![클러스터에 액세스](images/access-cluster.png)
    
3.  **로컬 액세스**를 선택한 다음 표시된 대로 **복사**를 누릅니다. ![로컬 액세스](images/local-access.png)
    
4.  **활동**을 누르고 **터미널**을 선택합니다. ![최종](images/click-terminal.png)
    
5.  복사한 명령을 터미널에 붙여넣습니다. **Do you want to create a new config file?**에서 **y**를 입력한 다음 **Enter**를 누릅니다. **브라우저를 통해 로그인하여 구성 파일을 생성하겠습니까?**의 경우 **y**를 입력한 다음 **Enter** 키를 누릅니다. ![OCI 구성](images/oci-config.png)
    
6.  Firefox 브라우저의 활성 세션을 클릭합니다.
    
    > 그림과 같이 _승인 완료_가 표시됩니다. ![승인 완료](images/authorization-complete.png)
    
7.  **Enter a passphrase for your private key**에서 비워 두고 **Enter** 키를 누릅니다. ![빈 비밀번호 구문](images/empty-passphrase.png)
    
8.  위쪽 화살표 키를 사용하여 **oce ce ...** 명령을 다시 실행하고 **New config written to the Kubeconfig file /home/opc/.kube/config**이 표시될 때까지 여러 번 다시 실행합니다. ![KubeConfig 생성](images/create-kubeconfig.png)
    
9.  다음 명령을 복사하여 붙여넣어 **~/.kube/config** 파일 권한을 변경합니다.
    
        <copy>chmod 600 ~/.kube/config</copy>
        

## 작업 2: OKE 스택용 Oracle WebLogic Server의 클라우드 리소스 보기

1.  OCI 콘솔에서 **탐색 메뉴**를 누르고 **개발자 서비스**를 선택합니다. 리소스 관리자 그룹에서 **스택**을 누릅니다.
    
2.  스택이 포함된 **구획**을 선택합니다.
    
3.  스택의 이름을 누릅니다.
    
4.  **애플리케이션 정보** 탭으로 이동하여 스택에서 생성된 리소스를 볼 수 있습니다. **배스천 인스턴스 공용 IP**를 복사하여 텍스트 파일에 붙여넣습니다. ![배스천 복사](images/copy-bastion.png)
    

## 작업 3: 터미널에서 프록시 구성 사용

이 작업에서는 원격 데스크톱에서 Oracle Kubernetes Cluster 노드로 **ssh 터널링**을 사용으로 설정합니다.

1.  터미널을 열고 다음 명령을 복사하여 붙여 넣어 원격 데스크탑에 개인 키 파일을 만듭니다.
    
        <copy>vi ~/.ssh/opc.key</copy>
        
2.  Press **i** to enter in insert mode, and paste the content of private key in this file and press escape and then enter **:wq** to save this file.
    
3.  다음 명령을 복사하여 붙여넣어 개인 키 파일 권한을 변경합니다.
    
        <copy>chmod 600 ~/.ssh/opc.key</copy>
        
4.  다음 명령을 복사하여 터미널에 붙여 넣은 다음 **bastion-public-ip**를 마지막 작업에서 복사한 공용 IP로 바꿉니다.
    
        <copy>ssh -D 1088 -fCqN -i ~/.ssh/opc.key opc@bastion-public-ip</copy>
        
5.  **~/.kube/config** 파일을 열고 아래와 같이 이 파일에 다음을 복사하여 붙여 넣습니다.
    
        <copy>proxy-url: socks5://localhost:1088</copy>
        
    
    ![프록시 설정](images/proxy-config.png)
    

## 작업 4: WebLogic Kubernetes 툴킷 UI와 Oracle Kubernetes 클러스터의 연결성 확인

이 작업에서는 `WebLogic Kubernetes Toolkit UI` 애플리케이션에서 _Oracle Kubernetes Cluster(OKE)_에 대한 연결을 확인합니다.

1.  WebLogic Kubernetes 툴 키트 UI로 돌아가서 _활동_을 누르고 WebLogic Kubernetes 툴 키트 UI 창을 선택합니다.
    
2.  _Kubernetes_ -> _클라이언트 구성_을 누른 다음 _접속 확인_을 누릅니다. ![접속 확인](images/verify-connectivity.png)
    
3.  _Verify Kubernetes Client Connectivity Success_ 창이 나타나면 _Ok_를 누릅니다. ![성공적으로 연결됨](images/successfully-connected.png)
    

## 작업 5: WebLogic Kubernetes 연산자를 Oracle Kubernetes 클러스터로 업데이트

이 절에서는 대상 Kubernetes 클러스터에 WebLogic Kubernetes 연산자("연산자")를 설치하기 위한 지원을 제공합니다.

1.  **WebLogic Operator**를 누릅니다. 다음 구성 세부정보를 지정하고 **운영자 업데이트**를 누릅니다.
    
    **Kubernetes 네임스페이스** - 연산자를 설치할 Kubernetes 네임스페이스입니다. 아래 스크린샷에 표시된 대로 **wko-operator-ns** 값을 입력합니다.
    
    **Kubernetes 서비스 계정** - 운영자가 Kubernetes API 요청을 생성할 때 사용할 Kubernetes 서비스 계정입니다. 아래 스크린샷에 표시된 대로 **wko-operator-sa** 값을 입력합니다.
    
    **운영자 설치에 사용할 Helm 릴리스 이름** - 이 설치를 식별하는 데 사용할 Helm 릴리스 이름입니다. 아래 스크린샷에 표시된 대로 **wko-weblogic-operator** 값을 입력합니다.
    
    ![WebLogic 피연산자](images/weblogic-operator.png)
    
    > **참고용:**  
    > ! 기본적으로 운영자의 _사용할 이미지 태그_ 필드는 GitHub 컨테이너 레지스트리의 최신 운영자 릴리스 버전에 해당하는 이미지 태그로 설정됩니다.  
    > 운영자는 관리할 Kubernetes 클러스터의 WebLogic 도메인을 알아야 합니다. Kubernetes 네임스페이스 레벨에서 이 작업을 수행하므로 운영자가 관리하도록 구성된 Kubernetes 네임스페이스의 WebLogic 도메인은 설치 중인 운영자 인스턴스에 의해 관리됩니다.  
    > _Kubernetes 네임스페이스 선택 전략_ 필드의 경우 _레이블 선택기_를 선택합니다. 즉, _weblogic-operator=enabled_ 레이블이 있는 모든 Kubernetes 네임스페이스가 이 연산자에 의해 관리됩니다.  
    > ![연산자 이미지](images/operator-image.png)
    
    > Enable Cluster Role Binding(클러스터 롤 바인딩 사용)을 사용으로 설정하면 운영자 설치에서 운영자가 모든 관리 네임스페이스에 사용할 Kubernetes ClusterRole 및 ClusterRoleBinding를 생성합니다.  
    > 기본적으로 운영자의 REST API는 Kubernetes 클러스터 외부에 노출되지 않습니다. REST API가 노출되도록 사용으로 설정하려면 _외부적으로 REST API 노출_을 사용으로 설정할 수 있습니다. ![롤 바인딩](images/role-binding.png)  
    
    > 이 창에서는 운영자의 Java 로깅 구성을 대체할 수 있습니다. 이 구성은 운영자와의 문제를 디버깅할 때 유용할 수 있습니다.  
    > ![Java 로깅](images/java-logging.png)  
    
    > _WebLogic Kubernetes Operator Image_, _Kubernetes Namespace Selection Strategy_, _WebLogic Kubernetes Role Bindings_, _External REST API Access_, _Third Party Integrations_ 및 _Java Logging_에 대한 자세한 내용은 [WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/) 설명서를 참조하십시오.
    
2.  _WebLogic Kubernetes Operator Installation Complete_가 표시되면 _Ok_를 누릅니다. ![운영자 설치됨](images/operator-installed.png)
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 10월
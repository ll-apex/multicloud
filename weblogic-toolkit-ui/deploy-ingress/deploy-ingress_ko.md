# Oracle Kubernetes 클러스터에 수신 컨트롤러 배치(OKE)

## 소개

이 실습에서는 _Traefik_ 수신 컨트롤러를 설치합니다. 나중에 애플리케이션 및 관리 서버에 액세스하도록 _수신 경로_를 업데이트합니다.

예상 시간: 10분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [OKE 클러스터에 수신 컨트롤러 배치](videohub:1_4kih00fi)

### 목표

이 실습에서는 다음을 수행합니다.

*   Kubernetes 클러스터에 수신 컨트롤러를 설치합니다.
*   수신 경로를 업데이트합니다.

## 작업 1: Oracle Kubernetes 클러스터에 수신 컨트롤러 설치

이 작업에서는 _Ingress Controller_를 설치합니다.

1.  _Ingress Controller_를 누릅니다. 미리 채워진 일부 값을 확인하고 동일하게 유지한 다음 _Install Ingress Controller_를 누릅니다. ![수신 컨트롤러 설치](images/install-ingress-controller.png)
    
    > **참고용:**  
    > _traefik-operator_ 수신 컨트롤러를 Kubernetes 네임스페이스 _traefik-ns_에 성공적으로 설치했습니다.
    
2.  _Ingress Controller Installation Complete_가 표시되면 _Ok_를 누릅니다. ![수신 컨트롤러 설치됨](images/ingress-controller-installed.png)
    

## 작업 2: 응용 프로그램에 액세스하도록 수신 경로 업데이트

이 작업에서는 관리 콘솔, 응용 프로그램에 액세스하기 위한 수신 경로를 추가합니다. 마지막으로 액세스 가능한 엔드포인트를 얻습니다.

1.  아래로 스크롤하고 _+_ icone을 눌러 _Ingress Route Configuration_을 추가합니다. ![수신 경로 추가](images/add-ingress-routes.png)
    
2.  표시된 대로 Edit 아이콘을 눌러 값을 수정합니다. ![수신 편집](images/edit-ingress.png)
    
3.  다음 세부정보를 입력하고 _확인_을 누릅니다.  
    이름: console  
    경로 표현식: /console  
    대상 서비스 이름 공간: test-domain-ns  
    대상 서비스: test-domain-admin-server  
    대상 포트: 7001  
    
    ![콘솔 수신](images/console-ingress.png)
    
4.  유사한 방법으로 다음 _opdemo_ 수신 경로도 추가합니다.  
    Name: opdemo  
    Path Expression: /opdemo  
    Target Service Namespace: test-domain-ns  
    Target Service: test-domain-cluster-cluster-1  
    Target Port: 8001  
    ![Opdemo 수신](images/opdemo-ingress.png)
    
5.  유사한 방법으로 다음 _remote-console_ 수신 경로도 추가합니다.  
    이름: remote-console  
    경로 표현식: /  
    대상 서비스 이름 공간: test-domain-ns  
    대상 서비스: test-domain-admin-server  
    대상 포트: 7001  
    ![원격 콘솔 수신](images/remote-console-ingress.png)
    
6.  수신 경로를 업데이트하려면 _Update Ingress Routes_를 누릅니다. ![수신 경로 업데이트](images/update-ingress-routes.png)
    
7.  _기존 수신 경로 업데이트_에서 _예_를 누릅니다.
    
8.  _Ingress Routes Update Complete_ 창이 표시되면 _Ok_를 누릅니다. ![수신 업데이트 완료](images/update-ingress-complete.png)
    
    > 이 IP를 지적하고 텍스트 파일에 저장해야 합니다.
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
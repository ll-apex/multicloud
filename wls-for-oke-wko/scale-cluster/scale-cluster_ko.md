# WebLogic 클러스터 확장(선택 사항)

## 소개

이 실습에서는 WebLogic 클러스터를 확장합니다. 여기서 값을 _3_으로 수정하고 도메인을 다시 배치합니다.

추정 시간: 5분

### 목표

이 실습에서는 다음을 수행합니다.

*   WebLogic 클러스터의 배율을 조정합니다.

## 작업 1: WebLogic Kubernetes 툴킷 UI를 사용하여 WebLogic 클러스터 확장

이 작업에서는 _Replica_ 값을 2에서 3으로 수정하고 도메인을 다시 배치해야 합니다.

1.  WebLogic Kubernetes Toolkit UI로 돌아가서 _WebLogic Domain_을 누릅니다. _클러스터_ 섹션으로 이동하고 _편집_ 아이콘을 누릅니다.  
    ![클러스터 크기 재설정](images/cluster-resize.png)
    
2.  복제본을 _2_에서 _3_으로 변경하고 _OK_를 누릅니다. ![복제본 변경](images/change-replicas.png)
    
3.  도메인을 다시 배치하려면 _Deploy Domain(도메인 배치)_을 누릅니다. ![도메인 재배치](images/redeploy-domain.png)
    
4.  _WebLogic Domain Deployment to Kubernetes Complete_ 창이 표시되면 _Ok_를 누릅니다. ![배포 완료](images/deployment-complete.png)
    
5.  _터미널_ 창으로 돌아가서 _작업_을 누르고 _터미널_ 창을 선택합니다. 다음 명령을 복사하여 터미널에 붙여 넣습니다.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![크기 조정 보기](images/view-scaling.png)
    
    > 도메인을 다시 배치하면 검사기 작업이 시작되어 test-domain-managed-server3에 대한 Pod 생성 프로세스가 시작되고, 이 Pod가 _실행 중_ 상태가 됩니다.
    
6.  응용 프로그램 페이지가 열려 있는 브라우저로 돌아갑니다. Refresh 버튼을 누르면 이제 세 개의 관리 서버 간의 로드 밸런싱이 표시됩니다. ![새 서버](images/new-server.png)
    
7.  WebLogic Remote Console로 돌아가서 _Monitoring Tree_ -> _Environment_ -> _Servers_를 누릅니다. 여기에 server3도 있습니다. ![원격 콘솔](images/remote-console.png)
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 10월
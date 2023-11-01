# Oracle Kubernetes 클러스터(OKE)에 WebLogic 도메인 배치

## 소개

이 실습에서는 WebLogic 도메인을 Kubernetes 클러스터에 배포합니다. Primary Image 섹션에서는 oracle 계정 인증서를 지정합니다. 보조 이미지 섹션에서는 Oracle Cloud 계정 인증서를 지정합니다. 여기서는 클러스터에 대한 복제본도 지정합니다.

예상 시간: 10분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [OKE 클러스터에 WebLogic 도메인 배치](videohub:1_wz94de1l)

### 목표

이 실습에서는 다음을 수행합니다.

*   Kubernetes 클러스터에 WebLogic 도메인을 배치합니다.

## 작업 1: Oracle Kubernetes 클러스터에 WebLogic 도메인 배치

이 작업에서는 WebLogic 도메인에 대한 Kubernetes 사용자정의 리소스를 Kubernetes 클러스터에 배포합니다.

1.  아래로 스크롤하여 Primary Image 섹션에 다음을 입력하고 _domain-secret_를 _Image Pull Secret Name_으로 입력하고 _Image Registry Pull Username_ 및 _Image Registry Pull Password_에 Oracle 계정 사용자 이름 및 암호를 사용합니다. _이미지 레지스트리 Pull 전자메일 주소_에 Oracle 전자메일 ID를 입력합니다. Oracle Container Registry에서 _weblogic_ 이미지에 대한 라이센스를 수락하는 데 사용한 것과 동일한 인증서입니다. ![기본 이미지 세부정보](images/primary-image-details.png)
    
    > **참고용:**  
    > Oracle Container Registry에서 이미지를 추출하고 있으므로 WebLogic Server Images의 사용권 계약에 동의하는 데 사용된 인증서를 지정하고 있습니다.
    
2.  아래로 스크롤하여 보조 이미지 섹션에서 **Specify Auxiliary Image Pull Credentials**의 확인란을 선택 취소합니다.
    
3.  _클러스터_ 섹션에서 표시된 대로 _편집_ 아이콘을 누릅니다. ![클러스터 크기 재설정](images/cluster-resize.png)
    
4.  _2_를 _Replicas(복제본)_로 입력한 다음 _OK(확인)_를 누릅니다. 복제본 크기는 WebLogic 도메인을 Kubernetes 클러스터에 성공적으로 배치한 후 _Running_ 상태의 관리 서버 수를 결정합니다. ![클러스터 복제본](images/cluster-replicas.png)
    
5.  데이터 소스 섹션에서 두 데이터 소스에 대한 _비밀번호_를 편집하려면 두 번 누릅니다. 두 데이터 소스에서 _tiger_를 암호로 지정할 수 있습니다. 완료되면 _Deploy Domain(도메인 배치)_을 누릅니다. ![데이터 저장소 비밀번호](images/datasource-password.png)
    
    > 이렇게 하면 WebLogic 도메인 테스트 도메인이 Kubernetes 네임스페이스 _test-domain-ns_에 배포됩니다.
    
6.  _WebLogic Domain Deployment to Kubernetes Complete_ 창이 표시되면 _OK_를 누릅니다. ![배포 완료](images/deployment-complete.png)
    
7.  터미널로 돌아가서 _작업_을 누르고 _터미널_ 창을 선택합니다. 다음 명령을 복사하여 터미널을 붙여 넣습니다. 유사한 출력이 표시되어야 합니다. 여기서 introspector용 Pod는 먼저 관리 서버에 대해 실행되고 관리 서버에 대한 이후 Pod는 _Running_ 상태로 전환됩니다.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Pod 상태](images/pod-status.png)
    
8.  _WebLogic Kubernetes 툴킷 UI_를 통해 도메인 상태를 확인할 수도 있습니다. _WebLogic Kubernetes Toolkit UI_로 돌아가서 _도메인 상태 가져오기_를 누릅니다. ![도메인 상태](images/domain-status.png)
    
9.  Domain Status(도메인 상태) 창에서 아래로 스크롤하여 모든 서버 Pod의 상태를 확인한 다음 _OK(확인)_를 누릅니다. ![서버 상태](images/server-status.png)
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
# 단순화된 솔루션을 통한 ALTERNATE VERSION

## 소개

이 실습에서는 수정된 bobbys-helidon-stock-application 이미지를 사용합니다. 이 이미지는 Oracle Cloud Container Registry 내의 공용 저장소에 저장됩니다. 수정된 bobbys-helidon-stock-application 이미지를 가리키는 수정된 bobs-books-comp-mod.yaml 파일도 사용합니다.

### 목표

이 실습에서는 다음을 수행합니다.

*   수정된 bobs-books-comp-mod.yaml 파일을 다운로드합니다.
*   kubectl을 사용하여 변경 사항 적용

### 필요 조건

*   Oracle Cloud Infrastructure에 OKE 클러스터를 생성하는 Lab 1을 실행합니다.
*   Kubernetes 클러스터에 Verrazzano를 설치하는 Lab 2를 실행합니다.
*   Bobs-Books 애플리케이션을 배포하는 Lab 3을 실행합니다.
*   환경에 따라 명령 및 URL을 붙여넣고 수정할 수 있는 텍스트 편집기가 있어야 합니다. 그런 다음 _Cloud Shell_에서 실행하기 위해 수정된 명령을 복사하여 붙여넣을 수 있습니다.

## 작업 1:수정된 bobs-books-comp-mod.yaml 파일 다운로드

1.  다음 명령을 실행하여 수정된 bobs-books-comp-mod.yaml 파일을 다운로드합니다.
    
        <copy>curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/alternate-version/bobs-books-comp-mod.yaml >~/bobs-books-comp-mod.yaml</copy>
        
    
    ![파일 다운로드](images/downloadfile.png " ")
    

## 작업 2: kubectl을 사용하여 변경 사항 적용

1.  변경사항을 적용하려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣습니다. 변경사항을 적용하면 새 구성요소에 대한 요청을 제공하기 위해 새 Pod가 초기화되고, 이전 구성요소와 연관된 Pod는 계속 요청을 처리합니다. 나중에 새 POD가 _Running_ 상태에 도달하면 이전 POD가 _Terminated_ 상태로 전환됩니다. 결국 새 POD만 _실행 중_ 상태가 됩니다.
    
        <copy>kubectl apply -f ~/bobs-books-comp-mod.yaml -n bobs-books</copy>
        
    
    ![변경사항 적용](images/applychanges.png " ")
    
    출력에서 확인할 수 있습니다. _component.core.oam.dev/bobby-helidon_만 구성되고 다른 구성 요소는 변경되지 않습니다.
    
2.  새 POD가 초기화되는 방식과 이전 POD가 _Terminating_ 상태로 전환되는 방식을 보려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣으십시오.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    다음과 유사한 출력이 표시됩니다.
    
        vera_zano@cloudshell:~ (us-ashburn-1)$ kubectl get pods -n bobs-books
        NAME                                               READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                                 2/2    Running  0         130m
        bobbys-front-end-adminserver                       4/4    Running  0         127m
        bobbys-front-end-managed-server1                   4/4    Running  0         126m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  0/2    PodInitializing  0         10s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Running  0         130m
        bobs-bookstore-adminserver                         4/4    Running  0         127m
        bobs-bookstore-managed-server1                     4/4    Running  0         126m
        mysql-65d864bf8c-xf64p                             2/2    Running  0         130m
        robert-helidon-bfdfb58b8-58qfs                     2/2    Running  0         130m
        robert-helidon-bfdfb58b8-lkw8m                     2/2    Running  0         130m
        roberts-coherence-0                                2/2    Running  0         130m
        roberts-coherence-1                                2/2    Running  0         130m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  1/2    Running  0         28s
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  2/2    Running  0         34s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        vera_zano@cloudshell:~ (us-ashburn-1)$
        
    
    모든 POD가 _Running_ 상태임을 확인한 후 _CTRL + C_를 눌러 이 명령을 강제 종료합니다.
    

_Cloud Shell_은 마지막 실습에도 필요할 때 열어 둡니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 2월
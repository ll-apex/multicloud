# Bobby의 장부 구성 YAML 파일 수정

## 소개

실습 6에서는 _bobby-helidon-stock-application_에 대해 업데이트된 Docker 이미지를 푸시했습니다. 이제 Verrazzano가 서비스에 영향을 주지 않고 업데이트된 애플리케이션 및 구성요소를 재배치하기를 원합니다. 이를 위해서는 Verrazzano가 새 이미지를 선택하고 포드를 시작하도록 YAML 파일을 구성해야 합니다. Pod가 _Running_ 상태이면 이전 애플리케이션 및 구성요소와 연관된 Pod를 종료합니다.

예상 시간: 10분

### 목표

이 실습에서는 다음을 수행합니다.

*   `bobs-books-comp.yaml` 파일을 수정합니다.
*   `kubectl`를 사용하여 변경 사항을 적용합니다.

### 필요 조건

환경에 따라 명령 및 URL을 붙여넣고 수정할 수 있는 텍스트 편집기가 있어야 합니다. 그런 다음 _Cloud Shell_에서 실행하기 위해 수정된 명령을 복사하여 붙여넣을 수 있습니다.

## 작업 1: bobs-books-comp.yaml 파일 수정

1.  애플리케이션 구성 파일 _bobs-books-comp.yaml_가 있습니다. Lab 2에서는 애플리케이션 YAML 파일을 다운로드했습니다. yaml 파일이 포함된 홈 디렉토리로 변경하려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣습니다.
    
        <copy>cd ~</copy>
        
2.  이 위치에는 bobs-books 응용 프로그램에 대한 구성 파일이 있습니다. Lab 5의 일환으로 bobbys-helidon-stock-application을 수정하고 해당 구성 요소에 대한 새로운 Docker 이미지를 구축했습니다. 실습 6에서는 해당 Docker 이미지를 Oracle Cloud Container Registry 저장소에 푸시했습니다. 이제 이 실습에서는 _bobs-books-comp.yaml_ 파일을 수정하여 Oracle Cloud Container Registry 저장소의 새 업데이트된 Docker 이미지를 가져오도록 합니다. _bobs-books-comp.yaml_ 파일을 수정하려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣습니다.
    
        <copy>vi bobs-books-comp.yaml</copy>
        
    
    ![파일 열기](images/openfile.png " ")
    
3.  Lab 5의 일부로 Docker 이미지 전체 이름을 저장했습니다. 다음 행을 복사하여 텍스트 편집기에 붙여 넣어야 합니다. 그런 다음 `docker image full name`을 Docker 이미지 이름으로 바꾸어야 합니다. 그런 다음 수정된 행을 복사하고 _i_ 키를 눌러 `*bobs-books-comp.yaml*` 파일에 텍스트를 삽입합니다. 행 번호 148에 출력을 붙여 넣고(들여쓰기를 유지해야 함) 다음 이미지에 표시된 것처럼 _#_을 사용하여 종료 행 번호 147을 주석 처리한 다음 _Esc_를 누르고 _:wq_를 입력하여 파일을 저장합니다.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![선 삽입](images/insert-line.png " ")
    

## 작업 2: `kubectl`를 사용하여 변경 사항 적용

1.  변경사항을 적용하려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣습니다. 변경사항을 적용하면 새 구성요소에 대한 요청을 제공하기 위해 새 Pod가 초기화되고, 이전 구성요소와 연관된 Pod는 계속 요청을 처리합니다. 나중에 새 POD가 _Running_ 상태에 도달하면 이전 POD가 _Terminated_ 상태로 전환됩니다. 결국 새 POD만 _실행 중_ 상태가 됩니다.
    
        <copy>kubectl apply -f bobs-books-comp.yaml -n bobs-books</copy>
        
    
    다음과 같은 결과가 표시됩니다.
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh unchanged
        component.core.oam.dev/robert-helidon unchanged
        component.core.oam.dev/bobby-coh unchanged
        component.core.oam.dev/bobby-helidon configured
        component.core.oam.dev/bobby-wls unchanged
        component.core.oam.dev/bobs-mysql-configmap unchanged
        component.core.oam.dev/bobs-mysql-service unchanged
        component.core.oam.dev/bobs-mysql-deployment unchanged
        component.core.oam.dev/bobs-orders-configmap unchanged
        component.core.oam.dev/bobs-orders-wls unchanged
        $
        
    
    출력에서 확인할 수 있습니다. _component.core.oam.dev/bobby-helidon_만 구성되고 다른 구성 요소는 변경되지 않습니다.
    
2.  새 POD가 초기화되는 방식과 이전 POD가 _Terminating_ 상태로 전환되는 방식을 보려면 다음 명령을 복사하여 _Cloud Shell_에 붙여넣으십시오.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    다음과 유사한 출력이 표시됩니다.
    
        $ kubectl get pods -n bobs-books
        NAME                                         READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                           2/2    Running      0         130m
        bobbys-front-end-adminserver                 4/4    Running      0         127m
        bobbys-front-end-managed-server1             4/4    Running      0         126m
        bobbys-helidon-stock-application-64fb55-zzp  0/2    PodInitializing  0     10s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Running      0         130m
        bobs-bookstore-adminserver                   4/4    Running      0         127m
        bobs-bookstore-managed-server1               4/4    Running      0         126m
        mysql-65d864bf8c-xf64p                       2/2    Running      0         130m
        robert-helidon-bfdfb58b8-58qfs               2/2    Running      0         130m
        robert-helidon-bfdfb58b8-lkw8m               2/2    Running      0         130m
        roberts-coherence-0                          2/2    Running      0         130m
        roberts-coherence-1                          2/2    Running      0         130m
        bobbys-helidon-stock-application-64fb55-zzp  1/2    Running      0         28s
        bobbys-helidon-stock-application-64fb55-zzp  2/2    Running      0         34s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        $
        
    
    모든 POD가 _Running_ 상태임을 확인한 후 _CTRL + C_를 눌러 이 명령을 강제 종료합니다.
    
    _Cloud Shell_은 마지막 실습에도 필요할 때 열어 둡니다.
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
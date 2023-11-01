# 보조 이미지를 생성하여 Oracle Container Image Registry로 푸시

## 소개

**기본 이미지** - Oracle Fusion Middleware 소프트웨어가 포함된 이미지입니다. 도메인에 대해 WebLogic 서버를 실행하는 모든 컨테이너의 기반으로 사용됩니다.

**보조 이미지** - WebLogic 배치 도구 소프트웨어 및 모델 파일을 제공하는 이미지입니다. 런타임 시 보조 이미지의 컨텐트가 기본 이미지의 컨텐트와 병합됩니다. ![이미지 구조](images/image-structure.png)

이 실습에서는 WebLogic 서버 12.2.1.3.0-ol8 이미지를 Primary Image로 사용합니다. 또한 보조 이미지를 생성하고 생성된 인증 토큰을 사용하여 Oracle Container Image Registry 저장소에 푸시합니다.

예상 시간: 10분

### 목표

이 실습에서는 다음을 수행합니다.

*   보조 이미지를 생성하고 이미지를 Oracle Cloud Container Image Registry로 푸시합니다.

## 작업 1: 보조 이미지 준비 및 보조 이미지 푸시

이 작업에서는 Oracle Cloud Container Registry로 푸시할 보조 이미지를 생성합니다.

1.  _이미지_를 누릅니다. Primary Image의 경우 아래 _weblogic_ Image.So을 사용하여 _Primary Image_ 섹션 아래에 기본값을 그대로 둡니다.
    
        <copy>container-registry.oracle.com/middleware/weblogic:12.2.1.3-ol8</copy>
        
    
    ![기본 이미지](images/primary-image.png)
    
    > **참고용:**  
    > 기본 이미지는 도메인 실행에 사용되는 이미지입니다. 수백 개의 도메인에 대해 하나의 기본 이미지를 재사용할 수 있습니다. 기본 이미지에는 OS, JDK 및 FMW 소프트웨어 설치가 포함되어 있습니다.
    
2.  보조 이미지 태그를 만들려면 다음 정보가 필요합니다.
    
    *   영역의 끝점
    *   테넌시 네임스페이스
3.  _지역의 끝점_을 찾습니다. 이 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)에 설명된 표를 참조하십시오. 표시된 예제에서 영역의 끝점은 _UK South (London)_(영역 이름)이고 끝점은 _lhr.ocir.io_입니다. 고유의 _영역 이름_에 대한 끝점을 찾아 텍스트 파일에 저장합니다. 다음 연습에도 필요합니다.
    
    ![끝점](images/end-point.png " ")
    
    > 이제 해당 지역에 대한 테넌시 네임스페이스와 엔드포인트가 모두 제공됩니다.
    
4.  실습 3에서는 이미 테넌시 네임스페이스를 텍스트 파일에 기록했습니다. 그렇지 않은 경우 테넌시의 네임스페이스를 찾으려면 다음과 같이 _햄버거 메뉴_ -> _개발자 서비스_ -> _컨테이너 레지스트리_를 선택합니다. 생성한 Repository를 선택하면 다음과 같이 Namespace가 나타납니다. ![테넌시 네임스페이스](images/tenancy-namespace.png)
    
5.  이제 해당 지역의 테넌시 네임스페이스와 끝점이 모두 있습니다. 다음 명령을 복사하여 텍스트 파일에 붙여넣습니다. 그런 다음 `END_POINT_OF_YOUR_REGION`를 영역 이름의 끝점으로 바꾸고 `NAMESPACE_OF_YOUR_TENANCY`를 테넌시의 네임스페이스로 바꿉니다. 표시된 대로 _Auxiliary Image_ 탭을 누릅니다. ![보조 탭](images/auxiliary-tab.png)
    
        <copy>END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/test-model-your_first_name:v1</copy>
        

> 예를 들어, 내 경우 보조 이미지 태그는 `lhr.ocir.io/tenancynamespace/test-model-ankit:v1`입니다.

6.  4단계에서는 테넌시 네임스페이스도 확인했습니다. `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`와 같이 보조 이미지 레지스트리 푸시 사용자 이름을 입력합니다.  
    

*   `NAMESPACE_OF_YOUR_TENANCY`를 테넌시의 네임스페이스로 바꾸기
*   `YOUR_ORACLE_CLOUD_USERNAME`을 Oracle Cloud 계정 사용자 이름으로 바꾼 다음 대체된 사용자 이름을 텍스트 파일에서 복사하여 _보조 이미지 레지스트리 푸시 사용자 이름_에 붙여넣습니다.

> 예를 들어, 내 경우 **보조 이미지 레지스트리 푸시 사용자 이름**은 `tenancynamespace/lab.user@oracle.com`입니다.

*   비밀번호의 경우 텍스트 파일(또는 저장 위치)에서 인증 토큰을 복사하여 붙여 넣고 **보조 이미지 레지스트리 푸시 사용자 이름**에 붙여 넣습니다. ![보조 이미지 세부정보](images/auxiliary-image-details.png)

7.  _보조 이미지 생성_을 누릅니다. ![보조 이미지 생성](images/create-auxiliary-image.png)
    
8.  이미 Lab 2에서 모델을 준비했으므로 _아니오_를 누르십시오. ![모델 준비](images/prepare-model.png)
    
9.  _WebLogic Deployer_를 저장할 _Downloads_ 폴더를 선택하고 표시된 대로 _Select_를 누릅니다. ![WDT 위치](images/wdt-location.png)
    
10.  보조 이미지가 성공적으로 생성되면 _보조 이미지 생성 완료_ 창에서 _확인_을 누릅니다. ![보조 생성됨](images/auxiliary-created.png)
    
    > **참고용:**  
    > 보조 이미지는 도메인마다 다릅니다. 보조 이미지에는 도메인을 정의하는 데이터가 포함되어 있습니다.
    
11.  _보조 이미지 푸시_를 눌러 Oracle Cloud 컨테이너 이미지 레지스트리 내의 저장소에 이미지를 푸시합니다. ![푸시 보조](images/push-auxiliary.png)
    
12.  이미지가 성공적으로 푸시되면 _푸시 이미지 완료_ 창에서 _확인_을 누릅니다. ![보조 푸시됨](images/auxiliary-pushed.png)
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 10월
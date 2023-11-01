# bobbys-helidon-stock-application용 Docker 이미지를 Oracle Cloud Container Registry로 푸시

## 소개

Lab 5에서는 bobbys-helidon-stock-application을 수정하고 새로운 Docker 이미지를 구축했습니다. 이 실습에서는 해당 이미지를 Oracle Cloud Container Registry 내의 저장소에 푸시합니다.

예상 시간: 10분

### 목표

이 실습에서는 다음을 수행합니다.

*   인증 토큰을 생성하여 Oracle Cloud Container Registry에 로그인합니다.
*   bobbys-helidon-stock-application Docker 이미지를 Oracle Cloud Container Registry 리포지토리로 푸시합니다.

### 필요 조건

환경에 따라 명령 및 URL을 붙여넣고 수정할 수 있는 텍스트 편집기가 있어야 합니다. 그런 다음 수정된 명령을 복사하여 붙여넣어 _Cloud Shell_에서 실행할 수 있습니다.

## 작업 1: Oracle Cloud Container Registry에 로그인하기 위한 인증 토큰 생성

이 단계에서는 Oracle Cloud Container Registry 로그인에 사용할 _인증 토큰_을 생성합니다.

1.  오른쪽 맨 위에 있는 사용자 아이콘을 선택한 다음 _내 프로파일_을 선택합니다.
    
    ![사용자 설정](images/user-settings.png " ")
    
2.  아래로 스크롤하여 _Auth Tokens_을 선택합니다.
    
    ![인증 토큰](images/auth-token.png " ")
    
3.  _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/generate-token.png " ")
    
4.  _`helidon-stock-application-your_first_name`_을 복사하여 _설명_ 상자에 붙여넣고 _토큰 생성_을 누릅니다.
    
    ![토큰 생성](images/token-create.png " ")
    
5.  생성된 토큰에서 _복사_를 선택하고 텍스트 파일에 붙여넣습니다. 나중에 복사할 수 없습니다.
    
    ![토큰 복사](images/copy-token.png " ")
    

## 작업 2: Oracle Cloud Container Registry Repository로 bobbys-helidon-stock-application Docker 이미지 푸시

1.  이 연습의 작업 1에서 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)을 열고 영역 이름의 끝점을 확인하여 텍스트 편집기로 복사했습니다. 예제에서 Region Name은 UK South(런던)입니다. 이 태스크에 대해 이 정보가 필요합니다. ![엔드포인트](images/end-point.png)
    
2.  다음 명령을 복사하여 텍스트 편집기에 붙여 넣은 다음 **`END_POINT_OF_REGION_NAME`**을 영역의 끝점으로 바꿉니다.
    
        <copy> docker login `END_POINT_OF_REGION_NAME`</copy>
        

3\. 이전 연습에서 테넌시 네임스페이스를 확인했습니다. 사용자 이름을 다음과 같이 지정합니다. \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`/oracleidentitycloudservice/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*. 여기서 \`\`NAMESPACE\_OF\_YOUR\_TENANCY\`\*\*을 테넌시의 네임스페이스로 바꾸고 \*\*\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*을 Oracle Cloud 계정 사용자 이름으로 바꾼 다음 대체된 사용자 이름을 텍스트 파일에서 복사하여 \*Cloud Shell\*에 붙여넣습니다. 비밀번호의 경우 텍스트 편집기나 저장 위치에서 인증 토큰을 붙여 넣습니다. !\[로그인 레지스트리\](images/login-registry.png " ") 3\. 이전 연습에서 테넌시 네임스페이스를 확인했습니다. 사용자 이름을 \`NAMESPACE\_OF\_YOUR\_TENANCY\`/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`으로 지정합니다. 도커 로그인을 수행하는 동안 테넌시 네임스페이스와 사용자 이름을 소문자로 사용해야 합니다. 여기서 \`NAMESPACE\_OF\_YOUR\_TENANCY\`을 테넌시의 네임스페이스로 바꾸고 \`YOUR\_ORACLE\_CLOUD\_USERNAME\`을 Oracle Cloud 계정 사용자 이름으로 바꾼 다음 대체된 사용자 이름을 텍스트 편집기에서 복사하여 \*Cloud Shell\*에 붙여넣습니다. 비밀번호 의 경우 텍스트 편집기나 저장 위치에서 인증 토큰을 붙여 넣습니다.

4.  컨테이너 레지스트리로 돌아가서 _햄버거 메뉴 -> 개발자 서비스 -> 컨테이너 레지스트리_를 선택합니다.
    
    ![컨테이너 레지스트리](images/container-registry.png " ")
    
5.  구획을 선택한 다음 _저장소 생성_을 누릅니다.
    
    ![저장소 생성](images/repository-create.png " ")
    
6.  구획을 선택하고 저장소 이름으로 _`helidon-stock-application-your_first_name`_을 입력한 다음 Access를 _Public_으로 선택하고 _Create_를 누릅니다.
    
    ![저장소 데이터](images/repository-data.png " ")
    
    _`helidon-stock-application-your_first_name`_ 저장소가 생성된 후 네임스페이스를 확인할 수 있으며 테넌시의 네임스페이스와 동일해야 합니다.
    
    !\[네임스페이스 확인\](images/verify-namespace.png" ")
    
7.  Lab 5의 일부로 Docker 이미지 전체 이름을 텍스트 편집기로 복사했습니다. Docker 이미지를 Oracle Cloud Container Registry 내의 저장소에 푸시하려면 텍스트 편집기에서 다음 명령을 복사하여 붙여 넣은 다음 _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`helidon-stock-application-your_first_name`:1.0_을 텍스트 편집기에 저장한 Docker 이미지 전체 이름으로 바꿉니다.
    
        <copy>docker push `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/helidon-stock-application-your_first_name:1.0</copy>
        
    
    ![Docker 푸시](images/docker-push.png " ")
    
    _docker push_ 명령이 성공적으로 실행된 후 _`helidon-stock-application-your_first_name`_ 저장소를 확장하면 이 저장소에 새 이미지가 업로드되었음을 알 수 있습니다.
    
    _Cloud Shell_ 및 컨테이너 레지스트리 저장소 페이지를 열어 둡니다. 다음 실습을 위해 필요합니다.
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 3월
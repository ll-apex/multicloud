# 실습 환경 설정

## 소개

이 실습에서는 Oracle Cloud Container Image Registry 내에 Repository를 생성합니다. 또한 Oracle Container Registry에서 WebLogic 서버 이미지에 대한 라이센스 합의서에 동의하게 됩니다.

예상 시간: 15분

### 목표

이 실습에서는 다음을 수행합니다.

*   Oracle Cloud 컨테이너 이미지 레지스트리 내에 저장소를 생성합니다.
*   Oracle Container Registry의 WebLogic 서버 이미지에 대한 라이센스를 수락하십시오.

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.
*   Oracle 계정이 있어야 합니다.
*   텍스트 편집기가 있어야 합니다.

## 작업 1: 저장소 만들기

이 작업에서는 공용 저장소를 생성합니다. 실습 5에서는 보조 이미지를 이 저장소로 푸시합니다.

1.  원격 데스크톱에서 Firefox를 열려면 **활동**을 누르고, 검색 상자에 **Fire**를 입력한 다음 표시된 것처럼 **Firefox**를 누릅니다. ![Firefox 열기](images/open-firefox.png)
    
2.  인증서를 사용하여 [Oracle Cloud 콘솔](https://cloud.oracle.com)에 로그인합니다.
    
3.  그림과 같이 콘솔에서 _햄버거 메뉴_ -> _개발자 서비스_ -> _컨테이너 레지스트리_를 선택합니다. ![컨테이너 레지스트리](images/container-registry.png)
    
4.  저장소를 생성할 수 있는 구획을 선택합니다. _저장소 생성_을 누릅니다. ![저장소 생성](images/create-repository.png)
    
5.  저장소 이름으로 _`test-model-your_firstname`_를 입력하고 _Public_으로 Access를 입력한 다음 _Create_를 누릅니다. ![저장소 세부정보](images/repository-details.png)
    
6.  저장소가 준비되면 텍스트 파일에서 테넌시 네임스페이스를 확인하십시오. ![노트 테넌시 NameSpace](images/tenancy-namespace.png)
    

## 작업 2: WebLogic 서버 이미지에 대한 라이센스 수락

이 작업에서는 Oracle Container Registry에 있는 WebLogic 서버 이미지의 라이센스 계약에 동의합니다. 실습 5에서와 같이 WebLogic Server 12.2.1.3.0 이미지를 기본 이미지로 사용합니다. 따라서 WebLogic Server Images에 액세스하려면 사용권 계약에 동의합니다.

1.  브라우저에서 Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/)에 대한 링크를 복사하여 붙여넣고 사인인합니다. 이를 위해서는 Oracle 계정이 필요합니다. ![컨테이너 레지스트리 사인인](images/container-registry-sign-in.png)
    
2.  \[사용자 이름\] 및 \[비밀번호\] 필드에 _Oracle 계정 인증서_를 입력한 다음 _사인인_을 누릅니다. ![로그인 컨테이너 레지스트리](images/login-container-registry.png)
    
3.  Oracle Container Registry의 홈 페이지에서 _weblogic_을 검색합니다. ![검색 WebLogic](images/search-weblogic.png)
    
4.  표시된 대로 _weblogic_을 누르고 언어로 _English_를 선택한 다음 _Continue_를 누릅니다. ![WebLogic을 누릅니다.](images/click-weblogic.png) ![언어 선택](images/select-language.png)
    
5.  _수락_을 눌러 라이센스 계약에 동의합니다. ![라이센스 동의](images/accept-license.png)
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 10월
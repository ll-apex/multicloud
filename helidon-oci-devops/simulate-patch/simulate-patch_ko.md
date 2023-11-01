# 패치 시나리오 시뮬레이트

## 소개

이 실습에서는 패치 적용 시나리오를 시뮬레이트합니다. 초기 환경이 테스트에 _Open JDK_를 사용하고 결국 운용 준비가 되었을 때 _Oracle JDK_를 사용하도록 이동한 경우입니다. 이전 빌드 및 배치 파이프라인은 이미 _Open JDK 20_을 사용할 Java 선호도로 설정합니다. 이 실습에서는 _Oracle JDK 20_으로 바뀝니다.

예상 시간: 10분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [패치 시나리오 시뮬레이션](videohub:1_4pecvhmf)

### 목표

이 실습에서는 다음을 수행합니다.

*   JDK 선호항목 수정
*   배치 파이프라인에서 JDK 새 선호도를 확인합니다.

### 필요 조건

*   Oracle Free Tier(체험판), 유료 또는 LiveLabs 클라우드 계정

## 작업 1: JDK 설치 프로그램 변경

1.  **Lab 2**에서 살펴본 바와 같이, 이전에 완료된 **배포 파이프라인 로그**에서 로그 하단 근처에 **Open JDK**를 사용 중인 것으로 확인되었습니다. ![jdk 열기](images/open-jdk.png)
    
2.  **코드 편집기**에서 파일 이름 **`build_spec.yaml`**를 눌러 엽니다. URL 경로를 **JDK 열기** 설치 프로그램(행 번호 #15)으로 설정하는 환경 변수 **`JDK20_TAR_GZ_INSTALLER`**를 주석 처리하고, URL 경로를 **Oracle JDK** 설치 프로그램(행 번호 #16)으로 설정하는 **`JDK20_TAR_GZ_INSTALLER`** 주석 처리를 해제합니다. ![jdk 수정](images/modify-jdk.png)
    

## 작업 2: 변경사항을 푸시하고 DevOps 파이프라인을 트리거합니다.

1.  다음 명령을 복사하여 터미널에 붙여넣어 **변경 사항을 커밋하고 푸시**합니다.
    
        <copy>git add .
        git status
        git commit -m "Replace OpenJDK 20 with OracleJDK 20"
        git push</copy>
        
    
    > 이 Git 푸시로 파이프라인이 트리거됩니다.
    

## 작업 3: 배치 파이프라인에서 JDK 새 유형 확인

1.  새 탭에서 [Cloud Console](https://cloud.oracle.com/)을 열고 _햄버거 메뉴_ -> _개발자 서비스_ -> _프로젝트_ 아래의 **DevOps**를 누릅니다. ![devops 프로젝트](images/devops-project.png)
    
2.  **Lab 1**에서 만든 구획을 선택한 다음 _devops-project-helidon-ocw-hol-string_을 눌러 **DevOps Project**를 엽니다. ![구획 선택](images/select-compartment.png)
    
3.  **최신 빌드 내역**에서 **실행** 및 상태가 **수락됨/진행 중**으로 표시됩니다. 아래와 같이 최신 실행을 누릅니다. ![빌드 내역](images/build-history.png)
    
4.  빌드 파이프라인이 **세 단계를 모두 완료**하면 아래와 같이 출력이 표시됩니다. ![먼저 실행된 빌드](images/build-run-first.png)
    
5.  빌드 실행 진행 중 세번째 단계에서 **세 개의 점**을 누른 후 아래와 같이 **배치 보기**를 누릅니다. 그러면 배치 파이프라인이 열립니다. ![배치 보기](images/view-deployment.png)
    
6.  여기에서 **배포 진행률**을 확인할 수 있습니다. 배치 파이프라인이 완료되면 아래와 같이 출력이 표시됩니다. ![배치 실행](images/deployment-run.png)
    
    > 그러면 OCI의 컴퓨트 인스턴스에 Helidon 애플리케이션이 성공적으로 배치됩니다.
    
7.  배치 파이프라인의 로그를 보려면 배치 단계 근처의 **세 개의 점**을 누르고 아래와 같이 **세부정보 보기**를 누릅니다. ![로그 보기](images/view-logs.png)
    
8.  로그를 아래로 스크롤하여 JDK flavour를 확인합니다. 아래와 같이 **Oracle JDK**여야 합니다. ![Oracle JDK](images/oracle-jdk.png)
    

이제 **다음 실습으로 진행할 수 있습니다.**

## 확인

*   **작성자** - Keith Lustria
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
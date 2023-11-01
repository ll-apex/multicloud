# 환경 설정

## 소개

이 실습에서는 워크샵에 필요한 환경을 설정하는 단계를 안내합니다.

[Lab1 연습](videohub:1_far2bboa)

예상 시간: 10분

### 코드 편집기 정보

코드 편집기를 사용하면 OCI 콘솔에서 직접 다양한 OCI 서비스에 대한 코드를 편집하고 배치할 수 있습니다. 이제 콘솔과 로컬 개발 환경 간에 전환할 필요 없이 서비스 워크플로우와 스크립트를 업데이트할 수 있습니다. 이를 통해 클라우드 솔루션을 신속하게 프로토타입화하고, 새로운 서비스를 탐색하고, 빠른 코딩 작업을 수행할 수 있습니다.

Code Editor와 Cloud Shell의 직접 통합을 통해 Cloud Shell에 사전 설치된 GraalVM Enterprise Native Image 및 JDK 17(Java Development Kit)에 액세스할 수 있습니다.

### 목표

*   코드 편집기 설정
*   필요한 Maven 및 JDK 버전 다운로드
*   Helidon 소스 코드 다운로드

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.
*   Code Editor를 열려면 Firefox가 브라우저여야 합니다.

## 작업 1: 코드 편집기 설정

1.  Cloud Console에서 다음과 같이 Code Editor 아이콘을 누릅니다. ![코드 편집기](images/code-editor.png)
    
2.  Code Editor에서 Terminal -> New Terminal을 누릅니다. ![터미널 열기](images/open-terminal.png)
    

## 작업 2: 필요한 Maven 및 JDK 버전 다운로드

1.  다음 명령을 복사하여 터미널에 붙여넣습니다. 필요한 버전의 JDK 및 Maven을 다운로드합니다.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/archive/jdk-19.0.2_linux-x64_bin.tar.gz
        tar -xzvf jdk-19.0.2_linux-x64_bin.tar.gz</copy>
        

## 태스크 3: Helidon 소스 코드 다운로드

1.  다음 명령을 복사하고 터미널에 붙여넣어 helidon 응용 프로그램의 소스 코드를 다운로드합니다.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  다음 명령을 복사하여 붙여넣어 _helidon-levelup-2023-main.zip_의 압축을 풉니다.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

이제 _Lab 2로 진행_할 수 있습니다.

## 확인

*   **작성자** - Joe DiPol
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **Last Updated By/Date** - Ankit Pandey, 2023년 2월
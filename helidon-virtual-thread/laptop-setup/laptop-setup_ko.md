# Helidon MP 애플리케이션 네이티브 이미지 구축

## 소개

이 실습에서는 로컬 Linux 시스템에서 환경을 설정하는 방법에 대해 알아봅니다.

예상 시간: 10분

### 목표

*   필요한 JDK 및 Maven을 구성합니다.
*   애플리케이션 소스 코드를 다운로드합니다.

### 필요 조건

*   프로젝트를 수정, 빌드 및 실행하려면 IDE가 있어야 합니다.

## 작업 1: 필요한 Maven 및 JDK 버전 다운로드

1.  IDE에서 터미널/콘솔을 엽니다.
    
2.  다음 명령을 복사하여 터미널에 붙여넣습니다. 필요한 버전의 JDK 및 Maven을 다운로드하고 필요한 Maven 및 JDK를 사용하도록 PATH 변수를 설정합니다.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.tar.gz
        tar -xzvf jdk-19_linux-x64_bin.tar.gz</copy>
        

## 태스크 2: Helidon 소스 코드 다운로드

1.  다음 명령을 복사하고 터미널에 붙여넣어 helidon 응용 프로그램의 소스 코드를 다운로드합니다.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  다음 명령을 복사하여 붙여넣어 _helidon-levelup-2023-main.zip_의 압축을 풉니다.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

이제 _다음 실습으로 진행_할 수 있습니다.

## 확인

*   **작성자** - Joe DiPol
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **Last Updated By/Date** - Ankit Pandey, 2023년 2월
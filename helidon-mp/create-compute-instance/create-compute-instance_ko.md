# 컴퓨트 인스턴스에서 Helidon 애플리케이션 고유 도커 이미지 실행

## 소개

이 실습에서는 Oracle Container Image Registry에서 도커 이미지를 추출하여 Oracle Cloud Infrastructure 내의 가상 머신에서 실행하는 프로세스를 안내합니다.

예상 시간: 15분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [컴퓨트 인스턴스에서 Helidon Application Native Docker 이미지 실행](videohub:1_dsfd22u5)

### 목표

이 실습에서는 다음을 수행합니다.

*   컴퓨팅 인스턴스 생성
*   컴퓨트 인스턴스에 Docker 설치
*   컴퓨트 인스턴스 내에서 애플리케이션 네이티브 이미지 도커 컨테이너를 실행합니다.

### 필요 조건

이 실습을 실행하려면 다음이 있어야 합니다.

*   Oracle Cloud 계정
*   컴퓨팅 인스턴스 생성 리소스 보유
*   컨테이너 레지스트리에서 컨테이너 패키지 Helidon _myproject-your\_first\_name_ 애플리케이션을 사용할 수 있습니다.

## 작업 1: 컴퓨트 인스턴스 생성

1.  클라우드 콘솔에서 _컴퓨트_ -> _인스턴스_를 누릅니다. ![컴퓨팅 인스턴스](images/compute-instance.png)
    
2.  올바른 구획을 선택하고 _인스턴스 생성_을 누릅니다. ![인스턴스 생성](images/create-instance.png)
    
3.  Select the following values and click _Create_.  
    **Name:** Leave default.  
    **Create in compatment** Select your own compartment. **Image:** Select _Oracle Linux 8_ image.  
    **Shape:** Select the shape **VM.Standard.E4.Flex** and then select 1 OCPU with 16GB memory.  
    **Primary network:** select _Create new virtual cloud network_ and leave default values.  
    **Subnet:** select _Create new public subnet_ and leave default values.  
    **Public IP address:** select _Assign a public IPv4 address_.  
    **Add SSH keys:** select _Generate a key pair for me_ and click _Save private key_ and _Save public key_ to save the key pair in your local machine. you will need to copy the private key to Code Editor later.
    
4.  인스턴스가 생성되면 _복사_를 눌러 인스턴스의 공용 IP를 복사합니다. ![IP 복사](images/copy-ip.png)
    

## 작업 2: 컴퓨트 인스턴스에 도커 설치

1.  Code Editor 내부의 터미널에서 다음 명령을 실행하여 전용(private) 키를 생성합니다.
    
        <copy>vi ~/opc.key</copy>
        
2.  삽입 모드로 들어가서 이 실습의 작업 1에서 다운로드한 개인 키의 내용을 붙여넣으려면 _i_를 누릅니다. _escape_ 키를 누른 다음 _:wq_를 입력하여 파일 컨텐츠를 저장합니다.
    
3.  터미널에서 다음 명령을 실행하여 파일 권한을 변경합니다.
    
        <copy>chmod 600 ~/opc.key</copy>
        
4.  고유의 _PUBLIC IP_와 함께 다음 명령을 실행하여 방금 생성된 컴퓨트 인스턴스에 접속합니다.
    
        <copy>ssh -i ~/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        
    
    ![SSH OPC](images/ssh-opc.png)
    
    > **Free tier 계정에서는 기본적으로 _.ssh_ 폴더가 없으므로 출력이 스크린샷과 다르게 표시됩니다.**
    
5.  다음 명령을 실행하여 컴퓨트 인스턴스에서 루트 사용자로 도커를 설치하고 도커를 실행할 수 있는 _opc_ 사용자 기능을 제공합니다.
    
        <copy>sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
        sudo dnf remove -y runc
        sudo dnf install -y docker-ce --nobest
        sudo systemctl enable docker.service
        sudo systemctl start docker.service
        sudo usermod -aG docker opc
        sudo reboot</copy>
        
6.  시스템이 재부트될 때까지 1-2분 정도 기다린 후 다음 명령을 실행하여 컴퓨팅 인스턴스에 다시 연결합니다.
    
        <copy>ssh -i ~/.ssh/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        

## 작업 3: 컴퓨트 인스턴스에서 인사말 앱 가져오기 및 실행

1.  다음 명령을 실행하여 Oracle Container Image Registry에서 도커 이미지를 가져옵니다.
    
        <copy>docker pull ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    아래와 유사한 출력이 나타납니다. ![이미지 풀기](images/docker-pull.png)
    
2.  다음 명령을 실행하여 응용 프로그램을 실행합니다.
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
3.  새 터미널과 ssh를 열어 인스턴스를 계산하고 다음 명령을 실행하여 응용 프로그램을 실행할 수 있습니다.
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        
    
    Oracle Cloud Infrastructure의 컴퓨트 인스턴스에 대한 Helidon 애플리케이션 배포를 완료하셨습니다.
    

## 확인

*   **작성자** - Dmitry Aleksandrov
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 4월
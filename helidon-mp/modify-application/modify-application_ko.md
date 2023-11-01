# 코드 편집기에서 Helidon MP 애플리케이션 수정

## 소개

이 연습에서는 Code Editor에서 JAVA Class에 커스텀 끝점을 추가합니다.

예상 시간: 10분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [코드 편집기에서 Helidon MP 애플리케이션 수정](videohub:1_sv1iug41)

### 목표

이 실습에서는 다음을 수행합니다.

*   Java 클래스에 사용자정의 끝점 추가
*   수정된 응용 프로그램 빌드 및 실행

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.

## 작업 1: 사용자정의 끝점 추가

1.  Code Editor에서 _GreetResource.java_ 파일을 눌러 엽니다. ![파일 열기](images/open-file.png)
    
2.  코드에서 볼 수 있듯이 완전히 MicroProfile 기반입니다. 즉, POJO 및 주석을 사용하여 모든 기능을 수행할 수 있습니다. 이러한 주석은 표준이므로 여러 공급업체에서 이식할 수 있습니다. 즉, 동일한 버전의 MicroProfile를 지원하는 다른 플랫폼에서 실행되는 코드를 쉽게 실행할 수 있습니다. 자세한 내용은 [여기](https://microprofile.io/)에서 확인할 수 있습니다.
    
3.  5개 제목 그룹에서 무작위로 제목을 제공하는 새 끝점을 만듭니다. 이 끝점을 생성하려면 아래와 같이 행 번호 80에 아래 코드를 복사하여 붙여넣으십시오.
    
        <copy>@Path("/title")
        @GET
        @Produces(MediaType.APPLICATION_JSON)
        public String getRandomTitle() {
            return new String[] {"Mr. ", "Mrs.", "Miss", "Dr.", "Dr. sc."} [(int)(Math.random()*5)];
        }</copy>
        
    
    ![코드 추가](images/add-code.png)
    
4.  파일 내용을 저장하려면 Code Editor에서 _File_ -> _Save_를 누릅니다.
    
    > AutoSave는 코드 편집기에서 기본적으로 사용으로 설정됩니다.
    

## 작업 2: 응용 프로그램 실행

1.  다음 명령을 복사하여 터미널에 붙여 넣어 응용 프로그램을 실행합니다.
    
        <copy>mvn clean package
        java -jar target/myproject.jar</copy>
        
2.  새 터미널/콘솔을 열고 다음 명령을 실행하여 응용 프로그램을 확인합니다.
    
        <copy>curl -X GET http://localhost:8080/greet/title</copy>
        Mr.
        
    
    > 이 명령은 5개 옵션 중 임의로 제목을 제공하며 이 명령을 여러 번 실행할 수 있습니다.
    
3.  _"java -jar target/myproject.jar" 명령이 실행되고 있는 터미널에 `Ctrl + C`를 입력하여 **myproject** 응용 프로그램을 정지합니다_. 그것은 매우 중요하다, 다른 경우 당신은 실험실에서 문제를 직면하게됩니다.
    

## 확인

*   **작성자** - Dmitry Aleksandrov
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 4월
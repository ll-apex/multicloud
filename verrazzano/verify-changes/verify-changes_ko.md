# 응용 프로그램 액세스 및 선별된 스택을 통해 변경 사항 확인

## 소개

실습 7에서는 bobbys-helidon-stock-application의 변경 사항을 적용했으며 Pod는 _Running_ 상태입니다. 이 실습에서는 애플리케이션, Verrazzano Console 및 Grafana Console의 변경 사항을 확인합니다.

예상 시간: 05분

### 목표

이 실습에서는 다음을 수행합니다.

*   Bobby's Books 응용 프로그램의 변경 사항을 확인합니다.
*   Grafana 콘솔에서 변경사항을 확인합니다.

### 필요 조건

*   환경에 따라 명령 및 URL을 붙여넣고 수정할 수 있는 텍스트 편집기가 있어야 합니다. 그런 다음 _Cloud Shell_에서 실행하기 위해 수정된 명령을 복사하여 붙여넣을 수 있습니다.

## 작업 1: Bobby's Books 응용 프로그램의 변경 사항 확인

1.  Bobby's Books 탭을 열고 Refresh를 선택합니다. 해당 탭을 닫은 경우 텍스트 편집기에서 다음 명령을 복사하여 붙여 넣고 XX.XX.XX.XX를 응용 프로그램의 EXTERNAL\_IP로 바꿉니다. 장부 이름은 모두 대문자로 표시됩니다.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![바비 책](images/bobbysbooks.png " ")
    

## 작업 2: Grafana 콘솔의 변경 사항 확인

1.  Verrazzano 홈 페이지로 돌아갑니다.
    
    ![Verrazzano 홈](images/verrazzao-home.png " ")
    
2.  Grafana에 대한 링크를 선택하여 _Grafana 콘솔_을 엽니다.
    
    ![Grafana 링크](images/grafana-link.png " ")
    
3.  _홈_을 선택하고 _Helidon_을 입력한 다음 _Helidon Monitoring Dashboard_를 선택합니다.
    
    ![Helidon 검색](images/search-helidon.png " ")
    
4.  ServiceID에서 _bobs-books\_default\_bobs-books\_bobby-helidon_을 선택하고 인스턴스에서 새로 생성된 인스턴스를 선택합니다. 이 경우 수정된 _bobby-helidon-stock-application_에 대한 정보가 제공됩니다.
    
    ![새 구성요소](images/new-component.png " ")
    
    축하합니다! 실습을 성공적으로 완료했습니다.
    

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
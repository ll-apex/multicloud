# 소개

## 이 워크샵 정보

이 실습에서는 단일 Kubernetes 클러스터에 Verrazzano 플랫폼을 설치하고 Verrazzano 개념을 사용하여 샘플 애플리케이션을 배포하는 방법을 보여줍니다.

[Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/) 샘플 응용 프로그램은 다음 세 부분으로 구성됩니다.

*   REST 서비스가 포함된 Java EE 애플리케이션이자 MySQL 데이터베이스에 데이터를 저장하는 매우 간단한 JSP UI인 백엔드 "주문 처리" 애플리케이션입니다. 이 애플리케이션은 WebLogic 서버에서 실행됩니다.
*   프런트엔드 웹 스토어 "Robert's Books"는 일반 책 판매자입니다. 이는 Coherence에서 장부 데이터를 가져오는 Helidon 마이크로서비스로 구현되고, Coherence 캐시 저장소를 사용하여 주문 관리자의 데이터를 보관하며, React 웹 UI를 사용합니다.
*   프런트 엔드 웹 스토어 "Bobby's Books", 이는 전문 어린이 책 상점입니다. 이 서비스는 다른 Coherence 캐시 저장소에서 장부 데이터를 가져오고 주문 관리자와 직접 연결하며 WebLogic 서버에서 JSF 웹 UI를 실행하는 Helidon 마이크로서비스로 구현됩니다.

### 제품/기술 정보

Verrazzano는 멀티클라우드 및 하이브리드 환경에서 클라우드 네이티브 및 기존 애플리케이션을 배포하기 위한 엔드투엔드 엔터프라이즈 컨테이너 플랫폼입니다. 그것은 선별 된 오픈 소스 구성 요소들로 구성되어 있습니다 - 많은 당신이 이미 사용하고 신뢰 할 수 있습니다, 그리고 일부는 Verrazzano를 응집하고 사용하기 쉬운 플랫폼으로 만드는 모든 조각을 모으기 위해 특별히 작성되었습니다.

Verrazzano에는 다음과 같은 기능이 포함되어 있습니다.

*   하이브리드 및 다중 클러스터 워크로드 관리
*   WebLogic, Coherence 및 Helidon 애플리케이션에 대한 특별 처리
*   다중 클러스터 Infrastructure 관리
*   사전 유선 통합 애플리케이션 모니터링
*   통합된 보안
*   DevOps 및 GitOps 역량 강화

![Verrazzano ](images/verrazzano.png)

### 목표

1\. Oracle Cloud Infrastructure에서 Oracle Kubernetes Engine 인스턴스를 설정합니다. 1\. Oracle Cloud Infrastructure의 Oracle Kubernetes Engine 인스턴스와 상호 작용하도록 \`kubectl\`을 구성합니다. 2\. Verrazzano 플랫폼을 설치하고 알아보세요. 3. \*Bobby's Books\* 샘플 응용 프로그램을 배치합니다. 4. Helidon 기반 \*Stock\* 응용 프로그램 구성 요소를 수정하고 재배치합니다.

## 자세히 알아보기

*   [Verrazzano](https://verrazzano.io/)

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
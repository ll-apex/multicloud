# 소개

## 이 워크샵 정보

이 실습에서는 Oracle Code Editor를 사용하여 Helidon 4에서 제공되는 Helidon Nima WebServer을 사용하여 마이크로서비스를 구축, 실행 및 수정할 수 있습니다. Nima는 Java 19의 가상 스레드를 활용하기 위해 처음부터 작성되었으며 간단한 차단 프로그래밍 모델을 제공합니다.

보다 복잡한 반응적 프로그래밍과 비교되는 방법도 확인할 수 있습니다.

이 실습에서는 Helidon Project Starter를 사용하여 Helidon Microprofile 애플리케이션을 개발합니다. 사용자가 프로젝트에 추가할 Helidon 기능을 선택할 수 있는 다양한 옵션을 제공하는 것은 매우 사용자 정의가 가능합니다. 그런 다음 가상 스레드를 사용하여 새 Helidon Nima WebServer에서 실행 중인 Helidon 4로 애플리케이션을 마이그레이션합니다.

이 워크샵은 가능한 한 자체 설명이 가능하도록 설계되었지만 언제든지 설명을 요청하거나 도움을 요청하십시오.

예상 시간: 90분

### 목표

*   OCI 코드 편집기 살펴보기
*   Nima 애플리케이션 빌드 및 실행
*   반응적 애플리케이션 빌드 및 실행
*   Nima 애플리케이션의 단순성 분석
*   Nima 및 Reactive 애플리케이션에 대한 스택 추적 비교
*   Helidon MP 애플리케이션 생성
*   Helidon 3 애플리케이션을 Helidon 4로 마이그레이션

**Helidon Nima 정보**

Helidon은 Oracle에서 도입된 오픈 소스 마이크로서비스 프레임워크로, 가볍고 빠른 마이크로서비스 기반 애플리케이션을 생성하기 위해 설계된 Java 라이브러리 모음을 제공합니다.

이전 버전의 Helidon 개발자는 두 가지 프로그래밍 모델에서 선택할 수 있었습니다. [Helidon MP](https://helidon.io/docs/v3/#/mp/introduction): Jakarta EE 개발자에게 친숙한 API를 갖춘 Eclipse MicroProfile 표준의 구현입니다. 그리고 [Helidon SE](https://helidon.io/docs/v3/#/se/introduction)는 린 반응적 프레임워크입니다.

Helidon 4에서 Oracle은 JEP 425 가상 스레드를 사용하기 위해 처음부터 작성된 웹 서버인 Níma를 소개합니다. Nima는 뛰어난 성능으로 사용하기 쉬운 프로그래밍 모델을 제공합니다. 가상 스레드를 사용하면 스레드는 더 이상 신중하게 풀링 및 관리할 희소한 리소스가 아닙니다. 대신 거의 무제한의 동시 요청을 처리하기 위해 필요에 따라 만들 수 있는 풍부한 리소스입니다. 각 요청이 전용 스레드에서 실행되기 때문에 데이터베이스 호출이나 다른 서비스처럼 차단 작업을 자유롭게 수행할 수 있습니다. 플랫폼 스레드를 차단하고 다른 요청을 굶어버리는 것에 대한 두려움 없이 간단한 동기식 방식으로 이를 수행할 수 있습니다. 오버헤드가 적고 동시성이 높은 서비스를 구현하기 위해 더 이상 복잡한 비동기 코드에 의존할 필요가 없습니다.

Helidon Níma 웹 서버는 Helidon 에코시스템에서 Netty를 대체하려고 합니다. 또한 다른 프레임워크에서 내장된 웹 서버 구성 요소로 사용할 수 있습니다.

### 필요 조건

이 실습에서는 다음 사항이 있다고 가정합니다.

*   Oracle Cloud 계정

## 자세히 알아보기

*   [https://helidon.io](https://helidon.io)

## 확인

*   **작성자** - Joe DiPol
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **Last Updated By/Date** - Ankit Pandey, 2023년 2월
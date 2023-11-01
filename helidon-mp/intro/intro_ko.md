# 소개

## 이 워크샵 정보

이 연습에서는 Helidon Project Starter를 사용하여 HTTP 마이크로 서비스 응용 프로그램을 개발합니다. 사용자가 프로젝트에 추가할 Helidon 기능을 선택할 수 있는 다양한 옵션을 제공하는 것은 매우 사용자 정의가 가능합니다.

그런 다음 OCI Code Editor를 살펴보고 Helidon 응용 프로그램을 엽니다. 또한 Oracle Cloud Infrastructure(OCI) Cloud Shell에서 GraalVM Enterprise Edition을 사용하는 방법을 배웁니다.

maven을 사용하여 Helidon MP 애플리케이션용 GraalVM 네이티브 이미지를 빌드합니다. 그런 다음 기존 Java 클래스에 사용자정의 끝점을 추가하여 애플리케이션을 수정합니다. 나중에 이 애플리케이션의 네이티브 도커 이미지를 빌드하여 Oracle Cloud Container Image Registry로 푸시할 것입니다. 또한 컴퓨트 인스턴스를 생성하고 여기에 도커 이미지를 가져온 다음 이 애플리케이션에 대한 도커 컨테이너를 실행합니다.

이 워크샵은 가능한 한 자체 설명이 가능하도록 설계되었지만 언제든지 설명을 요청하거나 도움을 요청하십시오.

예상 시간: 90분

### 목표

*   Helidon MP 애플리케이션 생성
*   OCI 코드 편집기 살펴보기
*   Helidon MP 애플리케이션에 대한 네이티브 이미지 생성
*   Helidon 애플리케이션에 대한 GraalVM 고유 도커 이미지 생성
*   가상 시스템 만들기
*   Helidon MP 애플리케이션용 도커 컨테이너 실행

### 필요 조건

이 실습에서는 다음 사항이 있다고 가정합니다.

*   Oracle Cloud 계정

## Helidon 정보

Helidon은 Oracle에서 도입된 오픈 소스 마이크로서비스 프레임워크로, 가볍고 빠른 마이크로서비스 기반 애플리케이션을 생성하기 위해 설계된 Java 라이브러리 모음을 제공합니다. 이 프레임워크는 마이크로서비스 작성을 위한 두 가지 프로그래밍 모델인 Helidon SE 및 Helidon MP를 지원합니다.

Helidon SE는 반응적 프로그래밍 모델을 지원하는 마이크로프레임워크로 설계되었지만 Helidon MP는 MicroProfile 사양의 구현입니다. MicroProfile는 Java EE에 뿌리를 두고 있으므로 MicroProfile API는 많은 주석 사용에 익숙한 선언적 접근 방식을 따릅니다. 따라서 Java EE 개발자에게 적합합니다.

MicroProfile 기능은 마이크로서비스 구현을 목표로 합니다. REST 클라이언트 정의, 애플리케이션 모니터링, 기술 및 기능 통계 읽기, 애플리케이션 구성을 위한 API를 찾을 수 있습니다. 또한 Helidon은 Microprofile API의 핵심 세트에 API를 추가하여 최신 클라우드 전용 애플리케이션을 작성하는 데 필요한 모든 기능을 제공합니다.

> [MicroProfile](https://microprofile.io/) 표준은 Jakarta EE를 기반으로 합니다. Jakarta EE와 마찬가지로 MicroProfile는 오픈 소스이며 Eclipse Foundation에서 개발합니다. MicroProfile를 사용한 구현은 Jakarta EE와 마찬가지로 표준을 구현하는 라이브러리 또는 애플리케이션 서버에서 이루어집니다.

## 자세히 알아보기

*   [https://helidon.io](https://helidon.io)

## 확인

*   **작성자** - Dmitry Aleksandrov
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 4월
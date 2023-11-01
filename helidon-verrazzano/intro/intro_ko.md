# 소개

## 이 워크샵 정보

이 실습에서는 참석자에게 Helidon 및 Verrazzano를 통한 실습 세션을 제공합니다. 서비스를 만들고 어셈블하여 엔터프라이즈 컨테이너 플랫폼에 배치하는 방법을 배웁니다.

BYOL(Bring Your Laptop) 세션이므로 Windows, OSX 또는 Linux 랩탑을 가져옵니다. 시작하기 전에 JDK 11+, Apache Maven(3.6) 및 Docker가 설치되어 있어야 합니다.

이 연습에서는 Helidon CLI 도구를 설치하고 HTTP 마이크로 서비스 응용 프로그램을 개발합니다. Verrazzano의 경우 Oracle Cloud Free Tier 계정을 사용하여 Oracle Cloud Infrastructure에서 Oracle Kubernetes Engine(OKE)을 설정합니다. Free Tier 계정은 엔터프라이즈 수준에서 마이크로서비스 애플리케이션을 실행하고 운영하는 방법을 탐색하고 배우기에 충분합니다.

이 워크샵의 목표는 Helidon 및 Verrazzano 사용에 대한 기본 사항을 배우고 프로젝트에서 도울 수있는 방법을 이해하는 것입니다. 이러한 프로젝트의 기능에 대해 자세히 알아보려면 Oracle Free Tier 클라우드 계정 및 Oracle Cloud Infrastructure를 사용하여 계속 탐색하십시오.

이 워크샵은 가능한 한 자체 설명이 가능하도록 설계되었지만 언제든지 설명을 요청하거나 도움을 요청하십시오.

> Oracle Kubernetes Engine(OKE)을 프로비저닝하고 Verrazzano를 설치하는 데 몇 분 정도 걸릴 수 있습니다. 시간을 절약하기 위해 개발 및 환경 설정을 병렬로 수행해야 합니다. 지침을 따르고 필요한 경우 작업을 전환합니다.

예상 시간: 90분

### 목표

*   Oracle Cloud Free Tier 계정을 설정합니다(아직 설정하지 않은 경우).
*   Oracle Cloud Infrastructure에서 Oracle Kubernetes Engine 인스턴스를 설정합니다.
*   Verrazzano 플랫폼을 설치합니다.
*   _Helidon MP_ 애플리케이션을 배치합니다.
*   OpenSearch, Grafana, Prometheus 등 Verrazzano 도구를 사용하여 _Helidon MP_ 애플리케이션을 모니터링합니다.

### 필요 조건

이 실습에서는 시스템에 다음이 설치되어 있다고 가정합니다.

*   JDK 11+
*   Apache Maven(3.6)
*   Docker

## Helidon 정보

Helidon은 Oracle에서 도입된 오픈 소스 마이크로서비스 프레임워크로, 가볍고 빠른 마이크로서비스 기반 애플리케이션을 생성하기 위해 설계된 Java 라이브러리 모음을 제공합니다. 이 프레임워크는 마이크로서비스 작성을 위한 두 가지 프로그래밍 모델인 Helidon SE 및 Helidon MP를 지원합니다.

Helidon SE는 반응적 프로그래밍 모델을 지원하는 마이크로프레임워크로 설계되었지만 Helidon MP는 MicroProfile 사양의 구현입니다. MicroProfile는 Java EE에 뿌리를 두고 있으므로 MicroProfile API는 많은 주석 사용에 익숙한 선언적 접근 방식을 따릅니다. 따라서 Java EE 개발자에게 적합합니다.

MicroProfile 기능은 마이크로서비스 구현을 목표로 합니다. REST 클라이언트 정의, 애플리케이션 모니터링, 기술 및 기능 통계 읽기, 애플리케이션 구성을 위한 API를 찾을 수 있습니다. 또한 Helidon은 Microprofile API의 핵심 세트에 API를 추가하여 최신 클라우드 전용 애플리케이션을 작성하는 데 필요한 모든 기능을 제공합니다.

> [MicroProfile](https://microprofile.io/) 표준은 Jakarta EE를 기반으로 합니다. Jakarta EE와 마찬가지로 MicroProfile는 오픈 소스이며 Eclipse Foundation에서 개발합니다. MicroProfile를 사용한 구현은 Jakarta EE와 마찬가지로 표준을 구현하는 라이브러리 또는 애플리케이션 서버에서 이루어집니다.

## Verrazzano 소개

Verrazzano는 멀티 클라우드 및 하이브리드 환경에서 클라우드 네이티브 및 기존 애플리케이션을 배포하기 위한 엔드투엔드 엔터프라이즈 컨테이너 플랫폼입니다. 그것은 선별 된 오픈 소스 구성 요소들로 구성되어 있습니다 - 많은 당신이 이미 사용하고 신뢰 할 수 있습니다, 그리고 일부는 Verrazzano를 응집하고 사용하기 쉬운 플랫폼으로 만드는 모든 조각을 모으기 위해 특별히 작성되었습니다.

Verrazzano에는 다음과 같은 기능이 포함되어 있습니다.

*   하이브리드 및 다중 클러스터 워크로드 관리
*   WebLogic, Coherence 및 Helidon 애플리케이션에 대한 특별 처리
*   다중 클러스터 Infrastructure 관리
*   사전 유선 통합 애플리케이션 모니터링
*   통합된 보안
*   DevOps 및 GitOps 역량 강화

![Verrazzano ](images/verrazzano.png)

## 자세히 알아보기

*   [https://helidon.io](https://helidon.io)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
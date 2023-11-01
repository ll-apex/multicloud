# 소개

## 이 워크샵 정보

이 워크샵에서는 Verrazzano를 사용하여 Spring Boot 애플리케이션을 Oracle Kubernetes Engine(OKE) 클러스터에 배포하는 방법을 살펴봅니다.

오늘날의 디지털 시대에 기업들은 보다 빠르고 효율적으로 애플리케이션을 개발하고 배포할 수 있는 방법을 모색하고 있습니다. Kubernetes는 컨테이너 통합관리를 위한 실질적인 표준이 되어 조직이 컨테이너화된 애플리케이션을 배포, 관리 및 확장할 수 있게 되었습니다. OKE(Oracle Kubernetes Engine)는 OCI(Oracle Cloud Infrastructure)에서 컨테이너화된 애플리케이션의 배포 및 관리를 간소화하는 관리형 Kubernetes 서비스를 제공합니다.

Spring Boot는 웹 애플리케이션 및 마이크로서비스를 구축하는 데 사용되는 인기 있는 Java 기반 프레임워크입니다. 컨벤션 오버 구성(convention-over-configuration) 접근 방식을 통해 간소화된 개발 경험을 제공함으로써 컨테이너화된 애플리케이션을 Kubernetes에 구축하고 배포할 수 있는 탁월한 선택입니다.

Verrazzano는 클라우드 네이티브 애플리케이션의 배포 및 관리를 위한 완벽한 엔드투엔드 솔루션을 제공하는 오픈 소스 Kubernetes 네이티브 플랫폼입니다. 애플리케이션 토폴로지에 대한 통합 뷰와 배포, 모니터링 및 관리를 위한 통합 툴 세트를 제공하여 복잡한 다중 구성요소 애플리케이션의 배포를 간소화합니다.

이 워크샵은 가능한 한 자체 설명이 가능하도록 설계되었지만 언제든지 설명을 요청하거나 도움을 요청하십시오.

예상 시간: 90분

### 목표

*   Oracle Cloud Free Tier 계정을 설정합니다(아직 설정하지 않은 경우).
*   OCI에서 OKE 클러스터 설정
*   클러스터에 Verrazzano 설치 및 구성
*   Spring Boot 애플리케이션에 대한 컨테이너 이미지 빌드
*   Verrazzano를 사용하여 OKE 클러스터에 애플리케이션 배포
*   Verrazzano의 통합 도구를 사용하여 애플리케이션 모니터링 및 관리

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.

## 자세히 알아보기

**Springboot 정보**

Spring Boot는 웹 애플리케이션 및 마이크로서비스를 구축하는 데 사용되는 인기 있는 Java 기반 프레임워크입니다. 컨벤션 오버 구성(convention-over-configuration) 접근 방식을 통해 간소화된 개발 경험을 제공함으로써 컨테이너화된 애플리케이션을 Kubernetes에 구축하고 배포할 수 있는 탁월한 선택입니다.

Spring Boot는 임베디드 서버, 자동 구성, 웹 애플리케이션, 메시징 시스템 및 데이터 기반 애플리케이션 구축을 위한 다양한 라이브러리 및 도구와 같이 애플리케이션을 쉽게 구축하고 배포할 수 있는 다양한 기능을 제공합니다. 또한 테스트 및 디버깅을 위한 다양한 도구를 제공하므로 강력하고 신뢰할 수 있는 코드를 보다 쉽게 작성할 수 있습니다.

Spring Boot의 주요 이점 중 하나는 데이터베이스 시스템, 메시지 대기열 및 캐싱 시스템을 포함한 다른 기술 및 프레임워크와 쉽게 통합할 수 있다는 것입니다. 이를 통해 개발자는 대규모 워크로드를 처리하고 최신 클라우드 기반 애플리케이션의 요구사항을 충족할 수 있는 복잡한 분산 시스템을 구축할 수 있습니다.

간편성과 사용 편의성에 중점을 둔 Spring Boot는 최신 클라우드 네이티브 애플리케이션을 구축하려는 개발자 및 조직 중 인기 있는 선택이 되었습니다. 기여자 및 사용자로 구성된 활발한 커뮤니티를 보유하고 있으며 변화하는 업계의 요구를 충족하기 위해 지속적으로 진화하고 있습니다.

**Verrazzano 소개**

Verrazzano는 멀티 클라우드 및 하이브리드 환경에서 클라우드 네이티브 및 기존 애플리케이션을 배포하기 위한 엔드투엔드 엔터프라이즈 컨테이너 플랫폼입니다. 그것은 선별 된 오픈 소스 구성 요소들로 구성되어 있습니다 - 많은 당신이 이미 사용하고 신뢰 할 수 있습니다, 그리고 일부는 Verrazzano를 응집하고 사용하기 쉬운 플랫폼으로 만드는 모든 조각을 모으기 위해 특별히 작성되었습니다.

Verrazzano에는 다음과 같은 기능이 포함되어 있습니다.

*   하이브리드 및 다중 클러스터 워크로드 관리
*   WebLogic, Coherence 및 Helidon 애플리케이션에 대한 특별 처리
*   다중 클러스터 Infrastructure 관리
*   사전 유선 통합 애플리케이션 모니터링
*   통합된 보안
*   DevOps 및 GitOps 역량 강화

![Verrazzano ](images/verrazzano.png)

*   [https://spring.io/](https://spring.io/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 4월
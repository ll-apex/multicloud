# 소개

## 이 워크샵 정보

Kubernetes 클러스터에 Tomcat 애플리케이션을 배포하는 것은 여러 컨테이너 관리, 네트워킹 구성, 고가용성 및 확장성 보장을 포함하는 복잡한 프로세스일 수 있습니다. 이 프로세스를 간소화하기 위해 많은 조직이 Docker 및 Kubernetes와 같은 컨테이너화 기술로 전환하고 있습니다.

Docker를 사용하면 모든 플랫폼에 배포할 수 있는 휴대용 컨테이너 이미지를 만들 수 있으며, Kubernetes는 컨테이너의 배포 및 확장을 관리할 수 있는 강력한 통합관리 엔진을 제공합니다.

Oracle Verrazzano는 Kubernetes 클러스터에서 컨테이너화된 애플리케이션을 배포 및 운영하기 위한 통합 관리 경험을 제공하는 클라우드 네이티브 플랫폼입니다. 여러 클러스터에서 애플리케이션을 관리하고 모니터하는 데 사용할 수 있는 일관된 툴과 API 집합을 제공하여 복잡한 애플리케이션의 배치를 간소화합니다.

이 워크샵에서는 Verrazzano를 사용하여 Oracle Kubernetes 클러스터에 Tomcat 애플리케이션 Docker 이미지를 배포하는 프로세스를 살펴봅니다. Docker 이미지 생성, Verrazzano 구성, 애플리케이션 배포, 성능 모니터링과 관련된 단계를 다룹니다.

이 워크숍이 끝나면 Verrazzano를 사용하여 컨테이너화된 애플리케이션을 Oracle Kubernetes 클러스터에 배포하는 방법을 명확하게 이해할 수 있습니다.

이 워크샵은 가능한 한 자체 설명이 가능하도록 설계되었지만 언제든지 설명을 요청하거나 도움을 요청하십시오.

예상 시간: 90분

### 목표

*   Oracle Cloud Free Tier 계정을 설정합니다(아직 설정하지 않은 경우).
*   Oracle Cloud Infrastructure에서 Oracle Kubernetes Engine 인스턴스를 설정합니다.
*   Verrazzano 생산 프로파일을 설치합니다.
*   샘플 Tomcat 애플리케이션 도커 이미지를 생성합니다.
*   Verrazzano를 사용하여 OKE에 Tomcat 애플리케이션을 배치합니다.
*   Grafana, Prometheus 및 OpenSearch 대시보드 콘솔을 살펴보세요.

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.

## 자세히 알아보기

**Tomcat 소개**

Apache Tomcat은 Java 기반 웹 응용 프로그램을 배치하는 데 널리 사용되는 오픈 소스 웹 서버 및 서블릿 컨테이너입니다. Tomcat은 가볍고 효율적이며 사용하기 쉬운 디자인으로 웹 애플리케이션 관리 및 배포를 위한 다양한 기능을 제공합니다.

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

*   [https://tomcat.apache.org/](https://tomcat.apache.org/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
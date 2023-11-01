# 소개

## 이 워크샵 정보

이 워크샵에서는 **OCI DevOps 서비스**를 사용하여 **Helidon 마이크로서비스 애플리케이션**을 처음부터 끝까지 구축, 유지 관리 및 업그레이드하는 방법을 알아봅니다. 가상 스레드 기술과 함께 JDK20를 활용하여 전체 라이프사이클 관리 프로세스를 가속화하고 간소화하는 방법을 시연합니다.

추정시간: 5분

### 목표

이 워크샵에서는 다음 작업을 수행합니다.

*   구획, 동적 그룹 및 정책 생성
*   Terraform을 사용하여 DevOps 프로젝트 및 관련 리소스 생성
*   OCI를 사용하여 Helidon MP 애플리케이션을 구축하고 컴퓨팅 인스턴스에 배포 DevOps
*   OCI 모니터링 SDK 통합 표시
*   OCI 로깅 SDK 통합을 검증하여 로깅 서비스에 메시지를 푸시합니다.
*   패치 시나리오 시뮬레이션
*   Helidon MP 애플리케이션에서 오브젝트 스토리지 액세스 추가

### 필요 조건

*   Oracle Free Tier(체험판), 유료 또는 LiveLabs 클라우드 계정
*   [OCI 콘솔에 대한 기본적인 이해](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
*   [네트워킹 개요](https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm)
*   [구획에 대한 기본적인 이해](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/concepts.htm)
*   [OCI DevOps 서비스](https://docs.oracle.com/en-us/iaas/Content/devops/using/home.htm)

## Oracle DevOps

Oracle Cloud Infrastructure DevOps 서비스는 개발자를 위한 엔드투엔드 CI/CD 플랫폼을 제공합니다. OCI DevOps 서비스는 소프트웨어 라이프사이클의 모든 필수 요구사항을 포괄합니다. 다음과 같은 경우

*   **OCI 배포 파이프라인** - VM 및 베어메탈, OKE(Oracle Container Engine for Kubernetes) 및 OCI 기능과 같은 OCI 플랫폼에 대한 선언적 파이프라인 릴리스 전략을 사용하여 릴리스를 자동화합니다.
*   **OCI 아티팩트 저장소** - 변경할 수 없는 아티팩트를 포함하여 버전이 지정된 아티팩트를 저장할 위치입니다.
*   **OCI 코드 저장소** - OCI가 확장 가능한 코드 저장소 서비스를 제공했습니다.
*   **OCI 빌드 파이프라인** - 빌드, 테스트, 아티팩트 및 배포 호출을 자동화하는 확장 가능한 서버리스 서비스입니다.

![DevOps 아키텍처](images/oci-devops.png)

## 롤 재생 구조

OCI 서비스 및 기능을 사용하여 아래 언급된 아키텍처를 구축하고 배포할 것입니다.

![Devops 다이어그램](images/devops-diagram.png)

이제 **다음 실습으로 진행할 수 있습니다.**

## 자세히 알아보기

*   [참조 아키텍처: Oracle Cloud Infrastructure DevOps를 통한 최신 앱 배포 전략 이해](https://docs.oracle.com/en/solutions/mod-app-deploy-strategies-oci/index.html)
*   [https://helidon.io/](https://helidon.io/)
*   [https://medium.com/helidon](https://medium.com/helidon)
*   [https://github.com/helidon-io/helidon](https://github.com/helidon-io/helidon)
*   [https://www.youtube.com/Helidon\_Project](https://www.youtube.com/Helidon_Project)
*   [https://helidon.slack.com](https://helidon.slack.com)

## 확인

*   **작성자** - Keith Lustria
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **Last Updated By/Date** - Ankit Pandey, 2023년 5월
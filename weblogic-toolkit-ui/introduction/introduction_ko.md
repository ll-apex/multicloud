# 소개

## 이 워크샵 정보

이 워크샵은 온프레미스 WebLogic 서버 도메인을 컨테이너로 엔드 투 엔드 마이그레이션하여 Oracle Container Engine for Kubernetes(OKE)를 통해 OCI에서 실행할 수 있도록 합니다. WebLogic Kubernetes Toolkit UI와 WebLogic Deployer Tool 및 Weblogic Kubernetes Operator의 그래픽 인터페이스를 시연합니다. DevOps 지향 툴 세트를 사용하여 마이그레이션 프로세스를 간소화하고 가속화하는 방법을 보여줍니다.

![실험실 흐름](images/lab-flow.png)

예상 시간: 120분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [전체 워크샵](videohub:1_q1mmkimy)

### 제품/기술 정보

WebLogic Kubernetes 툴킷(WKT)은 Kubernetes 클러스터의 Linux 컨테이너에서 실행되도록 WebLogic 기반 애플리케이션을 프로비저닝하는 데 도움이 되는 오픈 소스 툴 모음입니다. WKT에는 다음과 같은 도구가 포함되어 있습니다.  

*   [WebLogic WDT(배치 도구)](https://github.com/oracle/weblogic-deploy-tooling) - WebLogic 도메인의 단일 메타데이터 모델 표현에서 작동하는 단일 용도의 수명 주기 도구 세트입니다.
*   [WebLogic WIT(이미지 도구)](https://github.com/oracle/weblogic-image-tool) - WebLogic 도메인을 실행하기 위한 Linux 컨테이너 이미지를 만드는 도구입니다.
*   [WebLogic Kubernetes Operator(WKO)](https://github.com/oracle/weblogic-kubernetes-operator) - WebLogic 도메인이 Kubernetes 클러스터에서 기본적으로 실행되도록 허용하는 Kubernetes 연산자입니다.

WKT UI는 WKT 툴, Docker, Helm, Kubernetes 클라이언트(kubectl)를 래핑하고 모델 생성 및 수정 과정을 안내하는 그래픽 사용자 인터페이스를 제공합니다. WebLogic 도메인, 도메인 실행에 사용할 Linux 컨테이너 이미지 생성, Kubernetes 클러스터의 도메인 배치 및 액세스에 필요한 소프트웨어 및 구성 설정 및 배치

### 목표

*   마켓플레이스 이미지에서 가상 머신 만들기
*   Oracle Cloud Infrastructure에 Oracle Kubernetes Engine 인스턴스 설정
*   WKT UI 프로젝트 수정 및 모델 파일 만들기
*   보조 이미지 생성 및 Oracle Container Image Registry에서 푸시
*   Oracle Kubernetes 클러스터(OKE)에 WebLogic Operator 배포
*   Oracle Kubernetes 클러스터(OKE)에 WebLogic 도메인 배치
*   Oracle Kubernetes 클러스터에 수신 컨트롤러 배치(OKE)
*   Managed Server Pod 간 로드 균형 조정 표시 및 WebLogic-Remote-Console 탐색
*   WebLogic 클러스터 확장
*   새 이미지의 롤링 재시작을 통해 배치된 애플리케이션 업데이트

## 자세히 알아보기

*   [WebLogic Kubernetes 툴킷 UI](https://oracle.github.io/weblogic-toolkit-ui/)

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 8월
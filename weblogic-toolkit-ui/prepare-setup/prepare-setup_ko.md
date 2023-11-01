# 설정 준비

## 소개

이 실습에서는 이 워크샵을 실행하는 데 필요한 리소스를 설정하는 데 필요한 ORM(Oracle Resource Manager) 스택 zip 파일을 다운로드하는 방법을 설명합니다. 이 워크샵에는 컴퓨트 인스턴스 및 VCN(가상 클라우드 네트워크)이 필요합니다.

예상 시간: 10분

### 목표

*   ORM 스택 다운로드
*   기존 VCN(가상 클라우드 네트워크) 구성

### 필요 조건

이 실습에서는 다음 사항이 있다고 가정합니다.

*   Oracle Free Tier 또는 유료 클라우드 계정

## 작업 1: ORM(Oracle Resource Manager) 스택 zip 파일 다운로드

1.  환경을 구축하는 데 필요한 리소스 관리자 zip 파일을 다운로드하려면 아래 링크를 누르십시오.
    
    _주 1:_ 워크샵에 대해 단일 스택 다운로드를 제공하는 경우 이 간단한 표현식을 사용합니다.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  다운로드 폴더에 저장합니다.
    

이 스택을 사용하여 인스턴스에 자체 포함/전용 VCN을 생성하는 것이 좋습니다. 권장 사항을 따르려면 _Task 3_으로 건너뜁니다. 기존 VCN을 사용하려면 아래 표시된 대로 다음 단계를 진행하여 필요한 송신 규칙으로 기존 VCN을 업데이트하십시오.

## 작업 2: 기존 VCN에 보안 규칙 추가

이 워크샵에서는 특정 개수의 포트를 사용할 수 있어야 하며, 전용 VCN을 생성하는 기본 ORM 스택 실행을 통해 충족해야 합니다. 기존 VCN을 사용하려면 다음 포트를 수신 규칙에 추가해야 합니다.

| 항구 | 설명 |
| :-- | :-- |
| 22개 | SSH |
| 80개 | noVNC 원격 데스크톱(NGINX 프록시) |
| 6080년 | noVNC 원격 데스크톱 |

1.  _네트워킹 >> 가상 클라우드 네트워크_로 이동합니다.
2.  네트워크를 선택합니다.
3.  Resources 아래에서 Security Lists를 선택합니다.
4.  Create Security List 버튼 아래에서 Default Security Lists를 누릅니다.
5.  Add Ingress Rule 버튼을 누릅니다.
6.  다음을 입력합니다.
    *   소스 CIDR: 0.0.0.0/0
    *   대상 포트 범위: _위 표 참조_
7.  Add Ingress Rules 버튼을 누릅니다.

## 작업 3: 컴퓨트 설정

위의 두 작업에서 세부 정보를 사용하여 실습 _환경 설정_으로 이동하여 ORM(Oracle Resource Manager) 및 다음 옵션 중 하나를 사용하여 워크샵 환경을 설정합니다.

*   스택 생성: _컴퓨트 + 네트워킹_
*   스택 생성: 위의 _작업 2_에 따라 보안 목록이 업데이트된 기존 VCN이 있는 _컴퓨트 전용_

이제 다음 실습을 진행할 수 있습니다.

## 확인

*   **작성자** - Rene Fontcha, LiveLabs 플랫폼 리드, NA 기술
*   **기여자** - Meghana Banka
*   **최종 업데이트 수행자/날짜** - Rene Fontcha, LiveLabs 플랫폼 리드, NA 기술, 2022년 2월
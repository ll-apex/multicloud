# 설정 준비

## 소개

이 실습에서는 이 워크샵을 실행하는 데 필요한 리소스를 설정하는 데 필요한 ORM(Oracle Resource Manager) 스택 zip 파일을 다운로드하는 방법을 설명합니다. 그런 다음 원격 데스크톱에 대한 액세스를 제공하는 컴퓨트 인스턴스 및 VCN(가상 클라우드 네트워크)을 생성합니다.

예상 시간: 10분

### 목표

*   ORM 스택 다운로드
*   리소스 관리자 스택을 사용하여 컴퓨트 + 네트워킹 생성

### 필요 조건

이 실습에서는 다음 사항이 있다고 가정합니다.

*   Oracle Free Tier 또는 유료 클라우드 계정

## 작업 1: ORM(Oracle Resource Manager) 스택 zip 파일 다운로드

1.  환경을 구축하는 데 필요한 리소스 관리자 zip 파일을 다운로드하려면 아래 링크를 누르십시오.
    
    _주 1:_ 워크샵에 대해 단일 스택 다운로드를 제공하는 경우 이 간단한 표현식을 사용합니다.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  다운로드 폴더에 저장합니다.
    

## 작업 2: 스택 생성: 컴퓨트 + 네트워킹

1.  **Task 1: Download Oracle Resource Manager (ORM) stack zip file**에서 다운로드한 ORM 스택 zip 파일을 식별합니다.
    
2.  왼쪽 상단 모서리에 햄버거 메뉴를 엽니다. **개발자 서비스**를 누르고 **리소스 관리자** > **스택**을 선택합니다. 스택을 설치할 구획을 선택합니다. **스택 생성**을 누릅니다. ![메뉴 스택](images/menu-stack.png) ![구획 선택](images/select-compartment.png)
    
3.  **내 구성**을 선택하고 **을 선택합니다. Zip** 파일 단추를 누르고 **찾아보기** 링크를 누른 다음 다운로드한 zip 파일을 선택하거나 파일 탐색기에 대한 끌어 놓기를 선택합니다. **다음**을 누릅니다. ![zip 찾아보기](images/browse-zip.png)
    
4.  다음을 입력하거나 선택하고 **다음**을 누릅니다.
    
    **Instance Count:** 기본값인 1을 적용합니다.
    
    **가용성 도메인 선택:** 드롭다운 목록에서 가용성 도메인을 선택합니다.
    
    **SSH를 통한 원격 액세스가 필요하십니까?** Keep Unchecked for Remote Desktop only Access - 기본값입니다.
    
    **조정 가능한 OCPU 수에 가변 인스턴스 구성 사용?:** 고정된 구성을 사용하려는 경우가 아니면 기본값을 선택된 상태로 유지합니다.
    
    **인스턴스 구성:** 기본값을 유지하거나 드롭다운 메뉴의 가변 구성 목록에서 선택합니다(예: VM.Standard.E4). 가변).
    
    **인스턴스당 OCPU 수 선택:** 표시된 기본값을 그대로 사용합니다. 예를 들어, (1)은 OCPU 1개와 메모리 16GB를 프로비전합니다.
    
    **기존 VCN 사용?:** 이 항목을 선택 해제된 상태로 유지하여 기본값을 적용합니다. 새 VCN이 생성됩니다. ![기본 구성](images/main-config.png) ![인스턴스 구성](images/instance-shape.png)
    
5.  **적용 실행**을 선택하고 **생성**을 누릅니다. ![적용 실행](images/run-apply.png)
    

이제 다음 실습을 진행할 수 있습니다.

## 확인

*   **작성자** - Ankit Pandey
*   **기여자** - Maciej Gruszka, Sid Joshi
*   **Last Updated By/Date** - Ankit Pandey, 2023년 10월
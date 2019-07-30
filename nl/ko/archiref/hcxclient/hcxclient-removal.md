---

copyright:

  years:  2016, 2019

lastupdated: "2019-06-17"

subcollection: vmware-solutions


---

# HCX 수정 또는 설치 제거
{: #hcxclient-removal-uninstall}

기존 설치가 업그레이드되거나 일부 또는 전체 하이브리드 클라우드 서비스 배치가 제거될 수 있습니다.

##  계층 2 네트워크 확장 해제
{: #hcxclient-removal-uninstall-unstretch-layer2}

연관된 Layer 2 Concentrator 서비스 가상 어플라이언스를 제거하거나 하이브리드 클라우드 서비스를 설치 제거하기 전에 계층 2 네트워크를 확장 해제해야 합니다.

1. 확장된 네트워크를 확인하십시오.
2. 하이브리드 클라우드 서비스 플러그인 페이지에서 하이브리드 서비스 탭을 보고 네트워크 확장 서비스 섹션을 확인하십시오. 활성 또는 스케줄된 작업이 진행 중인 경우 해당 작업이 완료되었거나 작업을 중지할 때까지 기다린 후 계속 진행하십시오.
3. 네트워크를 제거하려면 오른쪽에 있는 삭제(빨간색 X) 아이콘을 클릭하십시오.
4. 확인하려면 **확인**을 클릭하십시오.

## HCX 가상 어플라이언스 설치 제거
{: #hcxclient-removal-uninstall-uninst-hva}

서비스 어플라이언스는 하이브리드 클라우드 서비스 설치 제거를 준비하기 위해 설치 제거되거나 설치 아키텍처의 변경으로 인해 설치 제거될 수 있습니다. 다음 프로시저에 설명된 대로 하이브리드 클라우드 서비스를 사용하여 어플라이언스를 관리하십시오.

### HCX 가상 어플라이언스 설치 제거에 대한 전제조건
{: #hcxclient-removal-uninstall-prereq-uninst-hva}

* 설치 제거 태스크 중에 발생할 수 있는 마이그레이션의 실행 시간을 취소하거나 재설정하십시오.
* 실행 중인 마이그레이션의 vSphere Web Client 태스크 콘솔을 확인하고 마이그레이션이 완료될 때까지 기다리십시오.
* 모든 유형에서 활성 상태의 하이브리드 클라우드 서비스 태스크가 없는지 확인하십시오.

vSphere 인벤토리에서 가상 어플라이언스를 삭제하지 마십시오. 서비스 가상 어플라이언스와 상호작용하도록 항상 관리 포털을 사용하십시오.
{:note}

### HCX 가상 어플라이언스를 설치 제거하는 프로시저
{: #hcxclient-removal-uninstall-proc-uninst-hva}

1. vSphere Web Client 인터페이스의 왼쪽 분할창에서 하이브리드 클라우드 서비스 플러그인을 선택하십시오.
2. 가운데 분할창에서 **하이브리드 서비스** 탭을 클릭하십시오.
3. 하이브리드 클라우드 게이트웨이 어플라이언스를 찾고 세부사항을 표시할 항목을 클릭하십시오.
4. 오른쪽 하단에서 삭제 아이콘을 클릭하여 어플라이언스를 제거하십시오.
5. 확장된 네트워크가 하이브리드 클라우드 게이트웨이와 IP 주소를 공유하지 않는 경우 별도로 제거해야 합니다. 네트워크 확장 서비스 세부사항을 펼치고 삭제 아이콘을 클릭하여 Layer 2 Concentrator를 제거하십시오.

하이브리드 클라우드 게이트웨이와 하이브리드 클라우드 게이트웨이를 사용하는 하이브리드 서비스 가상 어플라언스는 vCenter 및 VCS 하이브리드 클라우드 서비스 클라우드 모두에서 제거됩니다.

## HCX Manager 설치 제거
{: #hcxclient-removal-uninstall-unist-hcxm}

온프레미스 데이터 센터에서 HCX 솔루션을 제거하기 전에 HCX Manager 어플라이언스를 설치 제거해야 합니다. 하이브리드 클라우드 서비스 가상 머신을 설치 제거하려면 다음 단계를 따르십시오.

1. 모든 계층 2 네트워크를 확장 해제하십시오.
2. 하이브리드 서비스 가상 어플라이언스를 제거하십시오.
3. 온프레미스 vCenter에서 하이브리드 클라우드 서비스 가상 머신을 끄십시오.
4. 하이브리드 클라우드 서비스 가상 머신을 삭제하십시오.

모든 가상 서비스 어플라이언스가 제거됩니다. 다음 요소가 남아 있을 수 있습니다.
* 로그
* 마이그레이션된 VM

### 수행할 작업
{: #hcxclient-removal-uninstall-what-next}

마이그레이션된 VM 및 로그는 수동으로 백업되거나 삭제될 수 있습니다.

## HCX 관리 포털에 로그인
{: #hcxclient-removal-uninstall-log-hcxmp}

하이브리드 클라우드 서비스 배치는 브라우저 기반 사용자 인터페이스를 사용하여 관리 포털에서 관리될 수 있습니다.

1. 웹 브라우저에서 하이브리드 클라우드 서비스에 지정된 IP 주소를 입력하고 포트 번호 9443을 지정하십시오. 예를 들면, `https://HCXip:9443`입니다.
2. 하이브리드 클라우드 서비스 사용자 인터페이스는 SSL을 사용하여 웹 브라우저 창에서 열립니다. 필요한 경우, 보안 인증서를 승인하십시오. VMware Hybridity 및 네트워킹 로그인 화면이 열립니다.
3. 사용자 이름 및 비밀번호를 입력하십시오. 기본적으로 사용자 이름은 Admin입니다. 비밀번호는 하이브리드 클라우드 서비스 가상 어플라이언스가 설치될 때 제공되는 값입니다.

## 관련 링크
{: #hcxclient-removal-related}

* [HCX 컴포넌트 용어집](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-components)
* [설치 환경 준비](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-planning-prep-install)
* [HCX 클라이언트 배치](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-vcs-client-deployment)
* [HCX 온프레미스 서비스 메시](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-vcs-mesh-deployment)
* [VMware 하이브리드 클라우드 마이그레이션](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-migrations)
* [매개변수 및 컴포넌트 모니터링](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-monitoring)
* [HCX 문제점 해결](/docs/services/vmwaresolutions/services?topic=vmware-solutions-hcxclient-troubleshooting)
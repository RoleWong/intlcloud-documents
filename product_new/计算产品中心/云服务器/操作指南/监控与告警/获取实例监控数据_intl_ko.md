## 작업 시나리오

Tencent Cloud는 기본적으로 모든 사용자에게 클라우드 모니터링 기능을 제공하므로 사용자가 수동으로 활성화하지 않아도 됩니다. 사용자가 Tencent Cloud 제품을 사용할 경우에만 클라우드 모니터링에서 데이터 수집 및 모니터링이 시작됩니다. 본 문서는 인스턴스 모니터링 데이터를 얻는 방법에 대해 소개합니다.

## 작업 순서
<dx-tabs>
::: 클라우드 서비스 콘솔을 통해 획득
<dx-alert infotype="explain" title="">
CVM의 콘솔에서 별도의 모니터링 데이터 읽기 기능 페이지를 제공합니다. 사용자는 페이지에서 CVM 인스턴스의 CPU, 메모리, 네트워크 대역폭, 디스크 등의 모니터링 데이터를 조회할 수 있으며, 조회 시간을 자유롭게 조정할 수 있습니다.
</dx-alert>
1. [CVM 콘솔](https://console.cloud.tencent.com/cvm)에 로그인하십시오.
2. 인스턴스의 관리 페이지에서 모니터링 데이터를 조회할 인스턴스 ID를 클릭하여 인스턴스 상세 페이지로 진입하십시오.
3. [모니터링] 탭을 클릭하여 인스턴스 모니터링 데이터를 획득하십시오.
:::
::: 클라우드 모니터링 콘솔을 통해 획득
<dx-alert infotype="explain" title="">
클라우드 모니터링 콘솔은 모든 서비스의 모니터링 데이터를 위한 통합된 게이트입니다. 사용자는 CVM의 CPU, 메모리, 네트워크 대역폭, 디스크 등의 모니터링 데이터를 조회할 수 있으며, 조회 시간을 자유롭게 조정할 수 있습니다.
</dx-alert>
1. [클라우드 모니터링 콘솔](https://console.cloud.tencent.com/monitor/overview)에 로그인하십시오.
2. 왼쪽 사이드바에서 [클라우드 서비스 모니터링]>[CVM]를 클릭한 뒤 "CVM-기본 모니터링" 관리 페이지로 진입하십시오.
3. 모니터링 데이터를 조회할 인스턴스 ID를 클릭한 뒤, 모니터링 상세 페이지로 진입하여 인스턴스 모니터링 데이터를 획득하십시오.
:::
::: API를 통해 획득
사용자는 GetMonitorData 인터페이스를 사용해 모든 제품의 모니터링 데이터를 얻을 수 있습니다. 자세한 내용은 [지표 모니터링 데이터 수집](https://cloud.tencent.com/document/product/248/31014)을 참조하십시오.
:::
</dx-tabs>


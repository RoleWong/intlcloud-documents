데이터 손실이나 손상을 방지하기 위해 데이터베이스를 자동 또는 수동으로 백업할 수 있습니다.

## 백업 개요
### 백업 방식
단일 노드(클라우드 디스크), 2노드(로컬 디스크) 및 3노드(로컬 디스크) TencentDB for MySQL 인스턴스는 데이터베이스의 **자동 백업** 및 **수동 백업**을 지원합니다.

### 백업 유형
2노드 및 3노드 TencentDB for MySQL 인스턴스는 두 가지 백업 유형을 지원합니다.
- **물리적 백업**, 물리적 데이터의 전체 복사본입니다(자동 백업 및 수동 백업 모두 지원).
- **논리적 백업**, SQL 문을 백업합니다(수동 백업의 경우에만 지원).
>?
>- 물리적 백업에서 데이터베이스를 복원하려면 먼저 xbstream을 사용하여 패키지의 압축을 해제해야 합니다. 자세한 내용은 [물리 백업을 사용하여 데이터베이스 복구](https://intl.cloud.tencent.com/document/product/236/31910)를 참고하십시오.
>- 단일 인스턴스의 테이블 수가 100만 개를 초과하면 백업이 실패하고 데이터베이스 모니터링이 영향을 받을 수 있습니다. 단일 인스턴스의 테이블 수를 적절하게 제어하고 100만 미만이 되도록 해야 합니다.
>- Memory 스토리지 엔진에 의해 생성된 테이블의 데이터는 Memory에 저장되므로 이러한 테이블에 대한 물리적 백업을 생성할 수 없습니다. 데이터 손실을 방지하려면 InnoDB 테이블로 교체하는 것이 좋습니다.
>- 인스턴스에 기본 키가 없는 테이블이 많은 경우 백업에 실패할 수 있으며 인스턴스의 고가용성에 영향을 줄 수 있습니다. 이러한 테이블에 대한 기본 키 또는 보조 인덱스를 적시에 생성해야 합니다.

| 물리적 백업의 장점 | 논리적 백업 단점 |
|---------|---------|
| <li>백업 속도가 빠릅니다.<li>스트리밍 백업과 압축을 지원합니다.<li>백업 성공률이 높습니다.<li>복구가 쉽고 효율적입니다.<li>RO 복제본 및 재해 복구 인스턴스 추가와 같은 백업 기반 결합 작업이 더 빨라집니다.<li>물리적 백업이 완료되는 평균 시간은 논리적 백업을 생성하는 데 필요한 평균 시간의 1/8입니다.<li>물리적 백업 가져오기 속도는 논리적 백업보다 약 10배 빠릅니다.| <li>SQL 문을 실행하고 인덱스를 구축하는 데 시간이 걸리므로 복구하는 데 오랜 시간이 필요합니다.<li>특히 대량의 데이터가 있는 경우 백업 속도가 느립니다.<li>백업 중 인스턴스에 대한 부담으로 인해 원본-복제본 지연이 증가할 수 있습니다.<li>부동 소수점의 정밀도 정보가 손실될 수 있습니다.<li>잘못된 뷰 및 기타 문제로 인해 백업이 실패할 가능성이 있습니다.<li>RO 복제본 및 재해 복구 인스턴스 추가와 같은 백업 기반 결합 작업이 느려집니다.</li> |

TencentDB for MySQL 단일 노드(클라우드 디스크)는 스냅샷 백업을 지원합니다. **스냅샷 백업**, 스토리지 레이어 디스크의 스냅샷을 생성하여 백업(자동 백업 및 수동 백업 모두 지원)합니다.

| 스냅샷 백업의 장점 | 스냅샷 백업의 단점 |
|---------|---------|
|<li>백업 속도가 빠릅니다. <li>상대적으로 크기가 작습니다. |다운로드가 지원되지 않습니다. |


### 백업 객체
 | 데이터 백업 | 로그 백업 |
|---------|---------|
 | MySQL용 2노드 및 3노드 TencentDB: <li>자동 백업은 전체 물리적 백업을 지원합니다. <li>수동 백업은 전체 물리적 백업, 전체 논리적 백업 및 단일 데이터베이스/테이블 논리적 백업을 지원합니다. <li>자동 및 수동 백업 모두 압축 및 다운로드할 수 있습니다. <br>MySQL용 단일 노드(클라우드 디스크): <li>자동 백업은 전체 스냅샷 백업을 지원합니다. <li>수동 백업은 전체 스냅샷 백업을 지원합니다. <li>자동 및 수동 백업 모두 다운로드를 지원하지 않습니다. | 단일 노드(클라우드 디스크), 2노드 및 3노드 TencentDB for MySQL은 binlog 백업을 지원합니다: <li>로그 파일은 인스턴스의 백업 용량을 차지합니다. <li>로그 파일은 다운로드할 수 있으나 압축할 수 없습니다. <li>로그 파일 보존 기간을 설정할 수 있습니다.</li>|

## 주의 사항
- 2019년 2월 26일부터 TencentDB for MySQL의 자동 백업 기능은 물리적 백업(기본 유형)만 지원하고 더 이상 논리적 백업을 제공하지 않습니다. 기존 자동 논리적 백업은 자동으로 물리적 백업으로 전환됩니다.
비즈니스 액세스에는 영향을 미치지 않지만 자동 백업 습관에는 영향을 미칠 수 있습니다. 논리적 백업이 필요한 경우 [TencentDB for MySQL 콘솔](https://console.cloud.tencent.com/cdb)에서 수동 백업 방법을 사용하거나 [CreateBackup](https://intl.cloud.tencent.com/document/product/236/15844) API를 호출하여 논리적 백업을 생성할 수 있습니다.
- 인스턴스 백업 파일은 백업 공간을 차지합니다. 백업 공간 사용을 적절하게 계획하는 것이 좋습니다. 프리 티어를 초과하는 백업 공간 사용에는 요금이 부과됩니다. 자세한 내용은 [백업 공간 과금 안내](https://intl.cloud.tencent.com/document/product/236/32344)를 참고하십시오.
- 사용량이 적은 시간에 데이터베이스를 백업하는 것이 좋습니다.
- 보존 기간 경과 후 필요한 백업 파일이 삭제되는 상황을 방지하기 위해 적시에 로컬 파일 시스템에 다운로드해야 합니다.
- 테이블 잠금으로 인한 백업 실패를 방지하기 위해 백업 프로세스 중에 DDL 작업을 수행하지 마십시오.
- 단일 노드 TencentDB for MySQL 인스턴스는 백업할 수 없습니다.

## [MySQL 데이터 자동 백업](id:ZDBFSJ)
### 자동 백업 설정
1. [MySQL 콘솔](https://console.cloud.tencent.com/cdb)에 로그인한 뒤 인스턴스 리스트에서 인스턴스 ID를 클릭하고, 관리 페이지로 이동하여 **백업 복구** > **자동 백업 설정**을 선택합니다.
![](https://qcloudimg.tencent-cloud.cn/raw/da6aad42478cbbddba52d02b1d677238.png)
2. 백업 설정 팝업 창에서, 각 백업 매개변수를 선택하고 **확인**을 클릭합니다. 매개변수 설명은 다음과 같습니다.
>?
> - 롤백 기능은 데이터 백업 및 로그 백업(binlog)의 백업 주기 및 보존 날짜에 의존합니다. 자동 백업 빈도 및 보존 기간을 줄이면 롤백이 영향을 받습니다. 필요에 따라 매개변수를 선택할 수 있습니다. 자세한 내용은 [데이터베이스 롤백](https://intl.cloud.tencent.com/document/product/236/7276)을 참고하십시오.
> 예를 들어 백업 주기를 월요일과 목요일로 설정하고 보존 기간을 7일로 설정하면 7일 이내(데이터 백업 및 로그 백업의 실제 보존 일수) 어느 시점으로든 데이터베이스를 롤백할 수 있습니다.
> - 자동 백업은 수동으로 삭제할 수 없으며, 백업 보존 기간을 설정할 수 있습니다. 백업이 만료되면 자동으로 삭제됩니다.
> - 데이터 백업 및 로그 백업 보존 일수를 늘리면 백업 공간에 대한 추가 요금이 발생할 수 있습니다.
> - 로그 백업의 보존 일수를 줄이면 인스턴스의 데이터 롤백 주기에 영향을 미칠 수 있습니다.

자동 백업 설정에서 데이터 백업 설정은 정기 보존 정책 활성화를 지원하며, 다음은 정기 백업 및 정기 백업 활성화에 대한 매개변수 설정 방법에 대한 설명입니다.
>!정기 백업 보존 시간은 일반 백업에 대해 설정된 보존 시간보다 클 수 있습니다.
>

### 일반 백업 설정 설명
| 매개변수     | 설명 | 
|---------|---------|
| 백업 시작 시간 | <li>기본 백업 시작 시간은 시스템에서 자동으로 할당됩니다. <li>필요에 따라 시작 시간을 설정할 수 있습니다. 사용량이 적은 시간으로 설정하는 것이 좋습니다. 이것은 백업 프로세스의 시작 시간일 뿐이며 종료 시간을 나타내지는 않습니다. <br>예를 들어 백업 시작 시간을 02:00 - 06:00으로 설정하면 시스템은 백엔드 백업 정책 및 백업 시스템 조건에 따라 02:00 - 06:00 사이의 특정 시점에 백업을 시작합니다.</br> | 
| 데이터 백업 보관 기간 | <li>MySQL 2노드, 3노드 데이터 백업 파일은 7일(기본값) - 1830일 동안 보관할 수 있으며 만료 시 자동으로 삭제됩니다. <li>MySQL 단일 노드(클라우드 디스크) 데이터 백업 파일은 7일(기본값) - 30일 동안 보관할 수 있으며 만료 시 자동으로 삭제됩니다. | 
| 백업 주기 | 데이터 보안을 위해 적어도 일주일에 두 번 데이터를 백업하십시오. 기본적으로 일주일 중 7일이 모두 선택됩니다. | 
| 로그 백업 보관 기간 | <li>MySQL 2노드, 3노드 로그 백업 파일은 7일(기본값) - 1830일 동안 보관할 수 있으며 만료 시 자동으로 삭제됩니다. <li>MySQL 단일 노드(클라우드 디스크) 로그 백업 파일은 7일(기본값) - 30일 동안 보관할 수 있으며 만료 시 자동으로 삭제됩니다. | 

![](https://qcloudimg.tencent-cloud.cn/raw/9c018f548a012b427daf0227f2a93642.png)

### [아카이브 백업 설정](id:DQBFBL)
>?단일 노드(클라우드 디스크) 인스턴스는 현재 아카이브 백업 설정 기능을 지원하지 않습니다.

| 매개변수     | 설명 | 
|---------|---------|
| 백업 시작 시간 | <li>기본 백업 시작 시간은 시스템에서 자동으로 할당됩니다. <li>필요에 따라 시작 시간을 설정할 수 있습니다. 사용량이 적은 시간으로 설정하는 것이 좋습니다. 이것은 백업 프로세스의 시작 시간일 뿐이며 종료 시간을 나타내지는 않습니다. <br>예를 들어 백업 시작 시간을 02:00 - 06:00으로 설정하면 시스템은 백엔드 백업 정책 및 백업 시스템 조건에 따라 02:00 - 06:00 사이의 특정 시점에 백업을 시작합니다.</br> | 
| 데이터 백업 보존 기간 | 데이터 백업 파일은 7(기본값) - 1830일 동안 보관할 수 있으며 만료 시 자동으로 삭제됩니다. | 
| 백업 주기 | 데이터 보안을 위해 적어도 일주일에 두 번 데이터를 백업하십시오. 기본적으로 일주일 중 7일이 모두 선택됩니다. | 
| 아카이브 백업 보존 기간 | 데이터 백업 파일은 90일 - 3650일 (기본값:1080일)동안 보존할 수 있으며 만료 시 자동 삭제됩니다. | 
| 정기 백업 보관 정책 | 월별, 분기별 또는 연간 기준으로 보관할 백업 수 설정을 지원합니다. | 
| 시작 시간 | 아카이브 백업을 시작하는 시간입니다. | 
| 로그 백업 보관 시간 | 로그 백업 파일은 7일(기본값) - 1830일 동안 보관할 수 있으며 만료 시 자동으로 삭제됩니다. | 

![](https://qcloudimg.tencent-cloud.cn/raw/78c89e11aa7acab08de33b2f609fc56e.png)

### 보존 계획 보기
백업 설정에서 정기 백업 보존 정책을 선택한 후 **보존 계획 보기**를 클릭하여 미리 볼 수 있습니다.
![](https://qcloudimg.tencent-cloud.cn/raw/9b7c21feff9bf1409c80f745fbf1d370.png)
- 파란색 날짜는 비 아카이브 백업용입니다.
- 빨간색 날짜는 아카이브 백업용입니다.
- 비 아카이브 백업 또는 아카이브 백업을 클릭하여 해당 날짜를 숨기고 미리 볼 수 있습니다.
- 백업 계획 미리보기는 현재 향후 1년의 백업용이며 참고용으로만 제공됩니다.

데모1: 백업 주기는 월요일, 수요일, 금요일, 일요일입니다. 2022년 1월 11일부터 매월 1개의 백업을 유지합니다.
![](https://qcloudimg.tencent-cloud.cn/raw/9815814482b467916689b36d7f7b523b.png)
데모2: 백업 주기는 월요일, 수요일, 금요일입니다. 2022년 1월 11일부터 분기당 3개의 백업을 유지합니다.
![](https://qcloudimg.tencent-cloud.cn/raw/9815814482b467916689b36d7f7b523b.png)
데모3: 아카이브 백업만 표시합니다.
![](https://qcloudimg.tencent-cloud.cn/raw/ec59982c8f84ae59e652f5efdfb65996.png)


## [MySQL 데이터 수동 백업](id:manual-backup)
수동 백업 기능을 사용하면 백업 작업을 수동으로 시작할 수 있습니다.
>?
>- MySQL 2노드, 3노드 인스턴스 수동 백업은 전체 물리적 백업, 전체 논리적 백업 및 단일 데이터베이스/테이블 논리적 백업을 지원합니다.
>- MySQL 2노드, 3노드 인스턴스 수동 백업은 콘솔의 백업 목록에서 수동으로 삭제할 수 있습니다. 더 이상 사용하지 않는 수동 백업을 삭제하여 공간을 확보할 수 있습니다. 수동 백업은 데이터베이스 인스턴스가 비활성화될 때까지 삭제되지 않는 한 보관될 수 있습니다.
>- MySQL 단일 노드(클라우드 디스크) 인스턴스 수동 백업은 전체 스냅샷 백업을 지원합니다.
>- MySQL 단일 노드(클라우드 디스크) 인스턴스 수동 백업은 삭제를 지원하지 않습니다.
>- 인스턴스가 매일 자동 백업을 수행하는 경우 수동 백업 작업을 시작할 수 없습니다.

**MySQL 2노드, 3노드 인스턴스 작업 단계**
1. [TencentDB for MySQL 콘솔](https://console.cloud.tencent.com/cdb)에 로그인하고 인스턴스 리스트 페이지에서 인스턴스 ID를 클릭하여 관리 페이지로 이동한 후 **백업 복구** > **수동 백업**을 선택합니다.
2. 백업 설정 팝업 창에서 백업 모드와 객체를 선택하고 별칭을 입력한 후 **확인**을 클릭합니다.
![](https://main.qcloudimg.com/raw/a16e644f51756b6a98597945e45329cd.png)
>?단일 데이터베이스/테이블 논리적 백업의 경우 왼쪽 열의 **데이터베이스 및 테이블 선택**에서 백업할 데이터베이스 또는 테이블을 선택하고 선택한 항목을 오른쪽 열에 추가합니다. 데이터베이스가 없으면 먼저 데이터베이스/테이블을 생성하십시오.
>
![](https://main.qcloudimg.com/raw/76924e2c76ac348b68ed113d679d6907.png)

**MySQL 단일 노드(클라우드 디스크) 인스턴스 작업 단계**
1. [TencentDB for MySQL 콘솔](https://console.cloud.tencent.com/cdb)에 로그인하고 인스턴스 리스트 페이지에서 대상 인스턴스 ID를 클릭하여 관리 페이지로 이동한 후 **백업 복구** > **수동 백업**을 선택합니다.
2. 비고명을 입력하고 **확인**을 클릭합니다.
![](https://staticintl.cloudcachetci.com/yehe/backend-news/ydfQ183_17.png)

## FAQ
#### 1. 보관 기간이 지난 백업 파일을 다운로드하거나 복원할 수 있나요?
만료된 백업 세트는 자동으로 삭제되며 다운로드 및 복원할 수 없습니다.
- 필요에 따라 백업 보관 기간을 적절하게 설정하거나 [MySQL 콘솔](https://console.cloud.tencent.com/cdb)에서 백업 파일을 로컬에 다운로드하시길 권장합니다(단일 노드 클라우드 디스크 인스턴스의 백업 파일은 현재 다운로드를 지원하지 않음).
- 콘솔에서 인스턴스 데이터를 수동으로 백업할 수 있습니다. 수동 백업 파일은 영구 저장됩니다.
>?수동 백업도 백업 공간을 차지합니다. 비용을 줄이기 위해 백업 공간 사용을 적절하게 계획하는 것이 좋습니다.

#### 2. 백업을 수동으로 삭제할 수 있나요?
- 자동 백업은 수동으로 삭제할 수 없습니다. 자동 백업의 보존 기간을 설정할 수 있으며 만료되면 자동으로 삭제됩니다. 
- 2노드, 3노드 인스턴스 수동 백업은 [TencentDB for MySQL 콘솔](https://console.cloud.tencent.com/cdb)의 백업 리스트에서 수동으로 삭제할 수 있으며, 단일 노드 클라우드 디스크 인스턴스의 수동 백업은 현재 삭제를 지원하지 않습니다. 수동 백업은 삭제되지 않는 한 영구적으로 보관할 수 있습니다.

#### 3. 데이터 백업과 로그 백업을 비활성화할 수 있나요?
아니요. 그러나 백업 빈도를 줄이고 [TencentDB for MySQL 콘솔](https://console.cloud.tencent.com/cdb)에서 더 이상 사용하지 않는 수동 백업을 삭제하여 공간 사용량을 줄일 수 있습니다(단일 노드 클라우드 디스크 인스턴스의 수동 백업은 현재 삭제를 지원하지 않음).

#### 4. 어떻게 하면 백업 용량 요금을 절약할 수 있나요?
- 더 이상 사용하지 않는 수동 백업 데이터를 삭제합니다. [TencentDB for MySQL 콘솔](https://console.cloud.tencent.com/cdb)에 로그인하고 인스턴스 ID/이름을 클릭하여 인스턴스 관리 페이지로 이동한 후 백업 및 복원 탭에서 수동 백업을 삭제할 수 있습니다. 단일 노드 클라우드 디스크 인스턴스의 수동 백업은 현재 삭제를 지원하지 않으니 유의하십시오. 
- 비핵심 업무에 대한 자동 데이터 백업 빈도를 줄입니다(콘솔에서 백업 주기 및 백업 파일 보관 기간을 조정할 수 있으며 최소 주 2회 이상이어야 합니다).
>?롤백 기능은 데이터 백업 및 로그 백업(binlog)의 백업 주기 및 보존 일수에 의존합니다. 자동 백업 빈도 및 보존 기간을 줄이면 롤백이 영향을 받습니다. 필요에 따라 매개변수를 선택할 수 있습니다. 자세한 내용은 [데이터베이스 롤백](https://intl.cloud.tencent.com/document/product/236/7276)을 참고하십시오.
>
- 비핵심 업무에 대한 데이터 및 로그 백업의 보관 기간을 단축합니다(보관 기간은 7일로 대부분의 경우 요구 사항을 충족할 수 있습니다).

| 비즈니스 시나리오             | 권장 백업 보관 기간                                                 |
| -------------------- | ------------------------------------------------------------ |
| 핵심 비즈니스             | 7일 - 1830일                                              |
| 비핵심, 비데이터 비즈니스 | 7일                                                      |
| 아카이브 비즈니스             | 7일. 실제 비즈니스 요구 사항에 따라 데이터를 수동으로 백업하고 사용 후 즉시 백업을 삭제하는 것이 좋습니다. |
| 테스트 비즈니스             | 7일. 실제 비즈니스 요구 사항에 따라 데이터를 수동으로 백업하고 사용 후 즉시 백업을 삭제하는 것이 좋습니다. |


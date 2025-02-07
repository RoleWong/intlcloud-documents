## 概要
バックアップスペースはあるリージョンにおけるすべてのMySQL インスタンスのバックアップファイルを保存することに使われています。バックアップファイルは自動データバックアップ、手動データバックアップ及びログバックアップから構成されています。

- 2ノード（ローカルディスク）と3ノード（ローカルディスク）のインスタンスの場合、TencentDB for MySQLは一定限度の無料バックアップスペースをリージョンごとに提供します。無料バックアップスペースのサイズは、お客様の対応リージョンでのすべての2ノードと3ノードインスタンス（マスターインスタンス、ディザスタリカバリインスタンスを含む）のストレージキャパシティの和です。計算例については、[計算式](#jfgs)をご参照ください。
>?
>- 読取専用インスタンスROを購入する場合は、バックアップスペースの贈与をもらうことができません。マスターインスタンス及び災害復帰インスタンスを購入する場合のみ、贈与を受け取ることが可能です。
>- ローカルディスクのインスタンスバックアップスペースの容量は[MySQLコンソール](https://console.cloud.tencent.com/mysql/backup/index)のデータベースバックアップページで表示できます。

- 単一ノード（クラウドディスクバージョン）インスタンスの場合、TencentDB for MySQLは、インスタンスのディメンションに応じて一定量の無料バックアップスペースを提供します。単一ノード（クラウドディスクバージョン）インスタンスの無料バックアップスペースの容量は、このインスタンスストレージキャパシティの200%です。
>?クラウドディスクバージョンのインスタンスバックアップスペースの容量は、インスタンスの[バックアップと復元]ページで確認できます。
>![](https://staticintl.cloudcachetci.com/yehe/backend-news/HMH5870_24.png)
>

## クラウドディスクとローカルディスクの無料限度の比較

| ストレージ種類 |無料限度の説明 |無料ディメンションの説明 |
|---------|---------|---------|
| クラウドディスク | ストレージキャパシティの200% | インスタンスのディメンション、単一ノードのクラウドディスクバージョンのインスタンスストレージキャパシティが50GBの場合、このインスタンスの無料バックアップスペースは100GBです。| 
| ローカルディスク | ストレージキャパシティの100% | リージョンのディメンション、Tencent CloudアカウントAには北京地域に2ノードインスタンスが2つあり、ストレージキャパシティがそれぞれ50GBと80GBである場合、北京地域でのこのアカウントの無料バックアップスペースは130GBです。| 


## 単一ノードクラウドディスクバージョンのバックアップの価格
無料限度を超えるバックアップスペースについて、リージョンごとの料金は次のとおりです。

| リージョン | 価格（米ドル/GB/時間） | 
|---------|---------|
| 上海、南京、重慶、成都、広州、北京 | 0.00003676 | 
| バージニア、バンコク、フランクフルト、東京、シンガポール、ソウル、トロント、ジャカルタ、シリコンバレー、サンパウロ、中国香港 | 0.00004118 | 

単一ノードのクラウドディスクバージョンのインスタンスは現在、上海、成都、広州、北京をサポートします。その他の都市はこれから利用できるようになります。

## ローカルディスクバックアップの価格
フリークォータ以外のバックアップスペースについては、中国大陸は0.000113ドル/GB/時間で課金します。ほかのリージョンの価格については[MySQL価格電卓](https://www.tencentcloud.com/pricing/cdb/calculator)をご参照ください。
課金スペースが1GB以下の場合は、実際の課金が発生しません。1時間以下の場合は、1時間として課金されます。MySQLの贈与ポリシーは柔軟的であり、圧倒的多数のインスタンスはバックアップスペースのために支払う必要がありません。

## クロスリージョンバックアップの説明[](id:kdybfjjfsm)
TencentDB for MySQLは監視管理またはディザーストリカバリーのためのクロスリージョンバックアップをサポートします。コンソールを使用して、クロスリージョンバックアップを開始することができます。[クロスリージョンバックアップ](https://cloud.tencent.com/document/product/236/73100)をご参照ください。クロスリージョンバックアップをオンにしても、デフォルトのバックアップには影響しません。両方が同時に存在し、自動バックアップが完了するとクロスリージョンバックアップがトリガーされます。つまり、自動バックアップはクロスリージョンバックアップのストレージデバイスにダンプされます。クロスリージョンバックアップファイルには、デフォルトのバックアップファイルの保存期間でなく、個別の保存期間が適用されます。

>!クロスリージョンバックアップによるバックアップファイルとログファイルは、無料のストレージ容量の枠を消費しません。また、マスターインスタンスが置かれるバックアップリージョンまでのファイルの使用容量が計算されます。

クロスリージョンバックアップファイルの使用容量について、中国大陸では1時間あたり0.000113ドル/GB、中国香港などでは1時間あたり0.000127ドル/GBです。
クロスリージョンバックアップ機能をサポートする地域は、北京、上海、広州、深セン、成都があり、他の都市はこれから利用できるようになります。

## バックアップの課金日程
- 2019年12月02日0時から、中国香港・中国マカオ・中国台湾地域及びほかの海外地域は正式に課金を行います。
- 2019年12月02日0時から、西南地域（成都、重慶）、華南地域（シンセン金融）、華東地域（上海金融）、華北地域（北京金融）は正式に課金を行います。
- 2019年12月05日0時から、華南地域（広州）は正式に課金を行います。
- 2019年12月09日0時から、華北地域（北京）は正式に課金を行います。
- 2019年12月10日0時から、華東地域（上海）は正式に課金を行います。
- 2019年12月10日の0時以降に追加されたリージョンの場合、バックアップも正式に課金されます。

## [計算式](id:jfgs)
- **ローカルディスクバックアップスペースの計算式**：
 - **無料バックアップスペース（単一リージョン）=このリージョンで購入したMySQL2ノードと3ノードのインスタンスのストレージキャパシティの和**
 - **課金部分（単一リージョン）=データのバックアップ容量（当該リージョン）+ログのバックアップスペース（当該リージョン）-無料バックアップスペース（当該リージョン）**

- **クラウドディスクバックアップスペースの計算式**：
 - **無料バックアップキャパシティ（単一インスタンス）**=**当該インスタンスで購入したストレージスペースの200%** 
 - **課金部分（単一インスタンス）=データのバックアップ容量（当該インスタンス）+ログのバックアップスペース（当該インスタンス）-無料バックアップスペース（当該インスタンス）**

>?ごみ箱内のTencentDB for MySQLインスタンスのバックアップは、引き続きバックアップスペースに含まれ、合計キャパシティサイズに計上されます。

**計算例**
例えば、広州3区で実行されているMySQL2ノードインスタンス（データベースストレージスペースを購入すると月額500GB）と広州4区で実行されているMySQL2ノードインスタンス（データベースストレージスペースを購入すると月額200GB）があります。すなわち、広州エリアでは毎月700GBの無料バックアップスペースが付いています。

顧客は広州地域での合計バックアップストレージスペースが700GBを超えた場合（例えばデータバックアップは800GB、ログバックアップは100GBである）、1時間当りの課金スペース = 800 + 100 - 700 = 200GBとなります。顧客はこの200GBのためにバックアップスペース費用を支払う必要があります。その他はこれによって類推することができます。


## バックアップのライフサイクル
### サブスクリプションインスタンス
- バックアップはインスタンスのライフサイクルに応じて変わっています。
- インスタンスが期限切れとなってから7日間は、バックアップは正常に実行されます。その間、無料バックアップスペースを超過したバックアップについては引き続き課金されます。
- インスタンスの期限切れ後8日目から、インスタンスは隔離され、ごみ箱に入れられます。この時点で自動バックアップは停止し、ロールバックと手動バックアップは禁止されますが、バックアップのダウンロードは引き続き許可されます（[バックアップリスト](https://console.cloud.tencent.com/mysql/backup/list/data)ページでダウンロード可能です）。このインスタンスバックアップで無料割り当てを超えるキャパシティについては、インスタンスがオフラインになるまで課金されます。ユーザーはコンソールのごみ箱で支払い更新操作を行い、インスタンスとバックアップを復元することができます。
- インスタンスは、ごみ箱で7日間隔離された後、正式にオフラインになります。この時点で、インスタンスは完全に破棄され、関連するデータバックアップも破棄されますので、必要なバックアップを適時に保存してください。

### [従量課金インスタンス](id:anliang_zhouqi)
- バックアップはインスタンスのライフサイクルに応じて変わっています。
- インスタンスの支払い延滞から24時間以内であれば、バックアップは通常どおり実行されます。
- インスタンスの支払い延滞から24時間以上経過すると、インスタンスは隔離され、ごみ箱に入れられます。この時点で自動バックアップは停止し、ロールバックと手動バックアップは禁止されますが、バックアップのダウンロードは引き続き許可されます（[バックアップリスト](https://console.cloud.tencent.com/mysql/backup/list/data) ページでダウンロード可能です）。このインスタンスバックアップで無料割り当てを超えるキャパシティについては、インスタンスがオフラインになるまで課金されます。ユーザーはコンソールのごみ箱で支払い更新操作を行い、インスタンスの復元とバックアップを行うことができます。
- インスタンスは、ごみ箱で3日間隔離された後、正式にオフラインになります。この時点で、インスタンスは完全に破棄され、関連するデータバックアップも破棄されますので、必要なバックアップを適時に保存してください。

## アカウントの支払い延滞に関する説明
### サブスクリプションインスタンス
- インスタンスが有効期間内であっても、アカウントの支払いが遅延すると、バックアップ関連サービスはダウングレードし、ロールバック、手動バックアップおよびバックアップのダウンロードは禁止されます。その間自動バックアップは継続し、無料キャパシティを超過したバックアップについては引き続き課金されます。
- ロールバック、手動バックアップおよびバックアップのダウンロードなどのサービスを利用したい場合は、アカウントの残高がプラスになるまでチャージしてください。

### 従量課金インスタンス
アカウントに支払い延滞があった場合は、バックアップがユーザーのインスタンスのライフサイクルとともに変わります、上述の従量課金バックアップのライフサイクルに関する説明をご参照ください。

## バックアップ商業化サービスの向上
>?下表に記載の数値は、単一のTencent Cloudアカウントが同じリージョンで対応できる各項目の最大値です。

| 向上ポイント             | アップグレード前         | アップグレード後          |
| ------------------ | -------------- | --------------- |
| データバックアップの保持期間   | 30日             | 1830日             |
| ログバックアップの保持期間 | 5日              | 1830日            |
| バックアップの圧縮率         | 一般圧縮率              | 極めて高い圧縮率               |
| binlog 集中化         | ローカルストレージ | 集中化ストレージ |

## バックアップのオーバーヘッドを削減するためのアドバイス
- 今後使用しない手動バックアップデータを削除します。手動バックアップは、[MySQLコンソール](https://console.cloud.tencent.com/cdb)のインスタンス管理ページ（対応するインスタンスIDまたは**操作**列の**管理**をクリックすると入れます）>バックアップ・復元ページで削除します。自動バックアップの有効期限が切れると、自動的に削除され、コンソールで手動削除することはできなくなります。
- 非コアビジネスのデータの自動バックアップ頻度を減らします（コンソールでバックアップ周期とバックアップ保留時間を調整することができ、1週間に少なくとも2回バックアップします）。
>?[ロールバック機能](https://intl.cloud.tencent.com/document/product/236/7276) は、バックアップサイクルとバックアップ保持日数内のデータバックアップ+ ログバックアップ（binlog）を基準にしています。自動バックアップ頻度と保持日数を短縮すると、インスタンスデータのロールバック期間の範囲に影響します。バックアップの設定は十分に検討してください。
>
- 非コアビジネスのデータバックアップおよびログバックアップの保存時間を短縮します（7日間のバックアップ保留時間は、既に大部分のシナリオの要件を満たすことができます）。

| サービスケース             | バックアップ保留時間                                                 |
| -------------------- | ------------------------------------------------------------ |
| 中核サービス             | 7日 - 1830日をお勧めします                                              |                               　
| 中核でない、データ類でないサービス | 7日をお勧めします                                                       |                                                  
| アーカイブ類サービス             | バックアップの保留時間を7日間とし、実際のサービスニーズにより手動でデータをバックアップし、使い終わったら適時に削除することをお勧めします |
| テスト類サービス             | バックアップの保留時間を7日間とし、実際のサービスニーズにより手動でデータをバックアップし、使い終わったら適時に削除することをお勧めします |


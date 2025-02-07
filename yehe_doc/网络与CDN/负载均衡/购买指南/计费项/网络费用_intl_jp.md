ここでは標準アカウントタイプの詳細な料金説明および料金の例についてご説明します。

## 料金の構成
標準アカウントタイプのCloud Load Balancer（CLB）の料金は、CLBインスタンス料金、パブリックネットワーク料金、クロスリージョンバインディング料金、ロードバランサキャパシティユニット（LCU）料金の4項目から成ります。
>? 
>+ LCU料金は「LCUタイプ」のCLBインスタンスにのみ発生します。現在「LCUタイプ」のCLBはベータ版テスト中です。使用をご希望の場合は、[ベータ版テスト申請](https://intl.cloud.tencent.com/apply/p/fj5bwo9e6rj)をご提出ください。
>+ 共有タイプのCLBにはLCU料金は発生しません。
<table>
<tr>
<th>アカウントタイプ</th>
<th>ネットワークタイプ</th>
<th>インスタンスタイプ</th>
<th>インスタンス料金</th>
<th>パブリックネットワーク料金</th>
<th>クロスリージョンバインディング料金</th>
<th>LCU料金</th>
</tr>
<tr>
<td rowspan="4" width="15%">標準アカウントタイプ</td>
<td rowspan="2">パブリックネットワーク</td>
<td >共有タイプ</td>
<td rowspan="2">&#10003; </td>
<td rowspan="2">&#10003; </td>
<td rowspan="2">&#10003; </td>
<td>-</td>
</tr>
<tr>
<td >LCUタイプ</td>
<td >&#10003; </td>
</tr>
<tr>
<td rowspan="2">プライベートネットワーク</td>
<td >共有タイプ</td>
<td rowspan="2">&#10003; </td>
<td rowspan="2">- </td>
<td rowspan="2">-</td>
<td >-</td>
</tr>
<tr>
<td >LCUタイプ</td>
<td >&#10003; </td>
</tr>
</table>

+ プライベートネットワークCLBにはパブリックネットワーク料金はかかりませんが、インスタンス料金は発生します（詳細な説明は[CLBインスタンスのアップグレードに伴う価格調整のお知らせ](https://intl.cloud.tencent.com/document/product/214/41565)をご参照ください）。
+ パブリックネットワークCLBにはCLBインスタンス料金とパブリックネットワーク料金がかかります。
+ パブリックネットワークCLBで<a href="https://intl.cloud.tencent.com/document/product/214/38441"> クロスリージョンバインディング2.0</a>機能を有効にすると、CLB上ではなく、Cloud Connect Network（CCN）上でクロスドメイン料金が計算されます。設定していない場合、料金は発生しません。

>! より柔軟で安定した、高品質なサービスをご提供するため、**2021年11月2日00:00:00**より、Tencent CloudのすべてのCloud Load Balancer（CLB）インスタンスについてアーキテクチャのアップグレードを実施し、アップグレード後はすべてのCLBで同時接続数5万、1秒あたりの新規接続数5000、1秒あたりの照会数（QPS）5000の保障能力を提供します。アーキテクチャのリニューアル後のCLBのプライベートネットワークおよびパブリックネットワークインスタンス価格は0.686米ドル/日（一部リージョンでは1.029米ドル/日）に調整されます。詳細については、[CLBインスタンスのアップグレードに伴う価格調整のお知らせ](https://intl.cloud.tencent.com/document/product/214/41565)をご参照ください。


## 従量課金[](id:pay-as-you-go)
支払い方法は後払いで、実際の使用量に応じて課金されます。

### 課金価格
CLBの料金 = [インスタンス料金](#traffic-instance) + [パブリックネットワーク料金](#traffic-width) + [LCU料金](#traffic-lcu)
#### [インスタンス料金](id:traffic-instance)
インスタンス料金の課金周期は1時間単位であり、1時間に1回決済されます。使用時間が1時間未満の場合は1時間として課金されます。

<table>
<thead>
<tr>
<th >リージョン</th>
<th >インスタンス料金（単位：米ドル/日）</th>
</tr>
</thead>
<tbody><tr>
<td>広州/上海/南京/北京/成都/重慶/中国香港/シンガポール/ジャカルタ/シリコンバレー/バージニア/トロント/モスクワ</td>
<td>0.686</td>
</tr>
<tr>
<td>バンコク/ムンバイ/ソウル/東京/フランクフルト/サンパウロ</td>
<td>1.029</td>
</tr>

</tbody></table>

>!
> - 従量課金のCLBインスタンスは作成時にあらかじめ1時間分のインスタンス料金がアカウントから差し引かれます。アカウントに十分な残高があることをご確認ください。
> - CLBはご購入後、使用していない状態（アクセスがなく、バックエンドホストとのバインドも行っていない状態）でも、継続的に1時間ごとのインスタンス料金が発生します。
#### [パブリックネットワーク料金](id:traffic-width)
<dx-accordion>

::: トラフィック課金[](id:traffic)
パブリックネットワークで伝送されたデータの合計（GB単位）によって課金されます。1時間に1回決済され、時間帯の違いによって業務トラフィックのピーク変動が大きくなるシーンに適しています。帯域幅の利用率が10％未満の場合、トラフィック課金でのご利用をお勧めします。
<dx-alert infotype="explain" title="">
- 現在課金対象のトラフィックはアウトバウンドトラフィック、すなわちCLBからパブリックネットワーク方向のトラフィックです。
- 突発的なトラフィックの増加によって料金が高額になることを防ぐため、帯域幅の上限を指定することによって制限することが可能です。この上限を超えた場合、デフォルトでパケットロスとなり、料金は計算されません。
- トラフィックの変換単位は1024です。例えば、1TB = 1024GB、1GB = 1024MBとなります。
</dx-alert>

<table>
<thead>
<tr>
<th width="70%">リージョン</th>
<th>パブリックネットワーク料金（単位：米ドル/GB）</th>
</tr>
</thead>
<tbody><tr>
<td>中国大陸/ソウル/中国香港/ジャカルタ</td>
<td>0.12</td>
</tr>
<tr>
<td>シンガポール</td>
<td>0.081</td>
</tr>
<tr>
<td>フランクフルト/トロント/シリコンバレー</td>
<td>0.077</td>
</tr>
<tr>
<td>モスクワ/東京</td>
<td>0.13</td>
</tr><tr>
<td>バージニア</td>
<td>0.075</td>
</tr><tr>
<td>バンコク/ムンバイ</td>
<td>0.1</td>
</tr><tr>
<td>サンパウロ</td>
<td>0.15</td>
</tr>
</tbody></table>
:::
::: 共有帯域幅パッケージ[](id:bag)
複数のIPを集計する課金形式であり、月次決済となります。業務の規模が大きく、なおかつパブリックネットワークを使用するインスタンス間でトラフィックピークシフトを形成できるシナリオに適しています。価格の詳細については、[共有帯域幅パッケージ - 標準BGP帯域幅パッケージ](https://intl.cloud.tencent.com/document/product/684/15254#bgp)をご参照ください。
:::
</dx-accordion>

#### [LCU料金](id:traffic-lcu)
具体的には[ロードバランサキャパシティユニット（LCU）課金説明](https://intl.cloud.tencent.com/document/product/214/41563)の、従量課金ノード下におけるLCU料金の説明をご参照ください。

### 課金の例
<dx-accordion>
::: 共有インスタンス
#### 事例1（パブリックネットワーク料金課金モデルがトラフィック課金の場合）：
広州リージョンで6月1日10:00:00～6月2日09:59:59の間に、従量課金かつパブリックネットワーク料金の課金モデルがトラフィック課金であるCLBを使用し、なおかつCLBからパブリックネットワークへの合計トラフィックが2GBであった場合は、次のようになります。
- インスタンス料金：インスタンス料金単価 × 使用時間 = 0.686米ドル/日 × 1日 = 0.686米ドル。
- パブリックネットワーク料金：1単位あたりのトラフィック料金 × 合計トラフィック = 0.12米ドル/GB × 2GB = 0.24米ドル。
合計料金 = インスタンス料金 + パブリックネットワーク料金 = 0.686米ドル +0.24米ドル = 0.926米ドル。

#### 事例2（パブリックネットワーク料金課金モデルが共有帯域幅パッケージの場合）：
広州リージョンで6月1日10:00:00～6月2日09:59:59の間に、従量課金かつパブリックネットワーク料金の課金モデルが共有帯域幅パッケージであるCLBを使用した場合は、次のようになります。
- インスタンス料金：インスタンス料金単価 × 使用時間 = 0.686米ドル/日 × 1日 = 0.686米ドル。
- パブリックネットワーク料金：<a href="https://intl.cloud.tencent.com/document/product/684/15254">共有帯域幅パッケージ料金</a>は月次決済のため、6月1日10:00:00～6月2日09:59:59の間にはインスタンス料金のみが決済されます。
合計料金 = インスタンス料金 = 0.686米ドル。
:::
::: LCUタイプインスタンス
#### 事例1（パブリックネットワーク料金課金モデルがトラフィック課金の場合）：
広州リージョンで6月1日10:00:00～6月2日09:59:59の間に、従量課金かつパブリックネットワーク料金の課金モデルがトラフィック課金であるCLBを使用し、なおかつCLBからパブリックネットワークへの合計トラフィックが2GBであり、なおかつ1時間あたりのロードバランサキャパシティユニット（LCU）平均消費数が2であった場合は、次のようになります。
- インスタンス料金：1時間あたりのインスタンス料金 × 使用時間 = 0.686米ドル/日 × 1日 = 0.686米ドル。
- パブリックネットワーク料金：1単位あたりのトラフィック料金 × 合計トラフィック = 0.12米ドル/GB × 2GB = 0.24米ドル。
- LCU料金 = LCU 2個 × 0.0072米ドル/個/時間 × 24時間 = 0.3456米ドル。
合計料金 = インスタンス料金 + パブリックネットワーク料金 = 0.686米ドル + 0.24米ドル + 0.3456米ドル = 1.2716米ドル。

#### 事例2（パブリックネットワーク料金課金モデルが共有帯域幅パッケージの場合）：
広州リージョンで6月1日10:00:00～6月2日09:59:59の間に、従量課金かつパブリックネットワーク料金の課金モデルが共有帯域幅パッケージであるCLBを使用し、なおかつ1時間あたりのロードバランサキャパシティユニット（LCU）平均消費数が2であった場合は、次のようになります。
- インスタンス料金：1時間あたりのインスタンス料金 × 使用時間 = 0.686米ドル/日 × 1日 = 0.686米ドル。
- パブリックネットワーク料金：<a href="https://intl.cloud.tencent.com/document/product/684/15254">共有帯域幅パッケージ料金</a>は月次決済のため、6月1日10:00:00～6月2日09:59:59の間にはインスタンス料金のみが決済されます。
- LCU料金 = LCU 2個 × 0.0072米ドル/個/時間 × 24時間 = 0.3456米ドル。
合計料金 = インスタンス料金 + LCU料金 = 1.0316米ドル。

:::
</dx-accordion>

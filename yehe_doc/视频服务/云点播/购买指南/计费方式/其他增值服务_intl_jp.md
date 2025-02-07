## ライブストリーミングトリミング

ライブストリーミング中に（ライブストリーミングがまだ終了していない時点で）、映像の一部を選択し、リアルタイムにトリミングして新しいビデオを生成する場合の料金です。
課金価格

| 課金項目       | 価格（米ドル/分） | 課金方式                   |
| :----------- | :---------------- | -------------------------- |
| ライブストリーミングリアルタイムトリミング | 0.0028            | トリミングして出力したビデオ時間に応じて課金 |

#### 課金説明

- **課金項目**：ライブストリーミングリアルタイムトリミング。
- **課金ルール**：ライブストリーミング中に（ライブストリーミングがまだ終了していない時点で）、映像の一部を選択し、リアルタイムにトリミングして新しいビデオを生成する場合の料金です。
- **課金の計算式**：ライブストリーミングトリミング料金=ライブストリーミング時にトリミングして生成したビデオ時間（分） × このビデオに対応するライブストリーミングトリミング課金項目単価（米ドル）。

#### 課金例

1月1日にVODの**ライブストリーミングリアルタイムトリミング**サービスを利用し、トリミングによって生成したビデオ時間が100分であった場合、1月2日に支払う必要があるライブストリーミングトリミング料金は次のとおりです。

1月1日のライブストリーミングトリミング料金 = 0.00098（米ドル/分）x 100（分）= 0.098（米ドル）。

機能の詳細については、[ライブストリーミングトリミング](https://www.tencentcloud.com/document/product/266/49280)をご参照ください。



## クライアントからのアップロードの高速化

クライアントからのアップロードの高速化機能を使用する際に発生する、アップロードのアクセラレーションのトラフィック料金です（グローバルリンクアクセラレーション、QUIC転送）。
課金価格

| アップロードタイプ     | 単価         |
| :----------- | :----------- |
| グローバルリンクアクセラレーション | 0.072米ドル/GB |
| QUIC転送    | 0.086米ドル/GB |

#### 課金説明

- **課金項目**：グローバルリンクアクセラレーショントラフィック、QUIC転送トラフィックに課金されます。
- **課金ルール**：ファイルをアップロードする際に、クライアントからのアップロードの高速化機能を使用することで発生する、アップロードのアクセラレーショントラフィックによって課金されます。
- **課金の計算式**：クライアントからのアップロードの高速化料金 = グローバルリンクアクセラレーショントラフィック（GB）× 単価（米ドル）+ QUIC転送トラフィック（GB）× 単価（米ドル）となります 。

#### 課金例

Video on Demandのグローバルリンクアクセラレーションアップロードサービスの使用によって発生するアップロードトラフィックは550GBです。VODのQUIC転送アップロードサービスの使用によって発生するアップロードトラフィックは100GBです。この場合に支払いが必要となるクライアントからのアップロードの高速化料金は、 550（GB）×0.072（米ドル）+100（GB）×0.086（米ドル）= 48.2（米ドル）となります。

機能の詳細については、[クライアントからのアップロードの高速化ドキュメント](https://www.tencentcloud.com/document/product/266/49083)をご参照ください。

## ショート動画(UGSV) SDKライセンス

ショート動画SDKライセンスの説明は次のとおりです：

- ショート動画のベース版ライセンスを無料で申請し、28日間トライアルが可能です。
- ショート動画SDKの利用によって発生したアクセラレーション、ストレージ、トラフィックなどのリソース消費は、オン・デマンド課金に従って計算されます。
- エンタープライズ版とエンタープライズプロ版のライセンスをご利用の場合、専用カスタマーサービスに御連絡ください。
ショート動画オン・ディマンドライセンスを有効にすると、返金対象外となります。


#### 課金価格

| バージョン               | 有効期間 | 単価（ドル） |
| ------------------ | ------ | ------------ |
| 体験版ライセンス     | 28日   | 0            |
| 軽量版ライセンス     | 1年   | 1899          |
| ベース版ライセンス     | 1年   | 9999            |

機能の詳細については、[UGSV SDKドキュメント](https://intl.cloud.tencent.com/document/product/1069/37914)をご参照ください。

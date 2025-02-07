ストレージタイプは、オブジェクトのCloud Object Storage（COS）におけるストレージのレベルおよびアクティブ度を表すことができます。アクセス頻度の高低および耐障害性の程度に基づいて区分し、COSは、標準ストレージ（マルチAZ）、低頻度ストレージ（マルチAZ）、INTELLIGENT_TIERINGストレージ（マルチAZ）、INTELLIGENT_TIERINGストレージ、標準ストレージ、低頻度ストレージ、アーカイブストレージ、ディープアーカイブストレージという複数のオブジェクトストレージタイプをご提供します。それぞれのストレージタイプによって、例えばオブジェクトのアクセス頻度、データ耐久性、データ可用性、アクセス遅延などの特性が異なります。ユーザーは、どのストレージタイプでデータをCOSにアップロードするかを、シーンに応じて選択することができます。

> !
> - オブジェクトをアップロードする際、ストレージタイプを設定しなければ、デフォルトのストレージタイプは**標準ストレージ**となります。
> - マルチAZに関するその他の説明については、[マルチAZの特徴の概要](https://intl.cloud.tencent.com/document/product/436/35208)をご参照ください。
> 

## 標準ストレージ（マルチAZ）/標準ストレージ

標準ストレージ（マルチAZ）（MAZ_STANDARD）と標準ストレージ（STANDARD）はどちらもホットデータタイプに該当します。両者はアクセス低遅延、高スループットなどの性能を有し、信頼性と可用性の高い、ハイパフォーマンスなCOSサービスをユーザーにご提供しています。

標準ストレージ（STANDARD）と比較して、標準ストレージ（マルチAZ）はより高いデータ耐久性とサービス可用性を有します。標準ストレージ（マルチAZ）は異なるストレージメカニズムを使用し、データを同一都市の異なるデータセンターに保存することで、同一リージョンの1つのデータセンターに障害が発生しても影響を受けず、ユーザーの業務の安定性をさらに保障することができます。

**適用ケース**

標準ストレージ（マルチAZ）と標準ストレージはどちらも、ホットビデオ、SNS画像、モバイルアプリケーション、ゲームプログラミング、静的ウェブサイトなどといった、大量のホットファイルへのリアルタイムアクセス、頻繁なデータインタラクションなどの業務シナリオに適しています。

標準ストレージ（STANDARD）は大多数のユースケースをカバーし、ストレージコストは標準ストレージ（マルチAZ）より低く、汎用型のストレージタイプであると言えます。

一方、標準ストレージ（多 AZ）はより高いデータ耐久性とサービス可用性を有し、例えば重要ファイル、商用データ、機微な情報などの、より要件の厳しい業務シナリオに適します。

## 低頻度ストレージ（マルチAZ）/低頻度ストレージ

低頻度ストレージ（マルチAZ）（MAZ_STANDARD_IA）と低頻度ストレージ（STANDARD_IA）はどちらも、信頼性が高く、ストレージコストが比較的低コストで、アクセス遅延が比較的少ないCOSサービスを提供可能です。両者はストレージ価格を抑えることを基本とした上で、Time To First Byte (TTFB)をミリ秒レベルに保ち、ユーザーがデータ取得シーンで待たされることなく、高速で読み取りを行えるよう保証します。標準ストレージとの明らかな違いは、ユーザーのデータアクセス時にデータ取得料金が発生する点です。

低頻度ストレージ（マルチAZ）は低頻度ストレージとは異なるストレージメカニズムを使用し、データを同一都市の異なるデータセンターに保存することで、ユーザーの業務の安定性をさらに保障し、1つのデータセンターに障害が発生しても影響を受けないようにすることができます。

**適用ケース**

低頻度ストレージ（マルチAZ）と低頻度ストレージはどちらもアクセス頻度が低い（例えば毎月平均1～2回のアクセス頻度）業務シナリオに適しています。例えば、クラウドストレージデータ、ビッグデータ分析、政府・企業業務データ、低頻度ファイル、監視データなどです。

> !低頻度ストレージ（マルチAZ）と低頻度ストレージにはどちらも保存期間およびストレージユニットの最低制限があり、保存期間が30日未満の場合は30日として計算します。単一のストレージファイルが64KB未満の場合は64KBとして計算し、64KB以上の場合は実際のサイズに基づいて計算します。価格に関する情報は、[COS価格](https://buy.intl.cloud.tencent.com/price/cos?lang=en&pg=)をご参照ください。
>

## INTELLIGENT_TIERINGストレージ（マルチAZ）/INTELLIGENT_TIERINGストレージ

INTELLIGENT_TIERINGストレージ（マルチAZ）（MAZ_INTELLIGENT_TIERING）タイプのオブジェクトは、標準ストレージ（マルチAZ）レイヤーと低頻度ストレージ（マルチAZ）レイヤーの2つのストレージレイヤーに保存できます。INTELLIGENT_TIERINGストレージ（INTELLIGENT_TIERING）タイプのオブジェクトは標準ストレージレイヤーと低頻度ストレージレイヤーの2つのストレージレイヤーに保存できます。COSはINTELLIGENT_TIERINGストレージ（マルチAZ）/INTELLIGENT_TIERINGストレージのオブジェクトのアクセス頻度に応じて、対応する2つのストレージレイヤーの間で自動的に切り替えを行います。データ取得料金がかからないため、ユーザーのストレージコストを削減できます。INTELLIGENT_TIERINGストレージに関するより詳しい説明は、 [INTELLIGENT_TIERINGストレージの概要](https://intl.cloud.tencent.com/document/product/436/38305)をご参照ください。

INTELLIGENT_TIERINGストレージ（マルチAZ）はINTELLIGENT_TIERINGストレージとは異なるストレージメカニズムを使用し、データを同一都市の異なるデータセンターに保存することで、ユーザーの業務の安定性をさらに保障し、1つのデータセンターに障害が発生しても影響を受けないようにすることができます。

**適用ケース**

INTELLIGENT_TIERINGストレージ（マルチAZ）とINTELLIGENT_TIERINGストレージはどちらもデータアクセスモードが一定ではないシナリオに適しています。業務のコスト面での要件が比較的厳しく、かつファイル読み取り性能の感度がそれほど高くなくてもよい場合は、このタイプのストレージを使用することでコストを削減できます。

>! INTELLIGENT_TIERINGストレージ（マルチAZ）とINTELLIGENT_TIERINGストレージタイプのファイルについては、単一のストレージファイルのサイズにかかわらず、すべて実際のデータサイズに基づいて課金されます。価格に関する情報は、[COS価格](https://buy.intl.cloud.tencent.com/price/cos?lang=en&pg=)をご参照ください。
>

## アーカイブストレージ

アーカイブストレージ（ARCHIVE）はコールドデータタイプに該当し、データ取得時は事前に復元（解凍）する必要があります。信頼性が高く、ストレージコストが極めて低い、長期保存型のCOSサービスの提供が可能です。アーカイブストレージには90日間の最低保存期間の要件があり、かつデータの読み取り前にデータの復元（解凍）を行っておく必要があります。

復元操作は3種類のモードをサポートしています。

- 高速取得モード：復元タスクは1～5分以内に完了します。
- 標準取得モード：復元タスクは3～5時間以内に完了します。
- バッチ取得モード：復元タスクは5～12時間以内に完了します。                      

>? 
> - 復元の操作ガイドについては、[アーカイブオブジェクトの復元](https://intl.cloud.tencent.com/document/product/436/30961)をご参照ください。
> - データ復元リクエストにはQPS制限が存在します。制限は100回/秒です。
> 

**適用ケース**

アーカイブストレージは、ファイルデータ、医療用画像、科学資料といったコンプライアンス関連ファイル、ライフサイクル関連ファイル、操作ログなどのアーカイブやリモートディザスタリカバリなど、データを長期保存する必要がある業務シーンに適しています。

>! アーカイブストレージには保存期間およびストレージユニットの最低制限があり、保存期間が90日未満の場合は90日として計算します。単一のストレージファイルが64KB未満の場合は64KBとして計算し、64KB以上の場合は実際のサイズに基づいて計算します。価格に関する情報は、[COS価格](https://buy.intl.cloud.tencent.com/price/cos?lang=en&pg=)をご参照ください。
>

## ディープアーカイブストレージ

ディープアーカイブストレージ（DEEP_ARCHIVE）では、ユーザーに、高信頼性でありながら、他のストレージの種類と比べてストレージコストが低い、長期保存型のオブジェクトストレージサービスを提供しています。ディープアーカイブストレージには最低180日の保存期間要件があり、かつデータ読み取りの前にデータの復元を行っておく必要があります。ディープアーカイブストレージのより詳しい説明は、[ディープアーカイブストレージ概要](https://intl.cloud.tencent.com/document/product/436/38304)をご参照ください。

復元操作は2種類のモードをサポートしています。

- 標準取得モード：復元時間は12～24時間です。
- バッチ取得モード：復元時間は24～48時間です。 

>? データ復元リクエストにはQPS制限が存在します。制限は100回/秒です。
>

**適用ケース**

ディープアーカイブストレージは、医療用画像データ、ビューデータ、ログデータなどといった、データを長期保存する必要がある業務シーンに適しています。

>! ディープアーカイブストレージには保存期間およびストレージユニットの最低制限があり、保存期間が180日未満の場合は180日として計算します。単一のストレージファイルが64KB未満の場合は64KBとして計算し、64KB以上の場合は実際のサイズに基づいて計算します。価格に関する情報は、[COS価格](https://buy.intl.cloud.tencent.com/price/cos?lang=en&pg=)をご参照ください。
>

## ストレージタイプの比較

| 比較項目           | 標準ストレージ<br>（マルチAZ）    | 低頻度ストレージ<br>（マルチAZ）     |  INTELLIGENT_TIERINGストレージ<br>（マルチAZ）    | INTELLIGENT_TIERING<br>ストレージ                                        | 標準ストレージ           | 低頻度ストレージ                     | アーカイブストレージ                                     | ディープアーカイブストレージ                                                 |
| ---------------- | ------------------------ | ------------------------ | ---------------- | ------------------------------------------------------- | ------------------ | ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
|   ストレージタイプパラメータ   |   MAZ_STANDARD    |  MAZ_STANDARD_IA  |  MAZ_INTELLIGENT_TIERING  |  INTELLIGENT_TIERING  |  STANDARD  |  STANDARD_IA  |  ARCHIVE  | DEEP_ARCHIVE   |
| データ耐久性       | 99.9999999<br>999%       | 99.9999999<br>999%           | 99.9999999<br>999%           |    99.9999999<br>99%                                       | 99.9999999<br>99%  | 99.9999999<br>99%            | 99.9999999<br>99%                                            | 99.9999999<br>99%                                            |
| サービス可用性       | 99.995%                   | 99.995%                       |  99.995%                       |  99.99%                                                  | 99.95%             | 99.9%                        | 99.9%                                                        | 99.9%                                                        |
| 応答             | ミリ秒レベル                   | ミリ秒レベル            | ミリ秒レベル              | ミリ秒レベル                                                  | ミリ秒レベル             | ミリ秒レベル                       | 事前に復元申請が必要です。復元操作は3種類のモードをサポートしています。<ul  style="margin: 0;"><li>高速取得モード：復元タスクは1～5分以内に完了します。</li><li>標準取得モード：復元タスクは3～5時間以内に完了します。</li><li>バッチ取得モード：復元タスクは5～12時間以内に完了します。</li></ul> | 事前に復元申請が必要です。復元操作は2種類のモードをサポートしています。<ul  style="margin: 0;"><li>標準取得モード：復元時間は12～24時間です。</li><li>バッチ取得モード：復元時間は24～48時間です。</li></ul> |
| オブジェクトの最小計算単位 | オブジェクトの実際のサイズによって計算       | 64KB               | オブジェクトの実際のサイズによって計算            | オブジェクトの実際のサイズによって計算 | オブジェクトの実際のサイズによって計算 | 64KB                         | 64KB                                                         | 64KB                                                         |
| 最短課金期間     | 制限なし                   | 30日             | 制限なし                 | 制限なし                                                    | 制限なし             | 30日                         | 90日                                                         | 180日                                                        |
| サポートするリージョン         | 現時点では北京、上海、広州、シンガポールリージョンのみ | 現時点では北京、上海、広州、シンガポールリージョンのみ    | 現時点では北京、上海、広州、シンガポールリージョンのみ     | 現時点では北京、南京、上海、広州、成都、重慶、東京、シンガポールリージョンのみ                    | 全リージョン           | 全リージョン                     | パブリッククラウドリージョン（ジャカルタを除く）に適用                                           | 現時点では北京、南京、上海、広州、成都、重慶、東京、シンガポールリージョンのみ                         |
| ストレージ料金         | 比較的高い                     | 比較的高い                | 切り替え後のストレージタイプのものと同じ            | 切り替え後のストレージタイプのものと同じ                                  | 標準               | 低い                           | 極めて低い                                                         | 極めて低い                                                         |
| データ取得料金     | なし                       | 比較的低い。実際の読み取りデータ量に応じて課金   | なし       | なし                                                      | なし                 | 比較的低い。実際の読み取りデータ量に応じて課金 | 比較的高い。実際の復元データ量に応じて課金。リカバリモードごとに料金が異なる           | 高い。実際の復元データ量に応じて課金。リカバリモードごとに料金が異なる             |
| リクエスト料金         | 標準                     | 比較的高い                 | 比較的高い。INTELLIGENT_TIERINGオブジェクト監視料金が別途必要               | 比較的高い。INTELLIGENT_TIERINGオブジェクト監視料金が別途必要                    | 標準               | 比較的高い                         | 標準（データを標準ストレージに復元する必要あり）                                 | 高い（データを標準ストレージに復元する必要あり）。このほかにディープアーカイブストレージタイプのデータを取得する際、データ取得リクエスト料金が別途必要 |
| データ処理         | サポートあり                     | サポートあり            | サポートあり                  | サポートあり                                                    | サポートあり               | サポートあり                         | サポートあり、ただし先に復元が必要                                             | サポートあり、ただし先に復元が必要                                             |

## ストレージタイプの切り替え

COSは複数のオブジェクトストレージタイプをご提供します。ストレージタイプは、オブジェクトのCOSにおけるストレージのレベルおよびアクティブ度を表すことができます。実際のご使用中に、ニーズに応じてオブジェクトのストレージタイプを変更したり、オブジェクトを比較的アクティブ度の低いストレージタイプ（低頻度ストレージ、アーカイブストレージ、ディープアーカイブストレージなど）に移行したりすることもできます。

>?
> - ストレージタイプを切り替える際は、現在のオブジェクトが保存されているバケットがそのストレージタイプに対応しているかどうかをまず確認してください。
> - オブジェクトが**アーカイブストレージ/ディープアーカイブストレージ**タイプの場合は、標準ストレージタイプに復元してからでなければ他のストレージタイプに切り替えることができません。詳細については、[アーカイブオブジェクトの復元](https://intl.cloud.tencent.com/document/product/436/30961)をご参照ください。
> 

ストレージタイプの切り替えに関する状況は下表のとおりです。

| ストレージタイプ              | 変更可能なストレージタイプ                                             | 移行可能なストレージタイプおよび移行の優先順位                                 |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 標準ストレージ（マルチAZ）     | 低頻度ストレージ（マルチAZ）、INTELLIGENT_TIERINGストレージ（マルチAZ）                    | 標準ストレージ（マルチAZ） > 低頻度ストレージ（マルチAZ） > INTELLIGENT_TIERINGストレージ（マルチAZ） |
| 低頻度ストレージ（マルチAZ）     | 標準ストレージ（マルチAZ）、INTELLIGENT_TIERINGストレージ（マルチAZ）                     | 低頻度ストレージ（マルチAZ） > INTELLIGENT_TIERINGストレージ（マルチAZ）                                                     |
| INTELLIGENT_TIERINGストレージ（マルチAZ） | 標準ストレージ（マルチAZ）、低頻度ストレージ（マルチAZ）                         | なし                                                           |
| INTELLIGENT_TIERINGストレージ         | 標準ストレージ、低頻度ストレージ、アーカイブストレージ、ディープアーカイブストレージ                   | INTELLIGENT_TIERINGストレージ > アーカイブストレージ > ディープアーカイブストレージ                       |
| 標準ストレージ              | INTELLIGENT_TIERINGストレージ、低頻度ストレージ、アーカイブストレージ、ディープアーカイブストレージ               | 標準ストレージ > 低頻度ストレージ > INTELLIGENT_TIERINGストレージ > アーカイブストレージ > ディープアーカイブストレージ |
| 低頻度ストレージ              | INTELLIGENT_TIERINGストレージ、標準ストレージ、アーカイブストレージ、ディープアーカイブストレージ               | 低頻度ストレージ > INTELLIGENT_TIERINGストレージ > アーカイブストレージ > ディープアーカイブストレージ            |
| アーカイブストレージ              | 復元後、INTELLIGENT_TIERINGストレージ、標準ストレージ、低頻度ストレージ、ディープアーカイブストレージに変更可能 | アーカイブストレージ > ディープアーカイブストレージ                                      |
| ディープアーカイブストレージ          | 復元後、INTELLIGENT_TIERINGストレージ、標準ストレージ、低頻度ストレージ、アーカイブストレージに変更可能   | なし                                                           |


操作ガイドの詳細は次のドキュメントをご参照ください。

- [ストレージタイプの変更](https://intl.cloud.tencent.com/document/product/436/30930) 
- [ライフサイクルの設定](https://intl.cloud.tencent.com/document/product/436/14605) 




## 機能説明

Appバックグラウンドは、このコールバックを通じて、ユーザーのシングルチャットメッセージをリアルタイムで操作できます。次を含めます：
-  シングルチャットメッセージの送信をリアルタイムで記録します（ログの記録、またはその他のシステムへの同期など）。
-  ユーザーのルール違反の発言リクエストをブロックします。テキスト、画像、カスタムメッセージなどのルール違反メッセージをカスタムブロックできます。
>! メッセージのプレコールバックはデフォルトで2秒のタイムアウトになり、調整することはお勧めしません。プレコールバックを使用して内容審査を処理すると、プレコールバック全体がタイムアウトする場合があります。

## 注意事項

- コールバックを有効にするには、コールバックURLを設定し、このコールバックに対応するスイッチをオンにする必要があります。設定方法の詳細については、[サードパーティのコールバック設定](https://intl.cloud.tencent.com/document/product/1047/34520)ドキュメントをご参照ください。
-  コールバックの方向は、IMバックグラウンドからAppバックグラウンドへのHTTP POSTリクエストを開始することです。
- Appバックグラウンドは、コールバックリクエストを受け取った後、リクエストURLのパラメータSDKAppIDが独自のSDKAppIDであるかどうかを確認する必要があります。
- シングルチャットメッセージ送信前後の2つのコールバックを同時に有効にし、シングルチャットメッセージ送信前のコールバックで発言禁止が返された場合、シングルチャットメッセージ送信後にコールバックはトリガーされません。
- シングルチャットメッセージ送信前後の2つのコールバックを同時に有効にし、シングルチャットメッセージ送信前のコールバックでメッセージボディが変更された場合、シングルチャットメッセージ送信後にコールバックは変更されたメッセージでコールバックします。
- その他のセキュリティ関連事項については、[サードパーティコールバック概要：セキュリティに関する考慮事項](https://intl.cloud.tencent.com/document/product/1047/34354)ドキュメントをご参照ください。

## コールバックをトリガーするシナリオ

-  Appユーザーは、クライアントを通じてシングルチャットメッセージを送信します。
-  App管理者は、REST API（sendmsgインターフェース）を通じてシングルチャットメッセージを送信します。

## コールバックの発生時間

IMバックグラウンドが、ユーザーによって送信されたシングルチャットメッセージを受信した後、そのメッセージをターゲットユーザーに送信する前。

## インターフェースの説明

### リクエストURLの事例

次の事例のApp設定のコールバックURLは`https://www.example.com`です。
**事例：**
```
https://www.example.com?SdkAppid=$SDKAppID&CallbackCommand=$CallbackCommand&contenttype=json&ClientIP=$ClientIP&OptPlatform=$OptPlatform
```

### リクエストパラメータの説明

| パラメータ     | 説明 |
| --- | --- |
| https | リクエストプロトコルはHTTPS、リクエスト方式はPOSTです |
| [www.example.com](http://www.example.com) | コールバックURL |
| SdkAppid | アプリケーション作成時にIMコンソールでアサインされたSDKAppID |
| CallbackCommand | C2C.CallbackBeforeSendMsgに固定します |
| contenttype | リクエストパッケージはJSONに固定します |
| ClientIP | クライアントIP、形式はたとえば127.0.0.1です |
| OptPlatform | クライアントプラットフォーム、値の選択は、[サードパーティコールバック概要：コールバックプロトコル](https://intl.cloud.tencent.com/document/product/1047/34354)のパラメータOptPlatformの意味を参照します |

### リクエストパッケージの事例

```
{
    "CallbackCommand": "C2C.CallbackBeforeSendMsg", // コールバックコマンド
    "From_Account": "jared", // 送信者
    "To_Account": "Jonh", // 受信者
    "MsgSeq": 48374, // メッセージのシーケンス番号
    "MsgRandom": 2837546, // メッセージ乱数
    "MsgTime": 1557481126, // メッセージ送信のタイムスタンプ（秒単位） 
    "MsgKey": "48374_2837546_1557481126", //REST APIがシングルチャットメッセージを撤回するためのメッセージの一意の識別子
    "OnlineOnlyFlag":1, //オンラインメッセージは1、そうでない場合は0となります。
    "MsgBody": [ // メッセージボディです。TIMMessageのメッセージオブジェクトをご参照ください
        {
            "MsgType": "TIMTextElem", // テキスト
            "MsgContent": {
                "Text": "red packet"
            }
        }
    ],
    "CloudCustomData": "your cloud custom data"
}
```

### リクエストパッケージフィールドの説明

| フィールド | タイプ | 説明 |
| --- | --- | --- |
| CallbackCommand | String | コールバックコマンド |
| From_Account | String | メッセージ送信者UserID |
| To_Account | String | メッセージ受信者UserID |
| MsgSeq | Integer | メッセージをタグつけるためのメッセージのシーケンス番号（32ビット符号なし整数）|
| MsgRandom | Integer | メッセージをタグつけるためのメッセージ乱数（32ビット符号なし整数）|
| MsgTime | Integer | メッセージの送信タイムスタンプ（秒単位）<br>。シングルチャットメッセージはMsgTimeによる並べ替えを優先します。同じ秒で送信されたメッセージはMsgSeqで並べ替えられます。MsgSeqの値が大きいほど、メッセージは遅くなります。  |
| MsgKey | String | メッセージの一意の識別子です。この識別子によって[REST APIシングルチャットメッセージを撤回](https://intl.cloud.tencent.com/document/product/1047/35015)できます |
|OnlineOnlyFlag|Integer|オンラインメッセージは1、そうでない場合は0となります。|
| MsgBody | Array | メッセージボディ。具体的には、[メッセージ形式の説明](https://intl.cloud.tencent.com/document/product/1047/33527)をご参照ください  |
| CloudCustomData | String | メッセージカスタムデータ（クラウドに保存され、反対側に送信され、プログラムをアンインストールして再インストールした後にプルできます）。|

### レスポンスパッケージの事例（発言の許可）

送信するメッセージの内容を変更せずにユーザーの発言が許可されます。

```
{
    "ActionStatus": "OK",
    "ErrorInfo": "",
    "ErrorCode": 0 // 0は発言が許可されることを意味します
}
```

### レスポンスパッケージの事例（発言の禁止）

ユーザーの発言は許可されていません。このメッセージは送信されず、ユーザーにエラーコード`20006`が返されます。

```
{
    "ActionStatus": "OK",
    "ErrorInfo": "",
    "ErrorCode": 1 // 1は発言が拒否されることを意味します
}
```

### レスポンスパッケージの事例（サイレントに破棄）

ユーザーの発言は許可されていません。このメッセージは送信されませんが、呼び出し元に成功が返され、メッセージが送信されたと呼び出し元に思わせます。

```
{
    "ActionStatus": "OK",
    "ErrorInfo": "",
    "ErrorCode": 2 // 2はサイレントに破棄することを意味します
}
```

### レスポンスパッケージの事例（メッセージ内容の変更）

次のレスポンス事例は、ユーザーが送信したシングルチャットメッセージを変更（カスタムメッセージまたはメッセージカスタムデータを追加)し、IMバックグラウンドが変更されたメッセージを送信することです。Appバックグラウンドは、この特性に基づいて、ユーザーが送信したメッセージに、たとえばユーザーレベル、タイトルなど、特別なコンテンツを追加できます。
**事例：**

```
{
    "ActionStatus": "OK",
    "ErrorInfo": "",
    "ErrorCode": 0, //  0である必要があります。この方法でのみ、変更後のメッセージを正常に送信できます
    "MsgBody": [ // AppApp変更後のメッセージ、存在しない場合はユーザーが送信したメッセージがデフォルトで使用されます
        {
            "MsgType": "TIMTextElem", // テキスト
            "MsgContent": {
                "Text": "red packet"
            }
        },
        {
            "MsgType": "TIMCustomElem", // カスタムメッセージ
            "MsgContent": {
                "Desc": " CustomElement.MemberLevel ", // 説明
                "Data": " LV1" // データ
            }
        }
    ],
    "CloudCustomData": "your new cloud custom data" // メッセージカスタムデータ
}
```

### レスポンスパッケージフィールドの説明

| フィールド | タイプ | 属性 | 説明 |
| --- | --- | --- | --- |
| ActionStatus | String | 必須 | リクエスト処理の結果、OKは処理が成功したことを意味し、FAILは失敗を意味します |
| ErrorCode | Integer | 必須 | エラーコード。0は発言が許可され、1は発現が拒否され、2はサイレントに破棄することを意味します。業務が発言を拒否したいと同時に、エラーコードErrorCodeおよびErrorInfoをクライアントに渡した場合は、エラーコードErrorCodeを\[120001, 130000]の範囲内に設定してください
| ErrorInfo | String | 	必須 | エラーメッセージ |
| MsgBody | Array | オプション | App変更後のメッセージボディ。IMバックグラウンドは、変更後のメッセージを受信側に送信します。具体的な形式については、[メッセージ形式の説明](https://intl.cloud.tencent.com/document/product/1047/33527)をご参照ください |
| CloudCustomData | String | オプション | App変更後のメッセージカスタムデータ（クラウドに保存され、対向側に送信され、プログラムをアンインストールして再インストールした後にプルできます）。IMバックグラウンドは、変更後のメッセージを受信側に送信します|

## 参考

- [サードパーティのコールバック概要](https://intl.cloud.tencent.com/document/product/1047/34354)
- [シングルチャットメッセージ送信後のコールバック](https://intl.cloud.tencent.com/document/product/1047/34365)
- REST API：[シングルチャットメッセージの個別送信](https://intl.cloud.tencent.com/document/product/1047/34919)
- REST API：[シングルチャットメッセージの一括送信](https://intl.cloud.tencent.com/document/product/1047/34920)


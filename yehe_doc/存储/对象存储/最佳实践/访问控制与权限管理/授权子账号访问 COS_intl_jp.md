## 概要

Cloud Object Storage（COS）のリソースについては、異なる企業間または社内の複数のチーム間で、チームまたは要員ごとに異なるアクセス権限を持つよう設定する必要があります。Cloud Access Management（CAM）を使用することで、バケットまたはオブジェクトごとに異なる操作権限を設定し、異なるチームまたは要員同士によるコラボレーションを可能にします。

まず、重要な概念であるルートアカウント、サブアカウント（ユーザー）およびユーザーグループについての理解が必要です。CAMの関連用語、設定の詳細説明については、CAMの[用語集](https://intl.cloud.tencent.com/document/product/598/18564)をご参照ください。

#### ルートアカウント
ルートアカウントはデベロッパーとも呼ばれます。ユーザーがTencent Cloudアカウントを申請すると、システムはTencent CloudサービスにログインするためのルートアカウントIDを作成します。ルートアカウントはTencent Cloudリソースの使用量の計算と課金における基本主体です。

ルートアカウントはデフォルトで、その名前の下にあるすべてのリソースへの完全なアクセス権限を有します。これには注文情報へのアクセス、ユーザーパスワードの変更、ユーザーおよびユーザーグループの作成、他のクラウドサービスリソースへのアクセスなどが含まれます。デフォルトでは、リソースにアクセスできるのはルートアカウントのみであり、他のユーザーからのアクセスはすべてルートアカウントによる権限承認を経る必要があります。

#### サブアカウント（ユーザー）とユーザーグループ
- サブアカウントとはルートアカウントが作成するエンティティであり、確実なIDおよびIDクレデンシャルを有し、Tencent Cloudコンソールにログインする権限を有します。
- サブアカウントはデフォルトではリソースを所有せず、所属するルートアカウントによる権限承認を受ける必要があります。
 - 1つのルートアカウントは複数のサブアカウント（ユーザー）を作成することができます。
 - 1つのサブアカウントは複数のルートアカウントに帰属することができ、複数のルートアカウントが各自のクラウドリソースを管理する上でそれぞれ役立てることができます。ただし、1つのサブアカウントは、同一時刻に1つのルートアカウントにしかログインできず、そこでそのルートアカウントのクラウドリソースを管理します。
- サブアカウントはコンソールを通じてデベロッパー（ルートアカウント）を、あるルートアカウントから別のルートアカウントに切り替えることができます。
 - サブアカウントがコンソールにログインすると、デフォルトのルートアカウントに自動的に切り替わり、デフォルトのルートアカウントが承認したアクセス権限を得ることができます。
 - デベロッパーを切り替えると、サブアカウントは切り替えたルートアカウントによって承認されたアクセス権限を得る一方、切り替え前のルートアカウントによって承認されたアクセス権限は無効になります。
- ユーザーグループは同一の職能を持つ複数のユーザー（サブアカウント）の集合です。業務上のニーズに応じてさまざまなユーザーグループを作成し、ユーザーグループに適切なポリシーをバインドして異なる権限を割り当てることができます。

## 操作手順
サブアカウントへのCOSアクセス権限の承認には、サブアカウントの作成、サブアカウントへの権限承認、サブアカウントによるCOSリソースへのアクセスという3つの手順があります。

### ステップ1：サブアカウントの作成
CAMコンソールでサブアカウントを作成し、サブアカウントにアクセス権限承認の設定を行うことができます。具体的な操作は次のとおりです。
1. [CAMコンソール](https://console.cloud.tencent.com/cam)にログインします。
2. **ユーザー>ユーザーリスト> ユーザーの新規作成**を選択し、ユーザー新規作成ページに進みます。
3. **カスタム作成**を選択し、**リソースへのアクセスおよびメッセージの受信が可能**タイプを選択し、**次のステップ**をクリックします。
4. 要件に従ってユーザー関連情報を入力します。
 - **ユーザー情報の設定**：サブユーザー名を入力します（例：Sub_user）。サブユーザーのメールアドレスを入力します。サブユーザーにメールアドレスを追加し、Tencent Cloudから送信される、WeChatにバインドされたメールを受信できるようにする必要があります。
 - **アクセス方法**：プログラムアクセスとTencent Cloudコンソールアクセスから選択します。その他の設定は必要に応じて選択できます。
4. ユーザー情報の入力完了後、**次のステップ**をクリックしてID認証を行います。
5. ID認証の完了後、サブユーザー権限の設定を行います。システムから提供されるポリシーに基づいて選択することで、例えばCOSのバケットリストのアクセス権限、読み取り専用権限などの簡単なポリシーを設定することができます。より複雑なポリシーを設定したい場合は、[ステップ2：サブアカウントへの権限承認](#.E6.AD.A5.E9.AA.A42.EF.BC.9A.E5.AF.B9.E5.AD.90.E8.B4.A6.E5.8F.B7.E6.8E.88.E4.BA.88.E6.9D.83.E9.99.90)を実行してください。
6. 入力したユーザー情報に誤りがないことを確認後、**完了**をクリックするとサブアカウントの作成が完了します。


<span id="サブアカウントへの権限承認"></span>
### ステップ2：サブアカウントへの権限承認
サブアカウントへの権限承認はCAMによって、サブアカウント（ユーザー）またはユーザーグループに対しポリシーを設定する方法で行うことができます。
1. [CAMコンソール](https://console.cloud.tencent.com/cam)にログインします。
2. **ポリシー > カスタムポリシーの新規作成 > ポリシー構文で作成**を選択し、ポリシー作成ページに進みます。
3. 実際のニーズに応じて**空白テンプレート**を選択して権限承認ポリシーをカスタマイズするか、またはCOSにバインドされた**システムテンプレート**を選択し、**次のステップ**をクリックします。
![](https://main.qcloudimg.com/raw/e1d43ba1ae7e2ae8bb22416c8c4b8127.png)
4. 覚えやすいポリシー名を入力します。**空白テンプレート**を選択した場合はポリシー構文を入力する必要があります。詳細については[ポリシーの例](#ポリシーの例)をご参照ください。ポリシーの内容をコピーして**ポリシー内容**のエディタボックス内に貼り付け、入力に間違いがないことを確認してから**完了**をクリックします。
![](https://main.qcloudimg.com/raw/31c73db7c212457539feafc5dd02d0e7.png)
5. 作成完了後、作成したポリシーをサブアカウントにバインドします。
![](https://main.qcloudimg.com/raw/1495c8b19dfff2aad622b794d8f5bc6a.png)
6. サブアカウントにチェックを入れ、**OK**をクリックして権限を承認すると、限定されたCOSリソースにサブアカウントを使用してアクセスできるようになります。
![](https://main.qcloudimg.com/raw/3667de5320e99321a78c5d1c22a49157.png)

### ステップ3：サブアカウントによるCOSリソースへのアクセス

上記のステップ1で設定した2種類のアクセス方法、プログラムアクセスとTencent Cloudコンソールアクセスについての説明は次のとおりです。

（1）プログラムアクセス

サブアカウントを使用し、プログラム（API、SDK、ツールなど）によってCOSリソースにアクセスする場合は、先にルートアカウントのAPPID、サブアカウントのSecretIdおよびSecretKey情報を取得する必要があります。サブアカウントのSecretIdとSecretKeyはCAMコンソールで生成できます。

1. ルートアカウントで[CAMコンソール](https://console.cloud.tencent.com/cam/capi)にログインします。
2. **ユーザーリスト**を選択し、ユーザーリストページに進みます。
3. サブアカウントのユーザー名をクリックし、サブアカウント情報詳細ページに進みます。
4. **APIキー**タブをクリックし、**キーの作成**をクリックしてこのサブアカウントのSecretIdとSecretKeyを作成します。

この時点で、サブアカウントのSecretIdとSecretKey、ルートアカウントのAPPIDによってCOSリソースにアクセスすることが可能になりました。 

>!
>サブアカウントはXML APIまたはXML APIベースのSDKによってCOSリソースにアクセスする必要があります。


#### XMLベースのJava SDKによるアクセスの例

XMLベースのJava SDKコマンドラインの場合、入力する必要があるパラメータは次のとおりです。
```
// 1 ID情報の初期化
COSCredentials cred = new BasicCOSCredentials("<ルートアカウントのAPPID>", "<サブアカウントのSecretId>", "<サブアカウントのSecretKey>");
```

実際の例は次のとおりです。
```
// 1 ID情報の初期化
COSCredentials cred = new BasicCOSCredentials("1250000000", "AKIDasdfmRxHPa9oLhJp****", "e8Sdeasdfas2238Vi****");
```

#### COSCMDコマンドラインツールによるアクセスの例

COSCMDの設定コマンドの場合、入力する必要があるパラメータは次のとおりです。
```sh
coscmd config -u <ルートアカウントのAPPID> -a <サブアカウントのSecretId> -s <サブアカウントのSecretKey>  -b <ルートアカウントのbucketname> -r <ルートアカウントのbucket所属リージョン>
```
実際の例は次のとおりです。
```sh
coscmd config -u 1250000000 -a AKIDasdfmRxHPa9oLhJp**** -s e8Sdeasdfas2238Vi**** -b examplebucket -r ap-beijing
```


（2）Tencent Cloudコンソール

サブユーザーに権限を承認した後、[サブユーザーログイン画面](https://www.tencentcloud.com/account/login/subAccount)でルートアカウントID、サブユーザー名、サブユーザーパスワードを入力してコンソールにログインし、**クラウド製品**の中の**Cloud Object Storage**を選択してクリックすると、ルートアカウント下のストレージリソースにアクセスすることができます。

<span id="ポリシーの例"></span>
## ポリシーの例
以下にいくつかの典型的なシナリオでのポリシーの例を挙げます。カスタムポリシーを設定する場合は、次の参照ポリシーをコピーして入力ボックスに貼り付け、**ポリシー内容を編集**することができます。実際の設定に応じて変更するだけで完了です。COSの一般的なシナリオにおけるその他のポリシー構文については、[アクセスポリシーの言語概要](https://intl.cloud.tencent.com/document/product/436/18023)または[CAM製品ドキュメント](https://www.tencentcloud.com/document/product/598)の**ビジネスユースケース**の部分をご参照ください。

### 事例1：サブアカウントにCOSの全読み取り書き込み権限を設定する

>!このポリシーは権限の範囲が大きいため、慎重に設定してください。

具体的なポリシーは次のとおりです。
```
{
    "version": "2.0",
    "statement":[
        {
            "action":[
                "name/cos:*"
            ],
            "resource": "*",
            "effect": "allow"
        },
        {
            "effect": "allow",
            "action": "monitor:*",
            "resource": "*"
        }
    ]
}
```

### 事例2：サブアカウントに読み取り専用権限を設定する
サブアカウントに読み取り専用権限のみを設定する場合の、具体的なポリシーは次のとおりです。
```
{
    "version": "2.0",
    "statement":[
        {
            "action":[
                "name/cos:List*",
                "name/cos:Get*",
                "name/cos:Head*",
                "name/cos:OptionsObject"
            ],
            "resource": "*",
            "effect": "allow"
        },
        {
            "effect": "allow",
            "action": "monitor:*",
            "resource": "*"
        }
    ]
}
```


### 事例3：サブアカウントに書き込み専用権限（削除を含めない）を設定する

具体的なポリシーは次のとおりです。
```
{
    "version": "2.0",
    "statement":[
        {
            "effect": "allow",
            "action":[
                "cos:ListParts",
                "cos:PostObject",
                "cos:PutObject*",
                "cos:InitiateMultipartUpload",
                "cos:UploadPart",
                "cos:UploadPartCopy",
                "cos:CompleteMultipartUpload",
                "cos:AbortMultipartUpload",
                "cos:ListMultipartUploads"
            ],
            "resource": "*"
        }
    ]
}
```


### 事例4：サブアカウントに特定のIPセグメントの読み取り書き込み権限を設定する
次に示す例では、IPネットワークセグメントが`192.168.1.0/24`および`192.168.2.0/24`のアドレスだけが読み取り書き込み権限を有するよう制限しています。
より豊富な発効条件を入力する場合は、[発効条件](https://www.tencentcloud.com/document/product/598/10608)をご参照ください。
```
{
    "version": "2.0",
    "statement":[
        {
            "action":[
    "cos:*"
            ],
            "resource": "*",
            "effect": "allow",
            "condition":{
                "ip_equal":{
                    "qcs:ip": ["192.168.1.0/24", "192.168.2.0/24"]
                }
            }
        }
    ]
}
```

## 基本概念

Tencent Cloud COSはHTTP/HTTPSプロトコルを使用してアクセスするWebストレージサービスです。COSには[REST API](https://intl.cloud.tencent.com/document/product/436/7751)または[COS SDK](https://intl.cloud.tencent.com/document/product/436/6474)を使用してアクセスできます。

COSへのアクセスリクエストを発行する際は、COSによる承認および認証を経てからでなければリソースの操作を行うことはできません。 このためCOSへのアクセスリクエストは、IDが識別可能かどうかによって、匿名リクエストと署名リクエストの2種類に分けられます。

- 匿名リクエスト：リクエストにAuthorizationまたは関連パラメータが含まれないか、または関連の文字からユーザーのID特性を識別できない場合、リクエストは匿名リクエストとみなされて認証を受けます。
- 署名リクエスト：署名付きのリクエストはHTTPヘッダーまたはリクエストパケットにAuthorizationフィールドが含まれています。このフィールドの内容はTencent Cloudのセキュリティ証明書のSecretID、SecretKeyとリクエストを結び付けるいくつかの特性値であり、暗号化アルゴリズムによって生成されます。

COS SDKを使用してアクセスする場合は、セキュリティ証明書を設定するだけでリクエストを発行できます。REST APIを使用してアクセスする場合は、[リクエスト署名](https://intl.cloud.tencent.com/document/api/436/7778)ドキュメントを参照して、ご自身でリクエスト署名を計算するか、またはCOS署名ツールによって直接生成する必要があります。

## セキュリティ証明書の取得

Tencent Cloud CAM（Cloud Access Management）はCOS向けにアカウントとセキュリティ証明書に関連する機能およびサービスを提供しています。これは主に、お客様がTencent Cloudアカウント下のリソースへのアクセス権限を安全に管理するために役立てられます。ユーザーはCAMによって、ユーザー（グループ）を作成、管理、および破棄できるほか、ID管理とポリシー管理を使用して、他のユーザーがTencent Cloudのリソースを使用する権限を制御できます。

### ルートアカウントのセキュリティ証明書

ルートアカウントにログイン後、CAMの[Tencent Cloud APIキー](https://console.cloud.tencent.com/cam/capi)ページによって、ルートアカウントのセキュリティ証明書のSecretIDおよびSecretKey キーを取得し管理することができます。以下はキーの例です。

- 36文字のアクセスキーID（SecretID）：AKIDHZRLB9Ibhdp7Y7gyQq6BOk1997xxxxxx
- 32文字のアクセスキーKey（SecretKey）：LYaWIuQmCSZ5ZMniUM6hiaLxHnxxxxxx

アクセスキーは一意のアカウントを識別するために用いられます。キー署名を使用してリクエストを送信すると、Tencent Cloudはリクエスト発行者のIDを識別し、承認してから、プリンシパル、リソース、アクション、条件などの認証を行い、この操作の実行を許可するかどうかを判断します。

>!ルートアカウントキーはそのアカウント下のすべてのリソースの全操作権限を有しています。キーの漏洩はクラウド上の資産損失につながるおそれがあるため、サブアカウントを作成して適正な権限を割り当て、サブアカウントのキーを使用してリクエストを作成し、リソースへのアクセスと管理を行うことを強く推奨します。

### サブアカウントのセキュリティ証明書

複数の次元でアカウント下のユーザーおよびクラウドリソースを管理したい場合は、ルートアカウント下に複数のサブアカウントを作成し、リソース権限に対する機能を複数の要員にそれぞれ管理させることができます。サブアカウント作成に関する説明については、CAMの[ユーザー管理](https://intl.cloud.tencent.com/document/product/598/13674)の関連ドキュメントをご参照ください。

サブアカウントを使用してAPIリクエストを発行する前に、サブアカウントのセキュリティ証明書を作成する必要があります。そうすることでサブアカウントも独自のキーペアを持ち、ID識別に役立てることができます。サブアカウントごとにユーザーポリシーを作成し、リソースに対するそれぞれのアクセス権限を制御することができます。またユーザーグループを作成し、ユーザーグループに統一のアクセスポリシーを追加することで、要員のグループ化およびリソースの統一管理に役立てることもできます。

> !サブアカウントは対応する権限を割り当てられた場合、リソースの作成または変更が可能ですが、この時点でリソースは引き続きルートアカウントに属するため、このリソースによって発生する料金はルートアカウントが支払う必要があります。

### 一時的なセキュリティ証明書

ルートアカウントまたはサブアカウントのセキュリティ証明書を使用したリソースへのアクセスのほかに、Tencent Cloudは、ロールを作成し、一時的なセキュリティ証明書を使用してTencent Cloudのリソースを管理することもサポートしています。ロールの基本概念および使用方法に関しては、CAMの[ロール管理](https://intl.cloud.tencent.com/document/product/598/19420)のドキュメントをご参照ください。

ロールは仮想IDであるため、ロールはパーマネントキーを所有しません。Tencent Cloud CAMは、一時的なセキュリティ証明書の発行に用いるSTS APIをご提供しています。
使用方法と事例の詳細については、[ロールの使用](https://intl.cloud.tencent.com/document/product/598/19419)のドキュメントを参照するか、またはSTS APIドキュメントで一時的なセキュリティ証明書の発行方法をご確認ください。一時的なセキュリティ証明書には通常、**限定的なポリシー**（アクション、リソース、条件）および**限定的な時間**（有効期間の開始および終了時間）のみが含まれます。このため、発行されたセキュリティ証明書は自ら配信または直接使用することができます。

一時的なセキュリティ証明書の発行インターフェースを呼び出すと、1対の一時キー（tmpSecretId/tmpSecretKey）および1個のセキュリティトークン（sessionToken）を取得でき、これらがCOSへのアクセスに用いることができるセキュリティ証明書を構成します。その例を次に示します。

- 41文字のセキュリティトークン（SecurityToken）：5e776c4216ff4d31a7c74fe194a978a3ff2xxxxxx
- 36文字の一時アクセスキーID（SecretID）: AKIDcAZnqgar9ByWq6m7ucIn8LNEuYxxxxxx
- 32文字の一時アクセスキーKey（SecretKey）: VpxrX0IMCpHXWL0Wr3KQNCqJixxxxxxx

このインターフェースはまた、`expiration`フィールドによって一時的なセキュリティ証明書の有効期間を返します。これは、この時間内においてのみ、このセキュリティ証明書を使用してリクエストを発行できることを意味します。

Tencent Cloud COSは一時キーの発行に使用できる、簡単なサーバーSDKをご提供しています。[COS STS SDK](https://github.com/tencentyun/qcloud-cos-sts-sdk)にアクセスして取得できます。一時的なセキュリティ証明書を取得した後、REST APIを使用してリクエストを発行する際、HTTPヘッダーまたはPOSTリクエストパケットのform-dataに`x-cos-security-token`フィールドの、このリクエストの使用を表すセキュリティトークンを渡し、その後一時アクセスキーペアを使用してリクエスト署名を計算する必要があります。COS SDKを使用したアクセスについては、各SDKドキュメントの関連するパートをご参照ください。

## アクセスドメイン名

### REST API

[リージョンとアクセスドメイン名](https://intl.cloud.tencent.com/document/product/436/6224)のドキュメントに、REST APIによるアクセスに用いることができるドメイン名をリストアップしています。

COSは仮想ホスティング形式のドメイン名を使用してバケットにアクセスすることを推奨しています。HTTPリクエストを発行する際には、`<BucketName-APPID>.cos.<Region>.myqcloud.com`のように`Host`ヘッダーにアクセスしたいバケットを直接入力します。仮想ホスティング形式のドメイン名を使用すると、仮想サーバー「ルートディレクトリ」に類似した機能を実装でき、favicon.ico、robots.txt、crossdomain.xmlに類似したファイルのホスティングに用いることができます。これらのファイルは、多くのアプリケーションプログラムがウェブサイトのホスティングを識別する際、デフォルトで仮想サーバー「ルートディレクトリ」の位置から検索する内容となります。

パス形式のリクエストを使用してバケットにアクセスすることもできます。例えば、`cos.<region>.myqcloud.com/<BucketName-APPID>/`のパスを使用してバケットにアクセスします。それに応じて、リクエストHostおよび署名には`cos.<region>.myqcloud.com`を使用する必要があります。当社のSDKはデフォルトではこのアクセス方式をサポートしていません。

### 静的ウェブサイトのドメイン名

バケットの静的ウェブサイト機能を有効にしている場合は、仮想ホスティング形式のドメイン名が割り当てられ、それによって関連機能を使用できます。静的ウェブサイトドメイン名の形式はREST APIとは異なる点があり、特定のインデックスページ、エラーページおよびリダイレクト設定のほか、静的ウェブサイトドメイン名ではGET/HEAD/OPTIONS Objectなどの数種類の操作のみがサポートされ、アップロードまたはリソースの設定などの操作はサポートされないという違いがあります。

静的ウェブサイトドメイン名は、例えば`<BucketName-APPID>.cos-website.<Region>.myqcloud.com`のようになります。コンソール上でバケットの「基本設定-静的ウェブサイト設定」モジュールによってこのドメイン名を取得することもできます。

## プライベートネットワークとパブリックネットワークアクセス

Tencent Cloud COSのアクセスドメイン名にはインテリジェントDNS解決を使用しており、インターネットの様々なキャリア環境の下で、お客様のCOSアクセスにとって最適なリンクを検知および指定します。Tencent Cloud内でCOSへのアクセス用のサービスをデプロイしている場合、同一リージョン内のアクセスに対しては自動的にプライベートネットワークアドレスが指定されます。リージョン間では現時点ではプライベートネットワークアクセスをサポートしておらず、デフォルトでパブリックネットワークアドレスに解決されます。

### プライベートネットワークアクセスの判断方法

同一リージョン内でのTencent Cloud製品へのアクセスは、自動的にプライベートネットワークを使用して接続され、それによるプライベートネットワークトラフィックには料金が発生しません。このため、Tencent Cloudの製品をお選びになる際は、できるだけ同一のリージョンを選択すると料金の節約になります。

>! パブリッククラウドリージョンと金融クラウドリージョン間のプライベートネットワークは相互運用されていません。

プライベートネットワークアクセスかどうかを確認するには、次の方法をご参照ください。

Tencent CVMのCOSアクセスを例にとると、プライベートネットワークを使用したCOSアクセスかどうかを判定するには、CVM上で`nslookup`コマンドを使用してCOSドメイン名を解決します。プライベートネットワークIPが返された場合、CVMとCOS間はプライベートネットワークアクセスであり、そうでない場合はパブリックネットワークアクセスであることがわかります。

>?プライベートIPアドレスの一般的な形式は`10.*.*.*`、`100.*.*.*`であり、VPCネットワークは一般的に`169.254.*.*`などです。これらの形式のIPはすべてプライベートネットワークに該当します。

`examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com`がターゲットバケットアドレスである場合、`nslookup`コマンドを実行すると次の情報を見ることができます。

![](https://main.qcloudimg.com/raw/49a7d7429ec2a96d271f6a63926286ea.png)

この中の`10.148.214.13`と`10.148.214.14`という2つのIPが、プライベートネットワークによるCOSアクセスであることを表しています。


### 接続性のテスト

#### 基本的な接続性テスト

COSはHTTPプロトコルを使用して外部へのサービス提供を行います。最も基本的な`telnet`ツールを使用して、COSアクセスドメイン名のポート80に対するアクセステストを行うことができます。

パブリックネットワークを経由するアクセスの例は次のとおりです。

```shell
telnet examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com 80
Trying 14.119.113.22...
Connected to gz.file.myqcloud.com.
Escape character is '^]'.
```

同リージョンのTencent Cloud CVM（基幹ネットワーク）を経由するアクセスの例は次のとおりです。

```shell
telnet examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com 80
Trying 10.148.214.14...
Connected to 10.148.214.14.
Escape character is '^]'.
```

同リージョンのTencent Cloud CVM（VPCネットワーク）を経由するアクセスの例は次のとおりです。

```shell
telnet examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com 80
Trying 169.254.0.47....
Connected to 169.254.0.47.
Escape character is '^]'.
```

どのアクセス環境であっても、コマンドに`Escape character is '^]'.`フィールドが返された場合は接続に成功したことを表します。

#### インターネット接続テストの説明

インターネット経由でCOSにアクセスする場合はキャリアネットワークを経由しますが、キャリアネットワークはお客様がICMPプロトコルの`ping`または`traceroute`などのツールを使用して接続性テストを行うことを禁じている可能性があります。このため、TCPプロトコルのツールを使用して接続性テストを行うことをお勧めします。
>!インターネット経由のアクセスは様々なネットワーク環境の影響を受ける可能性があります。アクセスがスムーズにいかない場合は、ローカルネットワークリンクのトラブルシューティングを行うか、または現地のキャリアに連絡してフィードバックを受けてください。

ご利用のキャリアがICMPプロトコルを許可している場合は、`ping`、`traceroute`または`mtr`ツールを使用してリンクの状況を確認することができます。キャリアがICMPプロトコルの使用を許可していない場合は、`psping`（Windows環境。Microsoft公式サイトからダウンロードしてください）または`tcping`（クロスプラットフォームソフトウェア）などのツールを使用して遅延テストを行うことができます。

#### プライベートネットワーク接続テストの説明

同リージョンのTencent Cloud VPCネットワーク経由でCOSにアクセスする場合は、ICMPプロトコルの`ping`または`traceroute`などのツールを使用して接続性テストを行えない可能性があります。基本的な接続性テストの、`telnet`コマンドを使用してテストを行うことをお勧めします。

`psping`または`tcping`などのツールを使用して、アクセスドメイン名のポート80に対する遅延テストを直接行ってみることもできます。テストの前に、`nslookup`コマンドによってすでに問い合わせを行っており、アクセスドメイン名がプライベートネットワークアドレスに正しく解決されていることを確実に確認してください。

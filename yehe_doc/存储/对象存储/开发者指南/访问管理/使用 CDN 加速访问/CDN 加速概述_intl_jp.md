Content Delivery Network(CDN)を使用してCloud Object Storage (COS)のアクセラレーションを行うことで、バケット内のコンテンツを広範囲にダウンロード、配信することができます。特に、同一のコンテンツを繰り返しダウンロードするユースケースに適しています。back-to-origin認証機能を使用すると、CDNを使用したプライベート読み取りバケット内のコンテンツのアクセラレーションを実現できます。CDN認証機能により、コンテンツは正当なユーザーにのみダウンロードされ、ダウンロード制限なしに伴うデータセキュリティやトラフィックコストなどの問題を防ぐことができます。


>?CDNアクセラレーションドメイン名を有効にした場合、CDNアクセラレーションドメイン名を使用してデータのダウンロードやアクセスを行うと、CDN back-to-originラフィックとCDNトラフィックが発生します。詳しくは、[COSをCDNオリジンサーバーとした場合発生するトラフィック](https://intl.cloud.tencent.com/document/product/436/33776)をご参照ください。


## Content Delivery Network

#### CDNの定義

CDNは既存のInternet上に追加された新しいネットワークアーキテクチャのレイヤーであり、世界各国に分布する高性能なアクセラレーションノードで構成されます。これらの高性能なサービスノードは一定のキャッシングポリシーに従ってお客様の業務コンテンツを保存します。お客様のユーザーが、ある業務コンテンツをリクエストすると、リクエストがユーザーから最も近いサービスノードにスケジューリングされ、サービスノードから直接迅速にレスポンスすることで、ユーザーのアクセス遅延を効果的に低減し、可用性を向上させます。

CDNはキャッシングおよびback-to-originを行います。つまり、ユーザーがあるURLにアクセスしたとき、解決対象となったエッジノードが要求されたキャッシング済みの内容をヒットしなかった場合、またはキャッシュが期限切れの場合は、オリジンサーバーに戻って要求されたコンテンツを取得します。

#### ユースケース

- レスポンス遅延およびダウンロード速度に対する要件が比較的厳しいケース。
- 地域、国、大陸を越えて数GBから数TBまでのデータを転送する必要があるケース。
- 同一のコンテンツを頻繁に繰り返してダウンロードする必要があるケース。

#### セキュリティのタイプ

- back-to-origin認証：ユーザーのリクエストしたデータがエッジノードでキャッシュにヒットしなかった場合、CDNはback-to-originを行ってデータを取得する必要があります。COSをオリジンサーバーとして使用し、back-to-origin認証を有効にすると、CDNエッジノードは特殊なサービスIDを使用してCOSオリジンサーバーにアクセスし、プライベートアクセスバケット内のデータの取得とキャッシングを行います。
 - CDNサービス認証：CDNエッジノードはCDNサービス権限を追加することにより、特殊なサービスIDでCOSオリジンサーバーにアクセスすることができるようになります。CDNサービス権限を追加しないと、back-to-origin認証を有効にできません。
- CDN認証設定：ユーザーがエッジノードにアクセスしてキャッシュデータを取得する際、エッジノードは認証設定ルールに従って、アクセスしているURLにおけるID認証フィールドを検証することで、権限のないアクセスを防ぎ、リンク不正アクセス防止を実現し、エッジノードによるデータキャッシングの安全性と信頼性を高めます。

## COSのアクセスノード

#### アクセスノードの定義

アクセスノードとはユーザーがバケットを作成する際、バケットのリージョンおよび名称に基づいて、システムが自動的にバケットに割り当てるアクセスドメイン名であり、このドメイン名によってバケット内のデータにアクセスすることができます。

静的ウェブサイト機能を有効にすると、もう１つの静的ウェブサイトのアクセスノードを入手します。このノードは、デフォルトのアクセスノードとは異なる、特殊な設定のレスポンス内容を表示するために用いられます。

#### アクセスノード

- アクセスノード：ユーザーがバケットを作成すると、COSはバケットに対し自動的に1つのXMLアクセスノードを割り当てます。 このアクセスノードの形式は&lt;BucketName-APPID>.cos.&lt;Region>.myqcloud.comであり、RESTful APIによるアクセスに適します。ユーザーはアクセスドメイン名によって、また[APIドキュメント](https://intl.cloud.tencent.com/document/product/436/7751)の実行ルールを確認することで、バケットの設定、オブジェクトのアップロードやダウンロードを行うことができます。
- 静的ウェブサイトノード：コンソール上のバケット基本設定画面で静的ウェブサイトのホスティング機能を有効にすることができます。有効にすると1つのアクセスノードが提供されます。アクセスノードの形式は&lt;BucketName-APPID>.cos-website.&lt;Region>.myqcloud.comです。静的ウェブサイトは特殊なインデックスページ（IndexPage）、エラーページ（ErrorPage）やリダイレクトなどをサポートしており、サポートはオブジェクトのダウンロード系の操作のみとなります。ユーザーは静的ウェブサイトのドメイン名によってコンテンツを取得できます。

#### アクセス権限

- パブリック読み取り：バケットをパブリック読み取りに設定すると、バケットのアクセスドメイン名によって誰でもそのバケットにアクセスすることができます。ユーザーがパブリック読み取りバケットをオリジンサーバーとしてback-to-originを行う場合は、CDN認証およびback-to-origin認証を使用せずに、そのままCDNアクセラレーションを有効にすることができます。
- プライベート読み取り：バケットをプライベート読み取りに設定すると、ユーザーはアクセスポリシーを作成することによってアクセス者を管理できます。これにはCDNサービス認証の管理なども含まれています。ユーザーがプライベート読み取りバケットをオリジンサーバーとしてback-to-originを行う場合、back-to-origin認証を有効にしてもCDN認証を有効にしていなければ、権限を持たない人がCDN経由で直接バケットにアクセスできるため、プライベート読み取りバケットについては、**CDN認証とback-to-origin認証ともに有効にすることでデータの安全性を保障するよう強く推奨します**。

## CDNアクセラレーションを使用したCOSアクセス

ユーザーは次の2種類のドメイン名を管理することでCOSに対するアクセラレーションアクセスを行うことができます：

- デフォルトのCDNアクセラレーションドメイン名：COSがユーザーに提供するデフォルトのCDNアクセラレーションドメイン名であり、形式は&lt;BucketName-APPID>.file.mycloud.comです。ユーザーは有効か無効を選択することができます。
- カスタムCDNアクセラレーションドメイン名：ユーザーはICP登録済みのカスタムドメイン名を自ら用意し、オリジンサーバーをCOSバケットに指定することで、カスタムドメイン名を使用したバケット内のオブジェクトに対するアクセラレーションアクセスを実現できます。

>? デフォルトのCDNアクセラレーションドメイン名とカスタムCDNアクセラレーションドメイン名を総称してCDNアクセラレーションドメイン名と呼びます。

#### パブリック読み取りバケット

バケットをパブリックアクセス可に設定すると、CDN back-to-originをCOSアクセスノードに設定した場合、back-to-origin認証を有効にしなくても、CDNエッジノードがバケット内のオブジェクトデータを取得してキャッシングすることが可能です。

CDNコンソールで[認証設定](https://intl.cloud.tencent.com/document/product/228/35237)を有効にすることで、バケット内のデータを**条件付きで**保護することが可能です。これは、CDNのこの機能を有効にしているかどうかにもかかわらず、バケットのアクセスドメイン名を知っているユーザーはバケット内のすべてのオブジェクトにアクセスできるためです。CDN認証の設定による、パブリック読み取りバケットに対するドメイン名のアクセス機能の違いについては、次の表をご参照ください：

| CDN認証設定 | CDNアクセラレーションドメイン名アクセス | COSドメイン名アクセス | 一般的なケース                                        |
| ------------ | ---------------- | ------------ | ----------------------------------------------- |
| 無効（デフォルト） | アクセス可           | アクセス可       | 全サーバーでパブリックアクセス可、CDNとオリジンサーバーどちらからのアクセスも可       |
| 有効         | URLを使用した認証が必要  | アクセス可       | CDNアクセスに対してはリンク不正アクセス防止が有効、ただしオリジンサーバーアクセスは保護されないため非推奨 |

#### プライベート読み取りバケット

バケットがデフォルトのプライベート読み取りである場合、CDN back-to-originをCOSアクセスノードに設定すると、CDNエッジノードは**データの取得とキャッシングが一切できなくなります**。そのため、CDNサービスIDをバケットアクセスポリシー（Bucket Policy）に追加して、そのIDによる次の操作の実行を許可する必要があります：

- GET Object：オブジェクトのダウンロード
- HEAD Object：オブジェクトのメタデータの検索
- OPTIONS Object：クロスドメイン設定のプリフライトリクエスト

[CDNコンソール](https://console.cloud.tencent.com/cdn)と[COSコンソール](https://console.cloud.tencent.com/cos5)ともにワンクリックによる権限付与の機能を提供しています。つまり、**CDNサービス認証の追加**をクリックして完了できます。この操作を実施した後、「Origin-pull認証」オプションをオンにしてください。これで、CDNエッジはそのサービスIDでCOSにおけるデータにアクセスします。

>!
> 1. バケットがプライベート読み取りに設定されている場合は、権限を追加し、back-to-origin認証を有効にしてください。これを行わなければCOSはアクセスが拒否されます。
> 2. CDNエッジは各ルートアカウントごとに1つのサービスアカウントを作成します。そのため、アカウントの権限はアクセラレーションドメイン名が所属するルートアカウントに対してのみ有効であり、異なるアカウント間でバインドしたアクセラレーションドメイン名はアクセスが拒否されます。

CDNサービス権限を追加し、back-to-origin認証を有効にすると、CDNエッジノードはデータをそのまま取得、キャッシングできるようになります。そのため、プライベートデータの保護が必要な場合は、[認証設定](https://intl.cloud.tencent.com/document/product/228/35237)を有効にして、バケット内のデータを保護することを強く推奨します。CDN認証設定による、プライベート読み取りバケットに対するドメイン名のアクセス機能の違いについては、次の表をご参照ください：

| CDN認証設定 | CDNアクセラレーションドメイン名アクセス | COSドメイン名アクセス    | 一般的なケース                            |
| ------------ | ---------------- | --------------- | ----------------------------------- |
| 無効（デフォルト） | アクセス可           | COSを使用した認証が必要 | CDNドメイン名に直接アクセス可能、オリジンサーバーデータ保護   |
| 有効         | URLを使用した認証が必要  | COSを使用した認証が必要 | 全リンクのアクセスを保護、CDN認証リンク不正アクセス防止をサポート |


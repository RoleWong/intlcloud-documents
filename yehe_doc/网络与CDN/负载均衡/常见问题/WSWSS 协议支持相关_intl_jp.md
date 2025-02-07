<span id="1"></span>
**製品内容**
- [WS/WSSとは何ですか。](#2)
- [WS/WSSを使用する必要があるのはなぜですか。](#3)

**製品購入**
- [WS/WSSはどのように課金されますか。](#4)

**製品の実現**
- [CLB上でWS/WSSを有効にするにはどうすればよいですか。](#5)
- [WS/WSSはどのリージョンでサポートされていますか。](#6)
<span id="2"></span>

### WS/WSSとは何ですか。
WebSocketは、単一のTCP接続上で全二重通信を行うプロトコルです。
WebSocketはクライアントとサーバー間のデータ交換をより簡単にし、サーバーがクライアントに対し能動的にデータをプッシュすることを許可します。WebSocket APIでは、ブラウザとサーバーが1回のハンドシェイクを完了するだけで、両者の間に持続的な接続を確立でき、双方向のデータ伝送が可能になります。</br>
[[ページ先頭に戻る]](#1)
<span id="3"></span>
### WS/WSSを使用する必要があるのはなぜですか。
WebSocketが登場するまでは、クライアントがサーバーのデータを取得する際はラウンドロビン方式でサーバーからデータをプル(Pull)するしかありませんでした。
このようなデータ交換方式には2つの大きな問題点があります。
1. 効率が低いこと。クライアントがリアルタイムデータを必要とする場合、頻繁にAjaxリクエストを送信してデータをプルする必要があります。
2. サーバーが能動的にデータをプッシュ(Push)できないこと。
WebSocketはこれらの問題を解決するために誕生しました。WebSocketはHTML5に伴ってリリースされた新しいプロトコルです。ブラウザとサーバー間の全二重通信(full-duplex)を実現し、メッセージベースのテキストおよびバイナリーデータを伝送できます。HTTPの持つ上記のような難しい課題をプロトコルの面から解決するものです。

WebSocketの主なメリットは次のとおりです。
1. 制御オーバーヘッドが小さいこと。接続確立後の制御に用いるパケットヘッダーが小さくなっています。HTTPリクエストでは毎回完全なヘッダーを含める必要があるのに対し、この部分のオーバーヘッドを大きく低下させることができます。
2. リアルタイム性に優れていること。WebSocketは全二重プロトコルであり、サーバーはデータをリアルタイムにクライアントにプッシュできます。
3. 接続状態が維持されること。

[[ページ先頭に戻る]](#1)

<span id="4"></span>

### WS/WSSはどのように課金されますか。
CLBはデフォルトでWS/WSSをサポートしており、追加料金はかかりません。

[[ページ先頭に戻る]](#1)
<span id="5"></span>

### CLB上でWS/WSSを有効にするにはどうすればよいですか。
**デフォルトで有効になっているため、追加設定は必要ありません**。
HTTPをリスニングするリスナーはデフォルトでWSをサポートし、HTTPSをリスニングするリスナーはデフォルトでWSSをサポートします。
WSSを使用する際、CLBはSSLオフロードを行います。
[[ページ先頭に戻る]](#1)
<span id="6"></span>

### WS/WSSはどのリージョンでサポートされていますか。
現在、WS/WSSプロトコルは**全リージョン**でサポートされています。

[[ページ先頭に戻る]](#1)


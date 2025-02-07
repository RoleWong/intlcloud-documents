## ユースケース
このドキュメントでは、Windows Server 2012 OSを例として、Windows イメージを作成する方法について説明します。別のバージョンのWindows Server OSを使用する場合は、このドキュメントを参照することもできます。

## 操作手順

### 準備作業

システムディスクイメージを作成する前に、以下の操作を行ってください：
<dx-alert infotype="explain" title="">
データディスクイメージを使用してエクスポートする場合は、この手順をスキップしてください。
</dx-alert>



#### OSのパーティションと起動方法の確認

1. OS画面で、<img src="https://main.qcloudimg.com/raw/f0c84862ef30956c201c3e7c85a26eec.png" style="margin: 0;"> をクリックして、Windows PowerShellウインドウを開きます。
2. Windows PowerShellウィンドウで、**diskmgmt.msc**を入力し、**Enter**キーを押して「ディスクの管理」ウィンドウを開きます。
3. チェックするディスク > **プロパティ**を右クリックし、**ボリューム**タブを選択して、ディスクのパーティション形式を確認できます。
2. ディスクのパーティション形式がGPTパーティションかどうかを確認します。
 - GPTパーティションの場合、サービスの移行は現在GPTパーティションをサポートしないため、[チケットを提出](https://console.intl.cloud.tencent.com/workorder/category)してフィードバックしてください。
 - そうでない場合は、次の手順に進みます。
3. 管理者としてコマンドプロンプト(CMD)を開き、次のコマンドを実行して、現在のOSがEFIモードで起動するかどうかを確認します。
```shellsession
bcdedit /enum {current}
```
次のような結果が返されます：
```shellsession
Windows ブートローダー
識別子             {current}
device                  partition=C:
path                    \WINDOWS\system32\winload.exe
description             Windows 10
locale                  ja-JP
inherit                 {bootloadersettings}
recoverysequence        {f9dbeba1-1935-11e8-88dd-ff37cca2625c}
displaymessageoverride  Recovery
recoveryenabled         Yes
flightsigning           Yes
allowedinmemorysettings 0x15000075
osdevice                partition=C:
systemroot              \WINDOWS
resumeobject            {1bcd0c6f-1935-11e8-8d3e-3464a915af28}
nx                      OptIn
bootmenupolicy          Standard
```
 - `path`パラメータに「efi」が含まれている場合、現在のOSがEFIモードで起動することを示意味します。[チケットを提出](https://console.intl.cloud.tencent.com/workorder/category)してフィードバックしてください。
 - `path` パラメータに「efi」が含まれていない場合、次の手順に進みます。

#### ソフトウエアのアンインストール

競合を引き起こす可能性があるドライバーとソフトウェア（VMware tools、Xen tools、Virtualbox GuestAdditions、および基盤となるドライバーを搭載する一部のソフトウェアを含む）をアンインストールします。

#### cloud-baseのインストール

インストールの詳細については、 [cloud-baseのインストール](https://intl.cloud.tencent.com/document/product/213/32364)ドキュメントをご参照ください。

#### Virtioドライバーの確認またはインストール

**コントロールパネル** > **プログラムと機能**を開き、検索バーでVirtioを検索します。
- 次の図に示すように、Virtioドライバーがすでにインストールされていることを示します。
![](https://main.qcloudimg.com/raw/ff1dffb01a7f77d515061bce184e033b.png)
- Virtio Driverがインストールされていない場合は、手動でインストールする必要があります。実際の状況に合わせて、ダウンロードバージョンを選択してください。
<dx-alert infotype="explain" title="">
- Tencent CloudはWindows Server 2003のインポートをサポートしていません。
- Windows Server 2008R2/2012R2/2016/2019/2022をご利用の場合は、Tencent Cloudカスタム版のVirtIOドライバーをインストールしてください。
- 別のバージョンのWindowsオペレーティングシステムを使用している場合は、まずTencent Cloudカスタム版のVirtIOドライバーをインストールしてみてください。不安定な場合は、コミュニティ版のVirtIOドライバーを試してみてください。
</dx-alert>
<dx-tabs>
::: （推奨）Tencent Cloudカスタム版をインストールします
Tencent Cloudカスタム版Virtioのダウンロードアドレスは以下のとおりです。実際のネットワーク環境に合わせてダウンロードしてください。
- パブリックネットワークのダウンロードアドレス：`http://mirrors.tencent.com/install/windows/virtio_64_1.0.9.exe`
- プライベートネットワークのダウンロードアドレス：`http://mirrors.tencentyun.com/install/windows/virtio_64_1.0.9.exe`
:::
::: コミュニティ版をインストールします
まずTencent Cloudカスタム版のVirtIOドライバーをインストールしてみてください。不安定な場合は、コミュニティ版のVirtIOドライバーを試してみてください。
[Virtio for Community Editionのダウンロードはこちらへ](https://www.linux-kvm.org/page/WindowsGuestDrivers/Download_Drivers)。
:::
</dx-tabs>


#### その他のハードウェア構成の確認

クラウドへの移行後のハードウェアの変更には、以下が含まれますが、これらに限定されません：
 - グラフィックカードが Cirrus VGAに変更されました。
 - ディスクがVirtio Disk に変更されました。
 - ENIがVirtio Nicに変更され、ローカルエリア接続がデフォルトで使用されます。

### イメージのエクスポート
実際のニーズに応じて、対応するツールを選択してイメージをエクスポートします。
<dx-tabs>
::: プラットフォームツールを使用してイメージをエクスポートする[](id:Useplatform)
VMWare vCenter ConvertまたはCitrix XenConvertなどの仮想化プラトフォームのイメージエクスポートツールを使用できます。詳細について、対象プラットフォームのエクスポートツールのドキュメントをご参照ください。


<dx-alert infotype="explain" title="">
現在、Tencent Cloudのサービス移行は、qcow2、vhd、raw、およびvmdk形式のイメージをサポートしています。
</dx-alert>


:::
::: disk2vhd を使用してイメージをエクスポートする[](id:Usedisk2vhd)
物理マシンのシステムをエクスポートする場合、またはイメージをエクスポートするためにプラットフォームツールを使用したくない場合は、disk2vhdツールを使用してイメージをエクスポートします。
1. [ここをクリックして](https://download.sysinternals.com/files/Disk2vhd.zip) disk2vhdツールをダウンロードします。
2. disk2vhdツールをインストールして実行します。
<dx-alert infotype="notice" title="">
 - 非システムディスクにdisk2vhdツールをインストールして実行してください。
 - disk2vhdは、Windowsシステムにボリュームシャドウコピーサービス（VSS）がインストールされた後にのみ起動できます。VSS機能の詳細については、 [Volume Shadow Copy Service](https://docs.microsoft.com/zh-cn/windows/win32/vss/volume-shadow-copy-service-portal?redirectedfrom=MSDN)をご参照ください。
</dx-alert>
3. 開いたdisk2vhdツールで、次の情報を設定した後、**Create**をクリックしてイメージをエクスポートします。
 - **Use Vhdx**：チェックを入れないでください。システムは現在vhdx 形式のイメージをサポートしていません。
 - **Use volume Shadow Copy**：データの整合性を確保するために、チェックを入れることをお勧めします。
 - **VHD File name**：.vhdファイルの保存場所を生成します。非システムディスクを選択してください。
 - **Volume to include**：イメージをエクスポートする際には、システムディスク全体をエクスポートする必要があります。**システムディスクのすべてのパーティション**を選択してください。そうでない場合は、イメージのインポート時にシステムが起動できないエラーが発生します。
システムディスクのパーティションは通常、C:\パーティションとその前のブートパーティション、recoveryパーティションで、通常2~3個です。すべて選択してください。
**設定例**
次の図に示すように、ディスクEでdisk2vhdツールを実行したら、システムディスクのすべてのパーティション（ブートパーティションとC:\パーティション）にチェックを入れ、「Use volume Shadow Copy」を選択し、「Use Vhdx」のチェックを解除します。イメージをエクスポートすると、生成された.vhdファイルがディスクEに保存されます。
![](https://qcloudimg.tencent-cloud.cn/raw/1e990f879e3920426abaa355f6d3adc1.png)


:::
</dx-tabs>


### イメージ形式の変換（オプション）
[イメージフォーマットの変換](https://intl.cloud.tencent.com/document/product/213/46192)を参照して、`qemu-img`を使用してイメージファイルをサポートされているフォーマットに変換してください。

### イメージの確認

<dx-alert infotype="explain" title="">
サービスを停止せずにイメージを作成するか、またはその他の理由により、作成したイメージファイルシステムが破損している可能性があります。イメージ作成後にエラーの有無を確認することをお勧めします。
</dx-alert>


イメージの形式が現在のプラットフォームでサポートしている形式と一致している場合は、イメージを直接開いてファイルシステムを確認できます。例えば、Windowsプラットフォームではvhd形式のイメージを追加でき、Linuxプラットフォームではqemu-nbd を利用してqcow2 形式のイメージを開くことができ、Xenプラットフォームではvhdファイルを直接開くことができます。

このドキュメントでは、Windowsプラットフォームを例として、「ディスクの管理」中の「VHDの接続」を通じて、vhd形式のイメージを表示する方法について説明します。
1. OS画面で、 <img src="https://main.qcloudimg.com/raw/3d815ac1c196b47b2eea7c3a516c3d88.png" style="margin:-4px 0px">を右クリックし、ポップアップメニューから**コンピュータの管理**を選択します。
2. **ストレージ** > **ディスク管理**を選択し、ディスク管理画面に移動します。
3. ウィンドウの上部で、**アクション** > **VHD追加**を選択します。以下の通りです。
![](https://main.qcloudimg.com/raw/90a6ce24b78ca128ade5018833011708.png)
次図のような結果が表示された場合、イメージの作成が成功したことを示します。
![](https://main.qcloudimg.com/raw/41eac48fe77d3773dcf1ac9121b251ce.png)

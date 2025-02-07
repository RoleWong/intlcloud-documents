## 概要
1つのCVMインスタンスは、複数のセキュリティグループにバインディングできます。CVMインスタンスが複数のセキュリティグループにバインディングされている場合、複数のセキュリティグループは優先順位（1、2など）で順に実行されます。 ユーザーは以下の操作でセキュリティグループの優先順位を調整可能です。

## 前提条件
CVMインスタンスが2つ以上のセキュリティグループに登録されています。

## 操作手順
1. [CVMコンソール](https://console.cloud.tencent.com/cvm/index)にログインします。
2. インスタンス管理画面で、CVMンスタンスIDをクリックして詳細ページを開きます。
3. **セキュリティグループ**のオプションタブを選択し、セキュリティグループ管理ページに移動します。
4. 「バインド済みのセキュリティグループ」モジュールで**並び替え**をクリックします。
    ![](https://qcloudimg.tencent-cloud.cn/raw/d43346b95d58120fe6ca411c49c594ca.png)
5. 次のアイコンをクリックし、上下にドラッグし、セキュリティグループの優先順位を調整します。位置が上であればあるほど、セキュリティグループの優先順位も高くなります。
    <img src="https://qcloudimg.tencent-cloud.cn/raw/3ae4c410501f99cde11339e41bb65030.png" />
5. 調整が完了したら、**保存**をクリックします。


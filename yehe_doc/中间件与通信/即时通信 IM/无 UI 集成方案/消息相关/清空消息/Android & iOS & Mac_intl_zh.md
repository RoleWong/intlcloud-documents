## 功能描述
清空消息分为两种：清空单聊消息、清空群聊消息。
清空消息会同时清空当前会话内所有的消息，包含**本地和云端**消息，但不会删除会话本身。

> ?
> - 如果不想清空云端消息，请**不要**使用本接口。
> - 5.4.666 及以上版本支持。

清空消息后，会话的 `lastMessage` 会变为空，那么：
* 如果您的 SDK 版本是 5.5.892 之前，使用了 `lastMesasge` 进行排序，此时会影响您的会话列表顺序。
* 如果你的 SDK 版本是 5.5.892 及以后，并且采用了 `orderKey` 进行排序，此时不影响您的会话列表顺序。
详情请参考 [会话列表](https://intl.cloud.tencent.com/document/product/1047/48326)。

### 清空单聊消息

您可以调用 `clearC2CHistoryMessage` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a29aa6e75c2238c35cc609bef0e5a46ce) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a005c7767172d9a3980974b68c780c33b) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMMessageManager.html#aade0f8d9a53a87473990714f17a297bc)) 清空单聊消息。


示例代码如下：

<dx-tabs>
::: Android
```java
V2TIMManager.getMessageManager().clearC2CHistoryMessage(userID, new V2TIMCallback() {
	@Override
	public void onSuccess() {
		// 清空单聊消息成功
	}

	@Override
	public void onError(int code, String desc) {
		// 清空单聊消息失败
	}
});
```
:::
::: iOS & Mac
```objectivec
[V2TIMManager.sharedInstance clearC2CHistoryMessage:userID
                                               succ:^{
    NSLog(@"清空单聊消息成功");
} fail:^(int code, NSString *desc) {
    NSLog(@"清空单聊消息失败, code: %d, desc: %@", code, desc);
}];
```
:::
::: Windows
```cpp
class Callback final : public V2TIMCallback {
public:
    using SuccessCallback = std::function<void()>;
    using ErrorCallback = std::function<void(int, const V2TIMString&)>;

    Callback() = default;
    ~Callback() override = default;

    void SetCallback(SuccessCallback success_callback, ErrorCallback error_callback) {
        success_callback_ = std::move(success_callback);
        error_callback_ = std::move(error_callback);
    }

    void OnSuccess() override {
        if (success_callback_) {
            success_callback_();
        }
    }
    void OnError(int error_code, const V2TIMString& error_message) override {
        if (error_callback_) {
            error_callback_(error_code, error_message);
        }
    }

private:
    SuccessCallback success_callback_;
    ErrorCallback error_callback_;
};

auto callback = new Callback;
callback->SetCallback(
    [=]() {
        // 清空单聊消息成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 清空单聊消息成功
        delete callback;
    });
```
:::
</dx-tabs>


### 清空群聊消息

您可以调用 `clearGroupHistoryMessage` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a6e1a1dce441243d0bd5ac2f8bcecb3d9) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a16c01c19a285e2bd11443c868c8256e6) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMMessageManager.html#a1207155633e3deb59616d4deb779d1eb)) 清空群聊消息。


示例代码如下：
<dx-tabs>
::: Android
```java
V2TIMManager.getMessageManager().clearGroupHistoryMessage(GroupID, new V2TIMCallback() {
	@Override
	public void onSuccess() {
		// 清空群聊消息成功
	}

	@Override
	public void onError(int code, String desc) {
		// 清空群聊消息失败
	}
});
```
:::
::: iOS & Mac
```objectivec
[[V2TIMManager sharedInstance] clearGroupHistoryMessage:groupID
                                                   succ:^{
    NSLog(@"清空群聊消息成功");
} fail:^(int code, NSString *desc) {
    NSLog(@"清空群聊消息失败, code: %d, desc: %@", code, desc);
}];
```
:::
::: Windows
```cpp
class Callback final : public V2TIMCallback {
public:
    using SuccessCallback = std::function<void()>;
    using ErrorCallback = std::function<void(int, const V2TIMString&)>;

    Callback() = default;
    ~Callback() override = default;

    void SetCallback(SuccessCallback success_callback, ErrorCallback error_callback) {
        success_callback_ = std::move(success_callback);
        error_callback_ = std::move(error_callback);
    }

    void OnSuccess() override {
        if (success_callback_) {
            success_callback_();
        }
    }
    void OnError(int error_code, const V2TIMString& error_message) override {
        if (error_callback_) {
            error_callback_(error_code, error_message);
        }
    }

private:
    SuccessCallback success_callback_;
    ErrorCallback error_callback_;
};

auto callback = new Callback;
callback->SetCallback(
    [=]() {
        // 清空群聊消息成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 清空群聊消息失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetMessageManager()->ClearGroupHistoryMessage(groupID, callback);
```
:::
</dx-tabs>


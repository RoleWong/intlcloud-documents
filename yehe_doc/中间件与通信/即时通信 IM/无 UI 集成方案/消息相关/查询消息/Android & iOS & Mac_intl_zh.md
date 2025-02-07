## 功能描述
您可以根据 messageID 调用 `findMessages` 查询本地消息。请注意：
1. 只支持查询本地消息，例如接收到的消息或者调用拉取历史消息接口获取到的消息。
2. 不支持查询直播群（AVChatRoom）的消息，因为直播群的消息不会保存在本地。

## 查询本地消息
查询本地消息的接口为 `findMessages` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#ad0dbaec04bc389d01f815f46c550e2fd) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a4a0c47d706d8784656225c1e9065f6f1) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMMessageManager.html#ac5531e73378b8b8eadd056ba99e5427e)) 。

示例代码如下：

<dx-tabs>
::: Android
```java
V2TIMManager.getMessageManager().findMessages(messageIDList, new V2TIMValueCallback<List<V2TIMMessage>>() {
    @Override
    public void onSuccess(List<V2TIMMessage> v2TIMMessages) {}

    @Override
    public void onError(int code, String desc) {}
});
```
:::
::: iOS & Mac
```objectivec
[V2TIMManager.sharedInstance findMessages:messageIDList
                                        succ:^(NSArray<V2TIMMessage *> *msgs) {
    // 查询消息成功
} fail:^(int code, NSString *desc) {
    // 查询消息失败
}];
```
:::
::: Windows
```cpp
template <class T>
class ValueCallback final : public V2TIMValueCallback<T> {
public:
    using SuccessCallback = std::function<void(const T&)>;
    using ErrorCallback = std::function<void(int, const V2TIMString&)>;

    ValueCallback() = default;
    ~ValueCallback() override = default;

    void SetCallback(SuccessCallback success_callback, ErrorCallback error_callback) {
        success_callback_ = std::move(success_callback);
        error_callback_ = std::move(error_callback);
    }

    void OnSuccess(const T& value) override {
        if (success_callback_) {
            success_callback_(value);
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

auto callback = new ValueCallback<V2TIMMessageVector>{};
callback->SetCallback(
    [=](const V2TIMMessageVector& messageVector) {
        // 查询消息成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 查询消息失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetMessageManager()->FindMessages(messageIDList, callback);
```
:::
</dx-tabs>


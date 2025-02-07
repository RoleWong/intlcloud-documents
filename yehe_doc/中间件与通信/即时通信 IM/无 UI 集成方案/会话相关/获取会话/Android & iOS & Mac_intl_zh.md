## 功能描述
IM SDK 提供获取会话的接口，可以获取指定的单个、多个会话的 `V2TIMConversation` 对象信息。

### 获取指定的单个会话
您可以调用 `getConversation`([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMConversationManager.html#a619aaff2bb5664e094d2341819b95096) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Conversation_08.html#ad4b7b80fbe0cff25027371b416ede9f9) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMConversationManager.html#a9891f4b029e7a1fd3d17398cbe1b367c)) 获取单个会话的信息，会话信息是一个 `V2TIMConversation` 对象。

示例代码如下：
<dx-tabs>
::: Android
```java
String conversationID = "conversationID";
V2TIMManager.getConversationManager().getConversation(conversationID, new V2TIMValueCallback<V2TIMConversation>() {
    @Override
    public void onSuccess(V2TIMConversation v2TIMConversation) {
        Log.i("imsdk", "success");
    }

    @Override
    public void onError(int code, String desc) {
        Log.i("imsdk", "failure, code:" + code + ", desc:" + desc);
    }
});
```
:::
::: iOS & Mac
```objectivec
[V2TIMManager.sharedInstance getConversation:@"conversationID" // 要查询的会话 ID
                                        succ:^(V2TIMConversation *conv) {
    // 获取成功，返回 V2TIMConversation 对象
} fail:^(int code, NSString *desc) {
    // 获取失败
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

V2TIMString conversationID = u8"conversationID";

auto callback = new ValueCallback<V2TIMConversation>{};
callback->SetCallback(
    [=](const V2TIMConversation& conversation) {
        // 获取单个会话成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 获取单个会话失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetConversationManager()->GetConversation(conversationID, callback);
```
:::
</dx-tabs>

### 获取指定的多个会话

`getConversationList`([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMConversationManager.html#a41ebb09032a6bbda0a78e8734c61fb93) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Conversation_08.html#ab1f5e86e270b122cb725266d234d9dd5) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMConversationManager.html#aa1aec337ed398f603f15f14ee9023d62)) 获取指定的会话列表，列表中存储的是 `V2TIMConversation` 对象。

示例代码如下：
<dx-tabs>
::: Android
```java
List<String> conversationIDList = new ArrayList<>();
conversationIDList.add("conversationID1");
conversationIDList.add("conversationID1");

V2TIMManager.getConversationManager().getConversationList(conversationIDList, new V2TIMValueCallback<List<V2TIMConversation>>() {
    @Override
    public void onSuccess(List<V2TIMConversation> conversationList) {
        Log.i("imsdk", "success");
    }

    @Override
    public void onError(int code, String desc) {
        Log.i("imsdk", "failure, code:" + code + ", desc:" + desc);
    }
});
```
:::
::: iOS & Mac
```objectivec
[V2TIMManager.sharedInstance getConversationList:@[@"convID1", @"convID2"] // 要查询的会话 ID 列表
                                            succ:^(NSArray<V2TIMConversation *> *list) {
    // 获取成功，list 中包含 V2TIMConversation 对象
} fail:^(int code, NSString *desc) {
    // 获取失败
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

V2TIMStringVector conversationIDList;
conversationIDList.PushBack(u8"conversationID1");
conversationIDList.PushBack(u8"conversationID2");

auto callback = new ValueCallback<V2TIMConversationVector>{};
callback->SetCallback(
    [=](const V2TIMConversationVector& conversationList) {
        // 获取多个会话成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 获取多个会话失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetConversationManager()->GetConversationList(conversationIDList, callback);
```
:::
</dx-tabs>


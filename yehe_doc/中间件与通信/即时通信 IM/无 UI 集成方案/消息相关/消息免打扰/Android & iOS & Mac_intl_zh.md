## 功能描述
通过设置单聊和群聊的消息接收选项，可以实现类似消息免打扰的功能。
IM SDK 支持三种类型的消息接收选项，消息接收选项在 `V2TIMReceiveMessageOpt` 中定义：

| 消息接收选项 | 功能描述 |
|---------|---------|
| V2TIM_RECEIVE_MESSAGE | 在线时正常接收消息，离线时接收离线推送通知 | 
| V2TIM_NOT_RECEIVE_MESSAGE | 在线和离线都不接收消息 | 
| V2TIM_RECEIVE_NOT_NOTIFY_MESSAGE | 在线时正常接收消息，离线时不接收离线推送通知 | 

> ? 消息接收选项仅增强版 SDK 5.3.425 及以上版本支持。

设置不同的 `V2TIMReceiveMessageOpt` 可以实现群消息免打扰：

**完全不接收消息**
消息接收选项设置为 `V2TIM_NOT_RECEIVE_MESSAGE` 后，单聊/群聊的任何消息都收不到，会话列表也不会更新。

**接收消息但不提醒，在会话列表界面显示小圆点（不显示未读数）**
1. 消息接收选项设置为 `V2TIM_RECEIVE_NOT_NOTIFY_MESSAGE`。
2. 当单聊/群聊收到新消息，会话列表需要更新时，通过会话 `V2TIMConversation` 中的 `unreadCount` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMConversation.html#ab6a7667ac8a9f7a17a38ee8e7caec98e) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/interfaceV2TIMConversation.html#a816b83eb32d84ea5345f14ced92bb7f6) / [Windows](https://im.sdk.qcloud.com/doc/en/structV2TIMTopicInfo.html#a15eb6e4566938daff8b8e82e45549a78)) 获取到消息未读数。
3. 根据 `V2TIMConversation` 的 `recvOpt` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMConversation.html#a82f673186669d31f7acd38c52d412ba2) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/interfaceV2TIMConversation.html#a851651878491c64d73aa83131134e6cc) / [Windows](https://im.sdk.qcloud.com/doc/en/structV2TIMTopicInfo.html#a851651878491c64d73aa83131134e6cc)) 判断获取到的消息接收选项为 `V2TIM_RECEIVE_NOT_NOTIFY_MESSAGE` 时显示小红点而非消息未读数。

> ? 此方式需使用 `unreadCount` 功能，仅适用于好友工作群（Work）、陌生人社交群（Public）和社群（Community），直播群（AVChatRoom）、临时会议群（Meeting）暂不支持。群组类型详见 [群组介绍](https://intl.cloud.tencent.com/document/product/1047/33529)。


## 设置单聊的消息接收选项
您可以调用 `setC2CReceiveMessageOpt`([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a6524143895cdee25fabd9aeeae73a3c5) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#ae628f19d856921d27081c3f40005e9d9) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMMessageManager.html#adf166f08b68a5df8de19d152bcf868b3)) 设置单聊的消息接收选项。
您可通过参数 `userIDList` 设置一批用户，但一次最大允许设置 30 个用户。

>! 该接口调用频率被限制为单个用户 1 秒内最多调用 5 次。

示例代码如下：
<dx-tabs>
::: Android
```java
// 设置在线和离线都不接收消息

List<String> userList = new ArrayList<>();
userList.add("user1");
userList.add("user2");

V2TIMManager.getMessageManager().setC2CReceiveMessageOpt(userList, V2TIMMessage.V2TIM_NOT_RECEIVE_MESSAGE, new V2TIMCallback() {
    @Override
    public void onSuccess() {
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
// 设置在线和离线都不接收消息

NSArray* array = [NSArray arrayWithObjects:@"user1", @"user2", nil]];
[[V2TIMManager sharedInstance] setC2CReceiveMessageOpt:array opt:V2TIM_NOT_RECEIVE_MESSAGE succ:^{
    NSLog(@"success");
} fail:^(int code, NSString *desc) {
    NSLog(@"failure, code:%d, desc:%@", code, desc);
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

V2TIMStringVector userIDList;
userIDList.PushBack("user1");
userIDList.PushBack("user2");
V2TIMReceiveMessageOpt opt = V2TIMReceiveMessageOpt::V2TIM_NOT_RECEIVE_MESSAGE;

auto callback = new Callback;
callback->SetCallback(
    [=]() {
        // 设置成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 设置失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetMessageManager()->SetC2CReceiveMessageOpt(userIDList, opt, callback);
```
:::
</dx-tabs>

## 获取单聊的消息接收选项
您可以通过调用 `getC2CReceiveMessageOpt`([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a9693dd66432f931ac0a1f2168d899501) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a1c743a6fe1d17a21dc80e584fd1de2d1) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMMessageManager.html#a30a4979460e73c897b6130ba40356afa)) 获取单聊的消息接收选项。

示例代码如下：
<dx-tabs>
::: Android
```java
List<String> userList = new ArrayList<>();
userList.add("user1");
userList.add("user2");

V2TIMManager.getMessageManager().getC2CReceiveMessageOpt(userList, new V2TIMValueCallback<List<V2TIMReceiveMessageOptInfo>>() {
    @Override
    public void onSuccess(List<V2TIMReceiveMessageOptInfo> v2TIMReceiveMessageOptInfos) {
        for (int i = 0; i < v2TIMReceiveMessageOptInfos.size(); i++){
            V2TIMReceiveMessageOptInfo info = v2TIMReceiveMessageOptInfos.get(i);
            Log.i("imsdk", "userId: " + info.getUserID() ", receiveOpt: " + info.getC2CReceiveMessageOpt());
        }
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
NSArray* array = [NSArray arrayWithObjects:@"user1", @"user2", nil]];
[[V2TIMManager sharedInstance] getC2CReceiveMessageOpt:array succ:^(NSArray<V2TIMReceiveMessageOptInfo *> *optList) {
    for (int i = 0; i < optList.count; i++) {
        V2TIMReceiveMessageOptInfo* info = optList[i];
        NSLog(@"userId: %@, receiveOpt: %@", info.userID, info.receiveOpt);
    }
} fail:^(int code, NSString *desc) {
    NSLog(@"failure, code:%d, desc:%@", code, desc);
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

V2TIMStringVector userIDList;
userIDList.PushBack("user1");
userIDList.PushBack("user2");

auto callback = new ValueCallback<V2TIMReceiveMessageOptInfoVector>{};
callback->SetCallback(
    [=](const V2TIMReceiveMessageOptInfoVector& receiveMessageOptInfoList) {
        for (size_t i = 0; i < receiveMessageOptInfoList.Size(); ++i) {
            const V2TIMReceiveMessageOptInfo& opt = receiveMessageOptInfoList[i];
            V2TIMString userID = opt.userID;
            V2TIMReceiveMessageOpt receiveOpt = opt.receiveOpt;
        }

        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 获取失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetMessageManager()->GetC2CReceiveMessageOpt(userIDList, callback);
```
:::
</dx-tabs>


## 设置群聊的消息接收选项
您可以调用 `setGroupReceiveMessageOpt`([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMMessageManager.html#a2735427ac22485626aea278a9d465b3e) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Message_08.html#a379eeef926e41ec5d48287e7fb55b80a) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMMessageManager.html#a866d06c28faf058f253f29be6f5b3fe2)) 设置群聊的消息接收选项。

示例代码如下：
<dx-tabs>
::: Android
```java
// 设置在线和离线都不接收消息

String groupID = "groupID";
V2TIMManager.getMessageManager().setGroupReceiveMessageOpt(groupID, V2TIMMessage.V2TIM_NOT_RECEIVE_MESSAGE, new V2TIMCallback() {
    @Override
    public void onSuccess() {
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
// 设置在线和离线都不接收消息

NSString *groupID = @"groupID";
[[V2TIMManager sharedInstance] setGroupReceiveMessageOpt:groupID opt:V2TIM_NOT_RECEIVE_MESSAGE succ:^{
    NSLog(@"success");
} fail:^(int code, NSString *desc) {
    NSLog(@"failure, code:%d, desc:%@", code, desc);
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

V2TIMString groupID = "groupID";
V2TIMReceiveMessageOpt opt = V2TIMReceiveMessageOpt::V2TIM_NOT_RECEIVE_MESSAGE;

auto callback = new Callback;
callback->SetCallback(
    [=]() {
        // 设置成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 设置失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetMessageManager()->SetGroupReceiveMessageOpt(groupID, opt, callback);
```
:::
</dx-tabs>

## 获取群聊的消息接收选项
您可以调用 `getGroupsInfo`([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMGroupManager.html#ada614335043d548c11f121500e279154) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Group_08.html#a9bca7e5318cfed44335566a783a6b568) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMGroupManager.html#a8c98b92b45c3a2c4e57901e6c4cd3435)) 获得群资料 `V2TIMGroupInfo` 对象列表，对象的 `recvOpt` 字段表示群组的消息接收选项。

示例代码如下：
<dx-tabs>
::: Android
```java
List<String> groupIDList = new ArrayList<>();
groupIDList.add("groupID1");
groupIDList.add("groupID2");
V2TIMManager.getGroupManager().getGroupsInfo(groupIDList, new V2TIMValueCallback<List<V2TIMGroupInfoResult>>() {
    @Override
    public void onSuccess(List<V2TIMGroupInfoResult> v2TIMGroupProfileResults) {
        for (V2TIMGroupInfoResult result : v2TIMGroupProfileResults) {
            V2TIMGroupInfo info = result.getGroupInfo();
            Log.i("imsdk", "recvOpt: " + info.getRecvOpt());
        }
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
NSArray* array = [NSArray arrayWithObjects:@"groupID1", @"groupID2", nil]];
[[V2TIMManager sharedInstance] getGroupsInfo:array succ:^(NSArray<V2TIMGroupInfoResult *> * groupResultList) {
    for (V2TIMGroupInfoResult *result in groupResultList){
        V2TIMGroupInfo *info = result.info;
        NSLog(@"recvOpt, %d", (int)info.recvOpt);
    }
} fail:^(int code, NSString *desc) {
    NSLog(@"failure, code:%d, desc:%@", code, desc);
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

V2TIMStringVector groupIDList;
groupIDList.PushBack("groupID1");
groupIDList.PushBack("groupID2");

auto callback = new ValueCallback<V2TIMGroupInfoResultVector>{};
callback->SetCallback(
    [=](const V2TIMGroupInfoResultVector& groupInfoResultList) {
        for (size_t i = 0; i < groupInfoResultList.Size(); ++i) {
            const V2TIMGroupInfo& info = groupInfoResultList[i].info;
            V2TIMReceiveMessageOpt recvOpt = info.recvOpt;
        }

        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 获取失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetGroupManager()->GetGroupsInfo(groupIDList, callback);
```
:::
</dx-tabs>


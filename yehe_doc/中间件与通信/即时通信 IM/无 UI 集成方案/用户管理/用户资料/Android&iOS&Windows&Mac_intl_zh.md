## 功能描述
用户可以查询自己、好友、非好友的信息；可以修改自己的昵称、头像、签名等信息；可以修改好友的备注、分组等信息。
相关方法在核心类 `V2TIMManager`、`V2TIMFriendshipManager(Android)` / `V2TIMManager(Friendship)(iOS & Mac)` 中。


## 关系链事件监听器
您可以调用 `addFriendListener` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMFriendshipManager.html#af09d0d2297fe73cc81b8e8941bcd35b2) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Friendship_08.html#a1de011b63b3c20b1be519dc7ba124704) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMFriendshipManager.html#ac4c542617008471fa1fe7a64ba963fbb)) 添加关系链事件监听器。

当不想再接收关系链事件时，可调用 `removeFriendListener` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMFriendshipManager.html#a6a823459f109bcd8744806f8f1b8bfea) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Friendship_08.html#aed40ccbbc4e15be79154077a6d3ec085) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMFriendshipManager.html#ae21ca2737c35305ecc1f25e054265ed8)) 移除关系链事件监听器。

> ! 只有预先设置好关系链事件监听器，才能正常接收到下文中的各种事件通知。

示例代码如下：
<dx-tabs>
::: Android
```java
// 添加关系链监听器
V2TIMManager.getFriendshipManager().addFriendListener(listener);
// 移除关系链监听器
V2TIMManager.getFriendshipManager().removeFriendListener(listener);
```
:::
::: iOS & Mac
```objectivec
// 添加关系链监听器
// self 为 id<V2TIMFriendshipListener>
[[V2TIMManager sharedInstance] addFriendListener:self];

// 移除关系链监听器
[[V2TIMManager sharedInstance] removeFriendListener:self];
```
:::
::: Windows
```cpp
class FriendshipListener final : public V2TIMFriendshipListener {
    // 成员 ...
};

// 添加关系链事件监听器，注意在移除监听器之前需要保持 friendshipListener 的生命期，以免接收不到事件回调
FriendshipListener friendshipListener;
V2TIMManager::GetInstance()->GetFriendshipManager()->AddFriendListener(&friendshipListener);

// 移除关系链监听器
V2TIMManager::GetInstance()->GetFriendshipManager()->RemoveFriendListener(&friendshipListener);
```
:::
</dx-tabs>


## 用户资料管理
### 查询和修改自己的资料
您可以调用 `getUsersInfo` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMManager.html#a7ca8c0f71a9875021fc35dfcaff68d1e) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/interfaceV2TIMManager.html#ac638c1dd6ec1e5204637e4b87129cd38) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMManager.html#ac7aa404aec07fc0d9823d9da5fd4e443)) 接口查询个人资料，其中参数 `userIDList` 需填入自己的 UserID。

示例代码如下：
<dx-tabs>
::: Android
```java
// 获取个人资料
String loginUser = V2TIMManager.getInstance().getLoginUser();
List<String> userIDList = new ArrayList<>();
userIDList.add(loginUser);
V2TIMManager.getInstance().getUsersInfo(userIDList, new V2TIMValueCallback<List<V2TIMUserFullInfo>>() {
  @Override
  public void onSuccess(List<V2TIMUserFullInfo> profiles) {
  	// 获取个人资料成功
  }
  @Override
  public void onError(int code, String desc) {
  	// 获取个人资料失败
  }
});
```
:::
::: iOS & Mac
```objectivec
// 获取个人资料
NSString *loginUser = [[V2TIMManager sharedInstance] getLoginUser];
[[V2TIMManager sharedInstance] getUsersInfo:@[loginUser] succ:^(NSArray<V2TIMUserFullInfo *> *infoList) {
    // 获取个人资料成功
} fail:^(int code, NSString *desc) {
    // 获取个人资料失败
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

V2TIMString loginUser = V2TIMManager::GetInstance()->GetLoginUser();
V2TIMStringVector userIDList;
userIDList.PushBack(loginUser);

auto callback = new ValueCallback<V2TIMUserFullInfoVector>{};
callback->SetCallback(
    [=](const V2TIMUserFullInfoVector& userFullInfoList) {
        // 获取个人资料成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 获取个人资料失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetUsersInfo(userIDList, callback);
```
:::
</dx-tabs>

您可以调用 `setSelfInfo` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMManager.html#af004ab2f1d1458de354883f1995b678a) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/interfaceV2TIMManager.html#a328a0d2d07e8d34e12effb73937c3437) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMManager.html#acb89663576c7f68cc4b9983733835e29)) 接口修改个人资料。
个人资料包括昵称、头像、签名、性别、出生日期、好友验证方式等，详情可参考 `V2TIMUserFullInfo`([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMUserFullInfo.html) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/interfaceV2TIMUserFullInfo.html) / [Windows](https://im.sdk.qcloud.com/doc/en/structV2TIMUserFullInfo.html)) 类定义。
资料修改成功后，您会收到 `onSelfInfoUpdated` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMSDKListener.html#a94852c92bb087e5aabb6cf6fe9ba77f8) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/protocolV2TIMSDKListener-p.html#ab3e8543e99934530763daa7eeffbab89) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMSDKListener.html#a0aa71b6e7638fddd65f3e82f7eb264c3)) 回调。

示例代码如下：
<dx-tabs>
::: Android
```java
// 设置个人资料
V2TIMUserFullInfo info = new V2TIMUserFullInfo();
info.setNickname("nickName");
info.setFaceUrl("faceUrl");
V2TIMManager.getInstance().setSelfInfo(info, new V2TIMCallback() {
  @Override
  public void onSuccess() {
		// 设置个人资料成功
  }

  @Override
  public void onError(int code, String desc) {
		// 设置个人资料失败
  }
});

// 监听个人资料变更回调
V2TIMManager.getInstance().addIMSDKListener(new V2TIMSDKListener() {
  @Override
  public void onSelfInfoUpdated(V2TIMUserFullInfo info) {
  	// 收到个人资料变更回调
  }
});
```
:::
::: iOS & Mac
```objectivec
// 设置个人资料
V2TIMUserFullInfo *info = [[V2TIMUserFullInfo alloc] init];
info.nickName = @"nickName";
info.faceURL = @"faceURL";
[[V2TIMManager sharedInstance] setSelfInfo:info succ:^{
    // 设置个人资料成功
} fail:^(int code, NSString *desc) {
    // 设置个人资料失败
}];

// 监听个人资料变更回调
[[V2TIMManager sharedInstance] addIMSDKListener:self];
- (void)onSelfInfoUpdated:(V2TIMUserFullInfo *)Info {
    // 收到个人资料变更回调
}
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

V2TIMUserFullInfo info;
info.nickName = u8"nickName";
info.faceURL = u8"faceUrl";
info.modifyFlag = V2TIMUserInfoModifyFlag::V2TIM_USER_INFO_MODIFY_FLAG_NICK |
                  V2TIMUserInfoModifyFlag::V2TIM_USER_INFO_MODIFY_FLAG_FACE_URL;

auto callback = new Callback{};
callback->SetCallback(
    [=]() {
        // 设置个人资料成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 设置个人资料失败
        delete callback;
    });

V2TIMManager::GetInstance()->SetSelfInfo(info, callback);

// 监听个人资料变更回调
class SDKListener final : public V2TIMSDKListener {
public:
    SDKListener() = default;
    ~SDKListener() override = default;

    void OnSelfInfoUpdated(const V2TIMUserFullInfo& info) override {
        // 收到个人资料变更回调
    }
    // 其他成员 ...
};
// 添加事件监听器，注意在移除监听器之前需要保持 sdkListener 的生命期，以免接收不到事件回调
SDKListener sdkListener;
V2TIMManager::GetInstance()->AddSDKListener(&sdkListener);
```
:::
</dx-tabs>

### 查询和修改好友资料
您可以调用 `getFriendsInfo` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMFriendshipManager.html#a8c4e51e508d140e4a7ce5f4caf49c870) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Friendship_08.html#a39f6752da11b595e4a5b6dcb0eb6a584) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMFriendshipManager.html#a9b263f612a1e7e35ee2c745b5f36a1e3)) 接口查询指定的好友资料。详情请参考 [好友管理](https://intl.cloud.tencent.com/document/product/1047/48159)。

### 查询非好友用户资料
您可以调用 `getUsersInfo` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMManager.html#a7ca8c0f71a9875021fc35dfcaff68d1e) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/interfaceV2TIMManager.html#ac638c1dd6ec1e5204637e4b87129cd38) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMManager.html#ac7aa404aec07fc0d9823d9da5fd4e443)) 接口查询非好友资料，其中参数 `userIDList` 填入非好友的 UserID 即可。

> ? 
> 1. 不能修改非好友的资料。
> 2. 非好友资料更新时，由于没有好友关系，后台无法向 SDK 发送系统通知，因此**无法实时更新**。为了避免每次获取用户资料都向后台发起网络请求，节省网络资源，SDK 增加了缓存逻辑，对同一个用户主动向后台拉取资料的时间间隔为 10 分钟。

示例代码如下：

<dx-tabs>
::: Android
```java
List<String> userIDList = new ArrayList<>();
userIDList.add("userA");
V2TIMManager.getInstance().getUsersInfo(userIDList, new V2TIMValueCallback<List<V2TIMUserFullInfo>>() {
  @Override
  public void onSuccess(List<V2TIMUserFullInfo> profiles) {
  	// 获取非好友资料成功
  }

  @Override
  public void onError(int code, String desc) {
  	// 获取非好友资料失败
  }
});
```
:::
::: iOS & Mac
```objectivec
// 获取非好友资料
[[V2TIMManager sharedInstance] getUsersInfo:@[@"userA"] succ:^(NSArray<V2TIMUserFullInfo *> *infoList) {
    // 获取非好友资料成功
} fail:^(int code, NSString *desc) {
    // 获取非好友资料失败
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
userIDList.PushBack(u8"userA");

auto callback = new ValueCallback<V2TIMUserFullInfoVector>{};
callback->SetCallback(
    [=](const V2TIMUserFullInfoVector& userFullInfoList) {
        // 获取非好友资料成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 获取非好友资料失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetUsersInfo(userIDList, callback);
```
:::
</dx-tabs>


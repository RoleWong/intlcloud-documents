## 功能描述
群成员资料类为 `V2TIMGroupMemberFullInfo`，内含群成员 userID、自定义信息、角色、禁言等信息。
相关方法在核心类 `V2TIMGroupManager(Android)` / `V2TIMManager(Group)(iOS & Mac)` 中。

[](id:getGroupMembersInfo)
## 获取群成员资料
您可以调用 `getGroupMembersInfo` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMGroupManager.html#adb08e1c4fa9aff407c7b2678757f66d5) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Group_08.html#a1ab284b80811bcc697d689d7b97edf04) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMGroupManager.html#a6db2fcfd78bbd71003ae31584c88c672)) 获取群成员资料。该接口支持批量获取，您可以一次传入多个 `userID` 获取多个群成员的资料，从而提升网络传输效率。

示例代码如下：

<dx-tabs>
::: Android
```java
List<String> userIDList = new ArrayList<>();
userIDList.add("userA");
userIDList.add("userB");
V2TIMManager.getGroupManager().getGroupMembersInfo("groupA", userIDList, new V2TIMValueCallback<List<V2TIMGroupMemberFullInfo>>() {
  @Override
  public void onSuccess(List<V2TIMGroupMemberFullInfo> v2TIMGroupMemberFullInfos) {
		// 获取成功
  }

  @Override
  public void onError(int code, String desc) {
		// 获取失败
  }
});
```
:::
::: iOS & Mac
```objectivec
[[V2TIMManager sharedInstance] getGroupMembersInfo:@"groupA" memberList:@[@"user1"] succ:^(NSArray<V2TIMGroupMemberFullInfo *> *memberList) {
    // 获取成功
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

V2TIMString groupID = "groupA";
V2TIMStringVector memberList;
memberList.PushBack("userA");
memberList.PushBack("userB");

auto callback = new ValueCallback<V2TIMGroupMemberFullInfoVector>{};
callback->SetCallback(
    [=](const V2TIMGroupMemberFullInfoVector& groupMemberFullInfoList) {
        // 获取成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 获取失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetGroupManager()->GetGroupMembersInfo(groupID, memberList, callback);
```
:::
</dx-tabs>

[](id:setGroupMemberInfo)
## 修改群成员资料
群主或管理员可以调用 `setGroupMemberInfo` ([Android](https://im.sdk.qcloud.com/doc/en/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMGroupManager.html#a6f1cf8ede41348b4cd7b63b8e4caa77b) / [iOS & Mac](https://im.sdk.qcloud.com/doc/en/categoryV2TIMManager_07Group_08.html#a40b97ee4b138f93e1b2073d1bdff3756) / [Windows](https://im.sdk.qcloud.com/doc/en/classV2TIMGroupManager.html#acd0e222e4c3d5997666aaf4126bd974e)) 接口修改群成员的群名片（`nameCard`）、自定义字段（`customInfo`）等与群成员相关的资料。

普通群成员可以调用 `setGroupMemberInfo` 设置自己的群名片（`nameCard`）、自定义字段（`customInfo`）等信息。

> ? 直播群（AVChatRoom）不存储群成员信息，设置直播群成员名片不适用于直播群。

如果要修改群成员自定义字段，您必须提前在 [即时通信 IM 控制台](https://console.cloud.tencent.com/im) 配置好该字段，配置页面如下图所示：
<img src="https://qcloudimg.tencent-cloud.cn/raw/9cd876542e8082db92d05810fd883b8d.png" alt="" style="zoom:90%;" />

> ! 群成员自定义字段最多可设置 5 个。字段创建后，该字段将不可删除，也无法修改字段名与字段类型。

示例代码如下：

<dx-tabs>
::: Android

```java
V2TIMGroupMemberFullInfo memberFullInfo = new V2TIMGroupMemberFullInfo();
// 指定修改的群成员
memberFullInfo.setUserID("userA");
// 设置修改的 nameCard 值
memberFullInfo.setNameCard("userA_namecard");
// 设置群成员自定义字段
Map<String, byte[]> customMap = new HashMap<>();
customMap.put("member_key1", "value1".getBytes());
memberFullInfo.setCustomInfo(customMap);
V2TIMManager.getGroupManager().setGroupMemberInfo("groupA", memberFullInfo, new V2TIMCallback() {
  @Override
  public void onSuccess() {
		// 修改成功
  }

  @Override
  public void onError(int code, String desc) {
		// 修改失败
  }
});
```
:::
::: iOS & Mac
```objectivec
V2TIMGroupMemberFullInfo *memberFullInfo = [[V2TIMGroupMemberFullInfo alloc] init];
// 指定修改的群成员
memberFullInfo.userID = @"user1";
// 设置修改的 nameCard 值
memberFullInfo.nameCard = @"user1_namecard";
// 设置群成员自定义字段
memberFullInfo.customInfo = @{@"member_key1" : [@"value1" dataUsingEncoding:NSUTF8StringEncoding]};
[[V2TIMManager sharedInstance] setGroupMemberInfo:@"groupA" info:memberFullInfo succ:^{
    // 修改成功
} fail:^(int code, NSString *desc) {
    // 修改失败
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

// 指定修改的群成员
V2TIMGroupMemberFullInfo info;
info.userID = "userA";
// 设置修改的 nameCard 值
info.nameCard = "userA_namecard";
info.modifyFlag = V2TIMGroupMemberInfoModifyFlag::V2TIM_GROUP_MEMBER_INFO_MODIFY_FLAG_NAME_CARD;
// 设置群成员自定义字段
V2TIMCustomInfo customInfo;
std::string str{u8"value1"};
customInfo.Insert("member_key1", {reinterpret_cast<const uint8_t*>(str.data()), str.size()});
info.customInfo = customInfo;
info.modifyFlag |= V2TIMGroupMemberInfoModifyFlag::V2TIM_GROUP_MEMBER_INFO_MODIFY_FLAG_CUSTOM_INFO;

auto callback = new Callback;
callback->SetCallback(
    [=]() {
        // 修改成功
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // 修改失败
        delete callback;
    });

V2TIMManager::GetInstance()->GetGroupManager()->SetGroupMemberInfo("userA", info, callback);
```
:::
</dx-tabs>


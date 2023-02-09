## Overview
When sending a message, the user may want to switch to another chat window before finishing the message. The unfinished message can be saved through the `setConversationDraft` API, so that the user can get back to it through the `draftText` field of the `V2TIMConversation` object and finish it.

> ! 
> 1. A conversation draft can contain only text.
> 2. A conversation draft will be stored only in the local database and not on the server. Therefore, it cannot be synced across devices and will not be available after the application is uninstalled and reinstalled.

## Setting a Conversation Draft
Call the `setConversationDraft` API ([Android](https://im.sdk.qcloud.com/doc/zh-cn/classcom_1_1tencent_1_1imsdk_1_1v2_1_1V2TIMConversationManager.html#ae7f2f52bf375dae69368eae42edb28ab) / [iOS and macOS](https://im.sdk.qcloud.com/doc/zh-cn/categoryV2TIMManager_07Conversation_08.html#a462cd163c03cdce230ed3647b414382b) / [Windows](https://im.sdk.qcloud.com/doc/zh-cn/classV2TIMConversationManager.html#a190fb079bf34077f71c340ec23e69ebf)) to set a conversation draft.
If the `draftText` parameter is empty, the draft is cleared.

Sample code:
<dx-tabs>
::: Android
```java
String conversationID = "conversationID";
String draftText = "The draft text";
V2TIMManager.getConversationManager().setConversationDraft(conversationID, draftText, new V2TIMCallback() {
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
::: iOS and macOS
```objectivec
NSString *conversationID = @"conversationID";
NSString *draftText = "The draft text";
[[V2TIMManager sharedInstance] setConversationDraft:conversationID draftText:draftText succ:^{
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

V2TIMString conversationID = u8"conversationID";
V2TIMString draftText = u8"The draft text";

auto callback = new Callback;
callback->SetCallback(
    [=]() {
        // Set the conversation draft successfully
        delete callback;
    },
    [=](int error_code, const V2TIMString& error_message) {
        // Failed to set the conversation draft
        delete callback;
    });

V2TIMManager::GetInstance()->GetConversationManager()->SetConversationDraft(conversationID, draftText,
                                                                            callback);
```
:::
</dx-tabs>

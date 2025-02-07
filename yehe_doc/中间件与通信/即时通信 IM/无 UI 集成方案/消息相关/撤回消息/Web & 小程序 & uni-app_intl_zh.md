## 功能描述

撤回单聊消息或者群聊消息。撤回成功后，消息对象的 isRevoked 属性值为 true。

>!
>- v2.4.0起支持。
>- 消息可撤回时间默认为2分钟。可通过 [控制台](https://console.cloud.tencent.com/im-detail/login-message) 调整消息可撤回时间。
>- 被撤回的消息，可以调用 [getMessageList](https://web.sdk.qcloud.com/im/doc/en/SDK.html#getMessageList) 接口从单聊或者群聊消息漫游中拉取到。接入侧须根据消息对象的 isRevoked 属性妥善处理被撤回消息的展示。如单聊会话内可展示为 "对方撤回了一条消息"；群聊会话内可展示为 "XXX撤回了一条消息"。
>- 可使用 REST API [撤回单聊消息](https://intl.cloud.tencent.com/document/product/1047/35015) 或 [撤回群聊消息](https://intl.cloud.tencent.com/document/product/1047/34965)。


**接口**

<dx-codeblock>
:::  js

tim.revokeMessage(message);

:::
</dx-codeblock>

**参数**

| Name               | Type     |Description                                                  |
| ------------------ | -------- |------------------------------------------------------------ |
| message          | Message | 消息实例 |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

// 主动撤回消息
let promise = tim.revokeMessage(message);
promise.then(function(imResponse) {
  // 消息撤回成功
}).catch(function(imError) {
  // 消息撤回失败
  console.warn('revokeMessage error:', imError);
});

:::
</dx-codeblock>


<dx-codeblock>
:::  js

// 收到消息被撤回的通知
tim.on(TIM.EVENT.MESSAGE_REVOKED, function(event) {
  // event.name - TIM.EVENT.MESSAGE_REVOKED
  // event.data - 存储 Message 对象的数组 - [Message] - 每个 Message 对象的 isRevoked 属性值为 true
});

:::
</dx-codeblock>

<dx-codeblock>
:::  js

// 获取会话的消息列表时遇到被撤回的消息
let promise = tim.getMessageList({conversationID: 'C2Ctest', count: 15});
promise.then(function(imResponse) {
  const messageList = imResponse.data.messageList; // 消息列表
  messageList.forEach(function(message) {
    if (message.isRevoked) {
      // 处理被撤回的消息
    } else {
      // 处理普通消息
    }
  });
});

:::
</dx-codeblock>
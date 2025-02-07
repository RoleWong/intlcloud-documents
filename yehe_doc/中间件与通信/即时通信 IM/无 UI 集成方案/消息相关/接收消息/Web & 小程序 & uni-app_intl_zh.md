## 功能描述

接收消息需要通过事件监听实现。

## 监听事件

>! 请在调用 [login](https://web.sdk.qcloud.com/im/doc/en/SDK.html#login) 接口前调用此接口监听事件，避免漏掉 SDK 派发的事件。

**接口**

<dx-codeblock>
:::  js

tim.on(eventName, handler, context);

:::
</dx-codeblock>

**参数**

| 名称    | 类型   | 描述                                                         |
| ------- | ------ | ------------------------------------------------------------ |
| eventName  | String | 事件名称。所有的事件名称都存放在 TIM.EVENT 变量中，如需要查看可以使用 console.log(TIM.EVENT) 把所有的事件显示出来。[事件列表](https://web.sdk.qcloud.com/im/doc/en/module-EVENT.html)。|
| handler | Function | 处理事件的方法，当事件触发时，会调用此 handler 进行处理。 |
| context | * \| undefined | 期望 handler 执行时的上下文 |

**返回值**

无

**示例**

<dx-codeblock>
:::  js

let onMessageReceived = function(event) {
  // event.data - 存储 Message 对象的数组 - [Message]
  const messageList = event.data;
  messageList.forEach((message) => {
    if (message.type === TIM.TYPES.MSG_TEXT) {
      // 文本消息 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.TextPayload
    } else if (message.type === TIM.TYPES.MSG_IMAGE) {
      // 图片消息 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.ImagePayload
    } else if (message.type === TIM.TYPES.MSG_SOUND) {
      // 音频消息 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.AudioPayload
    } else if (message.type === TIM.TYPES.MSG_VIDEO) {
      // 视频消息 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.VideoPayload
    } else if (message.type === TIM.TYPES.MSG_FILE) {
      // 文件消息 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.FilePayload
    } else if (message.type === TIM.TYPES.MSG_CUSTOM) {
      // 自定义消息 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.CustomPayload
    } else if (message.type === TIM.TYPES.MSG_MERGER) {
      // 合并消息 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.MergerPayload
    } else if (message.type === TIM.TYPES.MSG_LOCATION) {
      // 地理位置消息 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.LocationPayload
    } else if (message.type === TIM.TYPES.MSG_GRP_TIP) {
      // 群提示消息 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.GroupTipPayload
    } else if (message.type === TIM.TYPES.MSG_GRP_SYS_NOTICE) {
      // 群系统通知 - https://web.sdk.qcloud.com/im/doc/en/Message.html#.GroupSystemNoticePayload
    }
  });
};
tim.on(TIM.EVENT.MESSAGE_RECEIVED, onMessageReceived);

:::
</dx-codeblock>

## 取消监听事件

**接口**

<dx-codeblock>
:::  js

tim.off(eventName, handler, context, once);

:::
</dx-codeblock>

**参数**

| 名称    | 类型   | 描述                                                         |
| ------- | ------ | ------------------------------------------------------------ |
| eventName  | String | 事件名称。所有的事件名称都存放在 TIM.EVENT 变量中，如需要查看可以使用 console.log(TIM.EVENT) 把所有的事件显示出来。[事件列表](https://web.sdk.qcloud.com/im/doc/en/module-EVENT.html)。|
| handler | Function | 处理事件的方法，当事件触发时，会调用此 handler 进行处理。 |
| context | * \| undefined | 期望 handler 执行时的上下文 |
| once | Boolean \| undefined | 是否只解绑一次 |

**返回值**

无

**示例**

<dx-codeblock>
:::  js

let onMessageReceived = function(event) {
  // 收到推送的单聊、群聊、群提示、群系统通知的新消息，可通过遍历 event.data 获取消息列表数据并渲染到页面
  // event.name - TIM.EVENT.MESSAGE_RECEIVED
  // event.data - 存储 Message 对象的数组 - [Message]
};
tim.off(TIM.EVENT.MESSAGE_RECEIVED, onMessageReceived);

:::
</dx-codeblock>
## 消息类介绍

IM SDK 中 [Message](https://web.sdk.qcloud.com/im/doc/en/Message.html) 表示消息对象，用于描述一条消息具有的属性，如类型、消息的内容、所属的会话 ID 等。

| 属性 | 类型 | 默认值 | 说明 |
| --- |  --- | --- | --- |
| ID | String | -  | 消息 ID。从v2.18.0起，其拼接规则为 ${senderTinyID}-${clientTime}-${random}，与 NativeIM 消息的 ID 拼接规则一致。|
| type | String | - | 消息类型，具体如下：<br/><li>TIM.TYPES.MSG_TEXT --- 文本消息</li><li>TIM.TYPES.MSG_IMAGE --- 图片消息</li><li>TIM.TYPES.MSG_AUDIO --- 音频消息</li><li>TIM.TYPES.MSG_VIDEO --- 视频消息</li><li>TIM.TYPES.MSG_FILE --- 文件消息</li><li>TIM.TYPES.MSG_CUSTOM --- 自定义消息</li><li>TIM.TYPES.MSG_MERGER --- 合并消息（v2.10.1起支持）</li><li>TIM.TYPES.MSG_LOCATION --- 位置消息（v2.15.0起支持）</li><li>TIM.TYPES.MSG_GRP_TIP --- 群提示消息</li><li>TIM.TYPES.MSG_GRP_SYS_NOTICE --- 群系统通知消息</li>
| payload | Object | - | 消息的内容，具体如下：<br/><li>[文本](https://web.sdk.qcloud.com/im/doc/en/Message.html#.TextPayload)</li><li>[图片](https://web.sdk.qcloud.com/im/doc/en/Message.html#.ImagePayload)</li><li>[音频](https://web.sdk.qcloud.com/im/doc/en/Message.html#.AudioPayload)</li><li>[视频](https://web.sdk.qcloud.com/im/doc/en/Message.html#.VideoPayload)</li><li>[文件](https://web.sdk.qcloud.com/im/doc/en/Message.html#.FilePayload)</li><li>[自定义](https://web.sdk.qcloud.com/im/doc/en/Message.html#.CustomPayload)</li><li>[合并（v2.10.1起支持）](https://web.sdk.qcloud.com/im/doc/en/Message.html#.MergerPayload)</li><li>[地理位置（v2.15.0起支持）](https://web.sdk.qcloud.com/im/doc/en/Message.html#.LocationPayload)</li><li>[群提示消息](https://web.sdk.qcloud.com/im/doc/en/Message.html#.GroupTipPayload)</li><li>[群系统通知](https://web.sdk.qcloud.com/im/doc/en/Message.html#.GroupSystemNoticePayload)
| conversationID | String | - | 消息所属的会话 ID |
| conversationType | String | - | 消息所属会话的类型，具体如下：<br/><li>TIM.TYPES.CONV_C2C --- C2C(Client to Client, 端到端) 会话</li><li>TIM.TYPES.CONV_GROUP --- GROUP(群组) 会话</li><li>TIM.TYPES.CONV_SYSTEM --- SYSTEM(系统) 会话</li>
| to | String | - | 接收方的 userID |
| from | String | - | 发送方的 userID，在消息发送时，会默认设置为当前登录的用户 |
| flow | String | - | 消息的流向。<br/><li>in --- 为收到的消息</li><li>out --- 为发出的消息</li> |
| time | Number | - | 消息时间戳。单位：秒 |
| status | String | - | 消息状态。<br/><li>unSend --- 未发送</li><li>success --- 发送成功</li><li>fail --- 发送失败|
| isRevoked | Boolean | false | 是否被撤回的消息，true 标识被撤回的消息（v2.4.0起支持） |
| priority | String | TIM.TYPES.MSG_PRIORITY_NORMAL | 消息优先级，用于群聊（v2.4.2起支持）
| nick | String | - | 消息发送者的昵称（v2.6.0起，在 AVChatRoom 内支持，需提前调用 [updateMyProfile](https://web.sdk.qcloud.com/im/doc/en/SDK.html#updateMyProfile) 设置） |
| avatar | String | - | 消息发送者的头像地址（v2.6.0起，在 AVChatRoom 内支持，需提前调用 [updateMyProfile](https://web.sdk.qcloud.com/im/doc/en/SDK.html#updateMyProfile) 设置） |
| isPeerRead | Boolean | false | C2C 消息对端是否已读，true 标识对端已读（v2.7.0起支持） |
| nameCard | String | - | 非直播群消息发送者的群名片（v2.9.0起支持，也可称之为消息发送者的群昵称），需提前调用 [setGroupMemberNameCard](https://web.sdk.qcloud.com/im/doc/en/SDK.html#setGroupMemberNameCard) 设置 |
| atUserList | Array | - | 群聊时此字段存储被 at 的群成员的 userID（v2.9.0起支持） |
| cloudCustomData | String | - | 消息自定义数据（云端保存，会发送到对端，程序卸载重装后还能拉取到，v2.10.2起支持） |
| isDeleted | Boolean | false | 是否被删除的消息，true 标识被删除的消息（v2.12.0起支持） |
| isModified | Boolean | false | 是否被修改过的消息，true 标识被修改过的消息（v2.12.1起支持） |
| needReadReceipt | Boolean | false | 是否需要已读回执，true 标识需要（v2.18.0起支持，仅用于群消息，需要您购买旗舰版套餐）
| readReceiptInfo | Object | - | 消息已读回执信息（v2.18.0起支持）。<br/><li>readCount --- 消息已读数，可通过调用 [getMessageReadReceiptList](https://web.sdk.qcloud.com/im/doc/en/SDK.html#getMessageReadReceiptList) 查询；如果想要查询哪些群成员已读了消息，可调用 [getGroupMessageReadMemberList](https://web.sdk.qcloud.com/im/doc/en/SDK.html#getGroupMessageReadMemberList)</li><li>unreadCount --- 消息未读数，可通过调用 [getMessageReadReceiptList](https://web.sdk.qcloud.com/im/doc/en/SDK.html#getMessageReadReceiptList) 查询 |
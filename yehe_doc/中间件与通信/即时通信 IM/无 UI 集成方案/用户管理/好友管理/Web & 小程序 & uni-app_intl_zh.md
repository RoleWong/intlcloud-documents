## 功能描述

IM SDK 支持好友的管理，用户可以主动添加、删除好友，也可以设置仅针对好友才能发送消息。

### 获取好友列表

获取 SDK 缓存的好友列表。当好友列表有更新时，SDK 会派发事件 [TIM.EVENT.FRIEND_LIST_UPDATED](https://web.sdk.qcloud.com/im/doc/en/module-EVENT.html#.FRIEND_LIST_UPDATED)。

>! v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。

**接口**

<dx-codeblock>
:::  js

tim.getFriendList();

:::
</dx-codeblock>

**参数**

无

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.getFriendList();
promise.then(function(imResponse) {
  const friendList = imResponse.data; // 好友列表
}).catch(function(imError) {
  console.warn('getFriendList error:', imError); // 获取好友列表失败的相关信息
});


:::
</dx-codeblock>


### 添加好友

>! v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。

**接口**

<dx-codeblock>
:::  js

tim.addFriend(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| to     | String | 	用户 ID |
| source | String | 好友来源。<br/><li>加好友来源字段包含前缀和关键字两部分；加好友来源字段的前缀是：AddSource_Type_ ；关键字：必须是英文字母，且长度不得超过 8 字节，建议用一个英文单词或该英文单词的缩写。示例：加好友来源的关键字是 Android，则加好友来源字段是：AddSource_Type_Android</li>
| wording | String \| undefined | 加好友附言，长度最长不得超过 256 个字节 |
| type | String \| undefined | 加好友方式（默认双向加好友方式）：<br/><li>TIM.TYPES.SNS_ADD_TYPE_SINGLE 单向加好友（单向好友：用户 A 的好友表中有用户 B，但 B 的好友表中却没有 A）</li><li>TIM.TYPES.SNS_ADD_TYPE_BOTH 双向加好友（双向好友：用户 A 的好友表中有用户 B，B 的好友表中也有 A）</li> |
| remark | String \| undefined | 好友备注，备注长度最长不得超过 96 个字节 |
| groupName | String \| undefined | 分组名，分组名长度不得超过 30 个字节

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

// 添加好友
let promise = tim.addFriend({
  to: 'user1',
  source: 'AddSource_Type_Web',
  remark: '小橙子',
  groupName: '好友',
  wording: '我是 user0',
  type: TIM.TYPES.SNS_ADD_TYPE_BOTH,
});
promise.then(function(imResponse) {
  // 添加好友的请求发送成功
  const { code } = imResponse.data;
  if (code === 30539) {
    // 30539 说明 user1 设置了【需要经过自己确认对方才能添加自己为好友】，此时 SDK 会触发 TIM.EVENT.FRIEND_APPLICATION_LIST_UPDATED 事件
  } else if (code === 0) {
    // 0 说明 user1 设置了【允许任何人添加自己为好友】，此时 SDK 会触发 TIM.EVENT.FRIEND_LIST_UPDATED 事件
  }
}).catch(function(imError) {
  console.warn('addFriend error:', imError); // 添加好友失败的相关信息
});

:::
</dx-codeblock>


### 删除好友

删除好友，支持单向删除好友和双向删除好友。

>!
>- v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。
>- userIDList 建议一次最大 100 个，因为数量过多可能会导致数据包太大被后台拒绝，后台限制数据包最大为 1M。

**接口**

<dx-codeblock>
:::  js

tim.deleteFriend(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| userIDList     | Array | 	待删除的好友的 userID 列表，单次请求的 userID 数不得超过100 |
| type | String \| undefined | 删除模式（默认双向删除好友）：<br/><li>TIM.TYPES.SNS_DELETE_TYPE_SINGLE 单向删除（只将 B 从 A 的好友表中删除，但不会将 A 从 B 的好友表中删除）</li><li>TIM.TYPES.SNS_DELETE_TYPE_BOTH 双向删除（将 B 从 A 的好友表中删除，同时将 A 从 B 的好友表中删除）</li> |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.deleteFriend({
  userIDList: ['user1','user2'],
  type: TIM.TYPES.SNS_DELETE_TYPE_BOTH
});
promise.then(function(imResponse) {
  const { successUserIDList, failureUserIDList } = imResponse.data;
  // 删除成功的 userIDList
  successUserIDList.forEach((item) => {
    const { userID } = item;
  });
  // 删除失败的 userIDList
  failureUserIDList.forEach((item) => {
    const { userID, code, message } = item;
  });
  // 如果好友列表有变化，则 SDK 会触发 TIM.EVENT.FRIEND_LIST_UPDATED 事件
}).catch(function(imError) {
  console.warn('removeFromFriendList error:', imError);
});

:::
</dx-codeblock>

### 检查好友关系

>! v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。

**接口**

<dx-codeblock>
:::  js

tim.checkFriend(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| userIDList     | Array | 	要校验的 userID 列表，单次请求的 userID 数不得超过1000 |
| type | String \| undefined | 校验模式（默认双向校验好友关系）：<br/><li>TIM.TYPES.SNS_CHECK_TYPE_SINGLE 单向校验好友关系（只会检查 A 的好友表中是否有 B，不会检查 B 的好友表中是否有 A）</li><li>TIM.TYPES.SNS_CHECK_TYPE_BOTH 双向校验好友关系（既会检查 A 的好友表中是否有 B，也会检查 B 的好友表中是否有 A）</li> |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.checkFriend({
  userIDList: ['user0','user1'],
  type: TIM.TYPES.SNS_CHECK_TYPE_BOTH,
});
promise.then(function(imResponse) {
  const { successUserIDList, failureUserIDList } = imResponse.data;
  // 校验成功的 userIDList
  successUserIDList.forEach((item) => {
    const { userID, code, relation } = item; // 此时 code 始终为0
    // 单向校验好友关系时可能的结果有：
    // - relation: TIM.TYPES.SNS_TYPE_NO_RELATION A 的好友表中没有 B，但无法确定 B 的好友表中是否有 A
    // - relation: TIM.TYPES.SNS_TYPE_A_WITH_B A 的好友表中有 B，但无法确定 B 的好友表中是否有 A
    // 双向校验好友关系时可能的结果有：
    // - relation: TIM.TYPES.SNS_TYPE_NO_RELATION A 的好友表中没有 B，B 的好友表中也没有 A
    // - relation: TIM.TYPES.SNS_TYPE_A_WITH_B A 的好友表中有 B，但 B 的好友表中没有 A
    // - relation: TIM.TYPES.SNS_TYPE_B_WITH_A A 的好友表中没有 B，但 B 的好友表中有 A
    // - relation: TIM.TYPES.SNS_TYPE_BOTH_WAY A 的好友表中有 B，B 的好友表中也有 A
  });
  // 校验失败的 userIDList
  failureUserIDList.forEach((item) => {
    const { userID, code, message } = item;
  });
}).catch(function(imError) {
  console.warn('checkFriend error:', imError);
});

:::
</dx-codeblock>

### 设置只能给好友发消息

IM SDK 在发送单聊消息的时候，默认不检查好友关系。在客服场景中，如果用户需要先加客服为好友才能进行沟通非常不方便，因此该默认设置常用于在线客服等场景。
如需实现“先加好友，再发消息”的交互体验，您可以在 [即时通信 IM 控制台](https://console.cloud.tencent.com/im) >【功能配置】>【登录与消息】>【好友关系检查】中开启"发送单聊消息检查关系链"。开启后，用户只能给好友发送消息，当用户给非好友发消息时，SDK 会报 20009 错误码。


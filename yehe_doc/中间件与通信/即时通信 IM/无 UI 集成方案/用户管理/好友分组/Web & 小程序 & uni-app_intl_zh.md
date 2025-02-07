## 功能描述
在某些场景下，您可能需要对好友进行分组，例如分为 "大学同学"、"公司同事" 等，您可以调用以下接口实现。

## 好友分组

### 新建好友分组

>! v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。

**接口**

<dx-codeblock>
:::  js

tim.createFriendGroup(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| name     | String | 	分组名称  |
| userIDList | Array | 要添加到分组的好友 userID 列表 |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.createFriendGroup({
  name: '我的好友分组1',
  userIDList: ['user0','user1']
});
promise.then(function(imResponse) {
  const { friendGroup，failureUserIDList } = imResponse;
  // friendGroup - 好友分组实例
  // failureUserIDList - 失败的 userID 列表
  // 创建成功后，SDK 会触发 TIM.EVENT.FRIEND_GROUP_LIST_UPDATED 事件
}).catch(function(imError) {
  console.warn('getFriendGroupInfo error:', imError); // 获取失败
});

:::
</dx-codeblock>

### 删除好友分组

>! v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。

**接口**

<dx-codeblock>
:::  js

tim.deleteFriendGroup(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| name     | String | 	分组名称  |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.deleteFriendGroup({
  name: '我的好友分组1',
});
promise.then(function(imResponse) {
  console.log(imResponse.data); // 被删除的分组实例
  // 删除成功后，SDK 会触发 TIM.EVENT.FRIEND_GROUP_LIST_UPDATED 事件
}).catch(function(imError) {
  console.warn('deleteFriendGroup error:', imError); // 获取失败
});
:::
</dx-codeblock>

### 重命名好友分组

>! v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。

**接口**

<dx-codeblock>
:::  js

tim.renameFriendGroup(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| oldName     | String | 	旧的分组名称  |
| newName | String | 新的分组名称 |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.renameFriendGroup({
  oldName: '好友',
  newName: '闺蜜'
});
promise.then(function(imResponse) {
  console.log(imResponse.data); // FriendGroup 实例
  // 修改成功后，SDK 会触发 TIM.EVENT.FRIEND_GROUP_LIST_UPDATED 事件
}).catch(function(imError) {
  console.warn('updateMyProfile error:', imError);
});

:::
</dx-codeblock>


### 获取好友分组

获取 SDK 缓存的好友分组列表。当好友分组列表有更新时，SDK 会派发事件 [TIM.EVENT.FRIEND_GROUP_LIST_UPDATED](https://web.sdk.qcloud.com/im/doc/en/module-EVENT.html#.FRIEND_GROUP_LIST_UPDATED)。

>! v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。

**接口**

<dx-codeblock>
:::  js

tim.getFriendGroupList();

:::
</dx-codeblock>

**参数**

无

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.getFriendGroupList();
promise.then(function(imResponse) {
  const friendGroupList = imResponse.data; // 好友分组列表
}).catch(function(imError) {
  console.warn('getFriendGroupList error:', imError); // 获取好友分组列表失败的相关信息
});

:::
</dx-codeblock>


### 添加好友到一个分组

>! v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。

**接口**

<dx-codeblock>
:::  js

tim.addToFriendGroup(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| name     | String | 	分组名称  |
| userIDList | Array | 要添加的好友 userID 列表 |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.addToFriendGroup({
  name: '我的好友分组1',
  userIDList: ['user1','user2'],
});
promise.then(function(imResponse) {
  const { friendGroup, failureUserIDList } = imResponse.data;
  // friendGroup - 好友分组实例
  // failureUserIDList - 失败的 userID 列表
  // 添加成功后，SDK 会触发 TIM.EVENT.FRIEND_GROUP_LIST_UPDATED 事件
}).catch(function(imError) {
  console.warn('addToFriendGroup error:', imError); // 获取失败
});

:::
</dx-codeblock>

### 从分组中删除某好友

>! v2.13.0起支持，[升级指引](https://web.sdk.qcloud.com/im/doc/en/tutorial-03-sns.html)。

**接口**

<dx-codeblock>
:::  js

tim.removeFromFriendGroup(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| name     | String | 	分组名称  |
| userIDList | Array | 要移除的好友 userID 列表 |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.removeFromFriendGroup({
  name: '我的好友分组1',
  userIDList: ['user1','user2'],
});
promise.then(function(imResponse) {
  const { friendGroup, failureUserIDList } = imResponse.data;
  // friendGroup - 好友分组实例
  // failureUserIDList - 失败的 userID 列表
  // 移除成功后，SDK 会触发 TIM.EVENT.FRIEND_GROUP_LIST_UPDATED 事件
}).catch(function(imError) {
  console.warn('addToFriendGroup error:', imError); // 获取失败
});

:::
</dx-codeblock>
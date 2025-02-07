## 功能描述

群组管理功能指的是创建群组、加入群组、获取已加入的群组、退出群组和解散群组等。

[](id:listener)
## 群事件监听

**示例**

<dx-codeblock>
:::  js

let onGroupListUpdated = function(event) {
   console.log(event.data);// 包含 Group 实例的数组
};
tim.on(TIM.EVENT.GROUP_LIST_UPDATED, onGroupListUpdated);

:::
</dx-codeblock>



## 创建群组

>!
>- 该接口创建 TIM.TYPES.GRP_AVCHATROOM（直播群） 后，需调用 [joinGroup](https://web.sdk.qcloud.com/im/doc/en/SDK.html#joinGroup) 接口加入群组后，才能进行消息收发流程。
>- v2.19.1 起，该接口可以创建支持话题的社群。

**接口**

<dx-codeblock>
:::  js

tim.createGroup(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| name | String | 群名称，最长30字节 |
| type | String | 群组类型，包括：<br/><li>TIM.TYPES.GRP_WORK（好友工作群，默认）</li><li>TIM.TYPES.GRP_PUBLIC（陌生人社交群）</li><li>TIM.TYPES.GRP_MEETING（临时会议群）</li><li>TIM.TYPES.GRP_AVCHATROOM（直播群）</li><li>TIM.TYPES.GRP_COMMUNITY（社群，v2.17.0 起支持）</li> |
| groupID | String \| undefined | 群组ID。不填该字段时，会自动为群组创建一个唯一的群 ID |
| introduction | String \| undefined | 群简介，最长240字节 |
| notification | String \| undefined | 群公告，最长300字节 |
| avatar | String \| undefined | 群头像 URL，最长100字节 |
| maxMemberNum | Number \| undefined | 最大群成员数量，缺省时的默认值：好友工作群是200，陌生人社交群是2000，临时会议群是10000，直播群无限制 |
| joinOption | String | 申请加群处理方式。<br/><li>TIM.TYPES.JOIN_OPTIONS_FREE_ACCESS （自由加入）</li><li>TIM.TYPES.JOIN_OPTIONS_NEED_PERMISSION （需要验证）</li><li>TIM.TYPES.JOIN_OPTIONS_DISABLE_APPLY （禁止加群）</li>注意：TIM.TYPES.GRP_WORK, TIM.TYPES.GRP_MEETING, TIM.TYPES.GRP_AVCHATROOM 类型群组的该属性不允许修改。好友工作群禁止申请加群，临时会议群和直播群自由加入。 |
| memberList | Array \| undefined | 初始群成员列表，最多500个。创建直播群时不能添加成员。数组元素结构如下：<br/><li>userID --- String --- 必填，群成员的 userID</li><li>role --- String --- 成员身份，可选值只有Admin，表示添加该成员并设其为管理员</li><li>memberCustomField --- Array|undefined --- 群成员维度的自定义字段，默认情况是没有的，需要开通，详情请参阅 [自定义字段](https://intl.cloud.tencent.com/document/product/1047/33529)</li>
| groupCustomField | Array \| undefined | 群自定义字段。默认情况是没有的。开通群组维度的自定义字段详情请参见 [自定义字段](https://intl.cloud.tencent.com/document/product/1047/33529) |
| isSupportTopic | Boolean | 创建支持话题的社群时必填， true - 创建支持话题的社群 false - 创建普通社群。v2.19.1 起支持 |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

// 创建好友工作群
let promise = tim.createGroup({
  type: TIM.TYPES.GRP_WORK,
  name: 'WebSDK',
  memberList: [{
    userID: 'user1',
    // 群成员维度的自定义字段，默认情况是没有的，需要开通，详情请参阅自定义字段
    memberCustomField: [{key: 'group_member_test', value: 'test'}]
  }, {
    userID: 'user2'
  }] // 如果填写了 memberList，则必须填写 userID
});
promise.then(function(imResponse) { // 创建成功
  console.log(imResponse.data.group); // 创建的群的资料
  // 创建群时指定了成员列表，但是成员中存在超过了“单个用户可加入群组数”限制的情况
  // 一个用户 userX 最多允许加入 N 个群，如果已经加入了 N 个群，此时创建群再指定 userX 为群成员，则 userX 不能正常加群
  // SDK 将 userX 的信息放入 overLimitUserIDList，供接入侧处理
  console.log(imResponse.data.overLimitUserIDList); // 超过了“单个用户可加入群组数”限制的用户列表，v2.10.2起支持
}).catch(function(imError) {
  console.warn('createGroup error:', imError); // 创建群组失败的相关信息
});

:::
</dx-codeblock>

<dx-codeblock>
:::  js

// 创建支持话题的社群
let promise = tim.createGroup({
  type: TIM.TYPES.GRP_COMMUNITY,
  name: 'WebSDK',
  isSupportTopic: true,
});
promise.then(function(imResponse) { // 创建成功
  console.log(imResponse.data.group); // 创建的群的资料
}).catch(function(imError) {
  console.warn('createGroup error:', imError); // 创建群组失败的相关信息
});

:::
</dx-codeblock>


## 加入群组

不同类型的群组，加群的方法不同：

| 类型 | 加群方法 |
| --- | --- |
| 好友工作群（Work）| 必须由其他群成员邀请 |
| 陌生人社交群（Public）| 用户申请，群主或管理员审批 |
| 临时会议群（Meeting）| 用户可随意加入 |
| 社群（Community）| 用户可随意加入 |
| 直播群（AVChatRoom）| 用户可随意加入 |

>!
>- 好友工作群不允许申请加群，只能通过 [addGroupMember](https://web.sdk.qcloud.com/im/doc/en/SDK.html#addGroupMember) 方式添加。
>- 同一用户同时只能加入一个直播群。【例如】用户已在直播群 A 中，再加入直播群 B，SDK 会先退出直播群 A，然后加入直播群 B

**接口**

<dx-codeblock>
:::  js

tim.joinGroup(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| groupID | String | 群组 ID |
| applyMessage | String \| undefined | 附言 |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.joinGroup({ groupID: 'group1', type: TIM.TYPES.GRP_AVCHATROOM });
promise.then(function(imResponse) {
  switch (imResponse.data.status) {
    case TIM.TYPES.JOIN_STATUS_WAIT_APPROVAL: // 等待管理员同意
      break;
    case TIM.TYPES.JOIN_STATUS_SUCCESS: // 加群成功
      console.log(imResponse.data.group); // 加入的群组资料
      break;
    case TIM.TYPES.JOIN_STATUS_ALREADY_IN_GROUP: // 已经在群中
      break;
    default:
      break;
  }
}).catch(function(imError){
  console.warn('joinGroup error:', imError); // 申请加群失败的相关信息
});

:::
</dx-codeblock>


## 获取已加入的群组

**接口**

<dx-codeblock>
:::  js

tim.getGroupList(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
|  groupProfileFilter | Array \| undefined | 群资料过滤器。除默认拉取的群资料外，指定需要额外拉取的群资料，支持的值如下：<br/><li>TIM.TYPES.GRP_PROFILE_OWNER_ID（群主 ID）</li><li>TIM.TYPES.GRP_PROFILE_CREATE_TIME（群创建时间）</li><li>TIM.TYPES.GRP_PROFILE_LAST_INFO_TIME（最后一次群资料变更时间）</li><li>TIM.TYPES.GRP_PROFILE_MEMBER_NUM（群成员数量）</li><li>TIM.TYPES.GRP_PROFILE_MAX_MEMBER_NUM（最大群成员数量）</li><li>TIM.TYPES.GRP_PROFILE_JOIN_OPTION（申请加群选项）></li><li>TIM.TYPES.GRP_PROFILE_INTRODUCTION（群介绍）</li><li>TIM.TYPES.GRP_PROFILE_NOTIFICATION（群公告）</li><li>TIM.TYPES.GRP_PROFILE_MUTE_ALL_MBRS (全体禁言设置) v2.6.2起支持</li> |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

// 该接口默认只会拉取这些资料：群类型、群名称、群头像、最后一条消息的时间。
let promise = tim.getGroupList();
promise.then(function(imResponse) {
  console.log(imResponse.data.groupList); // 群组列表
}).catch(function(imError) {
  console.warn('getGroupList error:', imError); // 获取群组列表失败的相关信息
});

:::
</dx-codeblock>

<dx-codeblock>
:::  js

// 若默认拉取的字段不满足需求，可以参考下述代码，拉取额外的资料字段。
let promise = tim.getGroupList({
   groupProfileFilter: [TIM.TYPES.GRP_PROFILE_OWNER_ID],
});
promise.then(function(imResponse) {
  console.log(imResponse.data.groupList); // 群组列表
}).catch(function(imError) {
  console.warn('getGroupList error:', imError); // 获取群组列表失败的相关信息
});

:::
</dx-codeblock>

## 退出群组

>! 群主只能退出好友工作群，退出后该好友工作群无群主。

**接口**

<dx-codeblock>
:::  js

tim.quitGroup(groupID);

:::
</dx-codeblock>

**参数**

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
|  groupID | String | 群组 ID |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.quitGroup('group1');
promise.then(function(imResponse) {
  console.log(imResponse.data.groupID); // 退出成功的群 ID
}).catch(function(imError){
  console.warn('quitGroup error:', imError); // 退出群组失败的相关信息
});

:::
</dx-codeblock>

## 解散群组

>! 群主不能解散好友工作群。

**接口**

<dx-codeblock>
:::  js

tim.dismissGroup(groupID);

:::
</dx-codeblock>

**参数**

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
|  groupID | String | 群组 ID |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.dismissGroup('group1');
promise.then(function(imResponse) { // 解散成功
  console.log(imResponse.data.groupID); // 被解散的群组 ID
}).catch(function(imError) {
  console.warn('dismissGroup error:', imError); // 解散群组失败的相关信息
});

:::
</dx-codeblock>

## 转让群组

>! 只有群主拥有转让的权限。TIM.TYPES.GRP_AVCHATROOM（直播群）类型的群组不能转让。

**接口**

<dx-codeblock>
:::  js

tim.changeGroupOwner(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
|  groupID | String | 待转让的群组 ID |
| newOwnerID | String | 新群主的 ID |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.changeGroupOwner({
  groupID: 'group1',
  newOwnerID: 'user2'
});
promise.then(function(imResponse) { // 转让成功
  console.log(imResponse.data.group); // 群组资料
}).catch(function(imError) { // 转让失败
  console.warn('changeGroupOwner error:', imError); // 转让群组失败的相关信息
});

:::
</dx-codeblock>

## 处理申请加群（同意或拒绝）

>! 如果一个群有多位管理员，当有人申请加群时，所有在线的管理员都会收到 [申请加群的群系统通知](https://web.sdk.qcloud.com/im/doc/en/Message.html#.GroupSystemNoticePayload)。如果某位管理员处理了这个申请（同意或者拒绝），则其他管理员无法重复处理（即不能修改处理的结果）。

**接口**

<dx-codeblock>
:::  js

tim.handleGroupApplication(options);

:::
</dx-codeblock>

**参数**

参数 options 为 Object 类型，包含的属性值如下：

| Name               | Type     | Description                                                  |
| ------------------ | -------- | ------------------------------------------------------------ |
| handleAction | String | 处理结果 Agree(同意) / Reject(拒绝) |
| handleMessage | String \| undefined | 附言 |
| message | Message | 对应【群系统通知】的消息实例 |

**返回值**

`Promise` 对象。

**示例**

<dx-codeblock>
:::  js

let promise = tim.handleGroupApplication({
  handleAction: 'Agree',
  handleMessage: '欢迎欢迎',
  message: message // 申请加群群系统通知的消息实例
});
promise.then(function(imResponse) {
  console.log(imResponse.data.group); // 群组资料
}).catch(function(imError){
  console.warn('handleGroupApplication error:', imError); // 错误信息
});

:::
</dx-codeblock>

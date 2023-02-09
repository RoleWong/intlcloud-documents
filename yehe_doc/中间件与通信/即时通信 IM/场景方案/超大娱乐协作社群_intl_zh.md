## 功能介绍
社群模式（娱乐协作新利器），既支持**社群**-**分组**-**话题**三级结构，将消息相互区隔，又可运营超大规模成员，共用一套好友关系，还可将成员分组，设置成员组的查看、发言、管理等权限。
![](https://qcloudimg.tencent-cloud.cn/raw/7429b1c2a707a4f4f553cd7c2cbcb0db.jpg)
![](https://qcloudimg.tencent-cloud.cn/raw/8db16f94e4ab49ede5dc4207e9b20075.jpg)

## 适用场景
### 兴趣交友：新颖的用户增长模式
- 支持聚集超大规模爱好者，并通过**社群**-**分组**-**话题**让兴趣圈子保持细分垂直。
- 在开放的大社群中，为用户提供了封闭的小话题，这样舒服的中间地带让用户可以随意选择话题交流，提升了成员参与的积极性。
![](https://qcloudimg.tencent-cloud.cn/raw/38f61b81280afed8df7cd699bbb204fb.jpg)

### 游戏社交：提高用户粘性和活跃度
一个社群内多种话题，可完美解决玩家的获取资讯、招募队友、探讨剧情、分享攻略等诸多信息需求。玩游戏前可以了解咨询，玩游戏中可以随时语音（聊天室永远在线），在游戏结束后还能在话题中继续深入交流。
![](https://qcloudimg.tencent-cloud.cn/raw/e613a51181f771a6309731f8f5b5a09a.jpg)

### 粉丝运营：拥有一个高效的运营工具
告别一个又一个独立的分群（取代“深圳用户1群”“深圳用户2群”“广州用户1群”“上海用户1群”等多个群）运营，一个社群不同话题就搞定，不必再担心分身乏术，让粉丝运营更轻松精准！
![](https://qcloudimg.tencent-cloud.cn/raw/45d3d5e8818cea79dd65b3b7abbb8d86.jpg)

### 组织管理：实现一种清晰的分层级沟通方式
组织内所有成员可拉入同一个社群中，再依托社群的层级功能和权限设置，实现分层沟通。
![](https://qcloudimg.tencent-cloud.cn/raw/aa633c15337fbb8befe6c3f9a9a4f890.jpg)

## 技术优势
### 超大规模群成员
腾讯 IM 社群容量拓展了近万倍，满足您兴趣交友、粉丝运营、游戏社交、组织管理等场景下的海量成员需求。
### 消息可靠性
腾讯 IM 社群大幅拓展成员容量的同时继承了腾讯强劲的消息能力，依托20余年技术积累构建的完善且可靠的消息系统。为客户提供超过99.99%的消息收发成功率及服务可靠性，帮助客户轻松应对亿级海量并发。
### 消息推送性能
腾讯 IM 社群采用“快慢通道”+“两级合并推送”的全新消息推送架构，有效平衡时间和空间，提升系统推送性能，降低终端性能消耗，在超大群中也能为用户提供与常规群组一致的消息互动体验。

### 消息状态与用户权限维护
腾讯 IM 社群提供消息编辑、撤回、转发等丰富拓展能力。禁言、消息免打扰、未读消息计数、资料编辑等均支持用户进行全局、社群、话题级别的分别自定义。

## 终端 SDK 集成指南
>!社群（Community）话题（Topic）功能仅 IM 终端 SDK 6.2.2363 增强版及以上版本支持，需 [购买旗舰版](https://buy.cloud.tencent.com/avc?from=17182) 并 [申请开通](https://www.tencentcloud.com/document/product/1047/44322) 后方可使用。

下文以 Android 为例，介绍社群话题的接口功能。




### 社群话题的 API 使用
1. 首先调用接口 createGroup 创建支持话题的社群，可以参见 [社群管理](https://cloud.tencent.com/document/product/269/44494#.E7.A4.BE.E7.BE.A4.E7.AE.A1.E7.90.86) “创建社群”步骤来实现。
2. 然后通过调用 getJoinedCommunityList 接口，可以获取到创建和加入的该类社群列表。
>!社群用来管理群成员，但不可以在社群中收发消息。社群的其他功能可以参考“其他管理接口”列表。
>
3. 成功创建社群后，可以在社群下调用 createTopicInfoCommunity 创建多个话题，参见 [话题管理](https://cloud.tencent.com/document/product/269/44494#.E8.AF.9D.E9.A2.98.E7.AE.A1.E7.90.86) 中“创建话题”步骤实现。
4. 话题的管理还包括“删除话题”、“修改话题信息”、“获取话题列表”、“监听话题回调”功能。同时，话题也是可以供用户交流的地方，因此可以进行收发消息，相关接口可以参见 [话题消息](https://cloud.tencent.com/document/product/269/44494#.E8.AF.9D.E9.A2.98.E6.B6.88.E6.81.AF) 介绍。
5. 社群成员分组，可通过把分组信息设置到群成员自定义字段里，实现按照分组展示的效果。这需要拉取所有群成员到本地按照分组排序，若群人数较多，建议您在服务端自行实现。

## Web  SDK 集成指南
>!社群（Community）话题（Topic）功能仅 IM Web SDK v2.19.1 及以上版本支持，需 [购买旗舰版](https://buy.cloud.tencent.com/avc?from=17182) 并在 [**控制台**](https://console.cloud.tencent.com/im/qun-setting)>**群功能配置**>**社群** 打开开关后方可使用。
>
社群话题的接口功能如下：
### 下载并配置 Demo 源码快速体验
您可参见 [快速入门](https://www.tencentcloud.com/document/product/1047/45912)，快速体验 IM 的功能。
下面从 API 调用的角度来介绍下社群话题的使用：

### 社群话题的 API 使用
1. 首先调用接口 createGroup 创建支持话题的社群，可以参见 [社群管理](https://www.tencentcloud.com/document/product/1047/48172#.E7.A4.BE.E7.BE.A4.E7.AE.A1.E7.90.86) “创建社群”步骤来实现。
2. 然后通过调用 getJoinedCommunityList 接口，可以获取到创建和加入的该类社群列表。社群用来管理群成员，但不可以在支持话题的社群中收发消息。社群的其他功能可以参考普通群组API。
3. 成功创建社群后，可以在社群下调用 createTopicInfoCommunity 创建多个话题，参见 [话题管理](https://www.tencentcloud.com/document/product/1047/48172#.E8.AF.9D.E9.A2.98.E7.AE.A1.E7.90.86) “创建话题”步骤实现。
4. 话题的管理还包括“删除话题”、“修改话题信息”、“获取话题列表”、“监听话题回调”功能。同时，话题也是可以供用户交流的地方，因此可以进行收发消息，相关接口可以参见 [创建消息](https://www.tencentcloud.com/document/product/1047/48172#.E8.AF.9D.E9.A2.98.E6.B6.88.E6.81.AF) 介绍。
5. 社群成员分组，可通过把分组信息设置到群成员自定义字段里，实现按照分组展示的效果。这需要拉取所有群成员到本地按照分组排序，若群人数较多，建议您在服务端自行实现。

## 相关文档
- [群组管理](https://intl.cloud.tencent.com/document/product/1047/33530)
- [群组系统](https://intl.cloud.tencent.com/document/product/1047/33529)
- [SDK 下载](https://intl.cloud.tencent.com/document/product/1047/33996)
- [SDK 手册](https://web.sdk.qcloud.com/im/doc/en/TIM.html)
- [集成 SDK（Android）](https://intl.cloud.tencent.com/document/product/1047/34306)
- [集成 SDK（iOS）](https://intl.cloud.tencent.com/document/product/1047/34307)
- [集成 SDK（Web & 小程序 & uni-app）](https://www.tencentcloud.com/document/product/1047/34309)

## 联系我们
如果您有任何使用问题，请 [联系我们](https://intl.cloud.tencent.com/document/product/1047/41676)。

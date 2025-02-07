## 功能描述

TUIKit 从 v1.1.0 版本开始 ，主题风格颜色能力得到大幅度完善。

TUIKit默认提供颜色配置，您可以直接使用，无需任何配置。

但同时，您也可以很方便的自定义，TUIKit界面中，各处众多颜色配置。

## 自定义方式

### TUIKit颜色对象

在此对象中，您可以定义TUIKit界面中，各处的颜色配置。

请直接实例化一个 `TUITheme()` 对象。并修改里面的各个参数，以覆盖默认值，使用自定义颜色。

该对象构造函数，可配置的颜色有如下：

```dart
  // 应用主色
  // Primary Color For The App
  final Color? primaryColor;

  // 应用次色
  // Secondary Color For The App
  final Color? secondaryColor;

  // 提示颜色，用于次级操作或提示
  // Info Color, Used For Secondary Action Or Info
  final Color? infoColor;

  // 浅背景颜色，比主背景颜色浅，用于填充缝隙或阴影
  // Weak Background Color, Lighter Than Main Background, Used For Marginal Space Or Shadowy Space
  final Color? weakBackgroundColor;

  // 宽屏幕：浅白背景颜色，比浅背景颜色浅
  // Weak Background Color, Lighter Than Main Background, Used For Marginal Space Or Shadowy Space
  final Color? wideBackgroundColor;

  // 浅分割线颜色，用于分割线或边框
  // Weak Divider Color, Used For Divider Or Border
  final Color? weakDividerColor;

  // 浅字色
  // Weak Text Color
  final Color? weakTextColor;

  // 深字色
  // Dark Text Color
  final Color? darkTextColor;

  // 浅主色，用于AppBar或Panels
  // Light Primary Color, Used For AppBar Or Several Panels
  final Color? lightPrimaryColor;

  // 字色
  // TextColor
  final Color? textColor;

  // 警示色，用于危险操作
  // Caution Color, Used For Warning Actions
  final Color? cautionColor;

  // 群主标识色
  // Group Owner Identification Color
  final Color? ownerColor;

  // 群管理员标识色
  // Group Admin Identification Color
  final Color? adminColor;

  // 白色
  // white
  final Color? white;

  // 黑色
  // black
  final Color? black;

  // 输入框填充色
  // input fill color
  final Color? inputFillColor;

  // 灰色文本
  // grey text color
  final Color? textgrey;

  /// 新版本颜色从这里开始
  ///
  /// 会话列表背景颜色
  final Color? conversationItemBgColor;

  /// 会话列表边框颜色
  final Color? conversationItemBorderColor;

  /// 会话列表选中背景颜色
  final Color? conversationItemActiveBgColor;

  /// 会话列表置顶背景颜色
  final Color? conversationItemPinedBgColor;

  /// 会话列表Title字体颜色
  final Color? conversationItemTitleTextColor;

  /// 会话列表LastMessage字体颜色
  final Color? conversationItemLastMessageTextColor;

  /// 会话列表Time字体颜色
  final Color? conversationItemTitmeTextColor;

  /// 会话列表用户在线状态背景色
  final Color? conversationItemOnlineStatusBgColor;

  /// 会话列表用户离线状态背景色
  final Color? conversationItemOfflineStatusBgColor;

  /// 会话列表未读数背景颜色
  final Color? conversationItemUnreadCountBgColor;

  /// 会话列表未读数字体颜色
  final Color? conversationItemUnreadCountTextColor;

  /// 会话列表草稿字体颜色
  final Color? conversationItemDraftTextColor;

  /// 会话列表收到消息不提醒Icon颜色
  final Color? conversationItemNoNotificationIconColor;

  /// 会话列表侧滑按钮字体颜色
  final Color? conversationItemSliderTextColor;

  /// 会话列表侧滑按钮Clear背景颜色
  final Color? conversationItemSliderClearBgColor;

  /// 会话列表侧滑按钮Pin背景颜色
  final Color? conversationItemSliderPinBgColor;

  /// 会话列表侧滑按钮Delete背景颜色
  final Color? conversationItemSliderDeleteBgColor;

  /// 会话列表宽屏模式选中时背景颜色
  final Color? conversationItemChooseBgColor;

  /// 聊天页背景颜色
  final Color? chatBgColor;

  /// 聊天页背景颜色
  final Color? chatTimeDividerTextColor;

  /// 聊天页导航栏背景颜色
  final Color? chatHeaderBgColor;

  /// 聊天页导航栏Title字体颜色
  final Color? chatHeaderTitleTextColor;

  /// 聊天页导航栏Back字体颜色
  final Color? chatHeaderBackTextColor;

  /// 聊天页导航栏Action字体颜色
  final Color? chatHeaderActionTextColor;

  /// 聊天页历史消息列表字体颜色
  final Color? chatMessageItemTextColor;

  /// 聊天页历史消息列表来自自己时背景颜色
  final Color? chatMessageItemFromSelfBgColor;

  /// 聊天页历史消息列表来自非自己时背景颜色
  final Color? chatMessageItemFromOthersBgColor;

  /// 聊天页历史消息列表已读状态字体颜色
  final Color? chatMessageItemUnreadStatusTextColor;

  /// 聊天页历史消息列表小舌头背景颜色
  final Color? chatMessageTongueBgColor;

  /// 聊天页历史消息列表小舌头字体颜色
  final Color? chatMessageTongueTextColor;
```

### 配置方式

调用 TUIKit 提供的 `setTheme` 方法，传入上一步定义的颜色对象 `TUITheme()` 即可。

该方法可随时调用，动态修改。

```dart
final CoreServicesImpl _coreInstance = TIMUIKitCore.getInstance();
_coreInstance.setTheme(theme: TUITheme());
```

## 联系我们[](id:contact)

如果您在接入使用过程中有任何疑问，请通过如下方式联系我们。

- [Telegram Group](https://t.me/+1doS9AUBmndhNGNl)
- [WhatsApp Group](https://chat.whatsapp.com/Gfbxk7rQBqc8Rz4pzzP27A)

## 概述

TUI 组件默认内置了：轻量、活泼、深沉共三套主题。您可以任意切换或者修改内置主题，也可以按需新增主题。

<table style="text-align:center;vertical-align:middle;width:800px">
  <tr>
    <th style="text-align:center;" width="300px">轻量 light<br></th>
    <th style="text-align:center;" width="300px">活泼 lively<br></th>
    <th style="text-align:center;" width="300px">深沉 serious<br></th>
  </tr>
  <tr>
    <td style="text-align:center;"><img style="width:300px" src="https://im.sdk.qcloud.com/tools/resource/themes/android/demo_light.png"  />    </td>
    <td style="text-align:center;"><img style="width:300px" src="https://im.sdk.qcloud.com/tools/resource/themes/android/demo_lively.png" />     </td>
    <td style="text-align:center;"><img style="width:300px" src="https://im.sdk.qcloud.com/tools/resource/themes/android/demo_serious.png" />     </td>
	 </tr>
</table>

## 主题资源

您可以在任一 TUI 组件内部的 `res` 文件夹下看到该组件所支持的主题资源。以 TUIChat 组件为例，您可以在 `TUIChat/tuichat/src/main/` 下看到资源文件夹；`res-light`、`res-serious` 和 `res-lively` 文件夹中分别为 TUIChat 组件内置的轻量版、深沉版和活泼版主题资源， `res` 文件夹中为通用资源。

<img src="https://im.sdk.qcloud.com/tools/resource/themes/android/chat_resource.png" width="450px"/>

主题资源文件夹的目录结构与通用资源的目录结构一致。


## 应用主题

`TUIKit` 默认使用轻量版主题。当您需要对 TUI 组件以及您的 App 主工程设置主题时，可以调用 `TUIThemeManager` 的 `changeTheme` 方法来设置当前主题。

您可以参考 `TUIKitDemo` 的 [ThemeSelectActivity.java](https://github.com/TencentCloud/TIMSDK/blob/master/Android/Demo/app/src/main/java/com/tencent/qcloud/tim/demo/login/ThemeSelectActivity.java) 文件中的代码。

也可以使用如下方法切换主题：

```java
// 轻量版 themeID 为 0， 活泼版 themeID 为 1， 深沉版 themeID 为 2
TUIThemeManager.getInstance().changeTheme(context, themeID);
System.exit(0);
Intent intent = context.getPackageManager().getLaunchIntentForPackage(context.getPackageName());
context.startActivity(intent);
```


## 获取主题内资源

>? 主题属性皆定义在各组件的 `src/main/res/values/tui_theme_attrs.xml` 文件中，属性名不可重复。

应用主题成功之后, 在 Java 代码中，可以调用 TUIThemeManager.getAttrResId(context, attrID) 方法根据主题属性来获取资源 ID，然后再根据获取到的资源 ID 获取真正的资源，例如：

```java
mArrowImageView.setBackgroundResource(TUIThemeManager.getAttrResId(getContext(), R.attr.chat_jump_recent_down_icon));

replyContentTv.setTextColor(resources.getColor(TUIThemeManager.getAttrResId(context, R.attr.chat_other_reply_text_color))); 
```

在 XML 资源文件中，可以使用 ?attr/** 的方式，根据主题属性来使用当前主题下的资源，例如：

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="ring"
    android:innerRadius="22.5dp"
    android:thickness="1.5dp"
    android:useLevel="false">

    <solid android:color="?attr/core_primary_color" />
</shape>
```

```xml
<ImageView
    android:id="@+id/demo_login_theme_arrow"
    android:layout_width="9.6dp"
    android:layout_height="9.6dp"
    android:layout_gravity="center"
    android:background="?attr/demo_login_language_arrow" />
```
>! 上面两种方法只能获取当前已经应用成功了的主题的资源 ID，无法获取未应用的主题下的资源 ID。

## 修改内置主题

TUI 组件目前可以修改内置的主题，按照下列步骤，可对内置主题的颜色、字体、图片等资源做自定义变更。

1. 找到要修改的主题下具体的资源；
2. 替换或者修改资源；
3. 切换到对应主题下，查看效果。

例如，`TUIChat` 组件中自己发出的文本消息的气泡背景色，在不同主题下有不一样的颜色。
该背景色在内置的 “活泼” 主题下的色值为 <font color="#FF9D85">#FF9D85</font>，现在想要修改成 <font color="#EA286C">#EA286C</font>，您只需要按照如下步骤操作即可：

1. 从 `TUIChat` 源码中找到自己发送的文本消息的气泡使用的背景是 `R.attr.chat_bubble_self_bg` 属性：
   
   <img src="https://im.sdk.qcloud.com/tools/resource/themes/android/setChatBubbleBg.png" width="700px" />
   
   在 `tuichat/src/main/res-lively/values/lively_styles.xml` 文件中找到， `chat_bubble_self_bg` 属性对应的资源是 `@drawable/chat_bubble_self_bg_lively` ：

    <img src="https://im.sdk.qcloud.com/tools/resource/themes/android/searchLivelyBubbleBg.png" width="800px" />

    打开对应的资源文件，发现背景色是 `@color/chat_bubble_self_color_lively` ：

    <img src="https://im.sdk.qcloud.com/tools/resource/themes/android/chatBubbleLivelyBgColor.png" width="800px" />

2. 上一步已经找到要替换的资源，将 `@drawable/chat_bubble_self_bg_lively` 资源中的 `@color/chat_bubble_self_color_lively` 的色值替换为 `#EA286C` ：
   
    <img src="https://im.sdk.qcloud.com/tools/resource/themes/android/changedChatLivelyBubbleColor.png" width="500px" />

3. 保存文件，重新编译安装应用，切换主题为活泼版主题，即可看到效果：
   
<table style="text-align:center;vertical-align:middle;width:700px">
  <tr>
    <th style="text-align:center;" width="300px">修改前<br></th>
    <th style="text-align:center;" width="300px">修改后<br></th>
  </tr>
  <tr>
    <td style="text-align:center;"><img style="width:300px" src="https://im.sdk.qcloud.com/tools/resource/themes/android/chatLivelyBubbleBg.png"  />    </td>
    <td style="text-align:center;"><img style="width:300px" src="https://im.sdk.qcloud.com/tools/resource/themes/android/chatChangedBubbleBg.png" />     </td>
	 </tr>
</table>


## 新增主题

如果内置的 3 套主题无法满足您的需求，您可以自行按照如下步骤为组件新增一套全新的主题。
以添加一套 `商务版（Enterprise）` 主题为例：
1. 在每个组件中，与其他主题目录同级，在资源目录下新建主题目录 `res-enterprise`：
`res-enterprise/values/` 目录下新建 `enterprise_styles.xml` 文件，`enterprise_styles.xml` 文件中存储主题属性与真正资源的映射。

<img src="https://im.sdk.qcloud.com/tools/resource/themes/android/chatEnterpriseRes.png" width="400px" />


>! 1、`res-enterprise` 目录下必须包含所有要参与切换主题的资源，否则切换到 `商务版` 主题后应用会因为找不到资源而崩溃；
> 2、主题资源名不能跟系统资源名重复，也不可与已有资源名重复，否则会在编译期和运行期出现错误，因此要保证资源命名全局唯一。


2. 在 `enterprise_styles.xml` 文件中建立主题资源映射：
组件的 `src/main/res/values/tui_theme_attrs.xml` 文件中声明了需要参与切换主题的属性，这些属性需要在每个主题下都有对应的实现。
`src/main/res/values/enterprise_styles.xml` 文件中保存属性和资源的映射，例如：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="TUIChatEnterpriseTheme" parent="TUIBaseEnterpriseTheme">
        <item name="chat_bubble_self_bg">@drawable/chat_bubble_self_bg_enterprise</item>
        <item name="chat_bubble_other_bg">@drawable/chat_bubble_other_bg_enterprise</item>
        <item name="chat_input_area_bg">@color/chat_input_layout_bg_enterprise</item>
        <item name="chat_unread_dot_bg_color">@color/chat_unread_dot_bg_color_enterprise</item>
        <item name="chat_unread_dot_text_color">?attr/core_primary_color</item>
        <item name="chat_title_bar_more_menu">@drawable/chat_title_bar_more_menu_enterprise</item>
        <item name="chat_other_msg_text_color">@color/chat_other_msg_text_color_enterprise</item>

        ...

    </style>
</resources>
```

3. 在组件的 `build.gradle` 文件中添加配置，指定资源目录：
    指定资源目录参与 `App` 打包，每个组件的 `build.gradle` 文件中都需要添加编译资源目录：

```groovy
android {
    ...
    // 主题资源文件夹
    sourceSets {
        main {
            res.srcDirs += "src/main/res-light"
            res.srcDirs += "src/main/res-lively"
            res.srcDirs += "src/main/res-serious"
            res.srcDirs += "src/main/res-enterprise"
        }
    }
}
```

4. 应用启动时注册主题：
    注册了 `Enterprise` 主题之后，才可以应用 `Enterprise` 主题。每个组件以及 `App` 的主题都要进行注册。
    主题注册得越早越好，一般在 `Application` 启动时注册，这样 `Activity` 创建时就可以使用当前主题。

>!  `0-2` 分别为内置主题 ID，因此新增的主题 ID 必须大于等于 `3`。

```java
public class DemoApplication extends Application {
    @Override
    public void onCreate() {
        int enterpriseThemeID = 3;
        TUIThemeManager.addTheme(enterpriseThemeID, R.style.DemoEnterpriseTheme);
        TUIThemeManager.addTheme(enterpriseThemeID, R.style.TUIChatEnterpriseTheme);
        TUIThemeManager.addTheme(enterpriseThemeID, R.style.TUIContactEnterpriseTheme);
        TUIThemeManager.addTheme(enterpriseThemeID, R.style.TUIGroupEnterpriseTheme);
        // 切换主题
        TUIThemeManager.getInstance().changeTheme(this, enterpriseThemeID);
    }
}
```

5. 切换到新增的 `Enterprise` 主题，即可看到新增加的主题风格：

<img src="https://im.sdk.qcloud.com/tools/resource/themes/android/chatEnterpriseTheme.png" width="450px" />


## 主题样式表

### 基础样式

#### 存储位置

基础样式均存在于 `TUICore` 组件中，由各个组件引用。
基础样式提供了公共的 UI 规范，例如：首选背景色、分割线颜色等。可以通过修改基础样式来同时影响其他各个组件。

您可以在 `TUICore/tuicore/src/main/res/values/tui_theme_attrs.xml` 文件中看到 `TUICore` 的所有主题属性。主题属性对应的资源放在 `tuicore/src/main/res-***` 文件夹中。


#### UI 样式表

<img src="https://im.sdk.qcloud.com/tools/resource/themes/android/core_theme_attr_eg.png" width="1000px" />

**图标**

| 属性名称                          | 属性说明                  |
| --------------------------------- | ------------------------- |
| core_title_bar_back_icon          | 标题栏返回按钮图标        |
| core_default_group_icon_public    | 默认 Public 群头像图标    |
| core_default_group_icon_work      | 默认 Work 群头像图标      |
| core_default_group_icon_meeting   | 默认 Meeting 群头像图标   |
| core_default_group_icon_community | 默认 Community 群头像图标 |
| core_default_user_icon            | 默认用户头像图标          |
| user_status_online                | 用户在线状态图标          |
| user_status_offline               | 用户离线状态图标          |
| core_selected_icon                | 选中图标                  |

**背景色**

| 属性名称                            | 属性说明                 |
| ----------------------------------- | ------------------------ |
| core_light_bg_title_text_color      | 浅色背景下标题文字颜色   |
| core_light_bg_primary_text_color    | 浅色背景下主要文字颜色   |
| core_light_bg_secondary_text_color  | 浅色背景下次要文字颜色   |
| core_light_bg_secondary2_text_color | 浅色背景下再次文字颜色   |
| core_light_bg_disable_text_color    | 浅色背景下不可用文字颜色 |
| core_dark_bg_primary_text_color     | 深色背景下主要文字颜色   |
| core_bg_color                       | 主要背景色               |
| core_primary_color                  | 主题色                   |
| core_error_tip_color                | 错误提示颜色             |
| core_success_tip_color              | 成功提示颜色             |
| core_bubble_bg_color                | 气泡背景色               |
| core_divide_color                   | 分割线颜色               |
| core_border_color                   | 边框颜色                 |
| core_header_start_color             | 标题栏起始色             |
| core_header_end_color               | 标题栏终点色             |
| core_btn_normal_color               | 按钮常态颜色             |
| core_btn_pressed_color              | 按钮按下颜色             |
| core_btn_disable_color              | 按钮不可用颜色           |
| core_title_bar_bg                   | 标题栏背景               |
| core_title_bar_text_bg              | 标题栏文字背景色         |



### Chat 页面样式
#### 存储位置

您可以在 `TUIChat/tuichat/src/main/res/values/tui_theme_attrs.xml` 文件中看到 `TUIChat` 的所有主题属性。主题属性对应的资源放在 `tuichat/src/main/res-***` 文件夹中。

#### UI 样式表

<img src="https://im.sdk.qcloud.com/tools/resource/themes/android/chat_theme_attr_eg.png"  width="800px"  />

**图标**

| 属性名称                   | 属性说明             |
| -------------------------- | -------------------- |
| chat_title_bar_more_menu   | 标题栏菜单图标       |
| chat_reply_detail_icon     | 回复详情图标         |
| chat_jump_recent_down_icon | 消息列表向下跳转图标 |
| chat_jump_recent_up_icon   | 消息列表向上跳转图标 |

**背景色**

| 属性名称                          | 属性说明                       |
| --------------------------------- | ------------------------------ |
| chat_bubble_self_bg               | 己方消息的气泡背景             |
| chat_bubble_other_bg              | 对方消息的气泡背景             |
| chat_bubble_self_bg_color         | 己方消息的气泡背景颜色         |
| chat_bubble_other_bg_color        | 对方消息的气泡背景颜色         |
| chat_input_area_bg                | 输入界面背景色                 |
| chat_unread_dot_bg_color          | 未读图标背景色                 |
| chat_unread_dot_text_color        | 未读图标中文字颜色             |
| chat_other_msg_text_color         | 对方消息文字颜色               |
| chat_self_msg_text_color          | 己方消息文字颜色               |
| chat_self_custom_msg_text_color   | 己方自定义消息文字颜色         |
| chat_other_custom_msg_text_color  | 对方自定义消息文字颜色         |
| chat_self_custom_msg_link_color   | 己方自定义消息中链接文字颜色   |
| chat_other_custom_msg_link_color  | 对方自定义消息中链接文字颜色   |
| chat_tip_text_color               | 提示消息文字颜色               |
| chat_self_reply_quote_bg_color    | 己方回复和引用消息背景色       |
| chat_other_reply_quote_bg_color   | 对方回复和引用消息背景色       |
| chat_self_reply_line_bg_color     | 己方回复消息竖线背景色         |
| chat_other_reply_line_bg_color    | 对方回复消息竖线背景色         |
| chat_self_reply_quote_text_color  | 己方回复消息中原始消息文字颜色 |
| chat_other_reply_quote_text_color | 对方回复消息中原始消息文字颜色 |
| chat_self_reply_text_color        | 己方回复消息文字颜色           |
| chat_other_reply_text_color       | 对方回复消息文字颜色           |
| chat_read_receipt_text_color      | 已读回执文字颜色               |
| chat_react_text_color             | 表情回应己方文字颜色           |
| chat_react_other_text_color       | 表情回应对方文字颜色           |
| chat_pressed_bg_color             | 长按弹窗中按钮按下背景色       |


### Group 页面样式
#### 存储位置

您可以在 `TUIGroup/tuigroup/src/main/res/values/tui_theme_attrs.xml` 文件中看到 `TUIGroup` 的所有主题属性。主题属性对应的资源放在 `tuigroup/src/main/res-***` 文件夹中。

#### UI 样式表

<img src="https://im.sdk.qcloud.com/tools/resource/themes/android/group_theme_attr_eg.png" width="500px" />

| 属性名称       | 属性说明     |
| -------------- | ------------ |
| group_add_icon | 添加按钮图标 |

### Contact 页面样式
#### 存储位置

您可以在 `TUIContact/tuicontact/src/main/res/values/tui_theme_attrs.xml` 文件中看到 `TUIContact` 的所有主题属性。主题属性对应的资源放在 `tuicontact/src/main/res-***` 文件夹中。

#### UI 样式表

<img src="https://im.sdk.qcloud.com/tools/resource/themes/android/contact_theme_attr_eg.png" width="500px" />

| 属性名称                | 属性说明           |
| ----------------------- | ------------------ |
| contact_new_friend_icon | 新的联系人菜单图标 |
| contact_group_list_icon | 我的群聊菜单图标   |
| contact_black_list_icon | 黑名单菜单图标     |




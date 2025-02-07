TUIKit implements the sending and display for basic message types such as text, image, audio, video, and file messages by default. If these message types do not meet your requirements, you can add custom message types.

## Basic Message Types

<table>
     <tr>
         <th width="20%" style="text-align:center">Message Type</th>  
         <th style="text-align:center">Renderings</th>  
     </tr>
  <tr>
         <td style="text-align:center">Text message</td>
  <td style="text-align:center"><img src="https://qcloudimg.tencent-cloud.cn/raw/b488652bceb0d272126e9097fd7decab.png" width="320"/></td>
     </tr>
  <tr>
         <td style="text-align:center">Image message</td>
  <td style="text-align:center"><img src="https://qcloudimg.tencent-cloud.cn/raw/af0c06419dfea4030f9afa87cbcb89c4.png" width="320"/></td>
     </tr>
  <tr>
         <td style="text-align:center">Audio message</td>
  <td style="text-align:center"><img src="https://qcloudimg.tencent-cloud.cn/raw/91a2e6b3f77d785af48d44add17a7d54.png" width="320"/></td>
     </tr>
  <tr>
         <td style="text-align:center">Video message</td>
  <td style="text-align:center"><img src="https://qcloudimg.tencent-cloud.cn/raw/f268d0603f6ab209b195fc96e0c5a0f9.png" width="320"/></td>
     </tr>
  <tr>
         <td style="text-align:center">File message</td>
  <td style="text-align:center"><img src="https://qcloudimg.tencent-cloud.cn/raw/a9f8bd5cf2d5bfe6caa58f053bf88f1c.png" width="320"/></td>
     </tr>
</table>

## Customizing a Message

If the basic message types do not meet your requirements, you can customize messages as needed.
The following uses sending a custom hypertext message that can redirect to the browser as an example to help you quickly understand the implementation process.
The built-in custom message style of TUIKit is shown in the figure below:
<img src="https://qcloudimg.tencent-cloud.cn/raw/c2950cfae3c71c38552575e5e4b7880c.png" width = "300"/>

The following introduces how to use a custom message.

## Displaying a Custom Message

Information carried by a custom message is stored in `V2TimMessage.V2TimCustomElem.data` in string format. If a large amount of information needs to be delivered, the JSON format is recommended.

The basic logic for displaying a custom message is as follows: During parsing, parse a JSON string into a Map for instantiating a predefined class and then render the custom message body with the data in that object.

1. Define a class for the parsed custom message structure and write a fromJSON method to instantiate the class with the Map.
Take the custom message that contains a hyperlink and text as an example:
``` dart
class CustomMessage {
  // Define the content here as needed
  String? link;
  String? text;
  String? businessID;

  CustomMessage.fromJSON(Map json) {
    link = json["link"];
    text = json["text"];
    businessID = json["businessID"];
  }
}
```

2. Write a method to implement the parsing of the custom message to get a data object.
 Sample code:
``` dart
CustomMessage? getCustomMessageData(V2TimCustomElem? customElem) {
  try {
    if (customElem?.data != null) {
      final customMessage = jsonDecode(customElem!.data!);
      return CustomMessage.fromJSON(customMessage);
    }
    return null;
  } catch (err) {
    return null;
  }
}
```

3. In `TIMUIKitChat`, use `customMessageItemBuilder` in `messageItemBuilder` to render the custom message.
For the default custom message format in this example, the rendering effect after parsing is as follows:
<img src="https://qcloudimg.tencent-cloud.cn/raw/046e5d900a401babdb53acf0faa83c96.png" width="500"/><br>

Sample code:
```dart
messageItemBuilder: MessageItemBuilder(
  customMessageItemBuilder: (message, isShowJump, clearJump) {
    final CustomMessage customMessage = getCustomMessageData(message.customElem);
    if (linkMessage != null) {
      final String option1 = linkMessage.link ?? "";
      return Column(
        mainAxisAlignment: MainAxisAlignment.start,
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text(linkMessage.text ?? ""),
          MarkdownBody(
            data: TIM_t_para(
                "[View Details >>]({{option1}})", "[View Details >>]($option1)")(
                option1: option1),
            styleSheet: MarkdownStyleSheet.fromTheme(ThemeData(
                textTheme: const TextTheme(
                    bodyText2: TextStyle(fontSize: 16.0))))
                .copyWith(
              a: TextStyle(color: LinkUtils.hexToColor("015fff")),
            ),
          )
        ],
      );
    }
  }
),
```

## Sending a Custom Message

The general process of sending a custom message is as follows: use the SDK to create a custom message, convert the content to be delivered into the JSON string format and save it in `data`, and call the `sendMessage` API of `TIMUIKitChatController` to send the custom message.
The following is a code sample showing how to create and send a custom message on the "+" panel.

1. Instantiate a message controller and pass it into `TIMUIKitChat`.

```dart
final TIMUIKitChatController _timuiKitChatController =
      TIMUIKitChatController();

return TIMUIKitChat(
      controller: _timuiKitChatController,
      // ... Other parameters
      )
```

2. Add an item to the `extraAction` array of the `morePanelConfig` attribute of `TIMUIKitChat` to add the custom message sending button.
A button on the "+" panel consists of a text title and an image icon.

<img src="https://qcloudimg.tencent-cloud.cn/raw/cecb1fed6dc6ba810233cdb0a00d2acb.png" width = "200"/>

Sample code:
``` dart
morePanelConfig: MorePanelConfig(
  extraAction: [
    MorePanelItem(
        id: "customMessage",
        title: imt("Custom message"),
        onTap: (c) {
          _sendCustomMessage();
        },
        icon: Container(
          height: 64,
          width: 64,
          margin: const EdgeInsets.only(bottom: 4),
          decoration: const BoxDecoration(
              color: Colors.white,
              borderRadius: BorderRadius.all(Radius.circular(5))),
          child: SvgPicture.asset(
            "images/custom-msg.svg",
            package: 'tim_ui_kit',
            height: 64,
            width: 64,
          ),
        )
    ),
    // ... Other buttons on the "+" panel
  ],
  // ... Other parameters
)
```

3. Implement the custom message sending method.
The following is a code sample showing how to create and send a custom message via the clicking of the message sending button.
```dart
_sendCustomMessage() async {
  // Create a custom message. The `data`, `desc` and `extension` content can be defined by yourself.
  V2TimValueCallback<V2TimMsgCreateInfoResult> createCustomMessageRes =
      await TencentImSDKPlugin.v2TIMManager
          .getMessageManager()
          .createCustomMessage(
            data:
                '{"businessID":"text_link","link":"https://cloud.tencent.com/document/product/269/3794","text":"Welcome to Tencent Cloud IM group","version":4}',
            desc: 'Custom desc',
            extension: 'Custom extension',
          );
  if (createCustomMessageRes.code == 0) {
    String? id = createCustomMessageRes.data?.id;
    // Send the custom message
    V2TimValueCallback<V2TimMessage>? sendMessageRes =
        await _timuiKitChatController.sendMessage(
            messageInfo: createCustomMessageRes.data?.messageInfo);
    if (sendMessageRes!.code == 0) {
      // Message sent successfully
      sendMessageRes.data?.customElem?.data; //Custom `data`
      sendMessageRes.data?.customElem?.desc; //Custom `desc`
      sendMessageRes.data?.customElem?.extension; //Custom `extension`
    }
  }
}
```

[](id:feedback)



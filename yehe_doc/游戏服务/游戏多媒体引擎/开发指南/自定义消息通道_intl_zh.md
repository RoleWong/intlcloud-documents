
为方便 GME 开发者调试和接入腾讯云游戏多媒体引擎产品 API，本文为您介绍适用于 GME 用户自定义音频包附带消息功能的使用指引。

## 使用场景

GME 用户自定义音频包附带消息功能可以让开发者在 GME 音频包中携带自定义消息，作为信令传递广播给同房间的人。


## 前提条件

- **已开通实时语音服务**：可参见 [服务开通指引](https://intl.cloud.tencent.com/document/product/607/10782)。
- **已接入 GME SDK**：包括核心接口和实时语音接口的接入，详情可参见 [Native SDK 快速接入](https://intl.cloud.tencent.com/document/product/607/40858)、[Unity SDK 快速接入](https://intl.cloud.tencent.com/document/product/607/44544)、[Unreal SDK 快速接入](https://intl.cloud.tencent.com/document/product/607/44545)。


## 使用限制

调用此接口需要在房间类型为**标准**及**高清**（ITMG_ROOM_TYPE_STANDARD 及 ITMG_ROOM_TYPE_HIGHQUALITY）的情况下，发送端需要打开麦克风，接收端需要打开扬声器。

## 自定义消息功能接入

### 发送自定义消息

#### 接口原型


<dx-codeblock>
::: iOS 
-(int) SendCustomData:(NSData *)data repeatCout:(int)reaptCout;
:::
::: Android java
public abstract int SendCustomData(byte[] data,int repeatCout);
:::
::: Unity undefined
public abstract int SendCustomData(byte[] customdata,int repeatCout);
:::
</dx-codeblock>

#### 参数说明

|参数   |类型   |含义   |
|----------|-------|-------|
|data       |NSData * 、byte[]    |需要传递的信息|
|reaptCout  |int        |重复次数，填入-1为无限次重复发送|

#### 返回值
接口返回值为 QAV_OK 则表示成功。

回调返回1004表示参数错误，建议重新检查参数是否正确。返回1201表示房间不存在，建议检查房间号是否正确。

更多错误码请参见 [错误码](https://www.tencentcloud.com/document/product/607/33223) 文档。

#### 示例代码

**执行语句**


<dx-codeblock>
::: iOS 
-(IBAction)SendCustData:(UIButton*)sender {
    int ret = 0;
    NSString *typeString;
    switch (sender.tag) {
        case 1:
            ret = [[[ITMGContext GetInstance] GetRoom] SendCustomData:[NSData dataWithBytes:_shareRoomID.text.UTF8String length:_roomIdText.text.length] repeatCout:_shareOpenID.text.intValue];
            typeString = @"sendCustData";
            break;
          case 2:
            ret = [[[ITMGContext GetInstance] GetRoom] StopSendCustomData];
            typeString = @"recvCustData";
            break;
        default:
            break;
    }
    if(ret != 0) {
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"set fail" message:[NSString stringWithFormat:@"%@:errorcode :%d",typeString,ret] delegate:NULL cancelButtonTitle:@"OK" otherButtonTitles:nil];
        [alert show];
    }
}
:::
::: Android java
String strData = mEditData.getText().toString();
String repeatCount = mEditRepeatCount.getText().toString();
int nRet = ITMGContext.GetInstance(getActivity()).GetRoom().SendCustomData(strData.getBytes(), Integer.parseInt(repeatCount));
:::
::: Unity undefined
InputField SendCustom_Count_InputField = transform.Find("inroomPanel/imPanel/SendCustom_Count_InputField").GetComponent<InputField>();
InputField SendCustom_Data_InputField = transform.Find("inroomPanel/imPanel/SendCustom_Data_InputField").GetComponent<InputField>();

transform.Find("inroomPanel/imPanel/SendCustom_Btn").GetComponent<Button>().onClick.AddListener(delegate ()
       {
           string data = SendCustom_Data_InputField.text;
           string str_count = SendCustom_Count_InputField.text;
           int count = 0;
           if (int.TryParse(str_count, out count)) {
               Debug.Log(data+ count.ToString());
               byte[] byteData = Encoding.Default.GetBytes(data);
              int ret =  ITMGContext.GetInstance().GetRoom().SendCustomData(byteData, count);
              if(ret != 0 ) {
                 ShowWarnning(string.Format("send customdata failed err:{0}",ret));
              }
           }
       });
}
:::
</dx-codeblock>


**回调**


<dx-codeblock>
::: iOS 
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSString* log =[NSString stringWithFormat:@"OnEvent:%d,data:%@", (int)eventType, data];
    switch (eventType) {
                 case   ITMG_MAIN_EVENT_TYPE_CUSTOMDATA_UPDATE: {
            if (ITMG_CUSTOMDATA_AV_SUB_EVENT_UPDATE == ((NSNumber *)data[@"sub_type"]).intValue) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"customdata" message:[NSString stringWithFormat:@"内容:%@,from:%@ ",data[@"content"],data[@"senderid"]] delegate:NULL cancelButtonTitle:@"OK" otherButtonTitles:nil];
                                    [alert show];
            }
        }
            break;
    }
}
:::
::: Android java
if (ITMGContext.ITMG_MAIN_EVENT_TYPE.ITMG_MAIN_EVENT_TYPE_CUSTOMDATA_UPDATE == type) {
	int subtype  =  data.getIntExtra("sub_event",-1);
	if (subtype == 0) {
	    String content =  data.getStringExtra("content");
	    String sender = data.getStringExtra("senderid");
	    Toast.makeText(getActivity(), String.format("recv content =%s, from:%s", content, 
	        sender), Toast.LENGTH_SHORT).show();
	}
:::
::: Unity undefined
void OnEvent(int eventType,int subEventType,string data)
{
	Debug.Log (data);
	switch (eventType) {
     case (int)ITMG_MAIN_EVENT_TYPE.ITMG_MAIN_EVENT_TYPE_CUSTOMDATA_UPDATE:
      {
         if(subEventType == (int)ITMG_CUSTOMDATA_SUB_EVENT.ITMG_CUSTOMDATA_AV_SUB_EVENT_UPDATE) {
             _customData = JsonUtility.FromJson<CustomDataInfo>(data);
           ShowWarnning(string.Format("recve customdata {0}  from {1}",_customData.content,_customData.senderid));
         }
      }
     break;
}
:::
</dx-codeblock>



### 停止发送自定义消息
调用此接口停止发送自定义消息。

#### 接口原型

<dx-codeblock>
::: iOS 
-(int) StopSendCustomData;
:::
::: Android java
public abstract int StopSendCustomData();
:::
::: Unity undefined
public abstract int StopSendCustomData();
:::
</dx-codeblock>


#### 返回值

如果接口返回1003代表已经操作了 StopSendCustomData，SDK 正在进行这个操作，无需再次调用。

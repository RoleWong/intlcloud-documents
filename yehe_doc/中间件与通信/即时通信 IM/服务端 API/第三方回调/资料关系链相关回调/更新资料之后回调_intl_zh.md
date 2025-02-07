## 功能说明

App 后台可以通过该回调实时查看用户的更新资料操作。

## 注意事项

- 要启用回调，必须配置回调 URL，并打开本条回调协议对应的开关，配置方法详见 [第三方回调配置](https://intl.cloud.tencent.com/document/product/1047/34520) 文档。
- 回调的方向是即时通信 IM 后台向 App 后台发起 HTTP POST 请求。
- App 后台在收到回调请求之后，务必校验请求 URL 中的参数 SDKAppID 是否是自己的 SDKAppID。
- 其他安全相关事宜请参考 [第三方回调简介：安全考虑](https://intl.cloud.tencent.com/document/product/1047/34354) 文档。

## 可能触发该回调的场景

- App 用户通过客户端变更用户资料。
- App 管理员通过 REST API 变更用户资料。

## 回调发生时机

变更用户资料成功后触发。

## 接口说明
### 请求 URL 示例

以下示例中 App 配置的回调 URL 为 `https://www.example.com`。
**示例：**

```
https://www.example.com?SdkAppid=$SDKAppID&CallbackCommand=$CallbackCommand&contenttype=json&ClientIP=$ClientIP&OptPlatform=$OptPlatform
```

### 请求参数说明

| 参数 | 说明 |
| --- | --- |
| https | 请求协议为 HTTPS，请求方式为 POST |
| www.example.com | 回调 URL |
| SdkAppid | 创建应用时在即时通信 IM 控制台分配的 SDKAppID |
| CallbackCommand | 固定为：Profile.CallbackPortraitSet |
| contenttype | 固定值为：json |
| ClientIP | 客户端 IP，格式如：127.0.0.1 |
| OptPlatform | 客户端平台，取值参见 [第三方回调简介：回调协议](https://intl.cloud.tencent.com/document/product/1047/34354) 中 OptPlatform 的参数含义 |

### 请求包示例

```
{
  "CallbackCommand": "Profile.CallbackPortraitSet",
  "Operator_Account": "id1",
  "From_Account": "id1",
  "EventTime": 1656921052497,
  "ProfileItem": [
    {
      "Tag": "Tag_Profile_IM_Nick",
      "Value": "nick1"
    },
    {
      "Tag": "Tag_Profile_IM_Gender",
      "Value": "Gender_Type_Male"
    },
    {
      "Tag": "Tag_Profile_IM_AllowType",
      "Value": "AllowType_Type_NeedConfirm"
    },
    {
      "Tag": "Tag_Profile_Custom_Data",
      "Value": "your custom data"
    }
  ]
}
```

### 请求包字段说明

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| CallbackCommand | String | 回调命令|
| Operator_Account | String | 触发更新操作的用户的 UserID |
| From_Account | String | 更新用户资料的用户的 UserID |
| EventTime | Integer | 毫秒时间戳 |
| ProfileItem | Array | 更新成功的用户资料列表 |
| Tag | String | 更新成功的资料字段的名称，详情可参见 [资料字段](https://intl.cloud.tencent.com/document/product/1047/33520) |
| Value | uint32/string | 更新成功后的资料字段的值，详情可参见 [资料字段](https://intl.cloud.tencent.com/document/product/1047/33520) |

### 应答包示例

```
{
    "ActionStatus": "OK",
    "ErrorCode": 0,
    "ErrorInfo": ""
}
```

### 应答包字段说明

| 字段 | 类型 | 属性 | 说明 |
| --- | --- | --- | --- |
| ActionStatus | String | 必填 | 请求处理的结果，OK 表示处理成功，FAIL 表示失败 |
| ErrorCode | Integer | 必填 | 错误码，0表示 App 后台处理成功，1表示 App 后台处理失败 |
| ErrorInfo | String | 必填 | 错误信息 |

## 参考

- [第三方回调简介](https://intl.cloud.tencent.com/document/product/1047/34354)



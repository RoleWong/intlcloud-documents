## 适用场景

您可以直接发起请求下载对象存储（Cloud Object Storage，COS）中的对象，下载对象支持以下功能：

- 下载完整的单个对象：直接发起 GET 请求即可下载完整的对象数据。
- 下载单个对象的部分内容：可在 GET 请求中传入 Range 请求头部，支持检索一个特定的字节范围，不支持检索多个范围。

对象的元数据将会作为 HTTP 响应头部随对象内容一同返回，GET 请求支持使用 URL 参数的方式覆盖响应的部分元数据值。
例如 Content-Disposition 的响应值。支持修改的响应头部包括：
- Content-Type
- Content-Language
- Expires
- Cache-Control
- Content-Disposition
- Content-Encoding

## 使用方法

### 使用对象存储控制台

您可以使用对象存储控制台下载对象，详情请参见 [下载对象](https://intl.cloud.tencent.com/document/product/436/13322) 控制台指南文档。

### 使用 REST API

您可以直接使用 REST API 发起下载对象请求，详情请参见 [GET Object](https://intl.cloud.tencent.com/document/product/436/7753) API 文档。

### 使用 SDK

您可以直接调用 SDK 的下载对象方法，详情请参见下列各语言 SDK 文档：


- [Android SDK](https://intl.cloud.tencent.com/document/product/436/37675#.E4.B8.8B.E8.BD.BD.E5.AF.B9.E8.B1.A1)
- [C SDK](https://intl.cloud.tencent.com/document/product/436/31464#.E4.B8.8B.E8.BD.BD.E5.AF.B9.E8.B1.A1)
- [C++ SDK](https://intl.cloud.tencent.com/document/product/436/31465#.E4.B8.8B.E8.BD.BD.E5.AF.B9.E8.B1.A1)
- [.NET SDK](https://intl.cloud.tencent.com/document/product/436/30594)
- [Go SDK](https://intl.cloud.tencent.com/document/product/436/31466#.E4.B8.8B.E8.BD.BD.E5.AF.B9.E8.B1.A1)
- [iOS SDK](https://intl.cloud.tencent.com/document/product/436/37684#.E8.8E.B7.E5.8F.96.E5.AF.B9.E8.B1.A1)
- [Java SDK](https://intl.cloud.tencent.com/document/product/436/31468#.E4.B8.8B.E8.BD.BD.E5.AF.B9.E8.B1.A1)
- [JavaScript SDK](https://intl.cloud.tencent.com/document/product/436/31477#.E4.B8.8B.E8.BD.BD.E5.AF.B9.E8.B1.A1)
- [Node.js SDK](https://intl.cloud.tencent.com/document/product/436/31469#.E4.B8.8B.E8.BD.BD.E5.AF.B9.E8.B1.A1)
- [PHP SDK](https://intl.cloud.tencent.com/document/product/436/31470#.E4.B8.8B.E8.BD.BD.E5.AF.B9.E8.B1.A1)
- [Python SDK](https://intl.cloud.tencent.com/document/product/436/31471#.E4.B8.8B.E8.BD.BD.E5.AF.B9.E8.B1.A1)
- [小程序 SDK](https://www.tencentcloud.com/document/product/436/43882)

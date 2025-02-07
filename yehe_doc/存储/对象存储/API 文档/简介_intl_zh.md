腾讯云对象存储 COS 使用 XML API，这是一种轻量级的、无连接状态的接口，调用此接口您可以直接通过 HTTP/HTTPS 发出请求和接受响应，实现与腾讯云对象存储后台的交互操作。

由于使用了不同的数据传输框架，对象存储 COS 提供了独立于云 API 的接口和独立的 SDK，您可直接前往 COS 的 [API 操作列表](https://intl.cloud.tencent.com/document/product/436/10111) 了解详情，或前往 COS 的 [SDK 列表](https://intl.cloud.tencent.com/document/product/436/6474) 下载您需要的 SDK。云 API 的指南和对应的 SDK 不包含对象存储 COS 的操作功能。

>!
>- 如您已开始使用腾讯云 COS API，即代表您已阅读并同意 [《腾讯云服务协议》](https://intl.cloud.tencent.com/document/product/301/12905) 和 [《腾讯云对象存储服务等级协议》](https://intl.cloud.tencent.com/document/product/436/6227)。
>- COS 的可用地域（Region）的详细信息请查阅 [地域和访问域名](https://intl.cloud.tencent.com/document/product/436/6224) 文档。 
>- 在使用 API 或 SDK 发起请求前，建议您阅读 [创建请求概述](https://intl.cloud.tencent.com/document/product/436/30613) 文档了解发起访问的域名、安全鉴权概念以及内外网访问检查等信息。
>- 其他腾讯云产品的 API 格式，请参见 [API 中心介绍](https://intl.cloud.tencent.com/document/api)。



## API 概览

COS 支持的 API 接口请参见 [操作列表](https://intl.cloud.tencent.com/document/product/436/10111)。
<div class="rno-api-explorer">
    <div class="rno-api-explorer-inner">
        <div class="rno-api-explorer-hd">
            <div class="rno-api-explorer-title">
                推荐使用 API Explorer
            </div>
            <a href="https://console.cloud.tencent.com/api/explorer?Product=cos&Version=2018-11-26&Action=GetService&SignVersion=" class="rno-api-explorer-btn" hotrep="doc.api.explorerbtn" target="_blank"><i class="rno-icon-explorer"></i>点击调试</a>
        </div>
        <div class="rno-api-explorer-body">
            <div class="rno-api-explorer-cont">
                API Explorer 提供了在线调用、签名验证、SDK 代码生成和快速检索接口等能力。您可查看每次调用的请求内容和返回结果以及自动生成 SDK 调用示例。
            </div>
        </div>
    </div>
</div>

## 术语信息
使用 API 接口时会出现一些主要概念和术语，请见下表：
<style rel="stylesheet">
table th:nth-of-type(1) {
width: 150px;	
}
table th:nth-of-type(2) {
width:550px;	
}
</style>

|名称|	描述|
|---|---|
| APPID	|开发者访问 COS 服务时拥有的用户维度唯一资源标识，用以标识资源，可在 [API 密钥管理](https://console.cloud.tencent.com/capi) 页面获取|
| SecretId | 开发者拥有的项目身份识别 ID，用于身份认证，可在 [API 密钥管理](https://console.cloud.tencent.com/capi) 页面获取|
| SecretKey	| 开发者拥有的项目身份密钥，可在 [API 密钥管理](https://console.cloud.tencent.com/capi) 页面获取|
| Bucket | 存储桶，COS 中用于存储数据的容器。有关存储桶的进一步说明，请参见 [存储桶概述](https://intl.cloud.tencent.com/document/product/436/13312) 文档|
| BucketName-APPID  |  存储桶名称格式，用户在使用 API、SDK 时，需要按照此格式填写存储桶名称。例如 examplebucket-1250000000，含义为该存储桶 examplebucket 归属于 APPID 为1250000000的用户   |
| Object | 对象，COS 中存储的具体文件，是存储的基本实体 |
| ObjectKey | 对象键，对象（Object）在存储桶（Bucket）中的唯一标识。有关对象与对象键的进一步说明，请参见 [对象概述](https://intl.cloud.tencent.com/document/product/436/13324) 文档|
| Region | 地域信息，枚举值可参见 [可用地域](https://intl.cloud.tencent.com/document/product/436/6224) 文档，例如：ap-beijing、ap-hongkong、eu-frankfurt 等 |
| ACL |	访问控制列表（Access Control List），是指特定 Bucket 或 Object 的访问控制信息列表|
| CORS | 跨域资源共享（Cross-Origin Resource Sharing），<br>指发起请求的资源所在域不同于该请求所指向资源所在的域的 HTTP 请求|
| Multipart Uploads |分块上传，腾讯云 COS 服务为上传文件提供的一种分块上传模式|
|  Object Content    |      Object Content 是上传文件的二进制内容 |


## 快速入门

要使用腾讯云对象存储 API，需要先执行以下步骤：

1. 在腾讯云 [对象存储控制台](https://console.cloud.tencent.com/cos5) 开通腾讯云对象存储（COS）服务。
2. 在腾讯云 [对象存储控制台](https://console.cloud.tencent.com/cos5) 创建一个 Bucket。
3. 在访问管理控制台中的 [API 密钥管理](https://console.cloud.tencent.com/capi) 页面里获取 APPID，并创建 SecretId、SecretKey。
4. 编写一个请求签名算法程序（或使用任何一种服务端 SDK），详情请参见 [请求签名](https://intl.cloud.tencent.com/document/product/436/7778) 文档。
5. 计算签名，调用 API 执行操作。



## 功能描述

本接口用于主动查询指定的文件哈希值计算任务结果。

## 请求

#### 请求示例

```shell
GET /file_jobs/<jobId> HTTP/1.1
Host: <BucketName-APPID>.ci.<Region>.myqcloud.com
Date: <GMT Date>
Authorization: <Auth String>

```

>?
> - Authorization: Auth String（详情请参见 [请求签名](https://intl.cloud.tencent.com/document/product/436/7778) 文档）。
> - 通过子账号使用时，需要授予相关的权限，详情请参见 [授权粒度详情](https://intl.cloud.tencent.com/document/product/1045/49896) 文档。
>

#### 请求头

此接口仅使用公共请求头部，详情请参见 [公共请求头部](https://intl.cloud.tencent.com/document/product/1045/43609) 文档。

#### 请求体

该请求无请求体。


## 响应

#### 响应头

此接口仅返回公共响应头部，详情请参见 [公共响应头部](https://intl.cloud.tencent.com/document/product/1045/43610) 文档。

#### 响应体

该响应体返回为 **application/xml** 数据，包含完整节点数据的内容展示如下：

```shell
<Response>
    <JobsDetail>
        <Code>Success</Code>
        <Message/>
        <JobId>f93984788066911ed89ed352d4d9d2084</JobId>
        <State>Submitted</State>
        <CreationTime>2022-07-18T15:16:43+0800</CreationTime>
        <EndTime>-</EndTime>
        <StartTime>-</StartTime>
        <QueueId>p2911917386e148639319e13c285cc774</QueueId>
        <Tag>FileHashCode</Tag>
        <Input>
            <BucketId>test-1234567890</BucketId>
            <Object>input/test.mp4</Object>
            <Region>ap-chongqing</Region>
        </Input>
        <Operation>
            <FileHashCodeConfig>
                <Type>MD5</Type>
                <AddToHeader>true</AddToHeader>
            </FileHashCodeConfig>
            <FileHashCodeResult>
                <MD5>xxxxxxxx</MD5>
                <FileSize>123</FileSize>
                <LastModified>2022-06-27T15:23:11+0800</LastModified>
                <Etag>xxxxxx</Etag>
            </FileHashCodeResult>
            <UserData>This is my data.</UserData>
        </Operation>
    </JobsDetail>
</Response>
```

具体的数据内容如下：

| 节点名称（关键字） | 父节点 | 描述             | 类型      |
| :----------------- | :----- | :--------------- | :-------- |
| Response           | 无     | 保存结果的容器。 | Container |

Container 节点 Response 的内容：

| 节点名称（关键字） | 父节点   | 描述                                                         | 类型      |
| :----------------- | :------- | :----------------------------------------------------------- | :-------- |
| JobsDetail         | Response | 任务的详细信息。                                           | Container |
| NonExistJobIds     | Response | 查询的 ID 中不存在任务，所有任务都存在时不返回。              | String    |

具体参数值，请前往 [文件哈希值计算`JobsDetail`查看](https://cloud.tencent.com/document/product/460/83085#jobdetail)。

#### 错误码

该请求操作无特殊错误信息，常见的错误信息请参见 [错误码](https://intl.cloud.tencent.com/document/product/1045/33700) 文档。


## 实际案例

#### 请求

```shell
GET /file_jobs/f93984788066911ed89ed352d4d9d2084 HTTP/1.1
Accept: */*
Authorization:q-sign-algorithm=sha1&q-ak=AKIDZfbOAo7cllgPvF9cXFrJD0a1ICvR****&q-sign-time=1497530202;1497610202&q-key-time=1497530202;1497610202&q-header-list=&q-url-param-list=&q-signature=28e9a4986df11bed0255e97ff90500557e0ea057
Host:test-1234567890.ci.ap-chongqing.myqcloud.com

```

#### 响应

```shell
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: 666
Connection: keep-alive
Date: Mon, 18 Jul 2022 19:37:29 GMT
Server: tencent-ci
x-ci-request-id: NjMxMDJhYTNfMThhYTk0MGFfYmU1OV8zZjc=

<Response>
    <JobsDetail>
        <Code>Success</Code>
        <Message/>
        <JobId>f93984788066911ed89ed352d4d9d2084</JobId>
        <State>Submitted</State>
        <CreationTime>2022-07-18T15:16:43+0800</CreationTime>
        <EndTime>-</EndTime>
        <StartTime>-</StartTime>
        <QueueId>p2911917386e148639319e13c285cc774</QueueId>
        <Tag>FileHashCode</Tag>
        <Input>
            <BucketId>test-1234567890</BucketId>
            <Object>input/test.mp4</Object>
            <Region>ap-chongqing</Region>
        </Input>
        <Operation>
            <FileHashCodeConfig>
                <Type>MD5</Type>
                <AddToHeader>true</AddToHeader>
            </FileHashCodeConfig>
            <FileHashCodeResult>
                <MD5>xxxxxxxx</MD5>
                <FileSize>123</FileSize>
                <LastModified>2022-06-27T15:23:11+0800</LastModified>
                <Etag>xxxxxx</Etag>
            </FileHashCodeResult>
            <UserData>This is my data.</UserData>
        </Operation>
    </JobsDetail>
</Response>
```

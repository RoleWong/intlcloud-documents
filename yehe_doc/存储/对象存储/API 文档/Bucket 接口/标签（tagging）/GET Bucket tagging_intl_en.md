## Overview

This API is used to query the existing bucket tags of a bucket.

>? If a sub-account needs to call this API (`GET Bucket tagging`), ensure that the root account has authorized it to do so.
>

<div class="rno-api-explorer">
    <div class="rno-api-explorer-inner">
        <div class="rno-api-explorer-hd">
            <div class="rno-api-explorer-title">
                API Explorer is recommended.
            </div>
            <a href="https://console.cloud.tencent.com/api/explorer?Product=cos&Version=2018-11-26&Action=GetBucketTagging&SignVersion=" class="rno-api-explorer-btn" hotrep="doc.api.explorerbtn" target="_blank"><i class="rno-icon-explorer"></i>Debug</a>
        </div>
        <div class="rno-api-explorer-body">
            <div class="rno-api-explorer-cont">
                API Explorer makes it easy to make online API calls, verify signatures, generate SDK code, search for APIs, etc. You can also use it to query the content of each request as well as its response.
            </div>
        </div>
    </div>
</div>

## Request

#### Sample request

```http
GET /?tagging HTTP 1.1
Host:<BucketName-APPID>.cos.<Region>.myqcloud.com
Date: GMT Date
Authorization: Auth String
```

>? 
> - In `Host: <BucketName-APPID>.cos.<Region>.myqcloud.com`, <BucketName-APPID> is the bucket name followed by the APPID, such as `examplebucket-1250000000` (see [Bucket Overview > Basic Information](https://intl.cloud.tencent.com/document/product/436/38493) and [Bucket Overview > Bucket Naming Conventions](https://intl.cloud.tencent.com/document/product/436/13312)), and <Region> is a COS region (see [Regions and Access Endpoints](https://intl.cloud.tencent.com/document/product/436/6224)).
> - Authorization: Auth String (see [Request Signature](https://intl.cloud.tencent.com/document/product/436/7778) for more information).
> 

#### Request headers

This API only uses [Common Request Headers](https://intl.cloud.tencent.com/document/product/436/7728).

#### Request body

The request body of this request is empty.

## Response

#### Response headers

This API only returns [Common Response Headers](https://intl.cloud.tencent.com/document/product/436/7729).


#### Response body
If the request is successful, **application/xml** data that contains information about the bucket’s tags will be returned.
```shell
<Tagging>
    <TagSet>
        <Tag>
            <Key>string</Key>
            <Value>string</Value>
        </Tag>
        <Tag>
            <Key>string</Key>
            <Value>string</Value>
        </Tag>
    </TagSet>
</Tagging>
```

The nodes are described as follows:

| Node Name (Keyword) | Parent Node | Description | Type |
| ------------------ | ------------------ | ------------------------------------------------------------ | ---------- |
| Tagging | None | Tagging configuration | Container |
| TagSet | Tagging | Tag set | Container |
| Tag | Tagging.TagSet | A single tag. Up to 50 tags are supported. | Containers |
| Key | Tagging.TagSet.Tag | Tag key, which can be up to 128 characters. A tag key can contain letters, digits, spaces, plus signs (+), minus signs (−), underscores (_), equals signs (=), dots (.), colons (:), and slashes (/). | String |
| Value | Tagging.TagSet.Tag | Tag value, which can be up to 256 characters. A tag value can contain letters, digits, spaces, plus signs (+), minus signs (−), underscores (_), equals signs (=), dots (.), colons (:), and slashes (/). | String |

#### Error codes

This API returns common error responses and error codes. For more information, please see [Error Codes](https://intl.cloud.tencent.com/document/product/436/7730).

## Sample

#### Request

The following example queries the tags of the `examplebucket-1250000000` bucket. COS parses the request and returns the {age:18} and {name:xiaoming} tags:

```shell
GET /?tagging HTTP/1.1
User-Agent: curl/7.29.0
Accept: */*
Host: examplebucket-1250000000.cos.ap-chengdu.myqcloud.com
Authorization: q-sign-algorithm=sha1&q-ak=AKIDrbAYjEBqqdEconpFi8NPFsOjrnX4****&q-sign-time=1516361923;1517361973&q-key-time=1516361923;1517361973&q-url-param-list=tagging&q-header-list=content-md5;host&q-signature=71251feb4501494edcfbd01747fa87300375****
Content-Length: 127
Content-Type: application/xml
```

#### Response

```shell
HTTP/1.1 200 OK
Content-Type: application/xml
Connection: close
Date: Fri, 19 Jan 2018 11:40:22 GMT
Server: tencent-cos
<Tagging>
    <TagSet>
        <Tag>
            <Key>age</Key>
            <Value>18</Value>
        </Tag>
        <Tag>
            <Key>name</Key>
            <Value>xiaoming</Value>
        </Tag>
    </TagSet>
</Tagging>
```

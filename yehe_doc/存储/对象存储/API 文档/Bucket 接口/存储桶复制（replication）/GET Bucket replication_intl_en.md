## Feature Description

This API is used to query the cross-bucket replication configuration of a bucket. When calling this API, you need to carry a request signature.

## Request

#### Sample request

```plaintext
GET /?replication HTTP/1.1
Host: <BucketName-APPID>.cos.<Region>.myqcloud.com
Date: GMT Date
Authorization: Auth String
```

>? 
> - Host: &lt;BucketName-APPID>.cos.&lt;Region>.myqcloud.com, where &lt;BucketName-APPID> is the bucket name followed by the `APPID`, such as `examplebucket-1250000000` (see [Bucket Overview > Basic Information](https://intl.cloud.tencent.com/document/product/436/38493) and [Bucket Overview > Bucket Naming Conventions](https://intl.cloud.tencent.com/document/product/436/13312)), and &lt;Region> is a COS region (see [Regions and Access Endpoints](https://www.tencentcloud.com/document/product/436/6224)).
> - Authorization: Auth String (See [Request Signature](https://intl.cloud.tencent.com/document/product/436/7778) for details.)
> 

#### Request headers

This API only uses [Common Request Headers](https://intl.cloud.tencent.com/document/product/436/7728).


#### Request body

The request body of this request is empty.

## Response

#### Response headers

This API only returns [Common Response Headers](https://intl.cloud.tencent.com/document/product/436/7729).

#### Response body

The response body returns **application/xml** data. The following contains all the nodes:

```plaintext
<ReplicationConfiguration>
    <Role>qcs::cam::uin/<OwnerUin>:uin/<SubUin></Role>
    <Rule>
        <Status></Status>
        <Priority></Priority>
        <ID></ID>
        <Filter>
            <And>
                <Prefix></Prefix>
                <Tag>
                    <Key></Key>
                    <Value></Value>
                </Tag>
                <Tag>
                    <Key></Key>
                    <Value></Value>
                </Tag>
            </And>
        </Filter>
        <Destination>
            <Bucket>qcs::cos:<Region>::<BucketName-APPID></Bucket>
            <StorageClass></StorageClass>
        </Destination>
        <DeleteMarkerReplication>
            <Status></Status>
        </DeleteMarkerReplication>
    </Rule>
</ReplicationConfiguration>
```

The nodes are described as follows:

| Node Name (Keyword) | Parent Node | Description | Type |
| ------------------------ | ----------------------------------------- | ------------------------------------------------------------ | --------- |
| ReplicationConfiguration | None | All replication configurations | Container |
| Role | ReplicationConfiguration | Request initiator identifier, formatted as <br>`qcs::cam::uin/&lt;OwnerUin>:uin/&lt;SubUin>` | String |
| Rule | ReplicationConfiguration | Specific configuration. You can set a maximum of 1,000 rules, which should apply to the same destination bucket | Container |
| ID | ReplicationConfiguration.Rule | Name of a specific rule | String |
| Status | ReplicationConfiguration.Rule | `Rule` status identifier. Enumerated values: `Enabled`, `Disabled` | String |
|Priority                   | ReplicationConfiguration.Rule             | Rule execution priority, which is used to handle the situation where the destination buckets are the same and the replication rule hits the same object. All bucket replication rules must carry or not carry `Priority`, which can be an integer in the range of 1–1000 and must be unique for each rule. <li>If all rules carry `Priority`, when the destination buckets are the same, the object filtering prefixes in different rules can overlap. When different rules hit the same object, the rule with the smallest `Priority` value will be triggered first. <li>If all rules do not carry `Priority`, the object filtering prefixes in different rules cannot overlap. | Integer |
Filter                   | ReplicationConfiguration.Rule             | Object filter. The bucket replication feature will replicate objects matching the object prefix and tag set in `Filter`. | Container |
|And                   | ReplicationConfiguration.Rule             | When filtering the objects to be replicated, if you want to use both object prefix and object tag as filter conditions, you need to wrap them with `And`. | Container |
| Prefix                   | ReplicationConfiguration.Rule.Filter             | Prefix matching policy. Policies cannot overlap; otherwise, an error will be returned. To match the root directory, leave this parameter empty. | String |
| Tag                  | ReplicationConfiguration.Rule.Filter.And             | You can filter the objects to be replicated by one or more (up to 10) object tags. If tags are added as filter conditions, the `DeleteMarkerReplication` option must be set to `false`.	 | String |
| Destination | ReplicationConfiguration.Rule | Destination bucket information | Container | 
| Bucket | ReplicationConfiguration.Rule.Destination | Resource identifier, formatted as <br>`qcs::cos:[region]::[BucketName-APPID]` | String | 
| StorageClass | ReplicationConfiguration.Rule.Destination | Storage class. Enumerated values: `STANDARD`, `INTELLIGENT_TIERING`, `STANDARD_IA`, `ARCHIVE`, `DEEP_ARCHIVE`. Defaults to the storage class of the source bucket. | String | 
| DeleteMarkerReplication             | ReplicationConfiguration.Rule | Whether to sync the delete marker |Container    |
|Status             | ReplicationConfiguration.Rule. DeleteMarkerReplication | Whether to sync the delete marker. Valid values: `Disabled`, `Enabled`. Default value: `Enabled`. |String    |

#### Error codes

This API returns common error responses and error codes. For more information, see [Error Codes](https://intl.cloud.tencent.com/document/product/436/7730).




## Samples

#### Request

The following example queries the configuration of the `originbucket-1250000000` bucket:

```plaintext
GET /?replication HTTP/1.1
Date: Fri, 14 Apr 2019 07:17:19 GMT
Authorization: q-sign-algorithm=sha1&q-ak=AKIDZfbOAo7cllgPvF9cXFrJD0a1ICvR98JM&q-sign-time=1503895278;1503895638&q-key-time=1503895278;1503895638&q-header-list=host&q-url-param-list=replication&q-signature=f77900be432072b16afd8222b4b349aabd837cb9
Host: originbucket-1250000000.cos.ap-guangzhou.myqcloud.com
Content-Length: 0
```

#### Response

After the request above is made, COS returns the following response, indicating that the cross-bucket replication configuration of the bucket is enabled. The `ReplicationConfiguration` indicates that all objects prefixed with `testPrefix` in the `originbucket-1250000000` bucket are to be replicated. Storage classes of the replicated objects are subject to those of the source bucket.

```plaintext
Content-Type: application/xml
Content-Length: 309
Connection: keep-alive
Date: Fri, 14 Apr 2019 07:17:19 GMT
Server: tencent-cos
x-cos-replication-rule-creation-time: Fri, 14 Apr 2019 07:06:19 GMT
x-cos-request-id: NWQwMzQ5ZmZfMjBiNDU4NjRfNjAwOV84MzA2****
<ReplicationConfiguration>
    <Role>qcs::cam::uin/100000000001:uin/100000000001</Role>
    <Rule>
        <Status>Enabled</Status>
        <ID>RuleId_01</ID>
        <Prefix>testPrefix</Prefix>
        <Destination>
            <Bucket>qcs::cos:ap-guangzhou::destinationbucket-1250000000</Bucket>
        </Destination>
    </Rule>
</ReplicationConfiguration>
```

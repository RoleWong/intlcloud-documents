## Overview

Node.js SDK provides APIs for getting object URLs and pre-signed request URLs. For details, see the descriptions and samples below.

> ?
>
> - You are advised to use a temporary key to generate pre-signed URLs for the security of your requests such as uploads and downloads. When you apply for a temporary key, follow the [Principle of Least Privilege](https://intl.cloud.tencent.com/document/product/436/32972) to avoid leaking resources besides your buckets and objects. Note that the action for applying for a temporary key needs to be added with the `"name/cos:GetObject"` permission.
> - If you need to use a permanent key to generate a pre-signed URL, you are advised to limit the permission of the permanent key to uploads and downloads only to avoid risks.

## Signature Calculation

In all COS XML API requests, the authentication credential `Authorization` is required for all operations on private resources for COS to determine whether a request is valid.

There are two ways to use the authentication credential:

1. Put it in the header parameter; field name: authorization.
2. Put it in the URL parameter; field name: sign.

The `COS.getAuthorization` method is used to calculate the authentication credential (Authorization), which is the signing information used to verify the validity of the request.

>! We recommend you use this method only for frontend debugging but not in actual projects, as it may disclose keys.

#### Sample code

Obtaining the authentication credential for file download:

[//]: # (.cssg-snippet-get-authorization)

```js
// Log in at https://console.cloud.tencent.com/cam/capi to view and manage the `SECRETID` and `SECRETKEY`.
var COS = require('cos-nodejs-sdk-v5');
var Authorization = COS.getAuthorization({
  SecretId: 'SECRETID',
  SecretKey: 'SECRETKEY',
  Method: 'get',
  Key: 'a.jpg',
  Expires: 60,
  Query: {},
  Headers: {},
});
```

#### Parameter description

| Parameter | Description | Type | Required |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------- |
| SecretId | Your `SecretId`. | String | Yes |
| SecretKey | Your `SecretKey`. | String | Yes |
| Method | HTTP request method such as `GET`, `POST`, `DELETE`, or `HEAD` | String | Yes |
| Key | Object key (object name) is the unique ID of an object in a bucket. **If the request operation is to be performed on a file, this parameter is required and should be a filename.** If the operation is on a bucket, this parameter should be left empty. | String | No |
| Query     | Request parameters to be included in the signature in the format of `{key: 'val'}`                                         | Object | No   |
| Headers   | Request headers to be included in the signature in the format of `{key: 'val'}`                                         | Object | No   |
| Expires | Signature expiration time in seconds. Default value: `900`. | Number | No |

#### Return value description

The returned value is the calculated authentication credential string `authorization`.

## Getting a Pre-signed Request URL

### Download request samples

Sample 1. Get an unsigned object URL

[//]: # (.cssg-snippet-get-presign-download-url-nosign)

```js
var url = cos.getObjectUrl({
  Bucket: 'examplebucket-1250000000', /* Your bucket name. Required. */
  Region: 'COS_REGION', /* Bucket region (required), such as ap-beijing */
  Key: '1.jpg', /* Object key stored in the bucket (required), such as `1.jpg` and `a/b/test.txt`. */
  Sign: false,
});
```

Sample 2. Get a signed object URL

[//]: # (.cssg-snippet-get-presign-download-url)

```js
var url = cos.getObjectUrl({
  Bucket: 'examplebucket-1250000000', /* Your bucket name. Required. */
  Region: 'COS_REGION', /* Bucket region (required), such as ap-beijing */
  Key: 'Profile photo.jpg', /* Object key stored in the bucket (required), such as `1.jpg` and `a/b/test.txt`. */
});
```

Sample 3. If the signing process is asynchronous, you need to get the signed URL through a callback

[//]: # (.cssg-snippet-get-presign-download-url-callback)

```js
cos.getObjectUrl(
  {
    Bucket: 'examplebucket-1250000000', /* Your bucket name. Required. */
    Region: 'COS_REGION', /* Bucket region (required), such as ap-beijing */
    Key: 'Profile photo.jpg', /* Object key stored in the bucket (required), such as `1.jpg` and `a/b/test.txt`. */
    Sign: false,
  },
  function (err, data) {
    console.log(err || data.Url);
  }
);
```

Sample 4. Specify the validity period of a link

[//]: # (.cssg-snippet-get-presign-download-url-expiration)

```js
cos.getObjectUrl(
  {
    Bucket: 'examplebucket-1250000000', /* Your bucket name. Required. */
    Region: 'COS_REGION', /* Bucket region (required), such as ap-beijing */
    Key: 'Profile photo.jpg', /* Object key stored in the bucket (required), such as `1.jpg` and `a/b/test.txt`. */
    Sign: true,
    Expires: 3600, // Unit: second
  },
  function (err, data) {
    console.log(err || data.Url);
  }
);
```

Sample 5. Get the file URL and download the file

[//]: # (.cssg-snippet-get-presign-download-url-then-fetch)

```js
var request = require('request');
var fs = require('fs');
cos.getObjectUrl(
  {
    Bucket: 'examplebucket-1250000000', /* Your bucket name. Required. */
    Region: 'COS_REGION', /* Bucket region (required), such as ap-beijing */
    Key: 'Profile photo.jpg', /* Object key stored in the bucket (required), such as `1.jpg` and `a/b/test.txt`. */
    Sign: true,
  },
  function (err, data) {
    if (err) return console.log(err);
    console.log(data.Url);
    var req = request(data.Url, function (err, response, body) {
      console.log(err || body);
    });
    var writeStream = fs.createWriteStream(__dirname + '/1.jpg');
    req.pipe(writeStream);
  }
);
```

Sample 6: Generating a pre-signed URL with the signature containing `Query` and `Header`

[//]: # (.cssg-snippet-get-obejct-url-with-params)

```js
cos.getObjectUrl(
  {
    Bucket: 'examplebucket-1250000000', /* Your bucket name. Required. */
    Region: 'COS_REGION', /* Bucket region (required), such as ap-beijing */
    Key: 'Profile photo.jpg', /* Object key stored in the bucket (required), such as `1.jpg` and `a/b/test.txt`. */
    Sign: true,
    /* The HTTP request parameters passed in should be the same as those of the actual request. This can prevent users from tampering with the HTTP request parameters. */
    Query: {
      'imageMogr2/thumbnail/200x/': '',
    },
    /* The HTTP request headers passed in should be included in the actual request. This can prevent users from tampering with the HTTP request headers that are signed here. */
    Headers: {
      host: 'xxx', /* Specify the host for access. Error code 403 will be reported for access by a non-specified host. */
    },
  },
  function (err, data) {
    console.log(err || data.Url);
  }
);
```

### Upload request samples

Sample 1. Get a pre-signed URL for `Put Object` upload

[//]: # (.cssg-snippet-get-presign-upload-url)

```js
var request = require('request');
var fs = require('fs');
cos.getObjectUrl(
  {
    Bucket: 'examplebucket-1250000000', /* Your bucket name. Required. */
    Region: 'COS_REGION', /* Bucket region (required), such as ap-beijing */
    Key: 'Profile photo.jpg', /* Object key stored in the bucket (required), such as `1.jpg` and `a/b/test.txt`. */
    Method: 'PUT',
    Sign: true,
  },
  function (err, data) {
    if (err) return console.log(err);
    console.log(data.Url);
    var readStream = fs.createReadStream(__dirname + '/1.jpg');
    var req = request(
      {
        method: 'PUT',
        url: data.Url,
      },
      function (err, response, body) {
        console.log(err || body);
      }
    );
    readStream.pipe(req);
  }
);
```

### Parameter description

| Parameter | Description | Type | Required |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------- | -------- |
| Bucket  | Bucket name in the format of `BucketName-APPID`. The bucket name entered here must be in this format. | String | Yes |
| Region  | Bucket region. For the enumerated values, see [Regions and Access Endpoints](https://intl.cloud.tencent.com/document/product/436/6224). | String | Yes |
| Key | Object key (object name), which is the unique identifier of an object in a bucket. **If the request operation is to be performed on a file, this parameter will be required and should be a filename.** If the operation is on a bucket, this parameter should be left empty. | String | Yes |
| Sign | Whether to return a signed URL. Default value: `true`. | Boolean | No |
| Protocol    | Valid values: `http:` (default value), `https:`. | String | No |
| Domain    | Bucket access domain name. Default value: `{BucketName-APPID}.cos.{Region}.myqcloud.com`. | String | No |
| Method | HTTP request method such as `GET`, `POST`, `DELETE`, or `HEAD`. Default value: `GET`. | String | No |
| Query     | Request parameters to be included in the signature in the format of `{key: 'val'}`                                         | Object | No   |
| Headers   | Request headers to be included in the signature in the format of `{key: 'val'}`                                         | Object | No   |
| Expires | Signature expiration time in seconds. Default value: `900`. | Number | No |

### Returned value description

A string is returned in one of the two ways:

1. If the signature can be calculated synchronously (for example, the SecretId and SecretKey have been passed in during instantiation), the signed URL will be returned by default.
2. Otherwise, an unsigned URL will be returned.

### Callback function description

```
function(err, data) { ... }
```

| Parameter | Description | Type |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| err    | The object returned when an error (network error or service error) occurs. If the request is successful, this parameter is empty. For more information, see [Error Codes](https://intl.cloud.tencent.com/document/product/436/7730). | Object |
| data         | The object returned when the request is successful. If an error occurs with the request, this parameter is empty.               | Object |
| - Url | Calculated URL | String |

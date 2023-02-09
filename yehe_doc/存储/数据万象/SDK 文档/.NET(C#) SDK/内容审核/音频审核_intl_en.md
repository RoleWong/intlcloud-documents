## Overview
This document describes how to use the content moderation feature provided by [Cloud Infinite (CI)](https://www.tencentcloud.com/document/product/1045). CI fully integrates the processing capabilities with the COS SDK.

>?To use the content moderation service, you need to have the permission to use CI:
- For root accounts, click [here](https://console.cloud.tencent.com/cam/role/grant?roleName=CI_QCSRole&policyName=QcloudCOSDataFullControl,QcloudAccessForCIRole,QcloudPartAccessForCIRole&principal=eyJzZXJ2aWNlIjoiY2kucWNsb3VkLmNvbSJ9&serviceType=%E6%95%B0%E6%8D%AE%E4%B8%87%E8%B1%A1&s_url=https%3A%2F%2Fconsole.cloud.tencent.com%2Fci) for role authorization.
- For sub-accounts, see [Authorizing Sub-Accounts to Access CI Services](https://intl.cloud.tencent.com/document/product/1045/33450).

This document provides an overview of APIs and SDK code samples for audio moderation.

| API | Description |
| ----------------------------------| ---------------------------------- |
| [Submitting audio moderation job](https://intl.cloud.tencent.com/document/product/436/48262) | Submits an audio moderation job.   |
| [Querying audio moderation job result](https://intl.cloud.tencent.com/document/product/436/48263)  | Queries the result of a specified audio moderation job. |

## SDK API References

For the parameters and method description of all the APIs in the SDK, see [API Documentation](https://cos-dotnet-sdk-doc-1253960454.file.myqcloud.com/).

## Submitting an Audio Moderation Job

#### Feature description

This API is used to submit an audio moderation job. The audio moderation feature is async. You can submit a job to moderate your audio files, and then use the API for querying audio moderation job result to query the moderation results.

The following example shows how to submit an audio moderation job and then query the result by `JobId`.

>?
> - To perform this operation, the bucket should have CI features enabled.
> - Audio moderation APIs are supported starting from v5.4.24. To download the latest SDK, go to [Releases](https://github.com/tencentyun/qcloud-sdk-dotnet/releases) or see [Getting Started](https://intl.cloud.tencent.com/document/product/436/30594).
> - To view the version change log, go to [GitHub](https://github.com/tencentyun/qcloud-sdk-dotnet/blob/master/CHANGELOG.md).
> 


#### Sample code

[//]: #	".cssg-snippet-SubmitAudioCensorJobModel"

```cs
using COSXML.Model.CI;
using COSXML.Auth;
using System;
using System.Threading;
using COSXML;

namespace COSSnippet
{
    public class SubmitAudioCensorJobModel {

      private CosXml cosXml;

      SubmitAudioCensorJobModel() {
        CosXmlConfig config = new CosXmlConfig.Builder()
          .SetRegion("COS_REGION") // Set the default region. For abbreviations of COS regions, visit https://cloud.tencent.com/document/product/436/6224. 
          .Build();
        
        string secretId = "SECRET_ID";   // SecretId of the TencentCloud API. For more information about how to obtain the API key, see https://console.cloud.tencent.com/cam/capi.
        string secretKey = "SECRET_KEY"; // SecretKey of the TencentCloud API. For more information about how to obtain the API key, see https://console.cloud.tencent.com/cam/capi.
        long durationSecond = 600;          // Validity period of the request signature in seconds
        QCloudCredentialProvider qCloudCredentialProvider = new DefaultQCloudCredentialProvider(secretId, 
          secretKey, durationSecond);
        
        this.cosXml = new CosXmlServer(config, qCloudCredentialProvider);
      }

      /// Submit the audio moderation job
      public string SubmitAudioCensorJob()
      {
        // Bucket name in the format of `BucketName-APPID`. You can get APPID by referring to https://console.cloud.tencent.com/developer.
        string bucket = "examplebucket-1250000000"; // Note: To perform this operation, the bucket should have the content moderation feature enabled.
        SubmitAudioCensorJobRequest request = new SubmitAudioCensorJobRequest(bucket);
        request.SetCensorObject("audio.mp3"); // Object key of the media file, which should be replaced with that of the actual media file in the bucket.
        // The scene to be moderated, such as `Porn` (pornography) and `Ads` (advertising). You can pass in multiple types and separate them by comma, such as `Porn,Ads`.
        request.SetDetectType("Porn,Ads");
        // Execute the request
        SubmitCensorJobResult result = cosXml.SubmitAudioCensorJob(request);
        Console.WriteLine(result.GetResultInfo());
        Console.WriteLine(result.censorJobsResponse.JobsDetail.JobId);
        Console.WriteLine(result.censorJobsResponse.JobsDetail.State);
        Console.WriteLine(result.censorJobsResponse.JobsDetail.CreationTime);
        return result.censorJobsResponse.JobsDetail.JobId;
      }

      static void Main(string[] args)
      {
        SubmitAudioCensorJobModel m = new SubmitAudioCensorJobModel();
        /// Submit the moderation job
        string JobId = m.SubmitAudioCensorJob();
        /// Print the `JobId` that uniquely identifies this moderation job
        Console.WriteLine("JobId : " + JobId);
      }
    }
}
```

>? For more complete samples, visit [GitHub](https://github.com/tencentyun/cos-snippets/blob/master/dotnet/dist/SubmitAudioCensorJob.cs).
>

## Querying Audio Moderation Job Result

#### Feature description

This API is used to query the status and result of an audio moderation job.

#### Sample code

[//]: #	".cssg-snippet-SubmitAudioCensorJobModel"

```cs
using COSXML.Model.CI;
using COSXML.Auth;
using System;
using System.Threading;
using COSXML;

namespace COSSnippet
{
    public class SubmitAudioCensorJobModel {

      private CosXml cosXml;

      SubmitAudioCensorJobModel() {
        CosXmlConfig config = new CosXmlConfig.Builder()
          .SetRegion("COS_REGION") // Set the default region. For abbreviations of COS regions, visit https://cloud.tencent.com/document/product/436/6224. 
          .Build();
        
        string secretId = "SECRET_ID";   // SecretId of the TencentCloud API. For more information about how to obtain the API key, see https://console.cloud.tencent.com/cam/capi.
        string secretKey = "SECRET_KEY"; // SecretKey of the TencentCloud API. For more information about how to obtain the API key, see https://console.cloud.tencent.com/cam/capi.
        long durationSecond = 600;          // Validity period of the request signature in seconds
        QCloudCredentialProvider qCloudCredentialProvider = new DefaultQCloudCredentialProvider(secretId, 
          secretKey, durationSecond);
        
        this.cosXml = new CosXmlServer(config, qCloudCredentialProvider);
      }

      /// Query the audio moderation job result
      public void GetAudioCensorJobResult(string JobId)
      {
        // Bucket name in the format of `BucketName-APPID`. You can get APPID by referring to https://console.cloud.tencent.com/developer.
        string bucket = "examplebucket-1250000000"; // Note: To perform this operation, the bucket should have the content moderation feature enabled.
        GetAudioCensorJobRequest request = new GetAudioCensorJobRequest(bucket, JobId);
        // Execute the request
        GetAudioCensorJobResult result = cosXml.GetAudioCensorJob(request);
        Console.WriteLine(result.GetResultInfo());

        // Read the moderation result
        Console.WriteLine(result.resultStruct.JobsDetail.JobId);
        Console.WriteLine(result.resultStruct.JobsDetail.State);
        Console.WriteLine(result.resultStruct.JobsDetail.CreationTime);
        Console.WriteLine(result.resultStruct.JobsDetail.Object);
        Console.WriteLine(result.resultStruct.JobsDetail.Result);
        Console.WriteLine(result.resultStruct.JobsDetail.AudioText);

        Console.WriteLine(result.resultStruct.JobsDetail.PornInfo.HitFlag);
        Console.WriteLine(result.resultStruct.JobsDetail.PornInfo.Score);
        Console.WriteLine(result.resultStruct.JobsDetail.PornInfo.Label);

        Console.WriteLine(result.resultStruct.JobsDetail.AdsInfo.HitFlag);
        Console.WriteLine(result.resultStruct.JobsDetail.AdsInfo.Score);
        Console.WriteLine(result.resultStruct.JobsDetail.AdsInfo.Label);

        // Information of audio sections
        for(int i = 0; i < result.resultStruct.JobsDetail.Section.Count; i++)
        {
            Console.WriteLine(result.resultStruct.JobsDetail.Section[i].Url);
            Console.WriteLine(result.resultStruct.JobsDetail.Section[i].OffsetTime);
            Console.WriteLine(result.resultStruct.JobsDetail.Section[i].Duration);
            Console.WriteLine(result.resultStruct.JobsDetail.Section[i].Text);
            Console.WriteLine(result.resultStruct.JobsDetail.Section[i].PornInfo.HitFlag);
            Console.WriteLine(result.resultStruct.JobsDetail.Section[i].PornInfo.Score);

            Console.WriteLine(result.resultStruct.JobsDetail.Section[i].AdsInfo.HitFlag);
            Console.WriteLine(result.resultStruct.JobsDetail.Section[i].AdsInfo.Score);
        }
      }

      static void Main(string[] args)
      {
        SubmitAudioCensorJobModel m = new SubmitAudioCensorJobModel();
        /// Enter the `JobId` obtained when you submit the moderation job
        string JobId = "xxx";
        /// Query the moderation job result
        m.GetAudioCensorJobResult(JobId);
      }
    }
}
```

>? For more complete samples, visit [GitHub](https://github.com/tencentyun/cos-snippets/blob/master/dotnet/dist/SubmitAudioCensorJob.cs).
>


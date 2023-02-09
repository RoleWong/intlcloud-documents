## Overview
You can return the instances in the console based on your business needs.
- After a monthly subscribed instance is returned, it will be moved to the TencentDB recycle bin and retained there for seven days. During the retention period, the instance cannot be accessed, but it can be restored after renewal.
- After a pay-as-you-go instance is returned, it will be moved to the TencentDB recycle bin and retained there for 24 hours. During the retention period, the instance cannot be accessed, but it can be restored after renewal.

When an instance is returned and its status has changed to **Isolating**, it will no longer generate fees.
>
>- After the instance is terminated, its data cannot be recovered, and its backup files will also be terminated, so the data cannot be restored in the cloud. Store your backup files safely elsewhere in advance.
>- After an instance is terminated, its IP resources will be released simultaneously. If the instance has read-only instances or disaster recovery instances:
>  - Read-only instances will be terminated at the same time.
>  - Disaster recovery instances will disconnect their sync connections and be promoted to source instances automatically.


## Directions
1. Log in to the [TencentDB for MySQL Console](https://console.cloud.tencent.com/cdb), select the target instance in the instance list, and select **More** > **Terminate/Return** or **Terminate/Return & Refund** in the "Operation" column.
![](https://main.qcloudimg.com/raw/ea0d80564ad54c6c9e3ea43070c71f20.png)
2. In the pop-up window, read and click **I have read and agreed to Termination Rules** and click **Terminate Now**.

>
![](https://main.qcloudimg.com/raw/6e376a4a726195aefee841930577a5b3.png)


## Free Tier

The free tier and billing mode of SCF will be adjusted as from 0:00, June 1, 2022 (Beijing time). By then, new users will get free tiers of more usage within three months of activation. As from the fourth month, users will no longer be entitled to free tiers, and the system will automatically grant a basic package tier (500,000 invocations, resource usage of 100,000 GBs, and public network outbound traffic of 2 GB) and deduct the basic package fees of 1.86 USD every month. For more information, see [Free Tier](https://intl.cloud.tencent.com/document/product/583/12282).
>? 
>- The function invocations in a calendar month is counted on the first day of the next month. If there is no any function-related usage incurred, including the function resource usage, function invocations, and public network outbound traffic, the **basic package fees** will not be charged in the current month. If any function usage is generated in the current month, the basic package fees will be charged next month.
>- HTTP-triggered function response traffic is not included in the free tier. For more information, see [HTTP-Triggered Function Billing](https://intl.cloud.tencent.com/document/product/583/45902).


## Billable Items and Billing Modes
### Billing Mode
SCF can be [**postpaid (pay-as-you-go)**](https://intl.cloud.tencent.com/document/product/583/42969) or [**prepaid (subscription package)**](https://www.tencentcloud.com/document/product/583/52230).

### Billable items
SCF has prepaid and postpaid (pay-as-you-go) billable items. Each part is billed according to its statistics and calculation method, and the fees are accurate to two decimal places in **USD**:
- Postpaid billable items include resource usage, invocation, public network outbound traffic, idle provisioned concurrency, basic package, and HTTP-triggered function response traffic.
- Prepaid billable items include subscription package (the deduction sequence is **free tier > namespace subscription package > region subscription package > pay-as-you-go (postpaid)**).

For more information, see [**Billable Items**](https://intl.cloud.tencent.com/document/product/583/44254).

## Billing of Other Products Used by SCF

>! SCF execution logs are supported by and delivered to CLS by default. For more information on log delivery, see [Log Delivery Configuration](https://intl.cloud.tencent.com/document/product/583/39778). Starting from Sept 5, 2022, CLS provides a [free tier](https://intl.cloud.tencent.com/document/product/614/37889) for new CLS users, and any excess will be billed according to its pricing.

Other products used by SCF, such as [CFS](https://intl.cloud.tencent.com/document/product/582/9553), [COS](https://intl.cloud.tencent.com/document/product/436/16871), and [CLS](https://intl.cloud.tencent.com/document/product/614/11254), will be billed according to their respective billing rules.

## Pricing

The five billable items of SCF are priced as follows:

- Resource usage: 0.0000167 USD/GBs (0.167 USD/10000 GBs)
- Invocation: 0.002 USD/10,000 invocations
- Public network outbound traffic**: 0.12 USD/GB for the Chinese mainland; variable by region
- Idle provisioned concurrency: 0.00000847 USD/GBs (0.0847 USD/10000 GBs). For more information, see [Billing Details](https://intl.cloud.tencent.com/document/product/583/42969#.E9.A2.84.E7.BD.AE.E5.B9.B6.E5.8F.91.E9.97.B2.E7.BD.AE.E8.B4.B9.E7.94.A8) and [Billing Example](https://intl.cloud.tencent.com/document/product/583/12285).
- Basic package fee: 0.06 USD/day. For example, the fee for May is 0.06 x 31 = 1.86 USD. For more information, see [Billing Example](https://intl.cloud.tencent.com/document/product/583/12285).
HTTP-triggered functions and event-triggered functions have the same prices. For HTTP-triggered functions using the default trigger, HTTP-triggered function response traffic will be additionally generated. For more information, see [HTTP-Triggered Function Billing](https://intl.cloud.tencent.com/document/product/583/45902).

## Supported Regions
SCF is currently supported in the following regions:
<table>
<tr>
<th width="50%">Region</th><th width="50%">Value</th>
</tr>
  <tr>
    <td>Southeast Asia (Bangkok)</td>
    <td>ap-bangkok</td>
  </tr>
  <tr>
    <td>North China (Beijing)</td>
    <td>ap-beijing</td>
  </tr>
  <tr>
    <td>Southwest China (Chengdu)</td>
    <td>ap-chengdu</td>
  </tr>
  <tr>
    <td>South China (Guangzhou)</td>
    <td>ap-guangzhou</td>
  </tr>
  <tr>
    <td>Hong Kong/Macao/Taiwan (China) (Hong Kong)</td>
    <td>ap-hongkong</td>
  </tr>
  <tr>
    <td>South Asia (Mumbai)</td>
    <td>ap-mumbai</td>
  </tr>
  <tr>
    <td>Northeast Asia (Seoul)</td>
    <td>ap-seoul</td>
  </tr>
  <tr>
    <td>East China (Shanghai)</td>
    <td>ap-shanghai</td>
  </tr>
  <tr>
    <td>Southeast Asia (Singapore)</td>
    <td>ap-singapore</td>
  </tr>
  <tr>
    <td>Northeast Asia (Tokyo)</td>
    <td>ap-tokyo</td>
  </tr>
  <tr>
    <td>Europe (Frankfurt)</td>
    <td>eu-frankfurt</td>
  </tr>
  <tr>
    <td>Europe (Moscow)</td>
    <td>eu-moscow</td>
  </tr>
  <tr>
    <td>US East (Virginia)</td>
    <td>na-ashburn</td>
  </tr>
  <tr>
    <td>US West (Silicon Valley)</td>
    <td>na-siliconvalley</td>
  </tr>
  <tr>
    <td>North America (Toronto)</td>
    <td>na-toronto</td>
  </tr>
</table>



## Billing Details

For billing details, see the following documents:

<table>
<thead>
<tr>
<th width="50%">Document Name</th>
<th width="50%">Link</th>
</tr>
</thead>
<tbody><tr>
<td>Free Tier</td>
<td><a href="https://intl.cloud.tencent.com/document/product/583/12282" target="_blank">View document</a></td>
</tr>
<tr>
<td>Pricing</td>
<td><a href="https://intl.cloud.tencent.com/document/product/583/12281" target="_blank">View document</a></td>
</tr>
<tr>
<td>Overdue Payment</td>
<td><a href="https://intl.cloud.tencent.com/document/product/583/12283" target="_blank">View document</a></td>
</tr>
<tr>
<td>Billing Example</td>
<td><a href="https://intl.cloud.tencent.com/document/product/583/12285" target="_blank">View document</a></td>
</tr>
</tbody></table>



Tencent Cloud EdgeOne provides pay-as-you-go subscriptions. **The service is activated once you connect your site with EdgeOne and specify the subscription plan**. The subscription fee is charged at the end of the billing cycle.

## Billable Items

The bill of EdgeOne includes the fees of the subscription plan, out-of-plan usage, and value-added services.

- Subscription plan (required): Specify a plan for each site. The subscription plan includes the basic features, with the configured quota.
- Out-of-plan usage: When the quota of the subscription plan is used up, you need to pay for the exceeding parts additionally based on the corresponding prices.
- Value-added services (optional): EdgeOne provides many VASs, such as HTTP/3 (QUIC). These services are billed separately. You can activate them as needed.

## Billing Cycle

EdgeOne is billed on a calendar month basis. The bill of the current month is generated on the 1st day of the next calendar month. For example, a bill for January (2021-01-01 00:00:00 to 2021-01-31 23:59:59) is generated on February 1st.

## EdgeOne Plans

EdgeOne provides the Standard and Enterprise plans. You can only specify one plan for one site. The details of the two plans are listed as below:

|    Item                                  |                            Standard                            |                            Enterprise                            |
| :----------------------------------- | :----------------------------------------------------------: | :----------------------------------------------------------: |
| Price                                 |                         AZs in the Chinese mainland: 590 USD/month<br/>Global AZs (excluding those in the Chinese mainland): 699 USD/month<br/>Global AZs: 710 USD/month                         | Global AZs (excluding those in the Chinese mainland): 5,500 USD/month as the starting price. For more information, [contact us](https://intl.cloud.tencent.com/contact-us). |
| Traffic after protection (security acceleration traffic)                    |                             3 TB                             |               Customizable, starting at 1 TB                           |
| Number of HTTP/HTTPS requests after protection (security acceleration requests) |                           50 million                           |                         	Customizable, starting at 10 million                          |
| CDN |                              ✓                               |                              ✓                               |
| Smart acceleration                             |                              ✓                               |                              ✓                               |
| L4 proxy via CNAME                 |                              ✕                               |                              ✓                               |
| L4 proxy via static IP                |                              ✕                               |                              ✓                               |
| DDoS mitigation                            |                              ✓                               |                              ✓                               |
| Enterprise DDoS mitigation (customizable)    |                              ✕                               |                              ✓                               |
| Web protection - Managed rules                  |                              ✓                               |                              ✓                               |
| Web protection - Rate limiting rules              |                             5                              |                          Customizable                          |
| Web protection - Custom rules                |                             20                             |                          Customizable                          |
| Bot management                         |                             Optional                             |                             Optional                             |
| Bot intelligence           |                Free for a limited time                  |                 Free for a limited time                  |
| Verification (JavaScript challenge and CAPTCHA) |                              ✓                               |                              ✓                               |
| Upload size limit                         |               500 MB (cannot be disabled)               |                           Unlimited                           |
| External certificate on edge node             |                              ✓                               |                              ✓                               |
| Load balancing tasks                       |                             5                              |                             10                             |
| Node cache TTL                         |                      Down to the second level                     |                      Down to the second level                      |
| Browser cache TTL                       |                     Down to the second level                     |                      Down to the second level                      |
| Cache purge (free for a limited time)                 | Matching type: Host/Prefix<br/>Quota per request: 1,000<br/>Daily quota: 10,000 | Matching type: Host/Prefix<br/>Quota per request: 1,000<br/>Daily quota: 20,000 <br/><br/>Matching type: Cache-Tag<br/>Quota per request: 100 tags<br/>Daily quota: 30,000 tags |
| Prefetch URL (free for a limited time)                 |             Quota per request: 1,000<br>Daily quota: 50,000              |      Quota per request: 5,000<br>Daily quota: 100,000      |
| WebSockets                           |                     Default timeout: 1-120s                     |                     Default timeout: 1-300s                     |
| Max data query period                 |                            90 days                             |                            180 days                             |
| Real-time logging                             |                           2 tasks                            |                           5 tasks                            |
| Rule engine rules                       |                            100                             |                            200                             |
| DNS records                           |                             500                              |                             1,000                             |



>!
>- All features offered in Standard plan are included in the Enterprise plan.
>- If the capability of the Enterprise plan still cannot meet your needs, please [contact us](https://intl.cloud.tencent.com/contact-us).
>- If the usage of your plan serves for less than one month, you are billed based on **the actual plan duration and usage**. Providing you subscribed to a Standard plan on the 15th day of this month, which has 30 days, then your subscription fee is $349.5 (699 × 15/30), and your monthly traffic usage is 1.5 TB (3 TB × 15/30) and the number of requests is 25,000K (50,000K × 15/30).

## Out-of-Plan Usage
### Standard plan - Security acceleration traffic
If the security acceleration traffic exceeds the limit of your Standard plan, you are billed at the following tiered prices.

<table>
<thead>
<tr>
<th width="60%">Monthly Out-of-Plan Traffic</th>
<th width="40%">Price (USD/GB)</th>
</tr>
</thead>
<tbody><tr>
<td align="left">[0,7) TB</td>
<td align="left">0.1200</td>
</tr>
<tr>
<td align="left">[7,47) TB</td>
<td align="left">0.1180</td>
</tr>
<tr>
<td align="left">[47,97) TB</td>
<td align="left">0.1149</td>
</tr>
<tr>
<td align="left">[97,247) TB</td>
<td align="left">0.1106</td>
</tr>
<tr>
<td align="left">[247,747) TB</td>
<td align="left">0.1055</td>
</tr>
<tr>
<td align="left">[747,1497) TB</td>
<td align="left">0.0995</td>
</tr>
<tr>
<td align="left">[1497,1997) TB</td>
<td align="left">0.0929</td>
</tr>
<tr>
<td align="left">≥ 1997 TB</td>
<td align="left">0.0857</td>
</tr>
</tbody></table>

### Standard plan - Security acceleration requests
If the number of security acceleration requests exceeds the limit of your Standard plan, you are billed at the following tiered prices.

<table>
<thead>
<tr>
<th width="60%">Monthly Out-of-Plan Requests</th>
<th width="40%">Price (USD/10K requests)</th>
</tr>
</thead>
<tbody><tr>
<td align="left">[0,5000) × 10K</td>
<td align="left">0.0214</td>
</tr>
<tr>
<td align="left">[5000,45000) × 10K</td>
<td align="left">0.0180</td>
</tr>
<tr>
<td align="left">[45000,95000) × 10K</td>
<td align="left">0.0144</td>
</tr>
<tr>
<td align="left">[95000,495000) × 10K</td>
<td align="left">0.0109</td>
</tr>
<tr>
<td align="left">≥ 495000 × 10K</td>
<td align="left">0.0079</td>
</tr>
</tbody></table>

### Enterprise plan - Unit pricing
For the out-of-plan fee of Enterprise plans, please [contact us](https://intl.cloud.tencent .com/contact-us). 

## Value-added Service Pricing
EdgeOne provides additional value-added services for enhanced capabilities.

### HTTP/3 (QUIC) requests
QUIC requests adopt monthly linear pricing model. The details are as below:

<table>
<thead>
<tr>
<th width="30%">Value-added Service</th>
<th width="30%">Billing Mode</th>
<th width="40%">Price (USD/10K requests)</th>
</tr>
</thead>
<tbody><tr>
<td align="left">QUIC requests</td>
<td align="left">Billed by the number of QUIC requests</td>
<td align="left">0.008</td>
</tr>
</tbody></table>

### Real-time log push

QUIC requests adopt monthly linear pricing model. The details are as below:

<table>
<thead>
<tr>
<th width="30%">Value-added Service</th>
<th width="30%">Billing Mode</th>
<th width="40%">Price (USD/1M logs)</th>
</tr>
</thead>
<tbody><tr>
<td align="left">Real-time log push</td>
<td align="left">Billed by the number of logs pushed in real time</td>
<td align="left">0.01</td>
</tr>
</tbody></table>

### Bot management
Fees are charged for the selected value-added features and excessive requests.

The service price varies based on the type of the selected value-added feature. The details are as below:

<table>
<thead>
<tr>
<th width="50%">Value-added Service</th>
<th width="30%">Billing Mode</th>
<th width="20%">Price (USD)</th>
</tr>
</thead>
<tbody><tr>
<td align="left">Bot management for Standard plan<br>50,000K requests/month</td>
<td align="left">Monthly linear pricing</td>
<td align="left">1,250 USD/month</td>
</tr>
<tr>
<td align="left">Bot management for Enterprise plan<br>The number of requests depends on the quota of your plan.</td>
<td align="left">Monthly linear pricing</td>
<td align="left"><a href="https://intl.cloud.tencent.com/contact-us">Contact us</a></td>
</tr>
</tbody></table>

The out-of-plan usage is charged on a tiered basis. View the table below:

<table>
<thead>
<tr>
<th width="60%">Requests</th>
<th width="40%">Price (USD/10K requests)</th>
</tr>
</thead>
<tbody><tr>
<td align="left">[0,4000) × 10K</td>
<td align="center">0.16</td>
</tr>
<tr>
<td align="left">[4000,8000) × 10K</td>
<td align="center">0.13</td>
</tr>
<tr>
<td align="left">[8000,12000) × 10K</td>
<td align="center">0.11</td>
</tr>
<tr>
<td align="left">[12000,32000) × 10K</td>
<td align="center">0.07</td>
</tr>
<tr>
<td align="left">[32000,92000) × 10K</td>
<td align="center">0.06</td>
</tr>
<tr>
<td align="left">[92000,192000) × 10K</td>
<td align="center">0.04</td>
</tr>
<tr>
<td align="left">≥ 192000 × 10K</td>
<td align="center">0.02</td>
</tr>
</tbody></table>

### Web protection rule pack
Web protection rule packs are billed using fixed monthly pricing.

<table>
<thead>
<tr>
<th width="50%">Value-added Service</th>
<th width="30%">Billing Mode</th>
<th width="20%">Price (USD)</th>
</tr>
</thead>
<tbody><tr>
<td align="left">Web protection rule pack (20 custom rules and 5 rate limiting rules)</td>
<td align="left">Monthly linear pricing</td>
<td align="left"><a href="https://intl.cloud.tencent.com/contact-us">Contact us</a></td>
</tr>
</tbody></table>

### L4 proxy instance

L4 proxy is also billed using fixed monthly pricing.

<table>
<thead>
<tr>
<th width="50%">Value-added Service</th>
<th width="30%">Billing Mode</th>
<th width="20%">Price (USD)</th>
</tr>
</thead>
<tbody><tr>
<td align="left">Static IP-based L4 proxy</td>
<td align="left">Monthly linear pricing</td>
<td align="left"><a href="https://intl.cloud.tencent.com/contact-us">Contact us</a></td>
</tr>
<tr>
<td align="left">CNAME-based L4 proxy</td>
<td align="left">Monthly linear pricing</td>
<td align="left"><a href="https://intl.cloud.tencent.com/contact-us">Contact us</a></td>
</tr>
</tbody></table>

### Smart acceleration
All SCDN subdomain names use smart acceleration to optimize the access experience of your users. After smart acceleration is enabled, the client traffic and upstream and downstream traffic of EdgeOne nodes (client ⇋ Tencent Cloud EdgeOne node server) will be billed.

### Media processing
#### Image resize
EdgeOne can resize and convert images to specified formats. This way, EdgeOne nodes can directly process images, respond to client requests with processed images, and cache the processed images.
The image resize feature is billed based on the number of image resize requests. The feature is available free of charge for a limited period of time. An announcement will be released when the free trial ends.

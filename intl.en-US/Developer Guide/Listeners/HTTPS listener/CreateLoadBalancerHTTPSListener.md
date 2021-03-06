# CreateLoadBalancerHTTPSListener {#slb_api_CreateLoadBalancerHTTPSListener .reference}

Create an HTTPS listener.

**Note:** The status of the newly created listener is stopped. After the listener is created, call the StartLoadBalancerListener API to start the listener to distribute traffic.

## Request parameters {#section_bqw_c1g_cz .section}

|Name|Type|Required|Description|
|:---|:---|:-------|:----------|
|Action|String| Yes|The action to perform. Valid value: CreateLoadBalancerHTTPSListener

|
|RegionId|String| Yes|The region of the SLB instance.You can obtain the region ID by calling the DescribeRegions API.

|
|LoadBalancerId|String| Yes|The ID of the SLB instance for which the UDP listener is created.|
|ListenerPort|Integer| Yes|The frontend port of the listener that is used to receive incoming traffic and distribute the traffic to the backend servers.\[1, 65535\]

|
|BackendServerPort|Integer| Yes|The port opened on the backend server to receive requests. Valid value:1-65535

**Note:** If the VServerGroupId parameter is not specified, this parameter is required.

|
|VServerGroupId|String| No|The ID of the VServer group to add to the listener.|
|ServerCertificateId|String | Yes|The ID of the server certificate.|
|CACertificateId|String| No|The ID of the CA certificate.If both the CA certificate and the server certificate are uploaded, mutual authentication is adopted; if only the server certificate is uploaded, one-way authentication is adopted.

|
|Bandwidth|Interger| Yes|The peak bandwidth of the listener. Valid value:-   -1: The peak bandwidth is not limited.
-   \[1-5000\]: The peak bandwidth of the listener. The total peak bandwidth of all listeners cannot exceed the peak bandwidth of the instance.

|
|Scheduler|String| No|The algorithm used to distribute traffic. Valid value:-   wrr \(default\): Backend servers with higher weights receive more requests than those with smaller weights.
-   wlc: A server with a higher weight will receive a larger percentage of live connections at any one time. If the weights are the same, the system directs network connections to the server with the fewest established connections.
-   rr: Requests are evenly and sequentially distributed to the backend servers.

|
|StickySession|String| Yes|Whether to enable session persistence. Valid value:on | off

|
|StickySessionType|String| No|The method used to handle the cookie. Valid value:-   insert: Insert the cookie.

Server Load Balancer adds a session cookie to the first response from the backend server and identifies the server which has sent the response. The next request will contain the cookie and the listener will distribute the request to the same backend server.

-   server: You can set the cookie name in the response.

Server Load Balancer will overwrite the original cookie when it discovers that a new cookie is set. The next time the client carries the new cookie to access the Server Load Balancer, the listener will distribute the request to the previously recorded backend server.

**Note:** When the value of the StickySession parameter is set to on, this parameter is required.


|
|CookieTimeout|Integer| No|The cookie timeout in seconds. Valid value: \[1,86400\]

**Note:** This parameter is required when the value of the StickySession parameter is on, and the value of the StickySessionType parameter is insert.

|
|Cookie|String|No|The cookie configured on the backend server.The cookie must be 1-200 characters in length and can only contain ASCII characters and numbers. It cannot contain commas, semicolons or spaces, nor can it begin with $.

This parameter is required when the value of StickySession is on, and the value of StickySessionType is server.

|
|IdleTimeout|String| No|Specify the idle connection timeout in seconds. The valid value is 1-60s and the default value is 15s.If no request is received during the specified timeout period, Server Load Balancer will temporarily terminate the connection and restart the connection when the next request comes.

|
|RequestTimeout|String| No|Specify the request timeout in seconds. The valid value is 1-180s and the default value is 60s.If no response is received from the backend server during the specified timeout period, Server Load Balancer will stop waiting and send an HTTP 504 error to the client.

|
|AclStatus|String| No|Whether to enable access control.Valid value: on | off \(default\)

|
|AclType|String| No|Select an access control method after enabling the access control function:-   white: Only requests from IP addresses or CIDR blocks in the selected access control lists are forwarded. It applies to scenarios where the application only allows access from some specific IP addresses.

Enabling whitelist poses some business risks. After a whitelist is configured, only the IP addresses in the list can access the listener. If you enable the whitelist without adding any IP entry in the corresponding access control list, all requests are forwarded.

-   black: Requests from IP addresses or CIDR blocks in the selected access control lists are not forwarded. It applies to scenarios where the application only denies access from some specific IP addresses.

If you enable a blacklist without adding any IP entry in the corresponding access control list, all requests are forwarded.


This parameter is required when the value of the AclStatus parameter is set to on.

|
|AclId|String| No|Select an access control list as the whitelist or the blacklist. This parameter is required when the value of the AclStatus parameter is set to on.

|
|Description|String| No|Configure the description of the listener.|
|HealthCheck|String| Yes|Whether to enable health check. Valid value:on | off

|
|HealthCheckDomain|String| No|The domain name used for health check. Valid value:-   $\_ip: The private IP of the backend server. When the $\_ip is specified or the health check domain is not specified, Server Load Balancer uses private IPs of backend servers as the domain names for health check.
-   domain: The domain name can be 1-80 characters in length, and can only contain letters, numbers, periods, or hyphens \(-\).

|
|HealthCheckURI|String| No|The URI used for health check.|
|HealthCheckConnectPort|Integer| No|The port used for health check. Valid value:-   1-65535: The port opened on the backend server to do health check.

**Note:** If this parameter is not specified, the listener configuration is used by default.


|
|HealthyThreshold|Integer| No|The number of consecutive successes of health check performed by the same LVS mode server on the same ECS instance \(from failure to success\).Valid value: 2-10

|
|UnhealthyThreshold|Integer| No|The number of consecutive failures of health check performed by the same LVS node server on the same ECS instance \(from success to failure\).Valid value: 2-10

|
|HealthCheckTimeout|Integer| No|The amount of time to wait for the response from the health check. If an ECS instance sends no response within the specified timeout period, the health check fails.Valid value: 1-300 \(seconds\)

**Note:** If the value of the HealthCheckInterval parameter is greater than the value of the HealthCHeckTimeout parameter, the HealthCHeckTimeout parameter is invalid, and the timeout is set to the value of the HealthCheckInterval parameter.

|
|HealthCheckInterval|Integer| No|The time interval between two consecutive health checks.Valid value: 1-50 \(seconds\)

|
|HealthCheckHttpCode|String| No|The HTTP status code indicating that the health check is normal. Separate multiple HTTP status codes by commas.Valid value: http\_2xx \(Default\) | http\_3xx | http\_4xx | http\_5xx

|
|Gzip|String| No|Whether to enable the Gzip compression.Valid value: on \(default\) | off

|
|TLSCipherPolicy|String| No| You can only specify the TLSCipherPolicy parameter for guaranteed-performance instances. Each security policy includes the TLS protocol versions that can be selected by HTTPS and the supporting cipher suites.

 The following four security policies are supported currently. For more information, see [Differences along TLS security policies](#section_ulc_g3q_32b). Select the policy according to actual situation.

 -   tls\_cipher\_policy\_1\_0:
    -   Supported TLS versions: TLSv1.0, TLSv1.1, and TLSv1.2.
    -   Supported encryption algorithm suites: ECDHE-RSA-AES128-GCM-SHA256, ECDHE-RSA-AES256-GCM-SHA384, ECDHE-RSA-AES128-SHA256, ECDHE-RSA-AES256-SHA384, AES128-GCM-SHA256, AES256-GCM-SHA384, AES128-SHA256, AES256-SHA256, ECDHE-RSA-AES128-SHA, ECDHE-RSA-AES256-SHA, AES128-SHA, AES256-SHA and DES-CBC3-SHA.
-   tls\_cipher\_policy\_1\_1:
    -   Supported TLS versions: TLSv1.1 and TLSv1.2.
    -   Supported encryption algorithm suites: ECDHE-RSA-AES128-GCM-SHA256, ECDHE-RSA-AES256-GCM-SHA384, ECDHE-RSA-AES128-SHA256, ECDHE-RSA-AES256-SHA384, AES128-GCM-SHA256, AES256-GCM-SHA384, AES128-SHA256, AES256-SHA256, ECDHE-RSA-AES128-SHA, ECDHE-RSA-AES256-SHA, AES128-SHA, AES256-SHA and DES-CBC3-SHA.
-   tls\_cipher\_policy\_1\_2
    -   Supported TLS version: TLSv1.2.
    -   Supported encryption algorithm suites: ECDHE-RSA-AES128-GCM-SHA256, ECDHE-RSA-AES256-GCM-SHA384, ECDHE-RSA-AES128-SHA256, ECDHE-RSA-AES256-SHA384, AES128-GCM-SHA256, AES256-GCM-SHA384, AES128-SHA256, AES256-SHA256, ECDHE-RSA-AES128-SHA, ECDHE-RSA-AES256-SHA, AES128-SHA, AES256-SHA and DES-CBC3-SHA.
-   tls\_cipher\_policy\_1\_2\_strict
    -   Supported TLS version: TLSv1.2.
    -   Supported encryption algorithm suites: ECDHE-RSA-AES128-GCM-SHA256, ECDHE-RSA-AES256-GCM-SHA384, ECDHE-RSA-AES128-SHA256, ECDHE-RSA-AES256-SHA384, ECDHE-RSA-AES128-SHA and ECDHE-RSA-AES256-SHA.

 |
|EnableHttp2|String| No|Whether to enable the HTTP/2 feature.Valid value: on \(default\)| off

|

## Differences along TLS security policies {#section_ulc_g3q_32b .section}

|policy|tls\_cipher\_policy\_1\_0|tls\_cipher\_policy\_1\_1|tls\_cipher\_policy\_1\_2|tls\_cipher\_policy\_1\_2\_strict|
|------|-------------------------|-------------------------|-------------------------|---------------------------------|
|TLS| |1.2/1.1/1.0|1.2/1.1|1.2|1.2|
|CIPHER|ECDHE-RSA-AES128-GCM-SHA256|✔|✔|✔|✔|
|ECDHE-RSA-AES256-GCM-SHA384|✔|✔|✔|✔|
|ECDHE-RSA-AES128-SHA256|✔|✔|✔|✔|
|ECDHE-RSA-AES256-SHA384|✔|✔|✔|✔|
|AES128-GCM-SHA256|✔|✔|✔| |
|AES256-GCM-SHA384|✔|✔|✔| |
|AES128-SHA256|✔|✔|✔| |
|AES256-SHA256|✔|✔|✔| |
|ECDHE-RSA-AES128-SHA|✔|✔|✔|✔|
|ECDHE-RSA-AES256-SHA|✔|✔|✔|✔|
|AES128-SHA|✔|✔|✔| |
|AES256-SHA|✔|✔|✔| |
|DES-CBC3-SHA|✔|✔|✔| |

## Response parameters {#section_ugs_f1g_cz .section}

|Name|Type|Description|
|:---|:---|:----------|
|RequestId|String |The ID of the request.|

## Examples {#section_ix5_h1g_cz .section}

**Request example**

``` {#public}
https://slb.aliyuncs.com/
&Action=CreateLoadBalancerHTTPSListener
&LoadBalancerId=lb-t4nj5vuz8ish9emfk1f20
&ListenerPort=443
&BackendServerPort=80
&ServerCertificateId=1231579085529123_15dbf6ff26f_1991415478_2054196746
&Bandwidth=-1
&HealthCheck=on
&HealthCheckDomain=$_ip
&HealthCheckURI=/test/index.html
&HealthCheckConnectPort=8080
&HealthyThreshold=4
&UnhealthyThreshold=4
&HealthCheckTimeout=3
&HealthCheckInterval=5
&VServerGroupId=rsp-cige6j5e7p
&TLSCipherPolicy=tls_cipher_policy_1_0
&CommonParameters
```

**Response example**

-   XML format

    ```
    <? xml version="1.0" encoding="UTF-8"? >
    <CreateLoadBalancerHTTPSListenerResponse>
    	<RequestId>CEF72CEB-54B6-4AE8-B225-F876FF7BA984</RequestId>
    </CreateLoadBalancerHTTPSListenerResponse>
    ```

-   JSON format

    ```
    {
      "RequestId": " CEF72CEB-54B6-4AE8-B225-F876FF7BA984"
    }
    ```



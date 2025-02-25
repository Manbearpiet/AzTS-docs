# CDN

**Resource Type:** Microsoft.Cdn/profiles

<!-- TOC -->

- [Azure_CDN_DP_Enable_Https](#azure_cdn_dp_enable_https)

<!-- /TOC -->
<br/>

___ 

## Azure_CDN_DP_Enable_Https 

### Display Name 
CDN endpoints must use HTTPS protocol while providing data to the client browser/machine or while fetching data from the origin server 

### Rationale 
Use of HTTPS ensures server/service authentication and protects data in transit from network layer man-in-the-middle, eavesdropping, session-hijacking attacks. 

### Control Spec 

> **Passed:** 
One of the following conditions is met:
>- CDN endpoints are configured with HTTPS protocol only or HTTP to HTTPs redirection rule.
>- No CDN endpoints are present in the CDN profile.
> 
> **Failed:** 
> CDN endpoints are not configured with HTTPS protocol only or HTTP to HTTPs redirection rule.

> 
### Recommendation 

- **Azure Portal** 
    Enable only HTTPs protocol for endpoints, to enable HTTPS protocol: Go to Azure Portal --> your CDN Profile --> your CDN Endpoint --> Origin --> Select HTTPS --> Save. 
	Note: In the interest of user experience, enable both HTTP and HTTPS protocol along with HTTP to HTTPS redirection rule configured in rules engine for all endpoints. To enable HTTP and HTTPS protocol: Go to Azure Portal --> your CDN Profile --> your CDN Endpoint --> Origin --> Select HTTPS and HTTP --> Save. Refer: https://docs.microsoft.com/en-us/azure/cdn/cdn-standard-rules-engine to configure HTTP to HTTPs redirection rule in rules engine. 

- **PowerShell** 

	 Enable only HTTPs protocol for endpoints using PowerShell: 
	 ```powershell
	$ce= Get-AzCdnEndpoint -EndpointName <EndpointName> -ProfileName <CDNprofile> -ResourceGroupName <RGName>;
	$ce.IsHttpAllowed =$false; 
	Set-AzCdnEndpoint -CdnEndpoint $ce
	```
	 Note: In the interest of user experience, enable both HTTP and HTTPS protocol along with HTTP to HTTPS redirection rule configured in rules engine for all endpoints. To enable HTTP and HTTPS protocol using PowerShell:
	 ```powershell
	$ce= Get-AzCdnEndpoint -EndpointName <EndpointName> -ProfileName <CDNprofile> -ResourceGroupName <RGName>;
	$ce.IsHttpAllowed =$true; 
	$ce.IsHttpsAllowed =$true; 
	Set-AzCdnEndpoint -CdnEndpoint $ce 
	```
	Refer: https://docs.microsoft.com/en-us/azure/cdn/cdn-standard-rules-engine to configure HTTP to HTTPs redirection rule in rules engine. 


### Azure Policy or ARM API used for evaluation 

- ARM API used to list existing CDN endpoints at subscription level: <br />
/subscriptions/{subscriptionId}/resourceGroups/{resourcegroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints?api-version=2019-12-31<br />
**Properties:** 
properties.isHttpAllowed, properties.isHttpsAllowed, properties.deliveryPolicy.rules
 <br />


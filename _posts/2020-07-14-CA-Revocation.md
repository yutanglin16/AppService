---
title: 'CRITICAL: Certificate Authority revocation due to security vulnerability'
author_name: "Yutang"
tags: 
    - certificate
toc: true
toc_sticky: true
---

A security researcher recently reported a series of Certificate Authority (CA) compliance vulnerabilities effecting multiple CA vendors that are used by Microsoft, our customers, and the greater technology industry. To prevent impact from this issue, CA vendors have begun revoking the impacted CAs on 10 July 2020, and issuing patched versions requiring CAs to be re-issued. Microsoft is partnering closely with the vendors to ensure the CA patching does not impact our services however self-issued CAs or CAs used in “Bring Your Own Certificate” (BYOC) scenarios are still at risk of being unexpectedly revoked.   

On 07/11/2020, DigiCert has announced an inconsistency in one of their recent audits and they are retiring/revoking EV certificates issued by intermediate CAs as listed [here](https://knowledge.digicert.com/alerts/DigiCert-ICA-Replacement). 

And CA/Browser forum members have also listed a set of CAs who don’t comply to security standards and will be revoked. The list can be found [here](https://misissued.com/batch/138/). You can find more information on this [here](https://groups.google.com/forum/#!topic/mozilla.dev.security.policy/EzjIkNGfVEE%5B1-25%5D). 

If you are using an EV certificate issued by one of the revoked ICAs, your application’s availability might be interrupted and depending on your application, you may receive a variety of error messages including but not limited to:  

- Invalid certificate/revoked certificate 
- Connection timed out 
- HTTP 502 

## Avoiding Application Interruptions
To avoid any interruption to your application due to this issue, or to re-issue a CA which has been revoked, you need to take the following actions:  

1. Check https://knowledge.digicert.com/alerts/DigiCert-ICA-Replacement for more information on how to re-issue your certificates 
1. Once your certificate has been reissued, [add the new certificate to your web app](https://docs.microsoft.com/en-us/azure/app-service/configure-ssl-certificate) by either uploading a new certificate to your web app or by updating the new certificate in your Key Vault 
1. Once you have added your certificate to your web app, you will need to update your bindings [refer to the next section](#safely-updating-bindings). 

## How to safely update your bindings <a name="safely-updating-bindings"></a>

### Certificate Imported from Key Vault to App Service 
If you are importing your certificate from Key Vault, App Service has a background job that will automatically update your bindings with the new version of your certificate within 48 hours.  

If you’re using Key Vault and you would like to immediately update your bindings and not wait for the background job following these steps below: 

***NOTE: If you are using IP SSL bindings – do not *delete* your bindings as your inbound IP can change. It is recommended to wait for the platform’s background job to update the bindings with the new version of your certificate to avoid losing your IP address.***

1. Upload the new certificate in Key Vault using a new certificate name 
1. Import the new certificate to your web app 
1. [**Update your binding**](#updating-bindings)
1. Delete the old certificate from App Service 

### Certificate Uploaded to App Service 

If you are uploading a certificate to your app web, you will need to update the bindings with your new certificate following the steps below: 

***Note: If you are using IP SSL bindings – do not *delete* your bindings as your IP inbound IP can change.  Instead you must only *update* the IP SSL bindings.***

1. Upload the new certificate to your web app 
1. [**Update your binding**](#updating-bindings)
1. Delete the old certificate from App Service 

## Updating bindings <a name="updating-bindings"></a>

### Through Azure Portal
On Azure portal, go to **“TLS/SSL settings”** under **“Settings”** on the left navigation of your resource, select the binding you would like to update, and look for the new certificate from the dropdown. 

![Updating bindings]({{site.baseurl}}/media/2020/07/updating-bindings.jpg)

### Through Scripts 
- Refer to the documentation on [sample scripts for Azure CLI and Powershell](https://docs.microsoft.com/en-us/azure/app-service/configure-ssl-certificate#automate-with-scripts)
    - Note: You can run “New-AzWebAppSSLBinding” to add the new certificate to the existing hostname 
- Refer to the blog [to rotate you certificates with Key Vault using ARM](https://azure.github.io/AppService/2016/05/24/Deploying-Azure-Web-App-Certificate-through-Key-Vault.html#rotating-certificate)
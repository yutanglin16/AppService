---
title: "Parse Server on Azure App Service Updated"
author_name: Adrian Hall 
layout: post
hide_excerpt: true
---
      [Adrian Hall (MSFT)](https://social.msdn.microsoft.com/profile/Adrian Hall (MSFT))  1/18/2017 5:19:01 PM  The open-source [Parse Server project](https://github.com/ParsePlatform/parse-server) has moved on since we first published [the Marketplace resource ](https://azure.microsoft.com/en-us/marketplace/partners/microsoft/parseserver/)for running your own version of Parse Server on Azure App Service. The Azure version of Parse Server uses all Azure resources - DocumentDb, Storage, Notification Hubs and App Service. Recently, Parse Server [got updated to v2.3.0](https://github.com/ParsePlatform/parse-server/releases/tag/2.3.0). If you wish to deploy a new Parse Server and use the new v2.3.0 codebase, then you can simply deploy the Marketplace resource. In the Azure Portal, click on the **+ NEW** resource. Use the search box to search for Parse Server and create the **Parse Server on managed Azure services** resource. All the relevant resources will be created for you. This allows you to get up and running quickly. If you have an existing Parse Server running with managed Azure services, you will need to upgrade your server rather than deploy a new server. To do this:  - Click **Resource Groups** and find the resource group containing your parse server resources.
 - Click the **App Service** that has the same name as your parse server.
 - Click the **Application settings** node in the menu.
 - Add the following App Settings: WEBSITE\_NODE\_DEFAULT\_VERSION = 4.4.0 WEBSITE\_NPM\_DEFAULT\_VERSION = 3.8.3 You can set these values by scrolling to the **App settings** section and filling in the key/value fields appropriately, then press Enter.
 - Click **Console** to open a management console.
 - Type the following: npm install -s buffer-shims npm install -s parse-server@2.3.x 
 - Restart your **App Service**.
  This will install the latest 2.3.x release of parse-server (currently 2.3.2). If you notice any red text in the console, then an error has occurred. You should analyze the error message. Please report any problems through [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazurewebsitespreview&filter=alltypes&sort=lastpostdesc) or [Stack Overflow](http://stackoverflow.com/questions/tagged/parse-server). Upgrading your Parse Server is identical to upgrading any other server code - try it on a non-production service first and ensure all your client-server communications still work properly. Things change between Parse Server versions. You should reach out to the Parse Server project directly if you are experiencing problems in your code.     
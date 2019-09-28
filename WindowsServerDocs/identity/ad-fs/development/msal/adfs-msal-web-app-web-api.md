---
title: AD FS MSAL Web 应用（服务器应用）调用 web Api
description: 了解如何生成通过 AD FS 2019 进行身份验证的 web 应用登录用户。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 870dbb4303d216f05bc372610f3121ff08fc8c25
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407844"
---
# <a name="scenario-web-app-server-app-calling-web-api"></a>场景：Web 应用（服务器应用）调用 Web API 
>适用于：AD FS 2019 及更高版本 
 
了解如何生成通过 AD FS 2019 进行身份验证的 web 应用登录，并使用[MSAL 库](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)来调用 web api。  
 
阅读本文之前，应熟悉[AD FS 概念](../ad-fs-openid-connect-oauth-concepts.md)和[授权代码授予流](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>概述 
 
![Web 应用调用 web api 概述](media/adfs-msal-web-app-web-api/webapp1.png)

在此流中，你将向 Web 应用（服务器应用）添加身份验证，从而使用户登录并调用 Web API。 从 Web 应用调用 Web API，并使用 MSAL 的[AcquireTokenByAuthorizationCode](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identity.client.acquiretokenbyauthorizationcodeparameterbuilder?view=azure-dotnet)令牌采集方法。 你将使用授权代码流, 在令牌缓存中存储获取的令牌。 然后, 在需要时, 控制器将以无提示方式从缓存中获取令牌。 MSAL 根据需要刷新该令牌。 

用于调用 Web Api 的 web 应用： 


- 是机密客户端应用程序。 
- 这就是他们使用 AD FS 注册了机密（应用程序共享密钥、证书或 AD 帐户）的原因。 在对 AD FS 的调用期间，会传入此机密，以获取令牌。  

为了更好地了解如何在 ADFS 中注册 Web 应用并将其配置为获取令牌来调用 Web API，我们使用[此处](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)提供的示例并演练应用注册和代码配置步骤。  

 
## <a name="pre-requisites"></a>先决条件 

- GitHub 客户端工具 
- AD FS 2019 或更高版本配置并运行 
- Visual Studio 2013 或更高版本 
 
## <a name="app-registration-in-ad-fs"></a>AD FS 中的应用注册 
本部分说明如何将 Web 应用作为机密客户端和 Web API 注册为 AD FS 中的信赖方（RP）。 

  1. 在 AD FS 管理 "中，右键单击"**应用程序组**"，然后选择"**添加应用程序组**"。  
  2. 在应用程序组向导上，为 **"** 输入**WebAppToWebApi** "，在 "**客户端-服务器应用程序**" 下选择用于**访问 Web API 模板的服务器应用程序**。 单击“下一步”。  
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp2.png)
  
  3. 复制 "**客户端标识符**" 值。 稍后将**在应用程序的 web.config 文件**中将其用作**ida： ClientId**的值。 对于 "**重定向 URI** - "，请输入以下内容： https://localhost:44326 。 单击“添加”。 单击“下一步”。 
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp3.png)
  
  4. 在 "配置应用程序凭据" 屏幕上，选中 "**生成共享机密**并复制机密"。 稍后将在**应用程序的 web.config 文件**中将其用作**ida： ClientSecret**的值。 单击“下一步”。  
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp4.png)
  
  5. 在 "配置 Web API" 屏幕上，输入**标识符：** https://webapi 。 单击**添加**。 单击“下一步”。 稍后将在**应用程序的 web.config 文件**中将此值用于**ida： GraphResourceId** 。 
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp5.png)
  
  6. 在 "应用访问控制策略" 屏幕上，选择 "**允许**每个人" 并单击 "**下一步**" 
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp6.png)
  
  7. 在 "配置应用程序权限" 屏幕上，确保已选中 " **openid** and **user_impersonation** "，然后单击 "**下一步**"。 
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp7.png)
  
  8. 在 "摘要" 屏幕上，单击 "**下一步**"。 
  
  9. 在 "完成" 屏幕上，单击 "**关闭**"。



## <a name="code-configuration"></a>代码配置 

本部分介绍如何将 ASP.NET Web 应用配置为登录用户和检索令牌，以调用 Web API 

  1. 从[此处](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)下载示例   
  
  2. 使用 Visual Studio 打开示例 
  
  3. 打开 web.config 文件。 修改以下内容： 
       - ida： ClientId-在上 AD FS 部分的 "应用注册 #3 中输入**客户端标识符**值。 
       - ida： ClientSecret-在上面 AD FS 部分的 "应用注册" 中输入 #4 的**机密**值。 
       - ida： RedirectUri-在上述 AD FS 部分中，输入 "应用注册 #3 中的"**重定向 URI** "值。 
       - ida：颁发机构-输入 https：//[你的 AD FS 主机名]/adfs。 例如， https://adfs.contoso.com/adfs 
       - ida：资源-在上述 AD FS 部分的 "应用注册 #5 中输入**标识符**值。 
      
          ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp8.png)
 
 
### <a name="test-the-sample"></a>测试示例 
本部分介绍如何测试上面配置的示例。 

  1. 更改代码后，重新生成解决方案 
  
  2. 在 Visual Studio 顶部，请确保已选择 "Internet Explorer"，并单击绿色箭头。 
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp9.png)

  3. 在主页上，单击 "登录"。 
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp10.png)

  4. 你将被重定向到 AD FS 登录页。 继续登录。 
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp11.png)

  5. 登录后，单击 "访问令牌"。  
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp12.png)

  6. 单击 "访问令牌" 将通过调用 Web API 获取访问令牌信息。 
  
      ![添加应用程序组](media/adfs-msal-web-app-web-api/webapp13.png)
 
 ## <a name="next-steps"></a>后续步骤
[AD FS OpenID Connect/OAuth 流和应用程序方案](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 
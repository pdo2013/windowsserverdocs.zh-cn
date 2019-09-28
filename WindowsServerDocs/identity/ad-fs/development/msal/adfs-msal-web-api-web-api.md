---
title: AD FS MSAL Web API 调用 Web API （代表方案）
description: 了解如何构建调用另一个 Web API 的 Web API。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 106262b63b5aad0eddb08618eb808d2d9ff5b425
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407808"
---
# <a name="scenario-web-api-calling-web-api-on-behalf-of-scenario"></a>场景：用于调用 Web API 的 web API （代表方案） 
> 适用于：AD FS 2019 及更高版本 
 
了解如何生成代表用户调用另一个 Web API 的 Web API。  
 
阅读本文之前，应熟悉[AD FS 的概念](../ad-fs-openid-connect-oauth-concepts.md)和[Behalf_Of 流程](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#on-behalf-of-flow)

## <a name="overview"></a>概述 


- 客户端（Web 应用）-不在下图中表示-调用受保护的 Web API 并在其 "Authorization" Http 标头中提供 JWT 持有者令牌。 
- 受保护的 Web API 将验证令牌，并使用 MSAL [AcquireTokenOnBehalfOf](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync?view=azure-dotnet#Microsoft_IdentityModel_Clients_ActiveDirectory_AuthenticationContext_AcquireTokenAsync_System_String_Microsoft_IdentityModel_Clients_ActiveDirectory_ClientCredential_Microsoft_IdentityModel_Clients_ActiveDirectory_UserAssertion_) 方法请求（从 AD FS）其他令牌，使其自身可以代表用户调用另一个 Web api （名为下游 web api）。 
- 受保护的 web API 使用此令牌来调用下游 API。 它还可以调用 AcquireTokenSilentlater 来请求其他下游 Api （但仍代表同一个用户）的令牌。 AcquireTokenSilent 在需要时刷新该令牌。  
 
     ![概述](media/adfs-msal-web-api-web-api/webapi1.png)
 
为了更好地了解如何在 ADFS 中代表身份验证方案进行配置，我们使用[此处](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof)提供的示例并演练应用注册和代码配置步骤。  
 
## <a name="pre-requisites"></a>先决条件 

- GitHub 客户端工具 
- AD FS 2019 或更高版本配置并运行 
- Visual Studio 2013 或更高版本 
 
## <a name="app-registration-in-ad-fs"></a>AD FS 中的应用注册 

本部分说明如何在 AD FS 中将本机应用作为公共客户端和 Web Api 注册为依赖方（RP） 

  1. 在 AD FS 管理 "中，右键单击"**应用程序组**"，然后选择"**添加应用程序组**"。  
  
  2. 在应用程序组向导上，为 **"** 输入**WebApiToWebApi** "，在 "**客户端-服务器应用程序**" 下，选择**本机应用程序访问 Web API**模板。 单击“下一步”。

      ![应用注册](media/adfs-msal-web-api-web-api/webapi2.png)

  3. 复制 "**客户端标识符**" 值。 稍后会将其用作应用程序的**app.config**文件中的**ClientId**值。 对于 "**重定向 URI** - "，请输入以下内容： https://ToDoListClient 。 单击**添加**。 单击“下一步”。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi3.png)
  
  4. 在 "配置 Web API" 屏幕上，输入**标识符：** https://localhost:44321/ 。 单击**添加**。 单击“下一步”。 稍后将在应用程序的**app.config** **和 web.config**文件中使用此值。  
 
      ![应用注册](media/adfs-msal-web-api-web-api/webapi4.png)

  5. 在 "应用访问控制策略" 屏幕上，选择 "**允许每个人**" 并单击 "**下一步**" 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi5.png)  

  6. 在 "配置应用程序权限" 屏幕上，选择 " **openid** and **user_impersonation**"。 单击“下一步”。  
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi6.png)  

  7. 在 "摘要" 屏幕上，单击 "**下一步**"。 

  8. 在 "完成" 屏幕上，单击 "**关闭**"。 


  9. 在 AD FS 管理 "中，单击"**应用程序组**"，并选择" **WebApiToWebApi**应用程序组 "。 右键单击并选择 **“属性”** 。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi7.png)  

  10. 在 WebApiToWebApi 属性屏幕上，单击 "**添加应用程序 ...** "。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi8.png)

  11. 在 "独立应用程序" 下，选择 "**服务器应用程序**"。  
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi9.png)

  12. 在 "服务器应用程序" https://localhost:44321/ 屏幕上，添加作为**客户端标识符**和**重定向 URI**。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi10.png)

  13. 在 "配置应用程序凭据" 屏幕上，选择 "**生成共享机密**"。 复制该机密供以后使用。
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi11.png)

  14. 在 "摘要" 屏幕上，单击 "**下一步**"。 

  15. 在 "完成" 屏幕上，单击 "**关闭**"。 

  16. 在 AD FS 管理 "中，单击"**应用程序组**"，并选择" **WebApiToWebApi**应用程序组 "。 右键单击并选择 **“属性”** 。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi12.png)

  17. 在 WebApiToWebApi 属性屏幕上，单击 "**添加应用程序 ...** "。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi13.png)

  18. 在 "独立应用程序" 下，选择 " **WEB API**"。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi14.png)  

  19. 在 "配置 Web API" https://localhost:44300 上，添加作为**标识符**。  
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi15.png)

  20. 在 "应用访问控制策略" 屏幕上，选择 "**允许每个人**" 并单击 "**下一步**" 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi16.png)

  21. 在 "配置应用程序权限" 屏幕上，单击 "**下一步**"。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi17.png)

  22. 在 "摘要" 屏幕上，单击 "**下一步**"。

  23. 在 "完成" 屏幕上，单击 "**关闭**"。  

  24. 在 WebApiToWebApi 上单击 "确定" – Web API 2 属性屏幕  

  25. 在 WebApiToWebApi 的 "属性" 屏幕上，选择 " **WebApiToWebApi – WEB API** "，然后单击 "**编辑 ...** "。  
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi18.png)

  26. 在 "WebApiToWebApi – Web API 属性" 屏幕上，选择 "**颁发转换规则**" 选项卡，然后单击 "**添加规则 ...** "。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi19.png)

  27. 在 "添加转换声明规则向导" 上，选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi20.png)

  28. 在**声明规则名称：** 字段和**x： [] = > 问题（声明 = x）** 中输入 " **PassAllClaims** "; "自定义规则中的声明规则：" 字段，然后单击 "完成"。  
   
      ![应用注册](media/adfs-msal-web-api-web-api/webapi21.png)

  29. 单击 "WebApiToWebApi – Web API 属性" 屏幕上的 "确定"

  30. 在 WebApiToWebApi 的 "属性" 屏幕上，选择 "选择 WebApiToWebApi – Web API 2"，然后单击 "编辑 ..."</br> 
  ![应用注册](media/adfs-msal-web-api-web-api/webapi22.png)

  31. 在 "WebApiToWebApi – Web API 2 属性" 屏幕上，选择 "颁发转换规则" 选项卡，然后单击 "添加规则 ..." 

  32. 在 "添加转换声明规则向导" 上，选择 "使用来自 dopdown 的自定义规则![发送声明"，然后单击 "下一应用注册"](media/adfs-msal-web-api-web-api/webapi23.png)

  33. 在声明规则名称：字段和**x： [] = > 问题（声明 = x）** 中输入 "PassAllClaims"; "**自定义规则**中的声明规则：" 字段，然后单击 "**完成**"。  
   
      ![应用注册](media/adfs-msal-web-api-web-api/webapi24.png)

  34.  单击 "WebApiToWebApi – Web API 2 属性" 屏幕上的 "确定"，然后单击 "WebApiToWebApi 属性" 屏幕。  
 

## <a name="code-configuration"></a>代码配置 

本部分演示如何将 Web API 配置为调用另一个 Web API 

  1. 从[此处](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof)下载示例  
  
  2. 使用 Visual Studio 打开示例 
  
  3. 打开 App.config 文件。 修改以下内容： 
       - ida：颁发机构-输入 https：//[你的 AD FS 主机名]/adfs/
       - ida： ClientId-在上面 AD FS 部分的 "应用注册 #3 中输入值。 
       - ida： RedirectUri-在上述 AD FS 部分中输入应用注册 #3 的值。 
       - todo： TodoListResourceId –在上述 AD FS 部分的 "应用注册" 中 #4 输入标识符值 
       - ida： todo： TodoListBaseAddress-在上述 AD FS 部分的 "应用注册 #4 中输入标识符值。 
      
            ![应用注册](media/adfs-msal-web-api-web-api/webapi25.png)

  4. 在 ToDoListService 下打开 web.config 文件。 修改以下内容： 
       - ida：受众-在上 AD FS 部分的应用注册中输入客户端标识符值 #12
       - ida： ClientId-在上 AD FS 部分的 "应用注册 #12 中输入客户端标识符值。 
       - IdaClientSecret-在上述 AD FS 部分中输入从应用注册 #13 复制的共享机密。
       - ida： RedirectUri-在上面 AD FS 部分的 "应用注册 #12 中输入 RedirectUri 值。 
       - IdaAdfsMetadataEndpoint-输入 https：//[你的 AD FS 主机名]/federationmetadata/2007-06/federationmetadata.xml 
       - ida： OBOWebAPIBase-在上面 AD FS 部分的 "应用注册 #19 中输入标识符值。 
       - ida：颁发机构-输入 https：//[你的 AD FS 主机名]/adfs 
  
          ![应用注册](media/adfs-msal-web-api-web-api/webapi26.png) 

 5. 在 WebAPIOBO 下打开 web.config 文件。 修改以下内容： 
       - IdaAdfsMetadataEndpoint-输入 https：//[你的 AD FS 主机名]/federationmetadata/2007-06/federationmetadata.xml 
       - ida：受众-在上 AD FS 部分的应用注册中输入客户端标识符值 #12 
 
          ![应用注册](media/adfs-msal-web-api-web-api/webapi27.png)
 
## <a name="test-the-sample"></a>测试示例 

本部分介绍如何测试上面配置的示例。 

更改代码后，重新生成解决方案 
 
  1. 在 Visual Studio 上，右键单击 "解决方案"，然后选择 "**设置启动项目 ...** " 
      
      ![应用注册](media/adfs-msal-web-api-web-api/webapi28.png)

  2. 在 "属性页" 中，请确保将每个项目的 "**操作**" 设置为 "**启动**"，TodoListSPA 除外。  
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi29.png)
  
  3. 在 Visual Studio 顶部，单击绿色箭头。  
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi30.png)

  4. 在本机应用程序的主屏幕上，单击 "**登录**"。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi31.png)

     如果看不到 "本机应用" 屏幕，请在系统上的项目存储库保存到的文件夹中搜索和删除 * msalcache 文件。 
  
  5. 你将被重定向到 AD FS 登录页。 继续登录。 
  
      ![应用注册](media/adfs-msal-web-api-web-api/webapi32.png)

  6. 登录后，在 "创建待办事项"**项**中的 "Web api 调用" 中输入文本 web api。 单击 "**添加项**"。  这将调用 Web API （待办事项列表服务），然后调用 Web API 2 （WebAPIOBO）并在缓存中添加项。  
 
      ![应用注册](media/adfs-msal-web-api-web-api/webapi33.png)
 
 ## <a name="next-steps"></a>后续步骤
[AD FS OpenID Connect/OAuth 流和应用程序方案](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 
 

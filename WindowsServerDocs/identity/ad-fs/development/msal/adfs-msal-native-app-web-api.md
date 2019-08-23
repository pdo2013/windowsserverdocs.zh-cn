---
title: AD FS MSAL 本机应用调用 Web API
description: 了解如何生成通过 AD FS 2019 进行身份验证的本机应用登录, 并使用 MSAL 库来调用 web Api。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f1de6229d5360a4ea95d285f34ad32532762edca
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2019
ms.locfileid: "69983555"
---
# <a name="scenario-native-app-calling-web-api"></a>方案：本机应用调用 Web API 
>适用于：AD FS 2019 及更高版本 
 
了解如何生成通过 AD FS 2019 进行身份验证的本机应用登录, 并使用[MSAL 库](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)来调用 web api。  
 
阅读本文之前, 应熟悉[AD FS 概念](../ad-fs-openid-connect-oauth-concepts.md)和[授权代码授予流](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>概述 
 
 ![概述](media/adfs-msal-native-app-web-api/native1.png)

在此流中, 你将向本机应用 (公共客户端) 添加身份验证, 从而使用户登录并调用 Web API。 若要从登录用户的本机应用程序调用 Web API, 可以使用 MSAL 的[AcquireTokenInteractive](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identity.client.ipublicclientapplication.acquiretokeninteractive?view=azure-dotnet#Microsoft_Identity_Client_IPublicClientApplication_AcquireTokenInteractive_System_Collections_Generic_IEnumerable_System_String__)标记获取方法。 为了实现这种交互, MSAL 利用了 web 浏览器。 

 
为了更好地了解如何在 ADFS 中配置本机应用以交互方式获取访问令牌, 让我们使用[此处](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi)提供的示例并演练应用注册和代码配置步骤。  
 

## <a name="pre-requisites"></a>先决条件 


- GitHub 客户端工具 
- AD FS 2019 或更高版本配置并运行 
- Visual Studio 2013 或更高版本 
 

## <a name="app-registration-in-ad-fs"></a>AD FS 中的应用注册 
本部分说明如何在 AD FS 中将本机应用作为公共客户端和 Web API 注册为信赖方 (RP) 

  1. 在**AD FS 管理**"中, 右键单击"**应用程序组**", 然后选择"**添加应用程序组**"。   
  
  2. 在应用程序组向导上, 为 "输入**NativeAppToWebApi** ", 在 "**客户端-服务器应用程序**" 下, 选择**本机应用程序访问 Web API**模板。 单击“下一步”。  
  
      ![应用注册](media/adfs-msal-native-app-web-api/native2.png)  

  3. 复制 "**客户端标识符**" 值。 稍后会将其用作应用程序的**app.config**文件中的**ClientId**值。 对于 "**重定向 URI** https://ToDoListClient ", 请输入以下内容:。 单击**添加**。 单击“下一步”。  
 
     ![应用注册](media/adfs-msal-native-app-web-api/native3.png) 

  4. 在 "配置 Web API" 屏幕上, 输入**标识符:** https://localhost:44321/ 。 单击**添加**。 单击“下一步”。 稍后将在应用程序的**app.config**和 web.config 文件中使用此值 。
 
     ![应用注册](media/adfs-msal-native-app-web-api/native4.png)   
  
  5. 在 "应用访问控制策略" 屏幕上, 选择 "**允许每个人**" 并单击 "**下一步**" 
  
     ![应用注册](media/adfs-msal-native-app-web-api/native5.png)   
  
  6. 在 "配置应用程序权限" 屏幕上, 确保已选中 " **openid** " 并单击 "**下一步**"。  
     
     ![应用注册](media/adfs-msal-native-app-web-api/native6.png) 

  7. 在 "摘要" 屏幕上, 单击 "**下一步**"。
  
  8. 在 "完成" 屏幕上, 单击 "**关闭**"。 
  
  9. 在 AD FS 管理 "中, 单击"**应用程序组**", 并选择" **NativeAppToWebApi**应用程序组 "。 右键单击并选择 **“属性”** 。
  
      ![应用注册](media/adfs-msal-native-app-web-api/native7.png)

  10. 在 NativeAppToWebApi 的 "属性" 屏幕上, 选择 " **NATIVEAPPTOWEBAPI – web api** ", 然后单击 "**编辑 ...** " 
  
      ![应用注册](media/adfs-msal-native-app-web-api/native8.png) 

  11. 在 NativeAppToWebApi – Web API 属性屏幕上, 选择 "**颁发转换规则**" 选项卡, 然后单击 "**添加规则 ...** " 
  
      ![应用注册](media/adfs-msal-native-app-web-api/native9.png) 

  12. 在 "添加转换声明规则向导" 上, 从 "**声明规则模板**" 下拉列表选择 "**转换传入声明**", 然后单击 "**下一步**"。  
  
      ![应用注册](media/adfs-msal-native-app-web-api/native10.png) 

  13. 在 "**声明规则名称:** " 字段中输入**NameID** 。 为**传入声明类型**选择**名称**:、**传出声明类型**的**名称 ID**和**传出名称 ID 格式**的**公用名**:。 单击 "**完成**"。
  
      ![应用注册](media/adfs-msal-native-app-web-api/native11.png) 

  14. 单击 "NativeAppToWebApi – Web API 属性" 屏幕上的 "确定", 然后单击 "NativeAppToWebApi 属性" 屏幕。  
 
## <a name="code-configuration"></a>代码配置 
本部分演示如何将本机应用配置为登录用户和检索令牌, 以调用 Web API 

1. 从[此处](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi)下载示例 

2. 使用 Visual Studio 打开示例 

3. 打开 App.config 文件。 修改以下内容: 
   - ida: 颁发机构-输入 https://[你的 AD FS 主机名]/adfs
   - ida: ClientId-在上 AD FS 部分的 "应用注册 #3 中输入**客户端标识符**值。 
   - ida: RedirectUri-在上述 AD FS 部分中, 输入 "应用注册 #3 中的"**重定向 URI** "值。
   - todo: TodoListResourceId –在上述 AD FS 部分的 "应用注册" 中 #4 输入**标识符**值 
   - ida: todo: TodoListBaseAddress-在上述 AD FS 部分的 "应用注册 #4 中输入**标识符**值。 
 
     ![代码配置](media/adfs-msal-native-app-web-api/native12.png)

 4. 打开 web.config 文件。 修改以下内容: 
    - ida: 受众-在上述 AD FS 部分的 "应用注册 #4 中输入**标识符**值 
    - idaAdfsMetadataEndpoint-输入 https://[你的 AD FS 主机名]/federationmetadata/2007-06/federationmetadata.xml 
    
      ![代码配置](media/adfs-msal-native-app-web-api/native13.png)
 
  
## <a name="test-the-sample"></a>测试示例 
本部分介绍如何测试上面配置的示例。 

  1. 更改代码后, 重新生成解决方案 
 
  2. 在 Visual Studio 上, 右键单击 "解决方案", 然后选择 "**设置启动项目 ...** "  
 
     ![应用测试](media/adfs-msal-native-app-web-api/native14.png)

  3. 在 "属性页" 上, 确保为每个项目将 "**操作**" 设置为 "**启动**" 
      
     ![应用测试](media/adfs-msal-native-app-web-api/native15.png)

  4. 在 Visual Studio 顶部, 单击绿色箭头。  
 
     ![应用测试](media/adfs-msal-native-app-web-api/native16.png)

  5. 在本机应用程序的主屏幕上, 单击 "**登录**"。  
  
     ![应用测试](media/adfs-msal-native-app-web-api/native17.png)

    如果看不到 "本机应用" 屏幕, 请在系统上的项目存储库保存到的文件夹中搜索和删除 * msalcache 文件。 

  6. 你将被重定向到 AD FS 登录页。 继续登录。 
  
      ![应用测试](media/adfs-msal-native-app-web-api/native18.png)

  7. 登录后, 在 "创建待办事项"**项**中输入文本 "**生成本机应用到 Web Api** "。 单击 "**添加项**"。  这将调用**待办事项列表服务 (WEB API)** , 并在缓存中添加项。 
    
       ![应用测试](media/adfs-msal-native-app-web-api/native19.png)
 
## <a name="next-steps"></a>后续步骤
[AD FS OpenID Connect/OAuth 流和应用程序方案](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 
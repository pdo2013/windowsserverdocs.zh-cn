---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: "版本 web 应用程序使用 OpenID 连接广告 FS 2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f8040b19576ac9de4ced43e6313cad69276a3d27
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>版本 web 应用程序使用 OpenID 连接广告 FS 2016

>适用于：Windows Server 2016

在 Windows Server 2012 R2 的广告 FS 初始 Oauth 支持上生成，广告 FS 2016 引入了使用 OpenId 连接登录的支持。  
  
## <a name="pre-requisites"></a>先决条件  
下面列出的先决条件所需之前完成本文档列表。 本文档假定广告 FS 已安装并广告 FS 场已创建。  
  
-   （免费试用版是正常）Azure AD 订阅  
  
-   GitHub 客户端工具  
  
-   广告 FS 在 Windows Server 2016 TP4 或更高版本  
  
-   Visual Studio 2013 或更高版本。  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>创建 AD FS 2016 中的应用程序组  
以下部分介绍了如何在广告 FS 2016 配置应用程序的组。  
  
#### <a name="create-application-group"></a>创建应用程序组  
  
1.  在广告 FS 管理应用程序组右键单击，然后选择**添加的应用程序组**。  
  
2.  在应用程序组向导上的名称输入**ADFSSSO**下**独立应用**选择**服务器应用或网站**模板。  单击**下一步**。  
  
    ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  复制**客户端标识符**值。  这将用于稍后作为值 ida：客户机 Id 应用程序 web.config 文件中。  
  
4.  输入以下**重定向 URI:** - **https://localhost:44320/**。  单击**添加**。 单击**下一步**。  
  
    ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  在**配置应用程序凭据**屏幕上，将签入**生成共享的机密**并复制机密。 单击**接下来**  
  
    ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  在**摘要**屏幕上，单击**下一步**。  
  
7.  在**Complete**屏幕上，单击**关闭**。  
  
8.  现在，新的应用程序组右键单击，然后选择**属性**。  
  
9. 在**ADFSSSO 属性**单击**添加的应用程序**。  
  
10. 在**添加新的应用程序的示例应用程序**选择**Web API**单击**下一步**。  
  
    ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. 在**配置 Web API**屏幕上，输入的以下**标识符** - **https://contoso.com/WebApp**。  单击**添加**。 单击**下一步**。  
  
    ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
12. 在**选择访问控制策略**屏幕上，选择**允许任何人**单击**下一步**。  
  
    ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. 在**配置应用程序的权限**屏幕上，请确保**openid**选择，然后单击**下一步**。  
  
    ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. 在**摘要**屏幕上，单击**下一步**。  
  
15. 在**Complete**屏幕上，单击**关闭**。  
  
16. 在**示例应用程序属性**单击**确定**。  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>下载和修改 MVP 应用进行身份验证 OpenId 通过连接和广告 FS  
此部分中讨论了如何示例 Web API 下载，并在 Visual Studio 中对其进行修改。   我们将使用是 Azure AD 示例[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)。  
  
若要下载的示例项目、使用 Git 错误修复并键入以下命令：  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>若要修改的应用  
  
1.  打开使用 Visual Studio 的示例。  
  
2.  编译应用，使所有缺少 NuGets 还原。  
  
3.  打开 web.config 文件。  修改以下值，因此外观如下：  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  打开 Startup.Auth.cs 文件，做出以下更改：  
  
    -   查看以下发表评论：  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
    -   调整 OpenId 连接中间初始化逻辑使用以下更改：  
  
        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  
  
        ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  
  
    -   远向下、修改如下所示以下 OpenId 连接中间选项：  
  
        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                RedirectUri = postLogoutRedirectUri,  
                PostLogoutRedirectUri = postLogoutRedirectUri 
        ```  
  
        ![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  
  
        通过更改上述，我们会进行以下：  
  
        -   使用颁发机构通信受信任的发行商有关的数据，而我们指定直接通过 MetadataAddress 发现文档位置  
  
        -   Azure AD 不强制 redirect_uri 请求中, 存在但 ADFS。 因此，我们需要将其添加此处  
  
## <a name="verify-the-app-is-working"></a>验证工作的应用  
一旦上述进行了更改，按 F5。  这将弹出示例页面。  单击登录。  
  
![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  
  
你将重定向到广告 FS 登录页面。  请继续操作并登录。  
  
![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  
  
这是成功后，你应该看到立即登录。  
  
![广告 FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  
  
## <a name="next-steps"></a>后续步骤
[广告 FS 开发](../../ad-fs/AD-FS-Development.md)  


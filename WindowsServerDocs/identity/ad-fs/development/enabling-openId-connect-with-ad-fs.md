---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: 使用 OpenID Connect 和 AD FS 2016 及更高版本构建 web 应用程序
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9b3d64558c27e7b4bda20b6af27e02d55431c94d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358793"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>使用 OpenID Connect 和 AD FS 2016 及更高版本构建 web 应用程序

## <a name="pre-requisites"></a>先决条件  
下面列出了在完成本文档之前需要满足的先决条件。 本文档假定已安装 AD FS 并且已创建 AD FS 场。  

-   GitHub 客户端工具  

-   Windows Server 2016 TP4 或更高版本中的 AD FS  

-   Visual Studio 2013 或更高版本。  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>在 AD FS 2016 及更高版本中创建应用程序组
以下部分介绍如何配置 AD FS 2016 及更高版本中的应用程序组。  

#### <a name="create-application-group"></a>创建应用程序组  

1.  在 AD FS 管理 "中，右键单击" 应用程序组 "，然后选择"**添加应用程序组**"。  

2.  在应用程序组向导上，为 "输入**ADFSSSO** "，在 "**客户端-服务器应用程序**" 下，选择 " **web 浏览器访问 web 应用程序**" 模板。  单击“下一步”。

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  复制 "**客户端标识符**" 值。  稍后将在应用程序的 web.config 文件中将其用作 ida： ClientId 的值。  

4.  对于 "**重定向 URI** - "，请输入以下内容： **https://localhost:44320/** 。  单击**添加**。 单击“下一步”。  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  在 "**摘要**" 屏幕上，单击 "**下一步**"。  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  在 "**完成**" 屏幕上，单击 "**关闭**"。  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>下载并修改示例应用程序，通过 OpenID Connect 和 AD FS 进行身份验证  
本部分介绍如何在 Visual Studio 中下载示例 Web 应用并对其进行修改。   我们将使用[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)的 Azure AD 示例。  

若要下载示例项目，请使用 Git Bash，并键入以下内容：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>修改应用程序  

1.  使用 Visual Studio 打开示例。  

2.  重新生成应用，以便还原所有缺少的 Nuget。  

3.  打开 web.config 文件。  修改以下值，使其类似于以下内容：  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  打开 Startup.Auth.cs 文件并进行以下更改：  

    -   注释掉以下内容：  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   调整 OpenId Connect 中间件初始化逻辑，并进行以下更改：  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  

    -   再往下修改 OpenId Connect 中间件选项，如下所示：  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  

        通过更改以上步骤，我们将执行以下操作：  

        -   我们不是使用授权来传达有关受信任颁发者的数据，而是通过 MetadataAddress 直接指定发现文档位置  

        -   Azure AD 不会强制在请求中存在 redirect_uri，但 ADFS 会执行此项。 那么，我们需要在此添加  

## <a name="verify-the-app-is-working"></a>验证应用是否正在运行  
进行上述更改后，按 F5。  这将显示示例页。  单击 "登录"。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  

你将被重定向到 AD FS 登录页。  继续登录。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  

成功后，你应该会看到你现在已登录。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

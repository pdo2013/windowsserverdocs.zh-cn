---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: 构建 web 应用程序使用 OpenID Connect 与 AD FS 2016 和更高版本
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbd42941f8952fc7f649636d2f3645f941240d49
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190423"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>构建 web 应用程序使用 OpenID Connect 与 AD FS 2016 和更高版本

## <a name="pre-requisites"></a>先决条件  
以下是完成本文档之前所需的系统必备组件的列表。 本文档假定已安装 AD FS，并且已创建的 AD FS 场。  

-   GitHub 客户端工具  

-   在 Windows Server 2016 TP4 或更高版本的 AD FS  

-   Visual Studio 2013 或更高版本。  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>在 AD FS 中创建应用程序组 2016年及更高版本
以下部分介绍如何配置应用程序组中 AD FS 2016 和更高版本。  

#### <a name="create-application-group"></a>创建应用程序组  

1.  在 AD FS 管理中，右键单击应用程序组，然后选择**添加应用程序组**。  

2.  在应用程序组向导中，为名称输入**ADFSSSO**并在**客户端-服务器应用程序**选择**Web 浏览器访问 web 应用程序**模板。  单击“下一步”  。

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  复制**客户端标识符**值。  它将用作更高版本作为值 ida： 应用程序 web.config 文件中的 ClientId。  

4.  输入的以下**重定向 URI:**  -  **https://localhost:44320/** 。  单击**添加**。 单击“下一步”  。  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  上**摘要**屏幕上，单击**下一步**。  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  上**完成**屏幕上，单击**关闭**。  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>下载并修改示例应用程序通过 OpenID Connect 和 AD FS 进行身份验证  
本部分讨论如何下载示例 Web 应用并在 Visual Studio 中对其进行修改。   我们将使用 Azure AD 的示例，则[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)。  

若要下载示例项目，请使用 Git Bash，并键入以下命令：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>若要修改应用程序  

1.  打开使用 Visual Studio 的示例。  

2.  重新生成应用，以便所有缺少的 Nuget 还原。  

3.  打开 web.config 文件。  修改以下值，以使外观如下所示：  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  打开 Startup.Auth.cs 文件并进行以下更改：  

    -   注释掉以下：  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   调整 OpenId Connect 中间件的初始化逻辑发生了以下变化：  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  

    -   进一步向下，修改以下所示的 OpenId Connect 中间件的选项：  

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

        通过更改上述我们将执行以下操作：  

        -   而不是使用颁发机构进行通信的受信任的颁发者有关的数据，我们指定直接通过 MetadataAddress 的发现文档位置  

        -   Azure AD 不强制实施的 redirect_uri 在请求中存在但 ADFS。 因此，我们需要在此处进行添加  

## <a name="verify-the-app-is-working"></a>验证应用程序正常工作  
一旦进行了上述更改，按 F5。  此时会弹出示例页面。  单击登录。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  

你将重定向到 AD FS 登录页。  请继续并登录。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  

成功完成之后可以看到现在登录。  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

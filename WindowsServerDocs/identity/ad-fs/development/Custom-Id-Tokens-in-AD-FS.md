---
title: 使用 OpenID Connect 或 OAuth 与 AD FS 2016 或更高版本时，自定义要在 id_token 中发出的声明
description: AD FS 2016 或更高版本中自定义 id 令牌概念的技术概述
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 88ae6837872c5a6cf6bb1d8533a0aa14b82ca573
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358906"
---
# <a name="customize-claims-to-be-emitted-in-id_token-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>使用 OpenID Connect 或 OAuth 与 AD FS 2016 或更高版本时，自定义要在 id_token 中发出的声明

## <a name="overview"></a>概述
[本文介绍](native-client-with-ad-fs.md)如何生成使用 AD FS 进行 OpenID connect 登录的应用。 但是，在默认情况下，id_token 中只提供一组固定的声明。 AD FS 2016 及更高版本可在 OpenID Connect 方案中自定义 id_token。

## <a name="when-are-custom-id-token-used"></a>何时使用自定义 ID 令牌？
在某些情况下，客户端应用程序可能没有要尝试访问的资源。 因此，它并不真正需要访问令牌。 在这种情况下，客户端应用程序实质上只需要一个 ID 令牌，但有一些其他声明可帮助提供此功能。

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>在 ID 令牌中获取自定义声明有哪些限制？

### <a name="scenario-1"></a>方案 1

![有效性](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode 设置为 form_post
2.  只有公用客户端才能获取 ID 令牌中的自定义声明
3.  信赖方标识符（Web API 标识符）应与客户端标识符相同

### <a name="scenario-2"></a>方案2

![有效性](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

在 AD FS 服务器上安装[KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472)
1.  response_mode 设置为 form_post
2.  公共和机密客户端都可以在 ID 令牌中获取自定义声明
3.  将作用域 allatclaims 分配到客户端– RP 配对。
可以使用 ADFSApplicationPermission cmdlet 来分配作用域，如以下示例中所示：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>创建和配置 OAuth 应用程序以处理 ID 令牌中的自定义声明
按照以下步骤，在用于接收 ID 令牌的 AD FS 中创建和配置应用程序，并提供自定义声明。

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>在 AD FS 2016 或更高版本中创建和配置应用程序组

1. 在 AD FS 管理 "中，右键单击" 应用程序组 "，然后选择"**添加应用程序组**"。

2. 在应用程序组向导上，为 "输入**ADFSSSO** "，在 "客户端-服务器应用程序" 下，选择**本机应用程序访问 web 应用程序**模板。 单击“下一步”。

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. 复制 "**客户端标识符**" 值。  稍后将在应用程序的 web.config 文件中将其用作 ida： ClientId 的值。

4. 对于 "**重定向 URI** - "，请输入以下内容： **https://localhost:44320/** 。  单击**添加**。 单击“下一步”。

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. 在 "**配置 WEB API** " 屏幕上，输入以下**标识符作为标识符** - 。 **https://contoso.com/WebApp**  单击**添加**。 单击“下一步”。  稍后将在应用程序的 web.config 文件中将此值用于**ida： ResourceID** 。

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. 在 "**选择访问控制策略**" 屏幕上，选择 "**允许每个人**" 并单击 "**下一步**"

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. 在 "**配置应用程序权限**" 屏幕上，确保已选中 " **openid** and **allatclaims** "，然后单击 "**下一步**"。

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. 在 "**摘要**" 屏幕上，单击 "**下一步**"。  

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. 在 "**完成**" 屏幕上，单击 "**关闭**"。

10. 在 AD FS 管理 "中，单击" 应用程序组 "以获取所有应用程序组的列表。 右键单击**ADFSSSO** ，然后选择 "**属性**"。 选择**ADFSSSO-WEB API** ，然后单击 "**编辑 ...** "

    ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. 在**ADFSSSO-WEB API 属性**屏幕上，选择 "**颁发转换规则**" 选项卡，然后单击 "**添加规则 ...** "

    ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. 在 "**添加转换声明规则向导**" 屏幕上，从下拉菜单中选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"

    ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. 在 "**添加转换声明规则向导**" 屏幕上，在 "**声明规则名称**" 中输入**ForCustomIDToken** ，并在**自定义规则**中输入以下声明规则。 单击“完成”

    ```  
    x:[]
    => issue(claim=x);  
    ```

    ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.png)

```

>[!NOTE]
>You can also use PowerShell to assign the allatclaims and openid scopes
>``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-id_token"></a>下载并修改示例应用程序，以在 id_token 中发出自定义声明

本部分介绍如何在 Visual Studio 中下载示例 Web 应用并对其进行修改。   我们将使用[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)的 Azure AD 示例。  

若要下载示例项目，请使用 Git Bash，并键入以下内容：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>修改应用程序

1.  使用 Visual Studio 打开示例。  

2.  重新生成应用，以便还原所有缺少的 Nuget。  

3.  打开 web.config 文件。  修改以下值，使其类似于以下内容：  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 above under section Create and configure an Application Group in AD FS 2016 or later]" />  
    <add key="ida:ResourceID" value="[Replace this with the Web API Identifier from #5 above]"  />
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with the Redirect URI from #4 above]" />  
    ```  

    ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_2.PNG)  

4.  打开 Startup.Auth.cs 文件并进行以下更改：  

    -   调整 OpenId Connect 中间件初始化逻辑，并进行以下更改：  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   注释掉以下内容：  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   再往下修改 OpenId Connect 中间件选项，如下所示：  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                Resource = resourceId,
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_4.PNG)

5.  打开 HomeController.cs 文件并进行以下更改：  

    -   添加以下内容：  

            ```  
            using System.Security.Claims;  
            ```

    -   更新 About （）方法，如下所示：  

        ```  
        [Authorize]
        public ActionResult About()
        {
            ClaimsPrincipal cp = ClaimsPrincipal.Current;
            string userName = cp.FindFirst(ClaimTypes.WindowsAccountName).Value;
            ViewBag.Message = String.Format("Hello {0}!", userName);
            return View();
        }
        ```  

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_5.PNG)

### <a name="test-the-custom-claims-in-id-token"></a>在 ID 令牌中测试自定义声明

进行上述更改后，按 F5。 这将显示示例页。 单击 "登录"。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

你将被重定向到 AD FS 登录页。 继续登录。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

成功后，你应该会看到你现在已登录。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

单击 "关于链接"。 你将看到 Hello [Username]，它是从 ID 令牌中的用户名声明检索到的

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

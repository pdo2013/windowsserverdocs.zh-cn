---
title: 自定义声明为与 AD FS 2016 使用 OpenID Connect 或 OAuth 时的 id_token 中发出或更高版本
description: 在 AD FS 2016 或更高版本中的自定义 id 令牌概念的技术概述
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 04573aa13689a0e6744b01a0fbf8b11b622b2706
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445473"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>自定义声明为与 AD FS 2016 使用 OpenID Connect 或 OAuth 时的 id_token 中发出或更高版本

## <a name="overview"></a>概述
文章[此处](native-client-with-ad-fs.md)演示了如何生成使用的 OpenID Connect 登录 AD FS 的应用程序。 但是，默认情况下有仅一组固定的声明在 id_token 中提供。 AD FS 2016 和更高版本具有自定义 id_token OpenID Connect 方案中的功能。

## <a name="when-are-custom-id-token-used"></a>当为自定义 ID 使用令牌？
在某些情况下，很可能客户端应用程序不具有它正尝试访问的资源。 因此，它实际上并不需要访问令牌。 在这种情况下，客户端应用程序实质上是需要仅 ID 令牌，但具有某些其他声明，以帮助的功能。

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>获取令牌 ID 的自定义声明的限制有哪些？

### <a name="scenario-1"></a>方案 1

![限制](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode 设置为 form_post
2.  只有公共客户端可以自定义声明中获取 ID 令牌
3.  信赖方标识符 （Web API 标识符） 应相同客户端标识符

### <a name="scenario-2"></a>方案 2

![限制](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

与[KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) AD FS 服务器上安装
1.  response_mode 设置为 form_post
2.  公共和机密客户端可以自定义声明中获取 ID 令牌
3.  将作用域 allatclaims 分配给客户端 – RP 对。
可以通过使用 Grant ADFSApplicationPermission cmdlet，如下面的示例所示分配作用域：

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>创建和配置的 OAuth 应用程序来处理在 ID 令牌中的自定义声明
请按照以下步骤来创建和配置 AD FS 中的应用程序，用于接收 ID 令牌与自定义声明。

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>创建和配置 AD FS 2016 或更高版本中的应用程序组

1. 在 AD FS 管理中，右键单击应用程序组，然后选择**添加应用程序组**。

2. 在应用程序组向导中，为名称输入**ADFSSSO**应用程序在客户端-服务器下选择**本机应用程序访问 web 应用程序**模板。 单击“下一步”  。

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. 复制**客户端标识符**值。  它将用作更高版本作为值 ida： 应用程序 web.config 文件中的 ClientId。

4. 输入的以下**重定向 URI:**  -  **https://localhost:44320/** 。  单击**添加**。 单击“下一步”  。

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. 上**配置 Web API**屏幕中，输入以下**标识符** -  **https://contoso.com/WebApp** 。  单击**添加**。 单击“下一步”  。  此值将用于更高版本**ida: ResourceID**应用程序 web.config 文件中。

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. 上**选择访问控制策略**屏幕上，选择**授权所有人**然后单击**下一步**。

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. 上**配置应用程序权限**屏幕上，请确保**openid**并**allatclaims**未选中，单击**下一步**。

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. 上**摘要**屏幕上，单击**下一步**。  

   ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. 上**完成**屏幕上，单击**关闭**。

10. 在 AD FS 管理中，单击应用程序组，若要获取所有应用程序组的列表。 右键单击**ADFSSSO** ，然后选择**属性**。 选择**ADFSSSO-Web API**单击**编辑...**

    ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. 上**ADFSSSO-Web API 属性**屏幕上，选择**颁发转换规则**选项卡，单击**添加规则...**

    ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. 上**添加转换声明规则向导**屏幕上，选择**使用自定义规则发送声明**从下拉列表单击**下一步**

    ![客户端](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. 上**添加转换声明规则向导**屏幕中，输入**ForCustomIDToken**中**声明规则名称**以下声明中的规则和**自定义规则**. 单击“完成” 

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

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-idtoken"></a>下载并修改示例应用程序发出的 id_token 中的自定义声明

本部分讨论如何下载示例 Web 应用并在 Visual Studio 中对其进行修改。   我们将使用 Azure AD 的示例，则[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)。  

若要下载示例项目，请使用 Git Bash，并键入以下命令：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>若要修改应用程序

1.  打开使用 Visual Studio 的示例。  

2.  重新生成应用，以便所有缺少的 Nuget 还原。  

3.  打开 web.config 文件。  修改以下值，以使外观如下所示：  

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

    -   调整 OpenId Connect 中间件的初始化逻辑发生了以下变化：  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   注释掉以下：  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   进一步向下，修改以下所示的 OpenId Connect 中间件的选项：  

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

    -   更新 about （） 方法，如下所示：  

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

一旦进行了上述更改，按 F5。 此时会弹出示例页面。 单击登录。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

你将重定向到 AD FS 登录页。 请继续并登录。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

成功完成之后可以看到现在登录。

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

单击关于链接。 你将看到 Hello [Username] 检索从 ID 令牌中的用户名声明

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: "版本服务器端应用程序对广告 FS 2016 使用 OAuth 机密客户端"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>版本服务器端应用程序对广告 FS 2016 使用 OAuth 机密客户端

>适用于：Windows Server 2016

在 Windows Server 2012 R2 的广告 FS 初始 Oauth 支持上生成，广告 FS 2016 引入了支持维护自己的密码，如某个应用或服务正在运行的 web 服务器上的客户端的支持。  这些客户端称为机密的客户端。    
下面是示意图 web 应用程序的 web 服务器上运行，并且作为机密客户端到广告 FS:  
  
## <a name="pre-requisites"></a>先决条件  
下面列出的先决条件所需之前完成本文档列表。 本文档假定广告 FS 已安装并广告 FS 场已创建。  
  
-   （免费试用版是正常）Azure AD 订阅  
  
-   GitHub 客户端工具  
  
-   广告 FS 在 Windows Server 2016 TP4 或更高版本  
  
-   Visual Studio 2013 或更高版本。  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>创建 AD FS 2016 中的应用程序组  
以下部分介绍了如何在广告 FS 2016 配置应用程序的组。  
  
#### <a name="create-the-application-group"></a>创建应用程序组  
  
1.  在广告 FS 管理应用程序组右键单击，然后选择**添加的应用程序组**。  
  
2.  在应用程序组向导上的名称输入**ADFSOAUTHCC**下**独立应用**选择**服务器应用或网站**模板。  单击**下一步**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  复制**客户端标识符**值。  将为值稍后使用它**ida：客户机 Id**应用程序 web.config 文件中。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  输入以下**重定向 URI:** - **https://localhost:44323**。  单击**添加**。 单击**下一步**。  
  
5.  在**配置应用程序凭据**屏幕上，将签入**生成共享的机密**并复制机密。  将后面的值为使用此**ida: AppKey**应用程序 web.config 文件中。  单击**下一步**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  在**摘要**屏幕上，单击**下一步**。  
  
7.  在**Complete**屏幕上，单击**关闭**。  
  
8.  现在，新的应用程序组右键单击，然后选择**属性**。  
  
9. 在**ADFSOAUTHCC 属性**单击**添加的应用程序**。  
  
10. 在**添加新的应用程序的示例应用程序**选择**Web API**单击**下一步**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. 在**配置 Web API**屏幕上，输入的以下**标识符** - **https://contoso.com/WebApp**。  单击**添加**。 单击**下一步**。  此值将用于稍后**ida: GraphResourceId**应用程序 web.config 文件中。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. 在**选择访问控制策略**屏幕上，选择**允许任何人**单击**下一步**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. 在**配置应用程序的权限**屏幕上，请确保**user_impersonation**选择，然后单击**下一步**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. 在**摘要**屏幕上，单击**下一步**。  
  
15. 在**Complete**屏幕上，单击**关闭**。  
  
16. 在**ADFSOAUTHCC 属性**单击**确定**。  
  
## <a name="upgrade-the-database"></a>升级数据库  
在创建此演练使用 Visual Studio 2015。   以获取使用 Visual Studio 2015 示例你将需要更新数据库文件。  使用以下步骤来执行此操作。  
  
此部分中讨论了如何示例 API Web 下载和升级在 Visual Studio 2015 数据库。   我们将使用是 Azure AD 示例[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity)。  
  
若要下载的示例项目、使用 Git 错误修复并键入以下命令：  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>若要升级数据库文件  
  
1.  打开 Visual Studio 中的项目、将告知你该应用所需的 SQL Server 2102 Express 弹出窗口或你将需要升级数据库。  单击确定。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  下一步编译应用程序通过选择版本-> 版本解决方案，在顶部。  这将还原所有 NuGet 包。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  现在在顶部，选择**视图** -> **服务器资源管理器**。  后，打开下**数据连接**，右键单击**DefaultConnection**选择**修改连接**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  在**修改连接**、选择**高级**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  在高级属性，找到数据来源，并使用下拉列表将其从更改**(LocalDb\v11.0)**到**(LoaclDB) MSSQLLocalDB**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  单击确定。 单击确定。  单击是来升级数据库。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  当此操作完成，超过右侧，将值复制在框中旁边**连接字符串。**  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  现在，打开 Web.config 文件并替换设置为在使用值复制上方的连接。  保存 Web.config 文件。  
  
    > [!NOTE]  
    > 上述步骤，以便我们能够获取新的连接是必需的。  否则，我们将运行 Update-Database 以下时，它将出的错误。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. 在 Visual Studio 的顶部，选择**视图** -> **其他 Windows** -> **程序包管理器控制台**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. 在底部，程序包管理器控制台输入：`Enable-Migrations`输入的词和。  
  
    > [!NOTE]  
    > 如果你收到指示 Enable-Migrations 不会识别为 cmdlet 错误，则输入 Install-Package EntityFramework 更新 EntityFramework。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. 在底部，程序包管理器控制台输入：`Add-Migration <anynamehere>`输入的词和。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. 在底部，程序包管理器控制台输入：`Update-Database`输入的词和。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>修改 Visual Studio 中 WebApi  
  
#### <a name="to-modify-the-sample-web-api"></a>修改示例 Web API  
  
1.  打开使用 Visual Studio 的示例。  
  
2.  打开 web.config 文件。  修改以下值：  
  
    -   客户 ida：机 Id-输入上述 #3 中的值。  
  
    -   ida: AppKey-输入 #5 上述中的值。  
  
    -   ida: GraphResourceId-输入上述 #11 中的值。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  打开 Startup.Auth.cs 文件 App_Start 下的，做出以下更改：  
  
    -   查看以下行发表评论：  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   在它的位置添加以下项：  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        < your_fsname > 替换联合身份验证服务 url，例如 adfs.contoso.com DNS 部分的位置  
  
        ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  打开 UserProfileController.cs 文件，做出以下更改：  
  
    -   查看以下发表评论：  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   这两个发生替换以下内容：  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   查看以下发表评论：  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   这两个发生替换以下内容：  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   现在，所有实例以下评论：  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   所有发生都替换以下内容：  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>测试解决方案  
在此部分中，我们将测试机密的客户端解决方案。  使用以下步骤，来测试解决方案。  
  
#### <a name="testing-the-confidential-client-solution"></a>测试客户端的机密解决方案  
  
1.  在 Visual Studio 的顶部，确保选择 Internet Explorer，然后单击绿色箭头。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  一旦 ASP.Net 页面时，单击**注册**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  输入用户名和密码，然后单击**注册**。  这将 SQL 数据库中创建本地帐户。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  通知 ASP.NET 站点现在，显示 Hello bsimon！。  单击**个人资料**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  这将显示页面，不带任何信息，并显示，我们必须单击此处进行登录。  单击**此处**。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  您现在将提示来登录到广告 FS。  
  
    ![广告 FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>后续步骤
[广告 FS 开发](../../ad-fs/AD-FS-Development.md)  


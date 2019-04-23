---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: 生成 OAuth 机密客户端使用 AD FS 2016 的服务器端应用程序
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869578"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>生成 OAuth 机密客户端使用 AD FS 2016 的服务器端应用程序

>适用于：Windows Server 2016

基于 Windows Server 2012 R2 中的 AD FS 中的初始 Oauth 支持，AD FS 2016 引入了对能够维护其自己的机密，如应用或 web 服务器上运行的服务的客户端支持。  这些客户端称为机密客户端。    
下面是 web 服务器上运行并充当机密客户端到 AD FS 的 web 应用程序的示意图：  
  
## <a name="pre-requisites"></a>先决条件  
以下是完成本文档之前所需的系统必备组件的列表。 本文档假定已安装 AD FS，并且已创建的 AD FS 场。  
  
-   Azure AD 订阅 （免费试用版是没问题）  
  
-   GitHub 客户端工具  
  
-   在 Windows Server 2016 TP4 或更高版本的 AD FS  
  
-   Visual Studio 2013 或更高版本。  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>在 AD FS 2016 中创建应用程序组  
以下部分介绍如何在 AD FS 2016 中配置应用程序组。  
  
#### <a name="create-the-application-group"></a>创建应用程序组  
  
1.  在 AD FS 管理中，右键单击应用程序组，然后选择**添加应用程序组**。  
  
2.  在应用程序组向导中，为名称输入**ADFSOAUTHCC**并在**独立应用程序**选择**服务器应用程序或网站**模板。  单击“下一步” 。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  复制**客户端标识符**值。  它将在稍后的值作为**ida: ClientId**应用程序 web.config 文件中。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  输入的以下**重定向 URI:** - **https://localhost:44323**。  单击**添加**。 单击“下一步” 。  
  
5.  上**配置应用程序凭据**屏幕上，选中**生成一个共享的机密**和复制的密码。  这将在稍后的值作为**ida: AppKey**应用程序 web.config 文件中。  单击“下一步” 。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  上**摘要**屏幕上，单击**下一步**。  
  
7.  上**完成**屏幕上，单击**关闭**。  
  
8.  现在，右键单击新的应用程序组，然后选择**属性**。  
  
9. 上**ADFSOAUTHCC 属性**单击**添加应用程序**。  
  
10. 上**添加到示例应用程序的新应用程序**选择**Web API**然后单击**下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. 上**配置 Web API**屏幕中，输入以下**标识符** - **https://contoso.com/WebApp**。  单击**添加**。 单击“下一步” 。  此值将用于更高版本**ida: GraphResourceId**应用程序 web.config 文件中。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. 上**选择访问控制策略**屏幕上，选择**授权所有人**然后单击**下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. 上**配置应用程序权限**屏幕上，请确保**user_impersonation**处于选中状态，然后单击**下一步**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. 上**摘要**屏幕上，单击**下一步**。  
  
15. 上**完成**屏幕上，单击**关闭**。  
  
16. 上**ADFSOAUTHCC 属性**单击**确定**。  
  
## <a name="upgrade-the-database"></a>升级数据库  
创建本演练中使用的 visual Studio 2015。   为了获得使用 Visual Studio 2015 的示例需要更新数据库文件。  请使用下面的过程执行此操作。  
  
本部分讨论如何下载示例 Web API 和升级 Visual Studio 2015 中的数据库。   我们将使用 Azure AD 的示例，则[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity)。  
  
若要下载示例项目，请使用 Git Bash，并键入以下命令：  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>若要升级的数据库文件  
  
1.  在 Visual Studio 中打开项目、 将弹出窗口，指出应用程序需要 SQL Server 2102 Express 或你将需要升级数据库。  单击确定。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  下一步编译通过选择生成应用程序顶部-> 生成解决方案。  这将还原所有 NuGet 包。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  现在在顶部，选择**视图** -> **服务器资源管理器**。  打开后下,**数据连接**，右键单击**DefaultConnection** ，然后选择**修改连接**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  上**修改连接**，选择**高级**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  在高级属性中，找到数据源，并使用下拉列表从其进行更改 **(localdb \ V11.0)** 到 **(LoaclDB) MSSQLLocalDB**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  单击确定。 单击确定。  单击是将数据库升级。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  完成后，通过在右侧的复制值中的下一步**连接字符串。**  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  现在，打开 Web.config 文件并替换为前面复制的值的连接字符串中的值。  保存 Web.config 文件。  
  
    > [!NOTE]  
    > 上面的步骤是必需的以便我们可以获取的新的连接字符串。  否则，当我们运行以下更新数据库时它就会出错。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. 在 Visual Studio 的顶部，选择**视图** -> **其他 Windows** -> **程序包管理器控制台**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. 在底部，在包管理器控制台输入：`Enable-Migrations`并按 enter。  
  
    > [!NOTE]  
    > 如果收到错误，指出 Enable-migrations 未被识别为 cmdlet 中，输入 Install-package EntityFramework 更新 entity Framework。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. 在底部，在包管理器控制台输入：`Add-Migration <anynamehere>`并按 enter。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. 在底部，在包管理器控制台输入：`Update-Database`并按 enter。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>修改 Visual Studio 中 WebApi  
  
#### <a name="to-modify-the-sample-web-api"></a>若要修改示例 Web API  
  
1.  打开使用 Visual Studio 的示例。  
  
2.  打开 web.config 文件。  修改以下值：  
  
    -   ida: ClientId-输入上面的 #3 中的值。  
  
    -   ida: AppKey-输入上面的 #5 中的值。  
  
    -   ida: GraphResourceId-输入上面的 #11 中的值。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  打开 App_Start 下的 Startup.Auth.cs 文件并进行以下更改：  
  
    -   注释掉以下行：  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   在它的位置中添加以下代码：  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        其中 < your_fsname > 将替换联合身份验证服务 url，例如 adfs.contoso.com 的 DNS 部分  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  打开 UserProfileController.cs 文件并进行以下更改：  
  
    -   注释掉以下：  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   这两个匹配项替换为以下：  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   注释掉以下：  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   这两个匹配项替换为以下：  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   现在注释掉以下的所有实例：  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   使用以下内容替换所有匹配项：  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>测试解决方案  
在本部分中，我们将测试机密客户端解决方案。  使用以下过程来测试解决方案。  
  
#### <a name="testing-the-confidential-client-solution"></a>测试机密客户端解决方案  
  
1.  在 Visual Studio 的顶部，请确保选择 Internet Explorer 并单击绿色箭头。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  一旦 ASP.Net 页出现，则单击**注册**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  输入用户名和密码，然后单击**注册**。  这将在 SQL 数据库中创建本地帐户。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  请注意，ASP.NET 站点现在，显示 Hello 是 bsimon 作为用户名 ！。  单击**配置文件**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  这将显示不包含任何信息页面，并说，我们必须单击此处登录。  单击**此处**。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  您现在将提示登录到的 AD FS。  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  


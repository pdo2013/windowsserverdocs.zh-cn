---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: 使用 AD FS 2016 或更高版本的 OAuth 机密客户端生成服务器端应用程序
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5b2bf036de1de8300e36c3413c551e51d408a4d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407868"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016-or-later"></a>使用 AD FS 2016 或更高版本的 OAuth 机密客户端生成服务器端应用程序


AD FS 2016 及更高版本为能够维护自己的机密的客户端（例如，在 web 服务器上运行的应用程序或服务）提供支持。  这些客户端称为机密客户端。
下面是在 web 服务器上运行的 web 应用程序的示意图，作为 AD FS 的机密客户端：  

## <a name="pre-requisites"></a>先决条件  
下面列出了在完成本文档之前需要满足的先决条件。 本文档假定已安装 AD FS。  

-   GitHub 客户端工具  

-   Windows Server 2016 TP4 或更高版本中的 AD FS  

-   Visual Studio 2013 或更高版本。  

## <a name="create-an-application-group-in-ad-fs-2016-or-later"></a>在 AD FS 2016 或更高版本中创建应用程序组
以下部分介绍如何配置 AD FS 2016 或更高版本中的应用程序组。  

#### <a name="create-the-application-group"></a>创建应用程序组  

1.  在 AD FS 管理 "中，右键单击" 应用程序组 "，然后选择"**添加应用程序组**"。  

2.  在应用程序组向导上，为 **"** 输入**ADFSOAUTHCC** "，在 "**客户端-服务器应用程序**" 下选择用于**访问 Web API 模板的服务器应用程序**。  单击“下一步”。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  

3.  复制 "**客户端标识符**" 值。  稍后将在应用程序的 web.config 文件中将其用作**ida： ClientId**的值。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  

4.  对于 "**重定向 URI** - "，请输入以下内容： **https://localhost:44323** 。  单击**添加**。 单击“下一步”。  

5.  在 "**配置应用程序凭据**" 屏幕上，选中 "**生成共享机密**并复制机密"。  稍后将在应用程序的 web.config 文件中将其用作**ida： ClientSecret**的值。  单击“下一步”。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)   

6. 在 "**配置 WEB API** " 屏幕上，输入以下**标识符作为标识符** - 。 **https://contoso.com/WebApp**  单击**添加**。 单击“下一步”。  稍后将在应用程序的 web.config 文件中将此值用于**ida： GraphResourceId** 。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  

7. 在 "**应用访问控制策略**" 屏幕上，选择 "**允许每个人**" 并单击 "**下一步**"  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  

8. 在 "**配置应用程序权限**" 屏幕上，确保已选中 " **openid** and **user_impersonation** "，然后单击 "**下一步**"。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  

9. 在 "**摘要**" 屏幕上，单击 "**下一步**"。  

10. 在 "**完成**" 屏幕上，单击 "**关闭**"。  

## <a name="upgrade-the-database"></a>升级数据库  
此演练中使用了 Visual Studio 2015。   为了使示例使用 Visual Studio 2015，需要更新数据库文件。  请使用下面的过程执行此操作。  

本部分介绍如何在 Visual Studio 2015 中下载示例 Web API 并升级数据库。   我们将使用[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity)的 Azure AD 示例。  

若要下载示例项目，请使用 Git Bash，并键入以下内容：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  

![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  

#### <a name="to-upgrade-the-database-file"></a>升级数据库文件  

1.  在 Visual Studio 中打开项目，将出现一个弹出窗口，告诉你该应用需要 SQL Server 2012 Express，或者需要升级数据库。  单击 "确定"。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  

2.  接下来，通过选择顶部的 "生成 > 生成解决方案" 来编译该应用程序。  这会还原所有 NuGet 包。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  

3.  现在，选择 "**查看** -> **服务器资源管理器**"。  打开后，在 "**数据连接**" 下，右键单击**DefaultConnection** ，然后选择 "**修改连接**"。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  

4.  在 "**修改连接**" 下的 "**数据库文件名（新建或现有）** " 下，选择 "**浏览**"，并提供**path\filename.mdf**。 单击对话框中的 **"是"** 。

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)

5.  在 "**修改连接**" 上，选择 "**高级**"。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  

6.  在 "高级属性" 上，找到 "数据源" 并使用下拉箭头将其从 **（LocalDb\v11.0）** 更改为 **（LocalDb） \MSSQLLocalDB**。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  

7.  单击 "确定"。 单击 "确定"。  单击 "是" 升级数据库。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  

8.  完成此过程后，请在右侧的 "**连接字符串**" 旁边的框中复制值。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  

9.  现在，打开 web.config 文件并将 connectionString 中的值替换为你在上面复制的值。  保存 web.config 文件。  

    > [!NOTE]  
    > 以上步骤是必需的，以便我们可以获取新的 connectionString。  否则，当我们运行下面的更新数据库时，将会出现错误。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  

10. 在 Visual Studio 的顶部，选择 "**查看** -> **其他 Windows**@no__t"**包管理器控制台**"。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  

11. 在底部的 "包管理器控制台" 中，输入： `Enable-Migrations` 并按 enter。  

    > [!NOTE]  
    > 如果收到一条错误消息，指出 "启用-迁移未被识别为 cmdlet"，请输入安装包 EntityFramework 以更新 EntityFramework。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  

12. 在底部的 "包管理器控制台" 中，输入： `Add-Migration <anynamehere>` 并按 enter。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  

13. 在底部的 "包管理器控制台" 中，输入： `Update-Database` 并按 enter。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  

## <a name="modify-the-webapi-in-visual-studio"></a>在 Visual Studio 中修改 WebApi  

#### <a name="to-modify-the-sample-web-api"></a>修改示例 Web API  

1.  使用 Visual Studio 打开示例。  

2.  打开 web.config 文件。  修改以下值：  

    -   ida： ClientId-输入上面 "创建应用程序组" 部分中 #3 的值。  

    -   ida： ClientSecret-输入上面 "创建应用程序组" 部分中 #5 的值。  

    -   ida： GraphResourceId-输入上面 "创建应用程序组" 部分中 #6 的值。  

    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  

3.  打开 App_Start 下的 Startup.Auth.cs 文件并进行以下更改：  

    -   注释掉以下行：  

        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   在此处添加以下内容：  

        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  

        其中 < your_fsname > 替换为联合身份验证服务 url 的 DNS 部分，例如 adfs.contoso.com  

        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  

4.  打开 UserProfileController.cs 文件并进行以下更改：  

    -   注释掉以下内容：  

        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  

    -   将两个匹配项替换为以下内容：  

        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  

        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  

    -   注释掉以下内容：  

        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  

    -   将两个匹配项替换为以下内容：  

        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  

        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  

    -   现在，注释掉以下所有实例：  

        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString() + "/OAuth");  
        ```  

    -   将出现的所有项替换为以下内容：  

        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString());  
        ```  

        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  

## <a name="test-the-solution"></a>测试解决方案  
在本部分中，我们将测试机密客户端解决方案。  使用以下过程来测试解决方案。  

#### <a name="testing-the-confidential-client-solution"></a>测试机密客户端解决方案  

1. 在 Visual Studio 顶部，请确保已选择 "Internet Explorer"，并单击绿色箭头。  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  

2. ASP.Net 页面出现后，单击页面右上方的 "**注册**"。  输入用户名和密码，然后单击 "**注册**" 按钮。  这会在 SQL 数据库中创建一个本地帐户。  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  

3. 请注意，ASP.NET 站点显示 Hello abby@contoso.com！。  单击 "**配置文件**"。  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  

4. 这会显示一个没有任何信息的页面，并且说我们必须单击此处登录。  单击**此处**。  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  

5. 此时，系统将提示你登录到 AD FS。  

   ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

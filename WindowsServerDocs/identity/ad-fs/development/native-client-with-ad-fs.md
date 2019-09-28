---
title: 使用 AD FS 2016 或更高版本的 OAuth 公共客户端生成本机客户端应用程序
description: 本演练提供有关使用 OAuth 公用客户端和 AD FS 2016 或更高版本生成本机客户端应用程序的说明
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.openlocfilehash: 442aef6daccda2ab3e95690a82f43f642e5a3f73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358747"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>使用 AD FS 2016 或更高版本的 OAuth 公共客户端生成本机客户端应用程序

## <a name="overview"></a>概述

本文介绍如何生成与受 AD FS 2016 或更高版本保护的 Web API 交互的本机应用程序。

1. .Net TodoListClient WPF 应用程序使用 Active Directory 身份验证库（ADAL）通过 OAuth 2.0 协议从 Azure Active Directory （Azure AD）获取 JWT 访问令牌
2. 调用 TodoListService web API 的/todolist) 终结点时，访问令牌用作持有者令牌以对用户进行身份验证。
 我们将使用此处 Azure AD 的应用程序示例，然后在 AD FS 2016 或更高版本中对其进行修改。

![应用程序概述](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>先决条件
下面列出了在完成本文档之前需要满足的先决条件。 本文档假定已安装 AD FS 并且已创建 AD FS 场。

* GitHub 客户端工具
* Windows Server 2016 或更高版本中的 AD FS
* Visual Studio 2013 或更高版本

## <a name="creating-the-sample-walkthrough"></a>创建示例演练

### <a name="create-the-application-group-in-ad-fs"></a>在 AD FS 中创建应用程序组

1. 在 AD FS 管理 "中，右键单击"**应用程序组**"，然后选择"**添加应用程序组**"。

2. 在应用程序组向导上，为 "名称" 输入你喜欢的任何名称，例如 NativeToDoListAppGroup。 选择**用于访问 WEB API 模板的本机应用程序**。 单击“下一步”。
 ![添加应用程序组](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. 在 "**本机应用程序**" 页上，记下 AD FS 生成的标识符。 这是 AD FS 将识别其公共客户端应用程序的 id。 复制 "**客户端标识符**" 值。 稍后将在应用程序代码中将其用作**ida： ClientId**的值。 如果需要，可以在此处提供任何自定义标识符。 重定向 URI 是任意值，例如，put https://ToDoListClient ![ 本机应用](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. 在 "**配置 WEB api** " 页上，设置 Web api 的标识符值。 在此示例中，此值应为要在其中运行 Web 应用的**SSL URL**的值。 可以通过单击解决方案中的 TooListServer 项目的属性来获取此值。 稍后将其用作 native client**应用程序的 app.config 文件中**的**todo： TodoListResourceId**值，也作为**todo： TodoListBaseAddress**。
![Web API](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. 浏览 "**应用访问控制策略**"，并将**应用程序权限配置**为使用默认值。 "摘要" 页的外观应如下所示。
![摘要](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

单击 "下一步"，然后完成向导。

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>将 NameIdentifier 声明添加到颁发的声明列表
演示应用程序在不同位置使用 NameIdentifier 声明中的值。 与 Azure AD 不同，AD FS 默认情况下不发出 NameIdentifier 声明。 因此，我们需要添加声明规则以颁发 NameIdentifier 声明，使应用程序可以使用正确的值。 在此示例中，用户的给定名称将作为令牌中用户的 NameIdentifier 值发出。
若要配置声明规则，请打开刚刚创建的应用程序组，然后双击 Web API。 选择 "颁发转换规则" 选项卡，然后单击 "添加规则" 按钮。 在声明规则的类型中，选择 "自定义声明规则"，然后添加声明规则，如下所示。

```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![NameIdentifier 声明规则](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>修改应用程序代码

本部分介绍如何在 Visual Studio 中下载并修改示例 Web API。   我们将使用[此处](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop)的 Azure AD 示例。  

若要下载示例项目，请使用 Git Bash，并键入以下内容：  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>修改 ToDoListClient

解决方案中的此项目表示 native client 应用程序。 我们需要确保客户端应用程序知道：

1. 需要在何处进行用户身份验证？
2. 客户端需要向身份验证机构提供什么 ID （AD FS）？
3. 需要访问令牌的资源的 ID 是多少？
4. Web API 的基址是什么？

需要进行以下代码更改，以便将上述信息获取给 native client 应用程序。

**App.config**

* 添加密钥**ida：权威**，其中包含描述 AD FS 服务的值。 例如， https://fs.contoso.com/adfs/
* 在 AD FS 中创建应用程序组期间，在 "**本机应用程序**" 页中修改**ida： ClientId**密钥，其值来自 "**客户端标识符**"。 例如，3f07368b-6efd-4f50-a330-d93853f4c855
* 在 AD FS 中创建应用程序组期间 **，在 "** **配置 Web API** " 页中修改**todo： todo： TodoListResourceId**的值。 例如， https://localhost:44321/
* 在 AD FS 中创建应用程序组期间，通过 "**配置 WEB API** " 页中的 "**标识符**" 中的值修改**todo： TodoListBaseAddress** 。 例如， https://localhost:44321/
* 在 AD FS 中创建应用程序组期间，将**ida： RedirectUri**的值设置为 "**本机应用程序**" 页中的 "**重定向 URI** " 中的值。 例如， https://ToDoListClient
* 为了便于阅读，你可以删除/注释 ida 的密钥 **：租户**和**ida： AADInstance**。

  ![应用配置](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* 备注 aadInstance 的行，如下所示

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* 为授权机构添加值，如下所示

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* 删除用于从 aadInstance 和租户创建**授权**值的行

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* 在函数**mainwindow.xaml**中，将 authContext 实例更改为

        authContext = new AuthenticationContext(authority,false);

    ADAL 不支持验证 AD FS 作为权限，因此我们必须传递 validateAuthority 参数的 false 值标志。

  ![主窗口](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>修改 TodoListService
此项目中需要更改两个文件– web.config 和 Startup.Auth.cs。 需要更改 web.config 以获取正确的参数值。 需要 Startup.Auth.cs 更改以将 WebAPI 设置为针对 AD FS 而不是 Azure AD 进行身份验证。

**Web.config**

* 注释密钥**ida：租户**，因为我们不需要它
* 为**ida：核证机关**添加密钥，其值指示联合身份验证服务的 FQDN，例如 https://fs.contoso.com/adfs/
* 修改 key **ida：受众**，其中包含你在 AD FS 中的 "添加应用程序组" 期间在 "**配置 web api** " 页中指定的 web api 标识符的值。
* 添加密钥**ida： AdfsMetadataEndpoint** ，其中包含与 AD FS 服务的联合元数据 URL 对应的值，例如： https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![Web 配置](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

按如下所示修改 ConfigureAuth 函数

    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseActiveDirectoryFederationServicesBearerAuthentication(
            new ActiveDirectoryFederationServicesBearerAuthenticationOptions
            {
                MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
                TokenValidationParameters = new TokenValidationParameters()
                {
                    SaveSigninToken = true,
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                }

            });
    }

实质上，我们要将身份验证配置为使用 AD FS 并进一步提供有关 AD FS 元数据的信息，并验证令牌，受众声明应该是 Web API 所需的值。
运行应用程序

1. 在解决方案 Nativeclient-ios-DotNet 上，右键单击并选择 "属性"。 将启动项目更改为 "多个启动项目"，并将 "TodoListClient" 和 "TodoListService" 设置为 "启动"。
![解决方案属性](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  按 F5 按钮，或选择菜单栏中的 "调试" > 继续。 这将启动本机应用程序和 WebAPI。 单击本机应用程序上的 "登录" 按钮，该按钮将弹出来自 AD AL 的交互式登录，并将其重定向到 AD FS 服务。 输入有效用户的凭据。
![登录](media/native-client-with-ad-fs-2016/sign-in.png)

在此步骤中，本机应用程序重定向到 AD FS 并获得一个 ID 令牌和一个用于 Web API 的访问令牌

3. 在文本框中输入一个待办事项，并单击 "添加项"。 在此步骤中，应用程序将进入 Web API 以添加待办事项，若要执行此操作，请向 AD FS 中获取的 WebAPI 提供访问令牌。 Web API 与受众值匹配，以确保令牌适用于该令牌，并使用来自联合元数据的信息验证令牌签名。

![登录](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

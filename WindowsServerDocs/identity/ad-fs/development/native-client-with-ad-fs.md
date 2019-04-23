---
title: 生成与 AD FS 2016 使用 OAuth 公共客户端的本机客户端应用程序
description: 说明如何构建使用 OAuth 公共客户端和 AD FS 2016 的本机客户端应用程序的演练
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 6302e282ebf7f84f741835d52333b99bec945f91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846478"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016"></a>生成与 AD FS 2016 使用 OAuth 公共客户端的本机客户端应用程序

## <a name="overview"></a>概述

本文介绍如何构建与 AD FS 2016 保护的 Web API 进行交互的本机应用程序。

1. .Net TodoListClient WPF 应用程序使用 Active Directory 身份验证库 (ADAL) 来通过 OAuth 2.0 协议从 Azure Active Directory (Azure AD) 获取 JWT 访问令牌
2. 访问令牌作为持有者令牌用于对用户进行身份验证时调用 TodoListService web API 的 /todolist 终结点。
 我们将使用此处的 Azure AD 应用程序示例并修改 ad FS 2016。

![应用程序概述](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>先决条件
以下是完成本文档之前所需的系统必备组件的列表。 本文档假定已安装 AD FS，并且已创建的 AD FS 场。 

* GitHub 客户端工具 
* Windows Server 2016 中的 AD FS
* Visual Studio 2013 或更高版本

## <a name="creating-the-sample-walkthrough"></a>创建示例演练

### <a name="create-the-application-group-in-ad-fs"></a>在 AD FS 中创建的应用程序组

1. 在 AD FS 管理中，右键单击**应用程序组**，然后选择**添加应用程序组**。

2. 在应用程序组向导中，为名称输入任何所需的名称，例如 NativeToDoListAppGroup。 选择**访问 web API 的本机应用程序**模板。 单击“下一步” 。
 ![添加应用程序组](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. 上**本机应用程序**页上，请注意生成 AD fs 的标识符。 这是与 AD FS 将识别公共客户端应用程序的 id。 复制**客户端标识符**值。 它将在稍后的值作为**ida: ClientId**应用程序代码中。 如果您希望您可以为任何自定义的标识符。 重定向 URI 是任意值，示例中，将放 https://ToDoListClient![本机应用](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. 上**配置 Web API**页上，为 Web API 设置的标识符值。 对于此示例中，应选择的值**SSL URL**其中 Web 应用程序应当在运行。 你可以通过单击 TooListServer 项目解决方案中的属性获取此值。 这将更高版本用作**todo:TodoListResourceId**中的值**App.config**文件的本机客户端应用程序，而且还作为**todo:TodoListBaseAddress**。
![Web API](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. 请通读**应用访问控制策略**并**配置应用程序权限**位置中的默认值。 摘要页应看到如下所示。
![摘要](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)
 
单击下一步，然后完成向导。

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>将 NameIdentifier 声明添加到颁发的声明的列表
演示应用程序在不同位置的 NameIdentifier 声明中使用的值。 与 Azure AD 中，AD FS 不会颁发默认情况下 NameIdentifier 声明。 因此，我们需要添加声明规则，使发出 NameIdentifier 声明，以便应用程序可以使用正确的值。 在此示例中，用户的给定名称颁发的令牌中用户的 NameIdentifier 值。
若要配置的声明规则，打开刚创建的应用程序组，并双击 Web API。 选择颁发转换规则选项卡，然后单击添加规则按钮。 在声明规则的类型中，选择自定义声明规则以及如何将声明规则，如下所示。
![NameIdentifier 声明规则](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>修改应用程序代码

#### <a name="modify-todolistclient"></a>修改 ToDoListClient

此解决方案中的项目表示本机客户端应用程序。 我们需要确保客户端应用程序知道：

1. 获取身份验证时所需的用户在哪里？
2. 什么是标识该客户端需要向身份验证机构 (AD FS) 提供？
3. 请求的访问令牌的资源 ID 是什么？
4. 什么是 Web API 的基址？

为了获取上述信息到本机客户端应用程序所需要的以下代码更改。

**App.config**

* 将密钥添加**ida： 颁发机构**值描述在 AD FS 服务，示例中， https://fs.contoso.com/adfs/
* 修改**ida: ClientId**键，其值从**本机应用程序**页在 AD FS 中的应用程序组创建过程。 有关 ex、 3f07368b-6efd-4f50-a330-d93853f4c855
* 修改**todo:TodoListBaseAddress**到基址的 Web API，例如 https://localhost:44321/
* 设置的值**ida: RedirectUri**期间在 AD FS 中添加应用程序组放在本机应用程序页中的值。
* 为了便于阅读您可以删除 / 注释的键**ida： 租户**并**ida: AADInstance**。

**MainWindow.xaml.cs**

* 删除 aadInstance 的行
* 按如下所示添加颁发机构的值

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* 删除创建的行**机构**从 aadInstance 和租户的值

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* 在函数中**MainWindow**，更改为 authContext 实例化

        authContext = new AuthenticationContext(authority,false);

    ADAL 不支持验证 AD FS 颁发机构，因此我们将传递 validateAuthority 参数的返回值 false 标志。    

#### <a name="modify-todolistservice"></a>修改 TodoListService
此项目 – 中的更改 Web.config 和 Startup.Auth.cs 需要两个文件。 Web.Config 更改，才能获取正确的参数值。 Startup.Auth.cs 更改所需设置 WebAPI 对 AD FS 而不是 Azure AD 进行身份验证。

**Web.config**

* 删除密钥**ida： 租户**因为我们不需要它
* 添加的键**ida： 颁发机构**值，该值指示联合身份验证的 FQDN 与服务，示例中， https://fs.contoso.com/adfs/
* 修改键**ida： 受众**的值中指定的 Web API 标识符**配置 Web API**期间在 AD FS 中添加应用程序组的页。
* 添加密钥**ida: AdfsMetadataEndpoint** AD FS 的联合身份验证元数据 URL 相对应的值与服务，例如： https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

**Startup.Auth.cs**

修改按如下所示的 ConfigureAuth 函数

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

从根本上讲，我们要配置身份验证使用 AD FS 和进一步提供有关 AD FS 元数据信息并验证令牌，audience 声明应为所需的 Web API 的值。
运行应用程序

1. 解决方案 Nativeclient-dotnet 中，右键单击，然后转到属性。 将如下所示启动项目更改为多个启动项目并将 TodoListClient 和 TodoListService 设置为开始。
![解决方案属性](media/native-client-with-ad-fs-2016/solutionproperties.png)
 
2.  按 F5 按钮或选择调试 > 继续在菜单栏中的。 这将启动本机应用程序和 WebAPI。 单击登录按钮上的本机应用程序，它从 AD AL 交互登录将弹出窗口，并将重定向到 AD FS 服务。 输入有效的用户的凭据。
![单一登录](media/native-client-with-ad-fs-2016/sign-in.png)
 
在此步骤中，本机应用程序重定向到 AD FS，并有一个 ID 令牌和访问令牌的 Web API

3.  输入要执行项在文本框中，然后单击添加项。 在此步骤中，应用程序访问 Web API 添加到待办事项，并为此，向从 AD FS 获取 WebAPI 提供访问令牌。 Web API 与受众值以确保令牌发送给它和验证令牌签名时使用来自联合身份验证元数据的信息相匹配。
 
![登录](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  
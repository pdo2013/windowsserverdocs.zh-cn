---
title: 使用 OAuth 和 ADAL 构建单一页面 web 应用程序.JS 与 AD FS 2016 或更高版本
description: 本演练提供有关使用 ADAL for JavaScript 针对 AngularJS 进行身份 AD FS 验证的说明，以保护基于的单页面应用程序
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/13/2018
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.openlocfilehash: d54c33e092204f208590bd15db0d3c7fe7f852f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407897"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>使用 OAuth 和 ADAL 构建单一页面 web 应用程序.JS 与 AD FS 2016 或更高版本

本演练提供了使用 ADAL for JavaScript 对 AD FS 进行身份验证的说明，该应用程序用于保护使用 ASP.NET Web API 后端实现的基于 AngularJS 的单页面应用程序。

在此方案中，当用户登录时，JavaScript 前端使用[适用于 javascript 的 Active Directory 身份验证库（ADAL）.JS）](https://github.com/AzureAD/azure-activedirectory-library-for-js)和隐式授权授予从 Azure AD 获取 ID 令牌（id_token）。 该令牌已缓存，客户端在调用其 Web API 后端（使用 OWIN 中间件进行保护）时，会将该令牌作为持有者令牌附加到请求。

>[!IMPORTANT]
>你可以在此处生成的示例仅供教育之用。 这些说明适用于公开模型所需元素的最简单的最小实现。 该示例可能不包括错误处理和其他相关功能的所有方面。

>[!NOTE]
>本演练**仅**适用于 AD FS Server 2016 及更高版本 

## <a name="overview"></a>概述
在此示例中，我们将创建一个身份验证流，其中，单个页面应用程序客户端将针对 AD FS 进行身份验证，以确保对后端上 WebAPI 资源的访问。 下面是整体身份验证流


![AD FS 授权](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

使用单页面应用程序时，用户将导航到起始位置，从该位置开始，并加载 JavaScript 文件和 HTML 视图的集合。 你需要配置 Active Directory 身份验证库（ADAL）来了解有关你的应用程序的关键信息，即 AD FS 实例的客户端 ID，以便能够将身份验证定向到你的 AD FS。

如果 ADAL 看到用于身份验证的触发器，它将使用应用程序提供的信息，并将身份验证定向到 AD FS STS。  在 AD FS 中注册为公用客户端的单页面应用程序将自动配置为隐式授权流。 授权请求导致 ID 令牌通过 #fragment 返回给应用程序。 对后端 WebAPI 的进一步调用将此 ID 令牌作为标头中的持有者令牌，以获取对 WebAPI 的访问权限。

## <a name="setting-up-the-development-box"></a>设置开发框
本演练使用 Visual Studio 2015。 项目使用 ADAL JS 库。 若要了解 ADAL，请阅读[Active Directory 身份验证库 .net。](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>设置环境
对于本演练，我们将使用的基本设置：

1.  直流：将托管 AD FS 的域的域控制器
2.  AD FS 服务器：域的 AD FS 服务器
3.  开发计算机：安装了 Visual Studio 并将开发示例的计算机

如果需要，可以仅使用两台计算机。 一个用于 DC/AD FS，另一个用于开发示例。

如何设置域控制器和 AD FS 超出了本文的讨论范围。 有关其他部署信息，请参阅：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md)
- [AD FS 部署](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>克隆或下载此存储库
我们将使用创建的用于将 Azure AD 集成到 AngularJS 单页面应用的示例应用程序，并将其修改为通过使用 AD FS 来保护后端资源。

从 shell 或命令行：

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>关于代码
包含身份验证逻辑的密钥文件如下所示：

**Node.js** -注入 adal 模块依赖关系，提供 adal 用于驱动协议与 AAD 的交互的应用配置值，并指示在没有以前的身份验证的情况下不应访问哪些路由。

**index .html** -包含对 adal 的引用

**HomeController**-演示如何利用 ADAL 中的登录（）和注销（）方法。

**UserDataController** -演示如何从缓存的 id_token 中提取用户信息。

**Startup.Auth.cs** -包含用于持有者身份验证 Active Directory 联合身份验证服务的 WebAPI 配置。

## <a name="registering-the-public-client-in-ad-fs"></a>在 AD FS 中注册公共客户端
在此示例中，WebAPI 配置为侦听 https://localhost:44326/ 。 应用程序组**web 浏览器访问 web 应用程序**可用于配置隐式授予流应用程序。

1. 打开 AD FS 管理控制台，然后单击 "**添加应用程序组**"。 在 "**添加应用程序组" 向导**中，输入应用程序的名称，"说明"，然后从 "**客户端-服务器应用程序**" 部分选择 " **web 浏览器访问 web 应用程序**模板"，如下所示

    ![创建新的应用程序组](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. 在下一页**本机应用程序**上，提供应用程序客户端标识符和重定向 URI，如下所示

    ![创建新的应用程序组](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. 在下一页上，**应用访问控制策略**将权限保留为 "*允许每个人*"

4. "摘要" 页的外观应如下所示

    ![创建新的应用程序组](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. 单击 "**下一步**" 以完成应用程序组的添加，然后关闭向导。

## <a name="modifying-the-sample"></a>修改示例
配置 ADAL JS

打开**app.config**文件并将**adalProvider**定义更改为：

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|配置|描述|
|--------|--------|
|实例|STS URL，例如 https://fs.contoso.com/|
|组织|将其保留为 "adfs"|
|ClientID|这是你在为单一页面应用程序配置公共客户端时指定的客户端 ID|

## <a name="configure-webapi-to-use-ad-fs"></a>将 WebAPI 配置为使用 AD FS
打开示例中的**Startup.Auth.cs**文件，并在开头添加以下内容：

    using System.IdentityModel.Tokens;

取消

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

并添加：

    app.UseActiveDirectoryFederationServicesBearerAuthentication(
    new ActiveDirectoryFederationServicesBearerAuthenticationOptions
    {
    MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
    TokenValidationParameters = new TokenValidationParameters()
    {
    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"],
    ValidIssuer = ConfigurationManager.AppSettings["ida:Issuer"]
    }
    }
    );

|参数|描述|
|--------|--------|
|ValidAudience|这会在令牌中配置要检查的 "受众" 的值。|
|ValidIssuer|这会在令牌中配置要检查的 "颁发者" 的值|
|MetadataEndpoint|这将指向 STS 的元数据信息|

## <a name="add-application-configuration-for-ad-fs"></a>为 AD FS 添加应用程序配置
更改 appsettings，如下所示：

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>运行解决方案
清理解决方案，重新生成并运行解决方案。 若要查看详细跟踪，请启动 Fiddler 并启用 HTTPS 解密。

浏览器（使用 Chrome 浏览器）将加载 SPA，并将显示以下屏幕：

![注册客户端](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

单击 "登录"。  ToDo 列表将触发身份验证流，而 ADAL JS 会将身份验证定向到 AD FS

![登录](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

在 Fiddler 中，可以看到令牌作为 URL 的一部分返回到 # 片段。

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

现在，你将可以调用后端 API 来为已登录用户添加 ToDo 列表项：

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

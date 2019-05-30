---
title: 构建使用 OAuth 和 ADAL 的单页 web 应用程序。JS 与 AD FS 2016 或更高版本
description: 它提供有关针对 AD FS 进行身份验证的说明使用 ADAL for JavaScript 保护 AngularJS 的演练基于单页面应用程序
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 24a9caba7a2745973d7c69c3bd7bc42717e7a06c
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266682"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>构建使用 OAuth 和 ADAL 的单页 web 应用程序。JS 与 AD FS 2016 或更高版本

本演练提供了用于针对 AD FS 保护 AngularJS javascript 使用 ADAL 进行身份验证的说明基于单页面应用程序，使用 ASP.NET Web API 后端实现。

在此方案中，当用户登录时，JavaScript 前端使用[Active Directory Authentication Library for JavaScript (ADAL。JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js)和隐式授权许可从 Azure AD 获取一个 ID 令牌 (id_token)。 令牌缓存和客户端作为附加到请求的持有者令牌时进行调用的 Web api 后端，使用 OWIN 中间件进行保护。

>警告：您可以在此处生成的示例是仅供教学使用。 这些说明适用于可能会公开模型的所需的元素的最简单、 最小实现。 该示例可能不包括错误处理的所有方面和其他相关功能。

>[!NOTE]
>本演练是适用**仅**到 2016年及更高版本的 AD FS 服务器 

## <a name="overview"></a>概述
在此示例中我们将创建的单页应用程序客户端将身份验证来保护对后端上的 WebAPI 资源访问的 AD FS 身份验证流。 下面是整个身份验证流


![AD FS 授权](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

使用单页面应用程序，当用户导航到起始位置，从在起始页和 JavaScript 文件的集合和加载 HTML 视图。 您需要配置 Active Directory 身份验证库 (ADAL)，了解有关应用程序，即 AD FS 实例，客户端 ID 的关键信息，以便可以定向到 AD FS 身份验证。

如果 ADAL 发现用于身份验证的触发器，它使用应用程序提供的信息，并将定向到您的 AD FS STS 的身份验证。  单页面应用程序，后者作为 AD FS 中的公共客户端进行注册，自动配置为隐式授权流。 授权请求会导致通过 #fragment 返回到应用程序的 ID 令牌。 进一步调用后端 WebAPI 将作为持有者令牌来访问 WebAPI 的标头中携带此 ID 令牌。

## <a name="setting-up-the-development-box"></a>设置开发环境中
本演练使用 Visual Studio 2015。 该项目使用 ADAL JS 库。 若要了解有关 ADAL，请阅读[Active Directory 身份验证库.NET。](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>设置环境
在本演练中我们将使用基本的设置：

1.  直流：AD FS 将在其中托管的域的域控制器
2.  AD FS 服务器：域的 AD FS 服务器
3.  开发计算机：我们已安装 Visual Studio 和我们的示例将开发计算机

您可以如果你想使用仅两台计算机。 一个用于 DC/AD FS，另一个用于开发示例。

如何设置域控制器和 AD FS 已超出本文的讨论范围。 有关其他部署信息，请参阅：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [AD FS 部署](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>克隆或下载此存储库
我们将使用创建用于将 Azure AD 集成到 AngularJS 单页面应用程序和修改它的示例应用程序以改为使用 AD FS 保护的后端资源。

从 shell 或命令行：

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>有关代码
包含身份验证逻辑的主要文件如下所示：

**App.js** -注入 ADAL 模块依赖项、 提供由 ADAL 使用用于驱动与 AAD 之间的协议交互的应用程序配置值和指示的路由不应访问身份验证。

**index.html** -包含对 adal.js 的引用

**HomeController.js**-演示如何利用 ADAL 中的 login （） 和 logout （） 方法。

**UserDataController.js** -演示如何从缓存的 id_token 提取用户信息。

**Startup.Auth.cs** -包含 WebAPI 持有者身份验证使用 Active Directory 联合身份验证服务的配置。

## <a name="registering-the-public-client-in-ad-fs"></a>在 AD FS 中注册公共客户端
在示例中，WebAPI 配置为侦听 https://localhost:44326/。 应用程序组**Web 浏览器访问 web 应用程序**可用于配置隐式授权流应用程序。

1. 打开 AD FS 管理控制台并单击**添加应用程序组**。 在中**添加应用程序组向导**输入的应用程序、 说明和选择名称**Web 浏览器访问 web 应用程序**模板从**客户端-服务器应用程序**部分如下所示

    ![创建新的应用程序组](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. 在下一页上**本机应用程序**、 提供应用程序客户端标识符和重定向 URI，如下所示

    ![创建新的应用程序组](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. 在下一页上**应用访问控制策略**保留为权限*授权所有人*

4. 摘要页应与下列内容类似

    ![创建新的应用程序组](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. 单击**下一步**若要完成的应用程序组添加并关闭向导。

## <a name="modifying-the-sample"></a>修改示例
配置 ADAL JS

打开**app.js**文件，并更改**adalProvider.init**定义：

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|配置|描述
|--------|--------
|实例|你 STS 的 URL，例如 https://fs.contoso.com/
|租户|将它保留为 adfs
|clientID|这是你在配置单页面应用程序的公共客户端时指定的客户端 ID

## <a name="configure-webapi-to-use-ad-fs"></a>WebAPI 配置为使用 AD FS
打开**Startup.Auth.cs**示例文件，并在开头添加以下内容： 

    using System.IdentityModel.Tokens;

删除：

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

|参数|描述
|--------|--------
|ValidAudience|这会配置 'audience' 的值，将针对其检查令牌中
|ValidIssuer|这将配置的值将根据检查令牌中的颁发者
|MetadataEndpoint|这指向您的 STS 的元数据信息

## <a name="add-application-configuration-for-ad-fs"></a>添加适用于 AD FS 的应用程序配置
更改 appsettings 按如下所示：

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>运行解决方案
清理解决方案，重新生成解决方案并运行它。 如果你想要看到详细的跟踪，启动 Fiddler 和启用 HTTPS 解密。

浏览器 （使用 Chrome 浏览器） 将加载 SPA，您将看到以下屏幕：

![注册客户端](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

单击登录。  待办事项列表将触发身份验证流和 ADAL JS 会将定向到 AD FS 身份验证

![登录](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

在 Fiddler 中可以看到令牌中的 # 片段的 URL 的一部分返回。

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

你将能够立即调用后端 API 将登录的用户的待办事项列表项添加：

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

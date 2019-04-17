---
title: "生成一个页面 web 应用程序使用 OAuth 和 ADAL.JS 与广告 FS 2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 7b3d48e1e38baffeb84b1f236efb43cfda5048c0
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016"></a>生成一个页面 web 应用程序使用 OAuth 和 ADAL.JS 与广告 FS 2016

>适用于：Windows Server 2016

此演练提供中进行身份验证广告 FS 使用适用于 JavaScript 保护 AngularJS ADAL 指令基于单个页面应用程序中，使用 ASP.NET Web API 后端实现。

>警告：你可以在以下版本示例是仅用于教育。 这些说明是针对可能以揭示模型的所需的元素的最简单的、最少实现。 示例所述可能不包括处理错误的所有方面，并且其他与相关的功能。

>[!NOTE]
>此演练是适用**仅**到 2016 年和更高的广告 FS 服务器 

## <a name="overview"></a>概述
在此示例中我们将创建了一个页面应用程序客户端将会进行身份验证广告 FS 来保护访问后端 WebAPI 资源身份验证流量。 下面是整体身份验证流


![广告 FS 授权](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

当使用单个页面应用程序，用户导航到的开始位置，从启动页面和一系列 JavaScript 文件的位置和加载 HTML 视图。 你需要进行配置 Active Directory 身份验证库 (ADAL)，了解有关你的应用程序，即广告 FS 实例的客户端 ID 的重要信息，以便它可以将定向到你的广告 FS 身份验证。

如果 ADAL 看到触发器进行身份验证，它将使用提供的应用程序的信息，并将定向到你的广告 FS STS 身份验证。  已注册为公共广告 FS 在客户端，一个页面应用程序将自动配置隐式授予流。 在将返回到该应用程序通过 #fragment ID 令牌结果授权请求。 向后端进一步呼叫 WebAPI 将访问 WebAPI 标题中的持有者标记为保留此 ID 标记。

## <a name="setting-up-the-development-box"></a>设置开发框
此浏览使用 Visual Studio 2015。 项目使用 ADAL JS 库。 要了解详细信息，请阅读 ADAL [Active Directory 身份验证库.NET。](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>环境设置
此演练我们将使用的基本设置：

1.  将在其中托管广告 FS 域哥伦比亚特区： 域控制器
2.  广告 FS Server: 广告 FS 域服务器
3.  我们已安装 Visual Studio，将会开发我们示例的开发计算机： 计算机

你可以如果你希望，使用仅两台计算机。 一个用于 DC/广告和对另一个用于开发示例。

如何设置域控制器和广告 FS 是阐述的这篇文章。 有关更多部署的信息，请参阅：

- [广告 DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [广告 FS 部署](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>复制或下载此知识库
我们将使用 Azure AD 结合 AngularJS 单个页面应用和修改其创建的示例应用程序可以改为使用广告 FS 安全的后端资源。

从 shell 或命令行：

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>有关该代码
关键文件包含身份验证逻辑如下所示：

**App.js** -插入 ADAL 模块相关性、 提供 ADAL 用于驾驶使用 AAD 的协议交互的应用配置值和指示不应无需以前身份验证访问的路线。

**index.html** -中 adal.js 参考

**HomeController.js**-演示如何利用 ADAL login() 和 logOut() 方法。

**UserDataController.js** -显示如何从缓存 id_token 提取用户信息。

**Startup.Auth.cs** -包含 WebAPI Active Directory 联合身份验证服务用于持有者身份验证的配置。

## <a name="registering-the-public-client-in-ad-fs"></a>在广告 FS 注册公用客户端
在此示例中，WebAPI 配置为在 https://localhost:44326 聆听 /。 应用程序组**访问 web 应用程序的 Web 浏览器**可用于配置隐式授予流应用程序。

1. 打开广告 FS 管理控制台中，单击**添加的应用程序组**。 在**添加的应用程序组向导**输入描述应用程序，选择名**访问 web 应用程序的 Web 浏览器**从模板**客户服务器应用程序**部分如下所示
    <br>![创建新的应用程序组](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. 在下一步页面**本机应用程序**、 提供客户端应用程序标识符，并将 URI 重定向，如下所示
    <br>![创建新的应用程序组](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. 在下一步页面**应用访问控制策略**离开作为权限*允许任何人*

4. 摘要页面应类似于以下
    <br>![创建新的应用程序组](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. 单击**下一步**以完成的应用程序组加并关闭向导。

## <a name="modifying-the-sample"></a>修改示例
配置 ADAL JS

打开**app.js**文件，然后更改**adalProvider.init**到的定义：

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
|实例|你 STS URL，例如 https://fs.contoso.com/
|租户|将其保留为 adfs
|客户机 Id|这是你在配置单个页面应用程序的公用客户端时指定的客户端 ID

## <a name="configure-webapi-to-use-ad-fs"></a>配置 WebAPI 使用广告 FS
打开**Startup.Auth.cs**文件在此示例中，然后开始添加以下： 

    using System.IdentityModel.Tokens;

删除：

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

然后添加：

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
|ValidAudience|此配置受众的值，将根据检查，令牌中
|ValidIssuer|这将配置的值的将根据令牌中检查的发行商
|MetadataEndpoint|这指向你 STS 的元数据的信息

## <a name="add-application-configuration-for-ad-fs"></a>添加广告 fs 应用程序配置
更改 appsettings 如下所示：

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>运行解决方案
清理解决方案、 重新生成解决方案和运行它。 如果你想要看到详细的跟踪，启动 Fiddler，并启用 HTTPS 解密。

浏览器将加载水疗，你将可以看到下面的屏幕：

![注册的客户端](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

单击登录。  任务列表将触发身份验证流和 ADAL JS 将定向到广告 FS 身份验证

![登录](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

在 Fiddler 您可以看到的标记返回作为 # 片段中 URL 的一部分。

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

你将能够现在呼叫后端 API 添加已登录的用户的应做事项列表项：

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>后续步骤
[广告 FS 开发](../../ad-fs/AD-FS-Development.md)  

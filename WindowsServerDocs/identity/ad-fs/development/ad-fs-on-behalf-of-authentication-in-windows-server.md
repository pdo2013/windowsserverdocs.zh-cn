---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: 在 AD FS 2016 或更高版本中，使用 OAuth （OBO）创建一个多层应用程序
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9c6c6e7d2c12b6b822989bba05370015f7cd1833
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407815"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>在 AD FS 2016 或更高版本中，使用 OAuth （OBO）创建一个多层应用程序


本演练提供了使用 Windows Server 2016 TP5 或更高版本中的 AD FS 来实现代表（OBO）身份验证的说明。 若要了解有关 OBO authentication 的详细信息，请阅读[AD FS OpenID connect/OAuth 流和应用程序方案](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)

>警告：你可以在此处生成的示例仅供教育之用。 这些说明适用于公开模型所需元素的最简单的最小实现。 该示例可能不包括错误处理的所有方面和其他相关功能，仅侧重于获取成功的 OBO 身份验证。

## <a name="overview"></a>概述

在此示例中，我们将创建一个身份验证流，其中，客户端将访问中间层 Web 服务，然后 Web 服务将代表经过身份验证的客户端进行操作以获取访问令牌。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

下面是示例将获得的身份验证流
1. 客户端向 AD FS 授权终结点进行身份验证并请求授权代码
2. 授权终结点将身份验证代码返回到客户端
3. 客户端使用身份验证代码，并将其显示到 AD FS 令牌终结点，以将中间层 Web 服务的访问令牌请求为 WebAPI
4. AD FS 返回中间层 Web 服务的访问令牌。 对于其他功能，中间层服务需要访问后端 WebAPI
5. 客户端使用访问令牌来使用中间层服务。
6. 中间层服务将访问令牌提供给 AD FS 令牌终结点，并为经过身份验证的用户请求后端 WebAPI 的访问令牌
7. AD FS 返回后端 WebAPI 到中间层服务 actiing 作为客户端的访问令牌
8. 中间层服务使用步骤7中 AD FS 提供的访问令牌将后端 WebAPI 作为客户端访问并执行所需的功能

## <a name="sample-structure"></a>示例结构

示例将包含三个模块


模块 | 描述
-------|------------
ToDoClient | 用户与之交互的 Native client
ToDoService | 用作后端 WebAPI 客户端的中间层 web API
WebAPIOBO | ToDoService 在用户添加 ToDoItem 时用于执行必备操作的后端 web api




## <a name="setting-up-the-development-box"></a>设置开发框

本演练使用 Visual Studio 2015。 该项目将使用 Active Directory 身份验证库（ADAL）。 若要了解 ADAL，请阅读[Active Directory 身份验证库 .net](https://msdn.microsoft.com/library/azure/mt417579.aspx)

该示例还使用 SQL LocalDB 版本11.0。 在处理示例之前，请安装 SQL LocalDB。

## <a name="setting-up-the-environment"></a>设置环境
我们将使用的基本设置：

1. **DC**：将托管 AD FS 的域的域控制器
2. **AD FS 服务器**：域的 AD FS 服务器
3. **开发计算机**：安装了 Visual Studio 并将开发示例的计算机

如果需要，可以仅使用两台计算机。 一个用于 DC/ADFS，另一个用于开发示例。

如何设置域控制器和 AD FS 超出了本文的讨论范围。 有关其他部署信息，请参阅：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md)
- [AD FS 部署](../AD-FS-Deployment.md)

该示例基于 Vittorio Bertocci 创建的 Azure 的现有 OBO 示例，[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)提供了这些示例。 按照说明克隆开发计算机上的项目，并创建示例的副本开始使用。

## <a name="clone-or-download-this-repository"></a>克隆或下载此存储库

从 shell 或命令行：

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>修改示例

打开解决方案 WebAPI-OnBehalfOf-DotNet 后，你会发现解决方案中有两个项目

* **ToDoListClient**：这将用作用户将与之交互的 OpenID 客户端
* **ToDoListService**：这将是中间层 Web 服务器应用/服务，它将与经过身份验证的用户的另一后端 WebAPI 交互 OBO

如您所见，我们将需要添加另一个项目，该项目将充当中间层 ToDoListService 将访问的资源。

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>为客户端和 Web 服务器应用配置 AD FS

在示例的当前形式中，身份验证配置为针对 Azure AD 执行。 我们想要更改身份验证机制，并将其定向到本地部署 AD FS。 为了实现此目的，我们需要配置 AD FS 以识别示例中所含的客户端和 Web 服务器应用。

**创建应用程序组**

打开 AD FS 管理 MMC 并添加新的应用程序组。 选择 WebAPI 模板。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

单击 "下一步"，将显示用于提供有关客户端应用程序的信息的页面。 为 AD FS 中的客户端应用程序指定适当的名称。 复制客户端标识符，并将其保存到稍后可访问的某个位置，因为 visual studio 中的应用程序配置需要这样做。

>注意:重定向 URI 可以是任意 URI，因为在本机客户端情况下它实际上不会使用它

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

单击 "下一步"，将显示用于提供有关 WebAPI 的信息的页面。 为 WebAPI 的 AD FS 项提供适当的名称，并输入 "重定向 URI" 作为在 Visual Studio 中为 ToDoListService 显示的 URI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

单击 "下一步"，将显示 "选择访问控制策略" 页。 确保在 "策略" 部分中看到 "允许每个人"。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

单击 "下一步"，将显示 "配置应用程序权限" 页。 在此页上，选择 "允许的作用域" 作为 openid （默认情况下处于选中状态）和 user_impersonation。 必须使用范围 "user_impersonation" 才能成功地从 AD FS 请求代表访问令牌。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

单击 "下一步" 将显示 "摘要" 页。 完成向导的其余部分并完成配置。

若要启用代表身份验证，我们需要确保 AD FS 返回的访问令牌的范围 user_impersonation 为客户端。 修改 ToDoListServiceWebApi 的声明颁发，使其包含以下三个自定义规则：

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**将 ToDoListService 添加为应用程序组中的客户端**

在此阶段，我们需要在 AD FS 中创建一个附加条目，以使 Web 服务器应用充当客户端而不是作为资源。 打开刚刚创建的应用程序组，然后单击 "添加应用程序"。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

将显示 "向 MySampleGroup 中添加新应用程序" 页。 在该页上，选择 "服务器应用程序或网站" 作为独立应用程序

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

单击 "下一步"，将显示页面以提供应用程序详细信息。 为 "名称" 部分中的配置项提供适当的名称。 确保客户端标识符与 ToDoListServiceWebAPI 的标识符相同

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

单击 "下一步"，随即会显示用于配置应用程序凭据的页面。 单击 "生成共享机密"。 系统会显示自动生成的密码。 在某个位置复制机密，因为在 visual studio 中配置 ToDoListService 时需要用到它。


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

单击 "下一步" 并完成向导。

### <a name="modifying-the-todolistclient-code"></a>修改 ToDoListClient 代码

#### <a name="modify-the-application-config"></a>修改应用程序配置

可在 WebAPI-DotNet 解决方案中找到 ToDoListClient 项目。 打开 App.config 文件并进行以下修改

* 注释 ida：租户密钥条目
* 对于 ida： RedirectURI 输入在 AD FS 中配置 MySampleGroup_ClientApplication 时提供的任意 URI。
* 对于 ida： ClientID 密钥，请提供在配置 MySampleGroup_ClientApplication 时 AD FS 提供的客户端 ID 标识符。
* 对于 ida： ToDoListResourceID 提供在 AD FS 中配置 ToDoListServiceWebApi 时提供的资源 ID。
* 注释密钥 ida： AADInstance
* 对于 ida： ToDoListBaseAddress 输入 ToDoListServiceWebApi 的资源 ID。 这将在调用 ToDoList WebAPI 时使用。
* 添加密钥 ida：颁发机构并提供值作为 AD FS 的 URI。

App.config 中的**appSettings**应如下所示：

    <appSettings>
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />-->
    <add key="ida:ClientId" value="c7f7b85c-497c-4589-877f-b17a0bd13398" />
    <add key="ida:RedirectUri" value="https://arbitraryuri.com/" />
    <add key="ida:TodoListResourceId" value="https://localhost:44321/" />
    <!--<add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
    <add key="ida:TodoListBaseAddress" value="https://localhost:44321" />
    <add key="ida:Authority" value="https://fs.anandmsft.com/adfs/"/>
    </appSettings>

#### <a name="modifying-the-code"></a>修改代码

**MainWindow.xaml.cs**

注释从应用程序配置读取租户信息的行

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

将字符串颁发机构的值更改为

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

更改代码以读取 ToDoListResourceId 和 ToDoListBaseAddress 的正确值

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

在函数 Mainwindow.xaml （）中，将 authcontext 初始化更改为：

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>添加后端资源

若要完成代理流，需要创建一个后端资源，ToDoListService 将代表经过身份验证的用户访问该资源。 根据要求，选择的后端资源可能会有所不同，但出于本示例的目的，可以创建一个基本的 WebAPI。

* 在解决方案资源管理器中右键单击解决方案 "WebAPI-DotNet"，并选择 "添加-> 新项目"
* 选择 ASP.NET Web 应用程序模板

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* 在下一次提示符下，单击 "更改身份验证"
* 选择 "工作和学校帐户"，并在右侧下拉列表中选择 "本地"
* 输入 AD FS 部署的 federationmetadata.xml 路径，并提供一个应用 URI （现在提供任意 URI，稍后将对其进行更改），然后单击 "确定" 将该项目添加到解决方案中。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* 在解决方案资源管理器中，右键单击新创建的项目下的 "控制器"。 选择外接 > 控制器
* 在模板选择中，选择 "Web API 2 控制器-空"，然后单击 "确定"。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* 指定适当的控制器名称

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* 将以下代码添加到控制器中


~~~
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http;
    namespace WebAPIOBO.Controllers
    {
        public class WebAPIOBOController : ApiController
        {
            public IHttpActionResult Get()
            {
                return Ok("WebAPI via OBO");
            }
        }
    }
~~~

当任何人向 WebAPI WebAPIOBO 发出 Get 请求时，此代码将直接返回字符串

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>将新的后端 WebAPI 添加到 AD FS

打开 MySampleGroup 应用程序组。 单击 "添加应用程序"，选择 "Web API 模板"，然后单击 "下一步"。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

在 "配置 Web API" 页上，为 WebAPI 项和标识符提供适当的名称。 该标识符应为 visual studio 中 WebAPIOBO 项目的值 SSL URL （类似于我们为 BackendWebAPIAdfsAdd 所做的操作）。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

继续完成向导的其余部分，就像配置 ToDoListService WebAPI 时一样。 在结束时，应用程序组应如下所示：

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>修改 ToDoListService 代码

#### <a name="modifying-the-application-config"></a>修改应用程序配置

* 打开 web.config 文件
* 修改以下项

| Key                      | ReplTest1                                                                                                                                                                                                                   |
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ida：受众             | 在配置 ToDoListService WebAPI 时 AD FS ToDoListService 的 ID，例如 https://localhost:44321/                                                                                         |
| ida： ClientID             | 在配置 ToDoListService WebAPI 时 AD FS ToDoListService 的 ID，例如 <https://localhost:44321/> </br>**Ida：受众和 ida： ClientID 彼此匹配非常重要** |
| ida： ClientSecret         | 这是在中配置 ToDoListService 客户端时 AD FS 生成的机密 AD FS                                                                                                                   |
| ida： AdfsMetadataEndpoint | 这是 AD FS 元数据的 URL，例如 https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml                                                                                             |
| ida： OBOWebAPIBase        | 这是将用于调用后端 API 的基址，例如 https://localhost:44300                                                                                                                     |
| ida：颁发机构            | 这是 AD FS 服务的 URL，示例 https://fs.anandmsft.com/adfs/                                                                                                                                          |

**Appsettings**节点中的所有其他 IDA： xxxxxxx-xxxx ... 键都可以注释掉或删除

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>将身份验证从 Azure AD 更改为 AD FS

* 打开文件 Startup.Auth.cs
* 删除以下代码

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

替换为

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

#### <a name="modifying-the-todolistcontroller"></a>修改 Todolistcontroller.cs

向 System.web 添加引用。 通过替换以下代码修改类成员

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The App Key is a credential used by the application to authenticate to Azure AD.
    // The Tenant is the name of the Azure AD tenant in which this application is registered.
    // The AAD Instance is the instance of Azure, for example public Azure or Azure China.
    // The Authority is the sign-in URL of the tenant.
    //
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string appKey = ConfigurationManager.AppSettings["ida:AppKey"];

    //
    // To authenticate to the Graph API, the app needs to know the Grah API's App ID URI.
    // To contact the Me endpoint on the Graph API we need the URL as well.
    //
    private static string graphResourceId = ConfigurationManager.AppSettings["ida:GraphResourceId"];
    private static string graphUserUrl = ConfigurationManager.AppSettings["ida:GraphUserUrl"];
    private const string TenantIdClaimType = "https://schemas.microsoft.com/identity/claims/tenantid";

替换为

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**修改用于名称的声明**

从 AD FS 我们发出了 Nmae 声明，但我们未发布 NameIdentifier 声明。 该示例使用 NameIdentifier 在 ToDo 项中使用唯一键。 为简单起见，你可以在代码中安全删除名称声明为的 NameIdentifier。 查找并将出现的所有 NameIdentifier 替换为名称。

**修改 Post 例程和 CallGraphAPIOnBehalfOfUser （）**

将以下代码复制并粘贴到 ToDoListController.cs 中，并将代码替换为 Post 和 CallGraphAPIOnBehalfOfUser

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
      if (!ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
        {
            throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found" });
        }

      //
      // Call the WebAPIOBO On Behalf Of the user who called the To Do list web API.
      //

      string augmentedTitle = null;
      string custommessage = await CallGraphAPIOnBehalfOfUser();

      if (custommessage != null)
        {
            augmentedTitle = String.Format("{0}, Message: {1}", todo.Title, custommessage);
        }
        else
        {
            augmentedTitle = todo.Title;
        }

      if (null != todo && !string.IsNullOrWhiteSpace(todo.Title))
        {
            db.TodoItems.Add(new TodoItem { Title = augmentedTitle, Owner = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value });
            db.SaveChanges();
        }
      }

      public static async Task<string> CallGraphAPIOnBehalfOfUser()
      {
        string accessToken = null;
        AuthenticationResult result = null;
        AuthenticationContext authContext = null;
        HttpClient httpClient = new HttpClient();
        string custommessage = "";

        //
        // Use ADAL to get a token On Behalf Of the current user.  To do this we will need:
        // The Resource ID of the service we want to call.
        // The current user's access token, from the current request's authorization header.
        // The credentials of this application.
        // The username (UPN or email) of the user calling the API
        //

        ClientCredential clientCred = new ClientCredential(clientId, clientSecret);
        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        string userName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn) != null ? ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value : ClaimsPrincipal.Current.FindFirst(ClaimTypes.Email).Value;
        string userAccessToken = bootstrapContext.Token;
        UserAssertion userAssertion = new UserAssertion(bootstrapContext.Token, "urn:ietf:params:oauth:grant-type:jwt-bearer", userName);

        string userId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
        authContext = new AuthenticationContext(authority, false);

        // In the case of a transient error, retry once after 1 second, then abandon.
        // Retrying is optional.  It may be better, for your application, to return an error immediately to the user and have the user initiate the retry.
        bool retry = false;
        int retryCount = 0;

        do
          {
              retry = false;
              try
                {
                    result = await authContext.AcquireTokenAsync(OBOWebAPIBase, clientCred, userAssertion);
                    //result = await authContext.AcquireTokenAsync(...);
                    accessToken = result.AccessToken;
                }
              catch (AdalException ex)
                {
                    if (ex.ErrorCode == "temporarily_unavailable")
                    {
                        // Transient error, OK to retry.
                        retry = true;
                        retryCount++;
                        Thread.Sleep(1000);
                    }
                }
          } while ((retry == true) && (retryCount < 1));

        if (accessToken == null)
          {
              // An unexpected error occurred.
              return (null);
          }

        // Once the token has been returned by ADAL, add it to the http authorization header, before making the call to access the To Do list service.
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

        // Call the WebAPIOBO.
        HttpResponseMessage response = await httpClient.GetAsync(OBOWebAPIBase + "/api/WebAPIOBO");


        if (response.IsSuccessStatusCode)
          {
              // Read the response and databind to the GridView to display To Do items.
              string s = await response.Content.ReadAsStringAsync();
              JavaScriptSerializer serializer = new JavaScriptSerializer();
              custommessage = serializer.Deserialize<string>(s);
              return custommessage;
          }
        else
          {
              custommessage = "Unsuccessful OBO operation : " + response.ReasonPhrase;
          }
        // An unexpected error occurred calling the Graph API.  Return a null profile.
        return (null);
    }

## <a name="running-the-solution"></a>运行解决方案


默认情况下，visual studio 配置为在点击 "调试" 运行时运行一个项目。

* 右键单击解决方案，然后选择 "属性"。
* 在 "属性" 页中，选择 "多个启动项目" 并将所有三个条目的操作更改为 "启动"。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

按 F5 并执行解决方案

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

单击 "登录" 按钮。 系统将提示你使用 AD FS 登录

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

登录后，在列表中添加 ToDo 项。 接下来，我们将对 ToDoListService 执行 Post 操作，该操作会进一步将发布到 WebAPIOBO web API。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

成功操作后，你将看到已将该项添加到列表，其中包含来自后端 Web API 的其他消息，该消息是使用 OBO 身份验证流访问的。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

你还可以在 Fiddler 上查看详细的跟踪。 启动 Fiddler 并启用 HTTPS 解密。 你可以看到，我们向/adfs/oautincludes 终结点发出两个请求。
在第一次交互中，我们向令牌终结点显示访问代码，并获取 https://localhost:44321/ ![ AD FS OBO @ no__t-2 的访问令牌

在第二个与令牌终结点交互的过程中，你可以看到，" **requested_token_use** " 设置为 " **on_behalf_of** "，并且我们正在使用为中间层 web 服务获取的访问令牌，即 https://localhost:44321/ 作为断言来获取代表令牌。
![AD FS OBO @ NO__T-1

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

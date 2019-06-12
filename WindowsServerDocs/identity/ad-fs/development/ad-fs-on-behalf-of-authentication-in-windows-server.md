---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: 生成使用 On-Behalf-Of (OBO) 与 AD FS 2016 或更高版本配合使用 OAuth 的多层应用程序
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 047f297cfaabff3cbbd45057a4198e2fd2e747de
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445453"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>生成使用 On-Behalf-Of (OBO) 与 AD FS 2016 或更高版本配合使用 OAuth 的多层应用程序


本演练提供了用于实现上代表的 (OBO) 身份验证使用 AD FS 中 Windows Server 2016 TP5 或更高版本的说明。 若要详细了解 OBO 身份验证，请阅读[面向开发人员的 AD FS 方案](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>警告：您可以在此处生成的示例是仅供教学使用。 这些说明适用于可能会公开模型的所需的元素的最简单、 最小实现。 该示例可能不包括错误处理的所有方面和其他相关功能，并且着重于仅在获取 OBO 身份验证成功。

## <a name="overview"></a>概述

在此示例中我们将创建其中一个客户端将访问中间层 Web 服务和 web 服务将操作代表经过身份验证的客户端获取访问令牌的身份验证流。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

下面是此示例将实现的身份验证流
1. 客户端向 AD FS 授权终结点进行身份验证并请求授权代码
2. 授权终结点返回到客户端身份验证代码
3. 客户端使用身份验证代码和其呈现给 AD FS 令牌终结点请求访问令牌为 WebAPI 的中间层 Web 服务
4. AD FS 将返回到中间层 Web 服务的访问令牌。 对于其他功能，中间层服务需要访问后端 WebAPI
5. 客户端使用访问令牌使用中间层服务。
6. 中间层服务提供到 AD FS 令牌终结点的访问令牌和请求访问令牌上代表的后端 WebAPI 的身份验证的用户
7. AD FS 将作为客户端到中间层服务 actiing 返回 WebAPI 后端的访问令牌
8. 中间层服务使用在步骤 7 中的 AD fs 提供的访问令牌来访问后端 WebAPI 作为客户端并执行必要的功能

## <a name="sample-structure"></a>示例结构

示例将包含以下三个模块


模块 | 描述
-------|------------
ToDoClient | 用户与之进行交互的本机客户端
ToDoService | 中间层 web API 的作用相当于后端 WebAPI 的客户端
WebAPIOBO | 后端 web api ToDoService 用于当用户添加 ToDoItem 时执行必要的操作




## <a name="setting-up-the-development-box"></a>设置开发环境中

本演练使用 Visual Studio 2015。 项目很大程度使用 Active Directory 身份验证库 (ADAL)。 若要了解有关 ADAL，请阅读[Active Directory 身份验证库.NET](https://msdn.microsoft.com/library/azure/mt417579.aspx)

该示例还使用 SQL LocalDB v11.0。 安装之前在此示例使用 SQL LocalDB。

## <a name="setting-up-the-environment"></a>设置环境
我们将使用的基本设置：

1. **DC**:AD FS 将在其中托管的域的域控制器
2. **AD FS 服务器**:域的 AD FS 服务器
3. **开发计算机**:我们已安装 Visual Studio 和我们的示例将开发计算机

您可以如果你想使用仅两台计算机。 一个用于 DC/ADFS，另一个用于开发示例。

如何设置域控制器和 AD FS 已超出本文的讨论范围。 有关其他部署信息，请参阅：

- [AD DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md)
- [AD FS 部署](../AD-FS-Deployment.md)

该示例是根据现有 OBO 示例针对 Azure 创建 Vittorio bertocci 撰写和可用[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)。 按照的说明来克隆在开发计算机上的项目并创建一份示例后，若要开始使用。

## <a name="clone-or-download-this-repository"></a>克隆或下载此存储库

从 shell 或命令行：

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>修改示例

一旦打开该解决方案 WebAPI OnBehalfOf DotNet.sln，你会注意到解决方案中有两个项目

* **ToDoListClient**:这将用作在用户将与交互的 OpenID 客户端
* **ToDoListService**:这将是中间层 web 服务器应用程序 / 服务，将进行身份验证的用户交互与另一个后端 WebAPI OBO

正如您所看到的我们将需要添加另一个项目更高版本将作为中间层 ToDoListService 将访问的资源。

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>在客户端和 web 服务器应用程序中配置 AD FS

在当前窗体的示例，身份验证配置为针对 Azure AD 完成。 我们想要更改身份验证机制，它针对 AD FS 直接部署在本地。 为此，我们需要在示例中配置 AD FS 以识别客户端和 web 服务器，我们的应用。

**创建应用程序组**

打开 AD FS 管理 MMC，并添加新的应用程序组。 选择本机应用程序 WebAPI 模板。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

单击下一步，你将会看到用于提供有关客户端应用程序的信息页面。 向客户端在 AD FS 中应用适当的名称。 复制客户端标识符并将其保存位置可在以后这将在 visual studio 中的应用程序配置所需访问。

>注意：重定向 URI 可以是任何任意一个 URI，因为实际上不使用时的本机客户端

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

单击下一步，你将会看到用于提供有关 WebAPI 信息页面。 Webapi 为 AD FS 条目的合适名称，并在 Visual Studio 中看到 todolistservice 的 URI 作为输入的重定向 URI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

单击下一步，将看到选择访问控制策略页。 请确保你看到"允许所有人"的策略部分中。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

单击下一步，您将看到配置应用程序权限页。 在此页上，选择作为 openid （默认选中） 和 user_impersonation 允许的作用域。 User_impersonation 作用域是需要能够成功地从 AD FS 请求上的委托访问令牌。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

单击下一步将显示摘要页。 完成该向导的其余部分，然后完成配置。

若要启用上代表的身份验证，我们需要确保 AD FS 返回到客户端具有 user_impersonation 作用域的访问令牌。 修改 ToDoListServiceWebApi 以包含以下三个自定义规则的声明颁发：

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**将 ToDoListService 添加为应用程序组中的客户端**

在此阶段，我们需要在 AD FS 的 web 服务器应用程序以处理作为客户端并不只是作为资源中进行的附加条目。 打开刚刚创建的应用程序组，然后单击添加应用程序。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

系统将显示"添加新的应用程序到 MySampleGroup"页。 在该页上，选择"服务器应用程序或网站"作为独立应用程序

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

单击下一步，您将看到页后，可以提供应用程序的详细信息。 提供名称部分中的配置条目的合适名称。 请确保客户端标识符相同 ToDoListServiceWebAPI 的标识符

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

单击下一步，您将看到页后，可以配置应用程序凭据。 单击"生成一个共享的机密"。 系统将显示自动生成的机密。 复制在某个位置的密码，而我们在 visual studio 中配置 ToDoListService 时，这将是所必需。


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

单击下一步并完成向导。

### <a name="modifying-the-todolistclient-code"></a>修改 ToDoListClient 代码

#### <a name="modify-the-application-config"></a>修改应用程序配置

转到你正在 ToDoListClient 项目 WebAPI OnBehalfOf DotNet 解决方案中。 打开 App.config 文件并进行以下修改之处

* 注释 ida： 租户密钥条目
* 对于 ida： 重定向 Uri，输入你在 AD FS 中配置 MySampleGroup_ClientApplication 时提供的任意 URI。
* Ida: ClientID 密钥提供客户端 ID 配置 MySampleGroup_ClientApplication 时提供的 AD FS 的标识符。
* 对于 ida: ToDoListResourceID 提供的资源 ID 指定在 AD FS 中配置 ToDoListServiceWebApi 时
* 注释密钥 ida: AADInstance
* 对于 ida: ToDoListBaseAddress 输入 ToDoListServiceWebApi 的资源 ID。 这将调用 ToDoList WebAPI 时使用。
* 添加密钥 ida： 颁发机构和 AD FS 作为 URI 提供的值。

你**appSettings** App.Config 中应看起来类似于此：

    <appSettings>
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />-->
    <add key="ida:ClientId" value="c7f7b85c-497c-4589-877f-b17a0bd13398" />
    <add key="ida:RedirectUri" value="https://arbitraryuri.com/" />
    <add key="ida:TodoListResourceId" value="https://localhost:44321/" />
    <!--<add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
    <add key="ida:TodoListBaseAddress" value="https://localhost:44321" />
    <add key="ida:Authority" value="https://fs.anandmsft.com/adfs/"/>
    </appSettings>

#### <a name="modifying-the-code"></a>修改的代码

**MainWindow.xaml.cs**

从应用程序配置读取租户信息的行注释

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

更改字符串机构设置为值

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

更改用于读取 ToDoListResourceId 和 ToDoListBaseAddress 的正确值的代码

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

函数在 mainwindow （） 更改为 authcontext 初始化：

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>添加后端资源

若要完成上的代理流，需要创建将访问 ToDoListService 的后端资源上代表的已经过身份验证的用户。 后端资源的选择而异根据要求，但在此示例中，您可以创建基本的 WebAPI。

* 右键单击解决方案资源管理器中的解决方案 WebAPI OnBehalfOf DotNet，然后选择添加-> 新建项目
* 选择 ASP.NET Web 应用程序模板

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* 在下一步提示上单击更改身份验证
* 选择工作和学校帐户，然后在右侧下拉列表选择本地
* 输入你的 AD FS 部署的 federationmetadata.xml 路径，并提供应用程序 URI （现在，提供任何 URI 并且将在以后更改它） 并单击确定以将项目添加到解决方案。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* 右键单击控制器下创建的新项目在解决方案资源管理器。 选择添加-> 控制器
* 在模板选择选择 Web API 2 控制器-空并单击确定。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* 为提供相应的控制器名称

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* 在控制器中添加以下代码


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

当任何人都将为 WebAPI WebAPIOBO Get 请求时，此代码只需将返回字符串

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>将新的后端 WebAPI 添加到 AD FS

打开 MySampleGroup 应用程序组。 单击添加应用程序，然后选择 Web API 模板，并单击下一步。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

配置 Web API 页上提供适当的名称以及 WebAPI 条目的标识符。 标识符应在 visual studio 中 （类似于我们所做的有关 BackendWebAPIAdfsAdd） 从 WebAPIOBO 项目 SSL URL 值。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

继续完成向导的其余部分相同作为我们配置 ToDoListService WebAPI 时。 在结束应用程序组应看到如下所示：

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>修改 ToDoListService 代码

#### <a name="modifying-the-application-config"></a>修改应用程序配置

* 打开 Web.config 文件
* 修改以下项

| 键                      | ReplTest1                                                                                                                                                                                                                   |
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ida:Audience             | 在提供给 AD FS 配置 ToDoListService WebAPI，例如，时 ToDoListService 的 ID https://localhost:44321/                                                                                         |
| ida:ClientID             | 在提供给 AD FS 配置 ToDoListService WebAPI，例如，时 ToDoListService 的 ID <https://localhost:44321/> </br>**它是非常重要，ida： 受众和 ida: ClientID 相互匹配** |
| ida:ClientSecret         | 这是当你在 AD FS 中配置 ToDoListService 客户端，AD FS 生成的密码                                                                                                                   |
| ida:AdfsMetadataEndpoint | 这是的 URL 为 AD FS 元数据例如 https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml                                                                                             |
| ida:OBOWebAPIBase        | 这是我们将使用为调用后端 API，例如的基址 https://localhost:44300                                                                                                                     |
| ida:Authority            | 这是 AD FS 服务的 URL 示例 https://fs.anandmsft.com/adfs/                                                                                                                                          |

密钥存储在所有其他 ida: XXXXXXX **appsettings**可以注释掉或删除节点

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>从 Azure AD 到 AD FS 更改身份验证

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

#### <a name="modifying-the-todolistcontroller"></a>修改 ToDoListController

添加对 System.Web.Extensions 的引用。 通过将下面的代码替换为修改的类成员

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

**修改用于名称声明，**

从 AD FS 中，我们正在发出 Nmae 声明，但我们不发布 NameIdentifier 声明。 此示例使用 NameIdentifier 到唯一的 ToDo 项中的键。 为简单起见，可以安全地在代码中删除与名称声明 NameIdentifier。 查找和替换名称为 NameIdentifier 的所有匹配项。

**修改 Post 例程和 CallGraphAPIOnBehalfOfUser()**

复制并粘贴以下代码中 ToDoListController.cs，然后的代码替换为用于 Post 和 CallGraphAPIOnBehalfOfUser

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


默认情况下 visual studio 配置为命中调试运行时运行一个项目。

* 右键单击解决方案并选择属性。
* 在属性页中选择多个启动项目并更改要启动的所有三个条目的操作。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

按 F5 并执行此解决方案

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

单击单一登录按钮。 系统会提示以使用 AD FS 登录

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

在您登录，在列表中添加 ToDo 项。 在幕后我们要为 ToDoListService 进一步将执行此操作发布到 WebAPIOBO web API 执行 Post 操作。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

在操作成功后会看到验证该项已添加到与其他消息列表从后端 Web API 使用 OBO 身份验证流访问。

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

此外可以在 Fiddler 查看详细的跟踪。 启动 Fiddler 和启用 HTTPS 解密。 您可以看到，我们对 /adfs/oautincludes 终结点上进行了两个请求。
在第一次互动，我们向令牌终结点的访问代码，并获取访问令牌 https://localhost:44321/ ![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

在与令牌终结点的第二个交互，您可以看到我们有**requested_token_use**设置为**on_behalf_of**并且我们使用的中间层 web 服务，即获取的访问令牌 https://localhost:44321/作为要获取上代表的令牌的断言。
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>后续步骤
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

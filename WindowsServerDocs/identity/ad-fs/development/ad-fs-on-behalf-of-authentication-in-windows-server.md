---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: "构建多级应用程序使用 On-Behalf-Of (OBO) 使用广告 FS 2016 的 OAuth"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8940cde2b78ce3ead499263e6fba0fbe28aae695
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016"></a>构建多级应用程序使用 On-Behalf-Of (OBO) 使用广告 FS 2016 的 OAuth

>适用于：Windows Server 2016

此演练提供了上代表的 (OBO) 使用实施身份验证广告 FS TP5 Windows Server 2016 中的说明。  O 了解 OBO 身份验证，请阅读[广告 FS 方案面向开发人员](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>警告：你可以在以下版本示例是仅用于教育。 这些说明是针对可能以揭示模型的所需的元素的最简单的、最少实现。 示例不可能包括处理错误的所有方面，并且其他与相关功能，并且仅在获取 OBO 身份验证成功着重于。

## <a name="overview"></a>概述

在此示例中我们将创建身份验证流在客户端将访问中间层 Web 服务和 web 服务然后起作用，身份验证的客户端，才能访问令牌代表。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.PNG)

下面是将达到示例身份验证流
1. 客户端验证到广告 FS 授权终止点，并请求验证码
2. 授权端点将身份验证代码返回给客户端
3. 客户端使用身份验证码，并作为 WebAPI 中间层 Web 服务呈现给广告 FS 令牌端点请求访问标记为
4. 广告 FS 返回到中期层 Web 服务的访问标记。 对于其他功能，中间层服务需要访问的后端 WebAPI
5. 客户端使用访问令牌使用中间层服务。
6. 中间层服务提供访问令牌到广告 FS 令牌终止点，并请求访问令牌用于在代表的后端 WebAPI 身份验证的用户
7. 广告 FS 的后端 WebAPI 访问令牌中间层服务 actiing 将作为返回给客户端
8. 中间层服务使用提供的第 7 步中的广告 FS 访问令牌访问后端 WebAPI 作为的客户端和执行必要的功能

## <a name="sample-structure"></a>示例结构

示例将包括三个模块


模块 | 描述
-------|------------
ToDoClient | 本机用户与之交互的客户端
ToDoService | 中间名层 web API 它将作为的后端 WebAPI 客户端
WebAPIOBO | 后端 web api ToDoService 用于用户添加 ToDoItem 时执行必要操作




## <a name="setting-up-the-development-box"></a>设置开发框

此浏览使用 Visual Studio 2015。 项目经常使用 Active Directory 身份验证库 (ADAL)。 要了解详细信息，请阅读 ADAL [Active Directory 身份验证库.NET](https://msdn.microsoft.com/library/azure/mt417579.aspx)

示例还使用 SQL LocalDB v11.0。 安装之前致力于示例 SQL LocalDB。

## <a name="setting-up-the-environment"></a>环境设置
我们将工作与的基本设置：

1. **DC**：将在其中托管广告 FS 域域控制器
2. **广告 FS 服务器**: 域广告 FS 服务器
3. **开发计算机**：我们已安装 Visual Studio，将会开发我们示例计算机

你可以如果你希望，使用仅两台计算机。 一个用于 DC/ADFS，另一个用于开发示例。

如何设置域控制器和广告 FS 是阐述的这篇文章。 有关更多部署的信息，请参阅：

- [广告 DS 部署](../../ad-ds/deploy/AD-DS-Deployment.md)
- [广告 FS 部署](../AD-FS-Deployment.md)

示例是基于创建者 Vittorio Bertocci Azure 针对现有 OBO 示例及可用[此处](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof)。 按照说明复制你的开发计算机上的项目并创建一份示例，若要开始使用。

## <a name="clone-or-download-this-repository"></a>复制或下载此知识库

从 shell 或命令行：

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>修改示例

立即打开 WebAPI-OnBehalfOf-DotNet.sln 的解决方案，你将注意到你解决方案-中有两个项目

* **ToDoListClient**：它将作为用户互动 OpenID 客户端
* **ToDoListService**：这将是中间层 web 服务器应用 / 服务，将会与交互另一台的后端 WebAPI OBO 身份验证的用户

如你所见，我们需要添加另一台项目稍后充当将由中间层 ToDoListService 访问的资源。

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>配置的客户端和 web 服务器应用广告 FS

在当前表单中的示例，身份验证配置为 Azure AD 对。 我们想要更改的身份验证机制并直接本地部署迈进广告 FS 它。 为了执行此操作，我们需要在样本中配置广告识别客户端和对我们的 web 服务器应用。

**创建一个应用程序组**

打开广告 FS 管理 MMC 并添加了新的应用程序组。 选择本机-应用程序 WebAPI 模板。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

单击下一步，你将看到对提供了有关客户端应用程序的信息页。 提供给客户端应用广告 FS 中适当的名称。 将复制的客户端标识符，并将其保存到某处这将需要在 visual studio 中的应用程序配置稍后可以访问。

>注意：非常不使用情形本机客户端，将重定向 URI 可以是任何任意 URI

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

单击下一步，你将看到提供有关 WebAPI 信息页面。 对于 WebAPI 提供有关广告 FS 条目合适的名称和与您在 Visual Studio 中看到的 ToDoListService URI 输入重定向 URI

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

单击接下来，你将看到选择访问控制策略页面。 确保你看到的"允许任何人"在策略部分。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

单击下一步，你将看到配置应用程序的权限页面。 在此页上，选择允许的范围 user_impersonation 以及 openid（选择默认）。 范围 'user_impersonation 有必要能够成功地从广告 FS 请求访问上代表的标记。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

接下来单击将显示汇总的页面。 完成向导中的其余部分，并完成配置。

启用上代表的身份验证，以便我们都需要确保广告 FS 返回给客户端与范围内 user_impersonation 访问标记。 修改 ToDoListServiceWebApi 包含以下三种自定义规则的索赔颁发：

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue open id scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "openid");

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**作为客户端应用程序组中添加 ToDoListService**

在这个阶段，我们需要广告 FS 采取措施，为客户端，而不仅仅资源 web 服务器应用中进行额外的项。 打开你刚创建的应用程序组，然后单击添加的应用程序。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

你将看到"添加新的应用程序 MySampleGroup"页面。 在此页上，选择"服务器应用或网站"作为独立应用程序

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

单击接下来，你将看到可提供应用的详细信息页面。 提供了合适的名称名称部分中的配置条目。 确保客户端标识符相同 ToDoListServiceWebAPI 的标识符

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

接下来单击，然后你将看到页面来配置应用程序的凭据。 单击"生成共享的密钥"。 你将看到的密码，可自动生成。 这将需要时，我们在 visual studio 中配置 ToDoListService 复制某些位置的幽静。


![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

单击下一步，并完成向导。

### <a name="modifying-the-todolistclient-code"></a>修改 ToDoListClient 代码

#### <a name="modify-the-application-config"></a>修改了应用程序的配置

转到你正在 WebAPI-OnBehalfOf 最低解决方案 ToDoListClient 项目。 打开 App.config 文件，然后进行以下修改

* Ida：租户项进行评论
* 有关 ida: RedirectURI 输入了你提供广告 FS 中配置 MySampleGroup_ClientApplication 时的任意 URI。
* 客户机 Id ida：键、提供客户端广告 FS 配置 MySampleGroup_ClientApplication 时提供的 ID 标识符。
* Ida: ToDoListResourceID 提供资源 ID 为你提供给广告 FS 中配置 ToDoListServiceWebApi 时
* 进行关键 ida: AADInstance 评论
* 对于 ida: ToDoListBaseAddress 输入 ToDoListServiceWebApi 的资源 ID。 这将调用的应做事项列表 WebAPI 时使用。
* 添加关键 ida：颁发机构并提供与 URI 广告 fs 的值。

你**appSettings**在 App.Config 应该与此类似：

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

评论应用程序的配置从阅读租户信息的行

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

更改为字符串颁发机构的值

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

若要阅读 ToDoListResourceId 和 ToDoListBaseAddress 正确的值的代码更改

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

在该函数中 MainWindow() 更改为 authcontext 初始化：

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>添加的后端资源

你需要完成上代表的流程，以便创建将访问 ToDoListService 的后端资源上代表的身份验证的用户。 根据需要，而异的后端资源的选项，但对于此示例中，你可以创建基本 WebAPI。

* 解决方案 ' WebAPI OnBehalfOf 最低在 solution explorer 中右键单击，然后选择添加-> 新建项目
* 选择 ASP.NET Web 应用程序模板

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* 在下一个提示上单击更改身份验证
* 选择工作并学校帐户，然后在正确的下拉列表中，依次选择本地
* 输入你的广告 FS 部署 federationmetadata.xml 路径，并提供应用 URI（目前提供任何 URI 和你稍后将更改），然后单击确定要将项目添加到解决方案。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* 右键单击控制器在下创建的新项目 solution explorer 中。 选择添加-> 控制器
* 在模板选择中，依次选择 Web API 2 控制器-清空，然后单击确定。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* 提供相应控制器名称

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* 控制器中添加以下代码


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

这段代码将只需返回字符串时的任何人都将获取请求 WebAPI WebAPIOBO 为

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>添加新的后端 WebAPI 广告 FS 到

打开应用程序 MySampleGroup 组。 单击添加的应用程序和选择 Web API 模板，并单击下一步。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

配置 Web API 页面上提供以及 WebAPI 条目的标识符适当的名称。 标识符（类似于我们所做的 BackendWebAPIAdfsAdd）visual studio 中应该 WebAPIOBO 项目中的值 SSL URL。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

完成向导中的其余部分相同按继续时，我们配置 ToDoListService WebAPI。 在结束你的应用程序组应该如下所示下方：

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>修改 ToDoListService 代码

#### <a name="modifying-the-application-config"></a>修改了应用程序的配置

* 打开 Web.config 文件
* 修改下键

| 键 | 值 |
|:-----|:-------|
|受众 ida:| 随着提供给广告 FS 配置 ToDoListService WebAPI，例如，同时 https://localhost:44321/ ToDoListService 的 ID|
|客户机 Id ida:| 随着提供给广告 FS 配置 ToDoListService WebAPI，例如，同时 https://localhost:44321/ ToDoListService 的 ID </br>**非常重要的 ida：受众和 ida：客户机 Id 匹配彼此**|
|ida: ClientSecret| 这是你已在广告 FS 配置 ToDoListService 客户端时，将生成广告 FS 幽静|
|ida: ADFSMetadata| 这是对你的广告 FS 元数据，例如 https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml URL|
|ida: OBOWebAPIBase| 这是我们将使用称呼后端 API，例如 https://localhost:44300 的基址|
|ida：颁发机构| 这是用于广告 FS 服务，https://fs.anandmsft.com/adfs/ 的示例 URL|


中的所有其他 ida: XXXXXXX 键**appsettings**节点可以注释掉或删除

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>广告 FS 到 Azure AD 从更改身份验证

* 打开文件 Startup.Auth.cs
* 删除以下代码

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

与

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

添加到 System.Web 参考。扩展。 修改类成员更换下面代码

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

与

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**修改索赔用于名称**

从广告 FS 我们发布 Nmae 声明中，但我们不发布 NameIdentifier 索赔。 示例使用 NameIdentifier 唯一的应做事项项目中的键。 为简单起见，你可以放心地删除与名声明 NameIdentifier，在代码中。 查找并替换名称的 NameIdentifier 所有实例。

**修改文章例程和 CallGraphAPIOnBehalfOfUser()**

复制和粘贴 ToDoListController.cs 在下面的代码的博客文章和 CallGraphAPIOnBehalfOfUser 替换代码

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
        if (!ClaimsPrincipal.Current.FindFirst("https://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
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
        //      The Resource ID of the service we want to call.
        //      The current user's access token, from the current request's authorization header.
        //      The credentials of this application.
        //      The username (UPN or email) of the user calling the API
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
                result = authContext.AcquireToken(OBOWebAPIBase, clientCred, userAssertion);
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


默认情况下 visual studio 配置为当你按下调试运行运行一个项目。

* 右键单击解决方案并选择属性。
* 属性页中选择启动的多个项目，然后更改为所有三种条目开始操作。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

按 F5 和执行解决方案

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

单击登录按钮。 系统将提示你使用的广告 FS 登录

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

之后你登录，在列表中添加的应做事项项。 在后台我们要向 ToDoListService 进一步将执行到 WebAPIOBO web API 的文章这些操作的博客文章。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

上成功操作，你将看到，项目已添加到列表其他消息后端这使用 OBO auth 流访问 Web API。

![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

你还可以 Fiddler 上看到详细的跟踪。 启动 Fiddler 并启用 HTTPS 解密。 你可以看到我们为 /adfs/oautincludes 端点进行两个请求。
在第一个交互，我们展示到令牌端点访问代码并获取 https://localhost:44321/ 令牌访问![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

在第二个与令牌端点交互，你可以看到我们**requested_token_use**设为**on_behalf_of**并且我们也在使用访问令牌中间层 web 服务，即肯定以获取在代表的标记为 https://localhost:44321/ 获得。
![广告 FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>后续步骤
[广告 FS 开发](../../ad-fs/AD-FS-Development.md)  

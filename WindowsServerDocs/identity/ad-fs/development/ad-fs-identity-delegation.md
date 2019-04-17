---
title: "与广告 FS 身份委派方案"
description: "此项 scenario 描述需要在访问需要执行访问控制检查身份委派链的后端资源的应用。"
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>与广告 FS 身份委派方案


[从.NET Framework 4.5 开始，Windows 身份基础 (WIF) 已完全集成到.NET Framework。 通过本主题中，WIF 3.5 解决 WIF 的版本中不再支持和仅用于当针对.NET Framework 3.5 SP1 或.NET Framework 4 开发。 有关详细信息 WIF.NET Framework 4.5，也称为 WIF 4.5，在看到.NET Framework 4.5 Development 指南中的 Windows 的身份基础文档。] 

此项 scenario 描述需要在访问需要执行访问控制检查身份委派链的后端资源的应用。 简单身份委派链通常包含初始呼叫和直接调用方的身份的信息。

与 Windows 平台今天上 Kerberos 委派模型的后端资源具有访问仅次于直接调用方的身份，而不的初始呼叫。 此模型通常称为受信任的子系统模型。 WIF 维护初始呼叫以及在使用演员属性委派链直接调用方的身份。

下图说明典型身份委派方案中 Fabrikam 员工访问公开 Contoso.com 应用程序中的资源。

![身份](media/ad-fs-identity-delegation/id1.png)

是虚构参与此项 scenario 的用户：

- Frank: Fabrikam 员工想要访问 Contoso 资源。
- 王：Contoso 应用程序开发人员实现应用程序中的所需的更改。
- Adam: Contoso IT 管理员。

都参与此项 scenario 组件：

- web1：需要初始呼叫委派的身份的后端资源的链接 Web 应用。 此应用程序与 ASP.NET 基础上生成。
- 访问 SQL Server，这要求委派的身份初始调用方，以及，直接调用方的 Web 服务。 使用 WCF 构建此服务。
- sts1: STS 处于索赔提供商，角色并发出应应用程序 (web1) 的索赔。 它已建立信任还与 Fabrikam.com 和应用程序。
- sts2: STS 处于身份提供商的 Fabrikam.com 角色并提供 Fabrikam 员工用来验证身份一个终止点。 它建立与 Contoso.com 信任，以便允许 Fabrikam 员工访问 Contoso.com 上的资源。

>[!NOTE] 
>术语"ActAs 令牌"，它通常在使用此方案，是指由 STS 并包含的用户身份一个标记。 参与者属性包含 STS 的身份。

在上图中显示，请在此情况下流时：


1. 配置 Contoso 应用程序以获取包含 Fabrikam 员工身份和演员属性中的直接调用方身份一个 ActAs 标记。 王已实施这些更改向应用程序。
2. 配置 Contoso 应用程序向后端服务传递 ActAs 标记。 王已实施这些更改向应用程序。
3. 通过调用 sts1 验证 ActAs 令牌配置 Contoso Web 服务。 Adam 已启用 sts1 处理委派请求。
4. Fabrikam 用户 Frank 访问 Contoso 应用程序，并向后端资源授予访问权限。

## <a name="set-up-the-identity-provider-ip"></a>设置身份提供商 (IP)

有三个选项可用于 Fabrikam.com 管理员，Frank:


1. 购买并安装 STS 产品，如 Active Directory® 联合身份验证服务 (广告 FS)。
2. 订阅云 STS 产品，如 LiveID STS。
3. 生成的自定义 STS 使用 WIF。

此示例情况下，我们假定 Frank 选择选项 1，并且会在广告 FS 安装 IP STS。 他还配置终止点，名为 \windowsauth 用户进行身份验证。 引用的广告 FS 产品文档和交谈 Adam，Contoso IT 管理员，通过 Frank 建立信任与 Contoso.com 域。

## <a name="set-up-the-claims-provider"></a>设置索赔提供程序

选项可用于 Contoso.com 管理员，Adam，都相同前面所述的身份提供商。 此示例情况下，我们假定 Adam 选择选项 1，并且会在广告 FS 2.0 安装 RP STS。

## <a name="set-up-trust-with-the-ip-and-application"></a>信任 IP 和应用程序设置

通过参考广告 FS 文档，Adam 建立信任 Fabrikam.com 和应用程序。

## <a name="set-up-delegation"></a>设置委派

广告 FS 提供委派处理。 通过参考广告 FS 文档，Adam 使 ActAs 标记处理。

## <a name="application-specific-changes"></a>更改特定应用程序

必须进行以下更改将对身份委派支持添加到现有的应用程序。 王使用 WIF 进行这些更改。


- 缓存引导令牌收到来自 sts1 该 web1。
- 使用使用已颁发令牌 CreateChannelActingAs 创建的后端 Web 服务的频道。
- 呼叫后端服务的方法。

## <a name="cache-the-bootstrap-token"></a>缓存的引导标记

引导令牌颁发 STS 的初始标记，并且应用程序，提取索赔。 在此示例情况下，为用户 Frank sts1 发出此令牌和应用程序缓存它。 下面的示例代码说明如何检索引导 ASP.NET 应用中标记：

```
// Get the Bootstrap Token
SecurityToken bootstrapToken = null;

IClaimsPrincipal claimsPrincipal = Thread.CurrentPrincipal as IClaimsPrincipal;
if ( claimsPrincipal != null )
{
    IClaimsIdentity claimsIdentity = (IClaimsIdentity)claimsPrincipal.Identity;
    bootstrapToken = claimsIdentity.BootstrapToken;
}
```
WIF 提供一种方法，[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)，用于创建扩大与指定的安全标记为 ActAs 元素令牌发放请求指定类型的频道。 可以将引导标记传递到此方法并返回信道对必要服务方法。 在此示例情况下，具有 Frank 的身份[演员](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx)属性设置为 web1 的身份。

下面的代码片段演示如何对通过 Web 服务调用[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)，然后返回的频道上调用 ComputeResponse，该服务的方法之一：

```
// Get the channel factory to the backend service from the application state
ChannelFactory<IService2Channel> factory = (ChannelFactory<IService2Channel>)Application[Global.CachedChannelFactory];

// Create and setup channel to talk to the backend service
IService2Channel channel;
lock (factory)
{
// Setup the ActAs to point to the caller's token so that we perform a 
// delegated call to the backend service
// on behalf of the original caller.
    channel = factory.CreateChannelActingAs<IService2Channel>(callerToken);
}

string retval = null;

// Call the backend service and handle the possible exceptions
try
{
    retval = channel.ComputeResponse(value);
    channel.Close();
} catch (Exception exception)
{
    StringBuilder sb = new StringBuilder();
    sb.AppendLine("An unexpected exception occurred.");
    sb.AppendLine(exception.StackTrace);
    channel.Abort();
    retval = sb.ToString();
}

```
## <a name="web-service-specific-changes"></a>Web 特定于服务的更改

由于 Web 服务 WCF 生成，并且启用 WIF，绑定配置 IssuedSecurityTokenParameters 使用了正确的发行商地址后，将自动通过 WIF 进行处理 ActAs 验证。 

Web 服务公开特定所需的应用程序的方法。 有任何特定的代码更改，需要在服务上。 下面的示例代码显示与 IssuedSecurityTokenParameters Web 服务的配置：

```
// Configure the issued token parameters with the correct settings
IssuedSecurityTokenParameters itp = new IssuedSecurityTokenParameters( "http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" );
itp.IssuerMetadataAddress = new EndpointAddress( "http://localhost:6000/STS/mex" );
itp.IssuerAddress = new EndpointAddress( "http://localhost:6000/STS" );

// Create the security binding element
SecurityBindingElement sbe = SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement( itp );
sbe.MessageSecurityVersion = MessageSecurityVersion.WSSecurity11WSTrust13WSSecureConversation13WSSecurityPolicy12BasicSecurityProfile10;

// Create the HTTP transport binding element
HttpTransportBindingElement httpBE = new HttpTransportBindingElement();

// Create the custom binding using the prepared binding elements
CustomBinding binding = new CustomBinding( sbe, httpBE );

using ( ServiceHost host = new ServiceHost( typeof( Service2 ), new Uri( "http://localhost:6002/Service2" ) ) )
{
    host.AddServiceEndpoint( typeof( IService2 ), binding, "" );
    host.Credentials.ServiceCertificate.SetCertificate( "CN=localhost", StoreLocation.LocalMachine, StoreName.My );

// Enable metadata generation via HTTP GET
    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
    smb.HttpGetEnabled = true;
    host.Description.Behaviors.Add( smb );
    host.AddServiceEndpoint( typeof( IMetadataExchange ), MetadataExchangeBindings.CreateMexHttpBinding(), "mex" );

// Configure the service host to use WIF
    ServiceConfiguration configuration = new ServiceConfiguration();
    configuration.IssuerNameRegistry = new TrustedIssuerNameRegistry();

    FederatedServiceCredentials.ConfigureServiceHost( host, configuration );

    host.Open();

    Console.WriteLine( "Service2 started, press ENTER to stop ..." );
    Console.ReadLine();

    host.Close();
}
```

## <a name="next-steps"></a>后续步骤
[广告 FS 开发](../../ad-fs/AD-FS-Development.md)  

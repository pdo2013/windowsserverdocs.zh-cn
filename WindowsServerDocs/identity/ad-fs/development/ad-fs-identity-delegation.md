---
title: 标识委派方案中使用 AD FS
description: 此方案中描述的应用程序需要访问后端资源的标识委派链执行访问控制检查。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819848"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>标识委派方案中使用 AD FS


[从.NET Framework 4.5 开始，Windows Identity Foundation (WIF) 已完全集成到.NET Framework。 版本的 WIF 所处理的本主题中，WIF 3.5 中，已弃用，仅用于针对.NET Framework 3.5 SP1 或.NET Framework 4 进行开发时。 有关在.NET Framework 4.5 中，也称为 WIF 4.5 中，WIF 的详细信息请参阅.NET Framework 4.5 开发指南中的 Windows Identity Foundation 文档。] 

此方案中描述的应用程序需要访问后端资源的标识委派链执行访问控制检查。 简单的标识委派链通常包含初始调用方和直接调用方的标识的信息。

目前在 Windows 平台上的 Kerberos 委派模型，使用后端资源可以访问仅直接调用方的标识而不属于的初始调用方。 此模型通常称为受信任的子系统模型。 WIF 维护的初始调用方，以及委派链使用执行组件属性中的直接调用方的标识。

下图说明了典型的标识委派方案中的 Fabrikam 员工可访问 Contoso.com 应用程序中公开的资源。

![标识](media/ad-fs-identity-delegation/id1.png)

在此方案中参与的虚构用户是：

- Frank:Fabrikam 员工想要访问 Contoso 资源。
- Daniel:Contoso 应用程序开发人员在应用程序中实现必要的更改。
- Adam:Contoso IT 管理员。

在此方案中所涉及的组件包括：

- web1:使用 Web 应用程序链接到要求进行初始调用方的委派的标识的后端资源。 此应用程序是使用 ASP.NET 构建的。
- Web 服务访问 SQL Server，这要求初始调用方，以及直接调用方的委派的标识。 使用 WCF 构建此服务。
- sts1:STS 角色的声明提供程序，并发出声明所需的应用程序 (web1)。 它已建立信任 Fabrikam.com 和与应用程序。
- sts2:STS 是 Fabrikam.com 的标识提供程序的角色中并提供 Fabrikam 员工使用进行身份验证终结点。 它建立与 Contoso.com 信任关系，以便允许 Fabrikam 员工访问 Contoso.com 上的资源。

>[!NOTE] 
>术语"ActAs 令牌"，它通常在这种情况下，是指由 STS 颁发并包含用户的标识的令牌。 执行组件属性包含 STS 的标识。

上图中所示，在此方案中的流是：


1. Contoso 应用程序配置为获取一个包含 Fabrikam 员工的标识和执行组件属性中的直接调用方的标识的 ActAs 令牌。 Daniel 已实现这些更改应用程序。
2. Contoso 应用程序配置为将 ActAs 令牌传递到后端服务。 Daniel 已实现这些更改应用程序。
3. Contoso Web 服务配置为通过调用 sts1 验证 ActAs 令牌。 Adam 具有启用 sts1 来处理委派请求。
4. Fabrikam 用户 Frank 访问 Contoso 应用程序，并对后端资源授予访问权限。

## <a name="set-up-the-identity-provider-ip"></a>设置标识提供者 (IP)

有三个选项可用于 Fabrikam.com 管理员，Frank:


1. 购买并安装一个 STS 产品，如 Active Directory® 联合身份验证服务 (AD FS)。
2. 订阅 LiveID STS 之类的云 STS 产品。
3. 生成自定义 STS 使用 WIF。

对于此示例方案中，我们假定 Frank 选择选项 1，并将 AD FS 安装为的 IP-STS。 他还配置一个终结点，名为 \windowsauth，用户进行身份验证。 通过引用 AD FS 产品文档并与 Adam，Contoso IT 管理员，Frank 建立信任关系的 Contoso.com 域。

## <a name="set-up-the-claims-provider"></a>设置声明提供程序

选项适用于 Contoso.com 管理员，Adam，是与标识提供程序的以前所述相同。 对于此示例方案中，我们假定 Adam 选择选项 1，并将 AD FS 2.0 安装为 RP-STS。

## <a name="set-up-trust-with-the-ip-and-application"></a>设置信任的 IP 和应用程序

通过引用 AD FS 文档，Adam 建立 Fabrikam.com 和应用程序之间的信任。

## <a name="set-up-delegation"></a>设置委派

AD FS 提供委派处理。 通过引用 AD FS 文档，Adam 可以 ActAs 令牌的处理。

## <a name="application-specific-changes"></a>特定于应用程序的更改

必须进行以下更改以将标识委托支持添加到现有应用程序。 Daniel 使用 WIF 来进行这些更改。


- 缓存的启动令牌从 sts1 收到该 web1。
- 使用与颁发的令牌 CreateChannelActingAs 创建后端 Web 服务的通道。
- 调用后端服务的方法。

## <a name="cache-the-bootstrap-token"></a>缓存引导标记

启动令牌是由 STS 颁发的初始令牌和应用程序从其提取声明。 在此示例方案中，此令牌颁发由 sts1 Frank 的用户和应用程序对其进行缓存。 下面的代码示例演示如何检索引导令牌中的 ASP.NET 应用程序：

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
WIF 提供了一种方法， [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)，创建增加与为 ActAs 元素指定的安全令牌的令牌颁发请求的内容的指定类型的通道。 您可以将启动令牌传递给此方法，然后返回通道上调用必要的服务方法。 在此示例方案中，Frank 的标识具有[Actor](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx)属性设置为 web1 的标识。

下面的代码段演示如何使用 Web 服务调用[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) ，然后对返回的通道调用服务的方法，ComputeResponse，之一：

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
## <a name="web-service-specific-changes"></a>Web 服务相关的更改

由于 Web 服务是使用 WCF 构建，并且已启用 wif，绑定使用正确的颁发者地址与 IssuedSecurityTokenParameters 进行配置后，自动由 WIF 处理的 ActAs 验证。 

Web 服务公开的应用程序所需的特定方法。 没有需要在服务上的特定代码更改。 下面的代码示例显示了具有 IssuedSecurityTokenParameters 的 Web 服务的配置：

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
[AD FS 开发](../../ad-fs/AD-FS-Development.md)  

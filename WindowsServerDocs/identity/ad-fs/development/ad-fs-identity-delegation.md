---
title: 具有 AD FS 的标识委托方案
description: 此方案描述需要访问后端资源的应用程序，这些资源需要标识委派链来执行访问控制检查。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2c461e1051e59fcdb533c00b45157545ffb15df1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867445"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>具有 AD FS 的标识委托方案


[从 .NET Framework 4.5 开始，Windows Identity Foundation （WIF）已完全集成到 .NET Framework 中。 本主题寻址的 WIF 版本 WIF 3.5 已弃用，只应在针对 .NET Framework 3.5 SP1 或 .NET Framework 4 进行开发时使用。 有关 .NET Framework 4.5 中的 WIF （也称为 WIF 4.5）的详细信息，请参阅 .NET Framework 4.5 开发指南中的 Windows Identity Foundation 文档。] 

此方案描述需要访问后端资源的应用程序，这些资源需要标识委派链来执行访问控制检查。 简单的标识委托链通常包含初始调用方的信息以及直接调用方的标识。

现在，在 Windows 平台上使用 Kerberos 委托模型，后端资源只能访问直接调用方的标识，而不能访问初始调用方的标识。 此模型通常称为受信任的子系统模型。 WIF 使用执行组件属性来维护初始调用方的标识，以及委托链中的直接调用方的标识。

下图显示了一个典型的标识委托方案，其中 Fabrikam 员工访问 Contoso.com 应用程序中公开的资源。

![标识](media/ad-fs-identity-delegation/id1.png)

参与此方案的虚构用户包括：

- Frank要访问 Contoso 资源的 Fabrikam 员工。
- Daniel在应用程序中实现必要更改的 Contoso 应用程序开发人员。
- AdamContoso IT 管理员。

此方案涉及的组件包括：

- web1一个 Web 应用程序，其中包含指向需要初始调用方的委托标识的后端资源的链接。 此应用程序是通过 ASP.NET 生成的。
- 访问 SQL Server 的 Web 服务，它需要初始调用方的委托标识以及直接调用方的委托标识。 此服务是通过 WCF 生成的。
- sts1:作为声明提供程序角色的 STS，并发出应用程序（web1）所需的声明。 它与 Fabrikam.com 和应用程序建立了信任关系。
- sts2:作为 Fabrikam.com 的标识提供者角色的 STS，提供 Fabrikam 员工用于进行身份验证的终结点。 它与 Contoso.com 建立了信任关系，以便 Fabrikam 员工可以访问 Contoso.com 上的资源。

>[!NOTE] 
>通常在此方案中使用的术语 "ActAs token" 是指由 STS 颁发并包含用户标识的令牌。 执行组件属性包含 STS 的标识。

如前面的关系图中所示，此方案中的流为：


1. Contoso 应用程序配置为获取一个 ActAs 令牌，其中包含 Fabrikam 员工的标识和参与者属性中的直接调用方的标识。 Daniel 已实现对应用程序所做的这些更改。
2. Contoso 应用程序配置为将 ActAs 令牌传递到后端服务。 Daniel 已实现对应用程序所做的这些更改。
3. Contoso Web 服务配置为通过调用 sts1 来验证 ActAs 标记。 Adam 已启用 sts1 来处理委托请求。
4. Fabrikam 用户 Frank 访问 Contoso 应用程序，并向其授予对后端资源的访问权限。

## <a name="set-up-the-identity-provider-ip"></a>设置标识提供程序（IP）

Fabrikam.com 管理员可以使用三个选项，Frank：


1. 购买并安装 STS 产品，如 Active Directory®联合身份验证服务（AD FS）。
2. 订阅云 STS 产品，如 LiveID STS。
3. 使用 WIF 生成自定义 STS。

在此示例方案中，假设 Frank 选择选项1，并将 AD FS 安装为 IP STS。 他还配置名为 "\windowsauth" 的终结点，对用户进行身份验证。 通过参考 AD FS 产品文档并与 Adam 通信，Contoso IT 管理员，Frank 建立了与 Contoso.com 域的信任。

## <a name="set-up-the-claims-provider"></a>设置声明提供程序

Contoso.com 管理员的可用选项（Adam）与前面为标识提供程序所述的选项相同。 在此示例方案中，我们假定 Adam 选择选项1并安装 AD FS 2.0 作为 RP-STS。

## <a name="set-up-trust-with-the-ip-and-application"></a>设置与 IP 和应用程序的信任关系

通过参考 AD FS 文档，Adam 在 Fabrikam.com 与应用程序之间建立了信任关系。

## <a name="set-up-delegation"></a>设置委派

AD FS 提供委派处理。 通过参考 AD FS 文档，Adam 允许处理 ActAs 令牌。

## <a name="application-specific-changes"></a>特定于应用程序的更改

若要向现有应用程序添加对标识委托的支持，必须进行以下更改。 Daniel 使用 WIF 进行这些更改。


- 缓存 web1 从 sts1 接收的启动标记。
- 使用带有已颁发令牌的 CreateChannelActingAs，创建到后端 Web 服务的通道。
- 调用后端服务的方法。

## <a name="cache-the-bootstrap-token"></a>缓存启动标记

启动标记是 STS 颁发的初始标记，应用程序从中提取声明。 在此示例方案中，此令牌由 sts1 为用户 Frank 颁发，应用程序将缓存该令牌。 下面的代码示例演示如何在 ASP.NET 应用程序中检索启动令牌：

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
WIF 提供了一个[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)方法，该方法创建一个指定类型的通道，该通道使用指定的安全令牌作为 ActAs 元素来补充令牌颁发请求。 可以将启动令牌传递给此方法，然后在返回的通道上调用必要的服务方法。 在此示例方案中，Frank 的标识已[将执行组件属性设置](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx)为 "web1's 标识"。

下面的代码片段演示了如何通过[CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx)调用 Web 服务，然后在返回的通道上调用服务的一种 ComputeResponse 方法：

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
## <a name="web-service-specific-changes"></a>特定于 Web 服务的更改

由于 Web 服务是使用 WCF 生成并为 WIF 启用的，因此，一旦使用正确的颁发者地址为 IssuedSecurityTokenParameters 配置了绑定，则 WIF 会自动处理 ActAs 的验证。 

Web 服务公开应用程序所需的特定方法。 不需要对服务进行特定的代码更改。 下面的代码示例演示了具有 IssuedSecurityTokenParameters 的 Web 服务的配置：

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

---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: 使用 AD FS 预身份验证发布应用程序
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: bd5c4c97e01942e7c5ab8ed1aba3fcf92030ac59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404264"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>使用 AD FS 预身份验证发布应用程序

>适用于：Windows Server 2016

@no__t 0This 内容与 Web 应用程序代理的本地版本相关。若要启用对云中的本地应用程序的安全访问，请参阅[Azure AD 应用程序代理内容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。 **  
  
本主题介绍如何使用 Active Directory 联合身份验证服务（AD FS）预身份验证通过 Web 应用程序代理发布应用程序。  
  
对于可使用 AD FS 预身份验证发布的所有类型的应用程序，必须将 AD FS 信赖方信任添加到联合身份验证服务。  
  
常规 AD FS 预身份验证流如下所示：  
  
> [!NOTE]  
> 此身份验证流不适用于使用 Microsoft Store 应用的客户端。  
  
1.  客户端设备尝试访问特定资源 URL 上的已发布 web 应用程序;例如 https://app1.contoso.com/ 。  
  
    资源 URL 是 Web 应用程序代理侦听传入 HTTPS 请求的公用地址。  
  
    如果启用了 HTTP 到 HTTPS 的重定向，则 Web 应用程序代理还会侦听传入的 HTTP 请求。  
  
2.  Web 应用程序代理使用 URL 编码的参数（包括资源 URL 和 appRealm （一个信赖方标识符））将 HTTPS 请求重定向到 AD FS 服务器。  
  
    用户使用 AD FS 服务器所需的身份验证方法进行身份验证;例如，用户名和密码、带一次性密码的双重身份验证，等等。  
  
3.  对用户进行身份验证后，AD FS 服务器将颁发一个安全令牌（"边缘令牌"），其中包含以下信息，并将 HTTPS 请求重定向回 Web 应用程序代理服务器：  
  
    -   用户尝试访问的资源标识符。  
  
    -   用户主体名称（UPN）形式的用户标识。  
  
    -   访问授权审批过期时间；也就是说，将向用户授予有限时间的访问权限，在此期限过后，需要重新对用户进行身份验证。  
  
    -   边缘令牌中的信息签名。  
  
4.  Web 应用程序代理从具有边缘令牌的 AD FS 服务器接收重定向的 HTTPS 请求，并按如下所示验证和使用该令牌：  
  
    -   验证边缘令牌签名是否来自在 Web 应用程序代理配置中配置的联合身份验证服务。  
  
    -   验证令牌是否是针对正确的应用程序颁发的。  
  
    -   验证令牌是否未过期。  
  
    -   根据需要使用用户标识；例如，如果后端服务器配置为使用集成 Windows 身份验证，则获取 Kerberos 票证。  
  
5.  如果边缘令牌有效，则 Web 应用程序代理使用 HTTP 或 HTTPS 将 HTTPS 请求转发到发布的 Web 应用程序。  
  
6.  现在，客户端可以访问发布的 Web 应用程序；但是，可将发布的应用程序配置为要求用户执行其他身份验证。 例如，如果发布的 Web 应用程序是 SharePoint 站点且不需要执行其他身份验证，则用户将在浏览器中看到该 SharePoint 站点。  
  
7.  Web 应用程序代理在客户端设备上保存 cookie。 Web 应用程序代理使用 cookie 来确定此会话已被预身份验证并且不需要进一步的预身份验证。  
  
> [!IMPORTANT]  
> 配置外部 URL 和后端服务器 URL 时，请确保输入完全限定的域名 (FQDN) 而不是 IP 地址。  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_1.1"></a>为 Web 浏览器客户端发布基于声明的应用程序  
若要发布使用声明进行身份验证的应用程序，必须向联合身份验证服务添加该应用程序的信赖方信任。  
  
在发布基于声明的应用程序以及从浏览器访问应用程序时，要执行的常规身份验证流如下：  
  
1.  客户端尝试使用 web 浏览器访问基于声明的应用程序;例如， https://appserver.contoso.com/claimapp/ 。  
  
2.  Web 浏览器将 HTTPS 请求发送到 Web 应用程序代理服务器，该服务器会将请求重定向到 AD FS 服务器。  
  
3.  AD FS 服务器对用户和设备进行身份验证，并将请求重定向回 Web 应用程序代理。 现在，该请求包含边缘令牌。 AD FS 服务器将单一登录（SSO） cookie 添加到请求，因为用户已对 AD FS 服务器执行身份验证。  
  
4.  Web 应用程序代理验证令牌，添加自身的 cookie，并将请求转发到后端服务器。  
  
5.  后端服务器将请求重定向到 AD FS 服务器，以获取应用程序安全令牌。  
  
6.  AD FS 服务器会将请求重定向到后端服务器。 现在，该请求包含应用程序令牌和 SSO Cookie。 已授予用户对应用程序的访问权限，并且用户无需输入用户名或密码。  
  
此过程描述如何发布 Web 浏览器客户端将要访问的基于声明的应用程序，例如 SharePoint 站点。 在开始之前，请确保已完成以下操作：  
  
-   在 AD FS 管理控制台中为该应用程序创建了信赖方信任。  
  
-   验证 Web 应用程序代理服务器上的证书是否适用于你要发布的应用程序。  
  

  
#### <a name="to-publish-a-claims-based-application"></a>发布基于声明的应用程序的步骤  
  
1.  在 Web 应用程序代理服务器上的远程访问管理控制台的**导航**窗格中，单击 " **Web 应用程序代理**"，然后在 "**任务**" 窗格中单击 "**发布**"。  
  
2.  在“发布新应用程序向导”上的“欢迎”页面上单击“下一步”。  
  
3.  在 "**预身份验证**" 页上，单击 " **Active Directory 联合身份验证服务（AD FS）** "，然后单击 "**下一步**"。  
  
4.  在“支持的客户端”页面上选择“Web 和 MSOFBA”，然后单击“下一步”。  
  
5.  在“信赖方”页面上的信赖方列表中，选择要发布的应用程序的信赖方，然后单击“下一步”。  
  
6.  在“发布设置” 页面上执行以下操作，然后单击“下一步”。  
  
    -   在“名称”框中输入应用程序的友好名称。  
  
        此名称仅在远程访问管理控制台的已发布应用程序列表中使用。  
  
    -   在“外部 URL”框中输入此应用程序的外部 URL，例如 https://sp.contoso.com/app1/ 。  
  
    -   在“外部证书”列表中，选择其使用者包含外部 URL 的证书 。  
  
    -   在“后端服务器 URL”框中，输入后端服务器的 URL。 请注意，当你输入外部 URL 时，会自动输入此值，仅当后端服务器 URL 不同时才应更改该值;例如， https://sp/app1/ 。  
  
        > [!NOTE]  
        > Web 应用程序代理可以将主机名转换为 Url，但无法转换路径名。 因此，你可以输入不同的主机名，但必须输入相同的路径名。 例如，你可以输入 https://apps.contoso.com/app1/ 的外部 URL 和 https://app-server/app1/ 的后端服务器 URL。 但是，不能输入 https://apps.contoso.com/app1/ 的外部 URL 和 https://apps.contoso.com/internal-app1/ 的后端服务器 URL。  
  
7.  在“确认” 页面上复查设置，然后单击“发布”。 你可以复制 PowerShell 命令来设置其他发布的应用程序。  
  
8.  在“结果”页面上，确保已成功发布该应用程序，然后单击“关闭”。  
  
@no__t 的***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://sp.contoso.com/app1/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://sp.contoso.com/app1/'  
    -Name 'SP'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'SP_Relying_Party'  
```  
  
## <a name="BKMK_1.2"></a>为 Web 浏览器客户端发布基于集成 Windows 身份验证的应用程序  
Web 应用程序代理可用于发布使用集成 Windows 身份验证的应用程序;也就是说，Web 应用程序代理根据需要执行预身份验证，然后可对使用集成 Windows 身份验证的已发布应用程序执行 SSO。 若要发布使用集成 Windows 身份验证的应用程序，必须向联合身份验证服务添加该应用程序的非声明感知信赖方信任。  
  
若要允许 Web 应用程序代理执行单一登录（SSO），并使用 Kerberos 约束委派执行凭据委派，必须将 Web 应用程序代理服务器加入域。 请参阅[计划 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
若要允许用户访问使用 Windows 集成身份验证的应用程序，Web 应用程序代理服务器必须能够为用户提供对已发布应用程序的委派。 可以在域控制器上针对任何应用程序执行此操作。 如果后端服务器在 Windows Server 2012 R2 或 Windows Server 2012 上运行，也可以在后端服务器上执行此操作。 有关详细信息，请参阅 [Kerberos 身份验证的新增功能](https://technet.microsoft.com/library/hh831747.aspx)。  
  
有关如何配置 Web 应用程序代理以发布使用集成 Windows 身份验证的应用程序的演练，请参阅[将站点配置为使用集成 windows 身份验证](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3)。  
  
对后端服务器使用集成 Windows 身份验证时，Web 应用程序代理和已发布应用程序之间的身份验证不是基于声明的，而是使用 Kerberos 约束委派对应用程序的最终用户进行身份验证。 下面描述了常规流：  
  
1.  客户端尝试使用 web 浏览器访问非基于声明的应用程序;例如， https://appserver.contoso.com/nonclaimapp/ 。  
  
2.  Web 浏览器将 HTTPS 请求发送到 Web 应用程序代理服务器，该服务器会将请求重定向到 AD FS 服务器。  
  
3.  AD FS 服务器对用户进行身份验证，并将请求重定向回 Web 应用程序代理。 现在，该请求包含边缘令牌。  
  
4.  Web 应用程序代理验证令牌。  
  
5.  如果令牌有效，则 Web 应用程序代理将代表用户从域控制器获取 Kerberos 票证。  
  
6.  Web 应用程序代理将 Kerberos 票证作为简单的受保护的 GSS-API 协商机制（SPNEGO）令牌的一部分添加到请求，并将请求转发到后端服务器。 该请求包含 Kerberos 票证；因此，已向用户授予对应用程序的访问权限，且无需用户进一步进行身份验证。  
  
此过程描述如何发布 Web 浏览器客户端将要访问的、使用集成 Windows 身份验证的应用程序，例如 Outlook Web App。 在开始之前，请确保已完成以下操作：  
  
-   在 AD FS 管理控制台中为该应用程序创建了非声明感知信赖方信任。  
  
-   已在域控制器上或者通过将 Set-ADUser cmdlet 与 -PrincipalsAllowedToDelegateToAccount 参数结合使用，将后端服务器配置为支持 Kerberos 约束委派。 请注意，如果后端服务器在 Windows Server 2012 R2 或 Windows Server 2012 上运行，则也可以在后端服务器上运行此 PowerShell 命令。  
  
-   确保将 Web 应用程序代理服务器配置为委派给后端服务器的服务主体名称。  
  
-   验证 Web 应用程序代理服务器上的证书是否适用于你要发布的应用程序。  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>发布非基于声明的应用程序的步骤  
  
1.  在 Web 应用程序代理服务器上的远程访问管理控制台的**导航**窗格中，单击 " **Web 应用程序代理**"，然后在 "**任务**" 窗格中单击 "**发布**"。  
  
2.  在“发布新应用程序向导”上的“欢迎”页面上单击“下一步”。  
  
3.  在 "**预身份验证**" 页上，单击 " **Active Directory 联合身份验证服务（AD FS）** "，然后单击 "**下一步**"。  
  
4.  在“支持的客户端”页面上选择“Web 和 MSOFBA”，然后单击“下一步”。  
  
5.  在“信赖方”页面上的信赖方列表中，选择要发布的应用程序的信赖方，然后单击“下一步”。  
  
6.  在“发布设置” 页面上执行以下操作，然后单击“下一步”。  
  
    -   在“名称”框中输入应用程序的友好名称。  
  
        此名称仅在远程访问管理控制台的已发布应用程序列表中使用。  
  
    -   在“外部 URL”框中输入此应用程序的外部 URL，例如 https://owa.contoso.com/ 。  
  
    -   在“外部证书”列表中，选择其使用者包含外部 URL 的证书 。  
  
    -   在“后端服务器 URL”框中，输入后端服务器的 URL。 请注意，当你输入外部 URL 时，会自动输入此值，仅当后端服务器 URL 不同时才应更改该值;例如， https://owa/ 。  
  
        > [!NOTE]  
        > Web 应用程序代理可以将主机名转换为 Url，但无法转换路径名。 因此，你可以输入不同的主机名，但必须输入相同的路径名。 例如，你可以输入 https://apps.contoso.com/app1/ 的外部 URL 和 https://app-server/app1/ 的后端服务器 URL。 但是，不能输入 https://apps.contoso.com/app1/ 的外部 URL 和 https://apps.contoso.com/internal-app1/ 的后端服务器 URL。  
  
    -   在“后端服务器 SPN” 框中输入后端服务器的服务主体名称，例如 HTTP/owa.contoso.com。  
  
7.  在“确认” 页面上复查设置，然后单击“发布”。 你可以复制 PowerShell 命令来设置其他发布的应用程序。  
  
8.  在“结果”页面上，确保已成功发布该应用程序，然后单击“关闭”。  
  
@no__t 的***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerAuthenticationSpn 'HTTP/owa.contoso.com'  
    -BackendServerURL 'https://owa.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://owa.contoso.com/'  
    -Name 'OWA'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Non-Claims_Relying_Party'  
```  
  
## <a name="BKMK_1.3"></a>发布使用 OFBA 的应用程序  
Web 应用程序代理支持从访问后端服务器上的文档和数据的 Microsoft Office 客户端（如 Microsoft Word）进行访问。 这些应用程序与标准浏览器之间的唯一差别在于，重定向到 STS 不是通过常规 HTTP 重定向完成的，而是使用中指定的特殊 OFBA 标头： [https://msdn.microsoft.com/library/dd773463(v=office.12).aspx](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx)。 后端应用程序可以是声明或 IWA。   
若要为使用 OFBA 的客户端发布应用程序，必须将该应用程序的信赖方信任添加到联合身份验证服务。 根据具体的应用程序，你可以使用基于声明的身份验证或集成 Windows 身份验证。 因此，必须根据应用程序添加相关的信赖方信任。  
  
若要允许 Web 应用程序代理执行单一登录（SSO），并使用 Kerberos 约束委派执行凭据委派，必须将 Web 应用程序代理服务器加入域。 请参阅[计划 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
如果应用程序使用基于声明的身份验证，则没有其他规划步骤。 如果应用程序使用了集成 Windows 身份验证，请参阅[为 Web 浏览器客户端发布基于集成 Windows 身份验证的应用程序](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2)。  
  
下面描述了使用基于声明的身份验证的 OFBA 协议的客户端的身份验证流。 对于此方案，身份验证可以使用 URL 中或正文中的应用程序令牌。  
  
1.  用户正在操作一个 Office 程序，并从“最近使用的文档”列表中打开了 SharePoint 站点上的某个文件。  
  
2.  该 Office 程序显示了一个窗口，其中包含的浏览器控件要求用户输入凭据。  
  
    > [!NOTE]  
    > 在某些情况下，由于客户端已经过身份验证，因此可能不显示该窗口。  
  
3.  Web 应用程序代理将请求重定向到 AD FS 服务器，后者将执行身份验证。  
  
4.  AD FS 服务器会将请求重定向回 Web 应用程序代理。 现在，该请求包含边缘令牌。  
  
5.  AD FS 服务器将单一登录（SSO） cookie 添加到请求，因为用户已对 AD FS 服务器执行身份验证。  
  
6.  Web 应用程序代理验证令牌并将请求转发到后端服务器。  
  
7.  后端服务器将请求重定向到 AD FS 服务器，以获取应用程序安全令牌。  
  
8.  请求已重定向到后端服务器。 现在，该请求包含应用程序令牌和 SSO Cookie。 已授予用户对 SharePoint 站点的访问权限，并且用户无需输入用户名或密码就能查看文件。  
  
发布使用 OFBA 的应用程序的步骤与基于声明的应用程序或非基于声明的应用程序的步骤相同。 对于基于声明的应用程序，请参阅为[Web 浏览器客户端发布基于声明的应用程序](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1)，有关非基于声明的应用程序，请参阅为[Web 浏览器客户端发布基于集成 Windows 身份验证的应用程序](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2)。 Web 应用程序代理会自动检测客户端，并将根据需要对用户进行身份验证。  
  
## <a name="publish-an-application-that-uses-http-basic"></a>发布使用 HTTP 基本版的应用程序  

HTTP Basic 是许多协议使用的授权协议，可通过 Exchange 邮箱连接富客户端（包括智能手机）。 有关 HTTP Basic 的详细信息，请参阅[RFC 2617](https://www.ietf.org/rfc/rfc2617.txt)。 通常，Web 应用程序代理使用重定向与 AD FS 进行交互;大多数丰富的客户端不支持 cookie 或状态管理。 通过这种方式，Web 应用程序代理可让 HTTP 应用接收到联合身份验证服务的应用程序的非声明信赖方信任。 请参阅[计划 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
下面和此图介绍了使用 HTTP Basic 的客户端的身份验证流：  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  用户尝试访问已发布的 web 应用程序的电话客户端。  
  
2.  应用向 Web 应用程序代理发布的 URL 发送 HTTPS 请求。  
  
3.  如果请求不包含凭据，则 Web 应用程序代理会将 HTTP 401 响应返回到包含身份验证 AD FS 服务器的 URL 的应用。  
  
4.  用户再次将 HTTPS 请求发送到应用，并在 www 身份验证请求标头中将 "授权" 设置为 "基本" 和 "用户名"，并将 "加密密码" 设置为 "基本 64"。  
  
5.  由于设备不能重定向到 AD FS，因此 Web 应用程序代理会向 AD FS 发送身份验证请求，其中包含包含用户名和密码的凭据。 令牌是代表设备获取的。  
  
6.  为了最大程度地减少发送到 AD FS 的请求数，Web 应用程序代理将使用缓存的令牌验证后续的客户端请求，前提是该令牌是有效的。 Web 应用程序代理会定期清除缓存。 可以使用性能计数器查看缓存大小。  
  
7.  如果令牌有效，则 Web 应用程序代理将请求转发到后端服务器，并向用户授予对发布的 Web 应用程序的访问权限。  
  
以下过程说明如何发布 HTTP 基本应用程序。  
  
#### <a name="to-publish-an-http-basic-application"></a>发布 HTTP 基本应用程序  
  
1.  在 Web 应用程序代理服务器上的远程访问管理控制台的**导航**窗格中，单击 " **Web 应用程序代理**"，然后在 "**任务**" 窗格中单击 "**发布**"。  
  
2.  在“发布新应用程序向导”上的“欢迎”页面上单击“下一步”。  
  
3.  在 "**预身份验证**" 页上，单击 " **Active Directory 联合身份验证服务（AD FS）** "，然后单击 "**下一步**"。  
  
4.  在 "**支持的客户端**" 页上选择 " **HTTP Basic** "，然后单击 "**下一步**"  
  
    如果希望仅允许从已加入工作区的设备访问 Exchange，请选中 "**仅对加入工作区的设备启用访问权限**" 框。 有关详细信息，请参阅[跨公司应用程序从任何设备加入工作区以实现 SSO 和无缝第二重身份验证](https://technet.microsoft.com/library/dn280945.aspx)。  
  
5.  在“信赖方”页面上的信赖方列表中，选择要发布的应用程序的信赖方，然后单击“下一步”。 请注意，此列表仅包含声明的信赖方。  
  
6.  在“发布设置” 页面上执行以下操作，然后单击“下一步”。  
  
    -   在“名称”框中输入应用程序的友好名称。  
  
        此名称仅在远程访问管理控制台的已发布应用程序列表中使用。  
  
    -   在 "**外部 url** " 框中，输入此应用程序的外部 url;例如，mail.contoso.com  
  
    -   在“外部证书”列表中，选择其使用者包含外部 URL 的证书 。  
  
    -   在“后端服务器 URL”框中，输入后端服务器的 URL。 请注意，当你输入外部 URL 时，会自动输入此值，仅当后端服务器 URL 不同时才应更改该值;例如，mail.contoso.com。  
  
7.  在“确认” 页面上复查设置，然后单击“发布”。 你可以复制 PowerShell 命令来设置其他发布的应用程序。  
  
8.  在“结果”页面上，确保已成功发布该应用程序，然后单击“关闭”。  
  
@no__t 的***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
此 Windows PowerShell 脚本为所有设备启用预身份验证，而不只是加入工作区的设备。  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
以下预先验证仅加入工作区的设备：  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -EnableHTTPRedirect:$true   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
## <a name="BKMK_1.4"></a>发布使用 OAuth2 的应用程序，如 Microsoft Store 应用  
若要为 Microsoft Store 应用发布应用程序，必须将应用程序的信赖方信任添加到联合身份验证服务。  
  
若要允许 Web 应用程序代理执行单一登录（SSO），并使用 Kerberos 约束委派执行凭据委派，必须将 Web 应用程序代理服务器加入域。 请参阅[计划 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
> [!NOTE]  
> Web 应用程序代理仅支持为使用 OAuth 2.0 协议的 Microsoft Store 应用发布。  
  
在 AD FS 管理控制台中，必须确保 OAuth 终结点启用了代理。 若要检查是否为 OAuth 终结点启用了代理，请打开 AD FS 管理控制台，展开“服务”，单击“终结点”，在“终结点” 列表中找到 OAuth 终结点，并确保“已启用代理” 列中的值为“是”。  
  
下面介绍了使用 Microsoft Store 应用的客户端的身份验证流：  
  
> [!NOTE]  
> Web 应用程序代理重定向到 AD FS 服务器进行身份验证。 由于 Microsoft Store 应用不支持重定向，因此，如果使用 Microsoft Store 应用，则需要使用 Set-webapplicationproxyconfiguration cmdlet 和 OAuthAuthenticationURL 参数设置 AD FS 服务器的 URL。  
>   
> 只能使用 Windows PowerShell 发布 Microsoft Store 应用。  
  
1.  客户端尝试使用 Microsoft Store 的应用程序访问已发布的 web 应用程序。  
  
2.  应用向 Web 应用程序代理发布的 URL 发送 HTTPS 请求。  
  
3.  Web 应用程序代理将 HTTP 401 响应返回到包含身份验证 AD FS 服务器的 URL 的应用。 此过程称为 "发现"。  
  
    > [!NOTE]  
    > 如果应用知道身份验证 AD FS 服务器的 URL，并且已经有一个包含 OAuth 令牌和边缘令牌的组合令牌，则会跳过此身份验证流中的步骤2和步骤3。  
  
4.  应用向 AD FS 服务器发送 HTTPS 请求。  
  
5.  该应用使用 web 身份验证代理生成一个对话框，用户可在其中输入凭据以便向 AD FS 服务器进行身份验证。 有关 Web 身份验证代理的信息，请参阅 [Web 身份验证代理](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx)。  
  
6.  身份验证成功后，AD FS 服务器将创建包含 OAuth 令牌和边缘令牌的组合令牌，并将令牌发送到应用。  
  
7.  应用将包含组合令牌的 HTTPS 请求发送到 Web 应用程序代理发布的 URL。  
  
8.  Web 应用程序代理将组合令牌拆分成两部分，并验证边缘令牌。  
  
9. 如果边缘令牌有效，则 Web 应用程序代理将请求转发到后端服务器，只包含 OAuth 令牌。 已授予用户对发布的 Web 应用程序的访问权限。  
  
此过程描述如何为 OAuth2 发布应用程序。 只能使用 Windows PowerShell 发布这种类型的应用程序。 在开始之前，请确保已完成以下操作：  
  
-   在 AD FS 管理控制台中为该应用程序创建了信赖方信任。  
  
-   确保 OAuth 终结点在 AD FS 管理控制台中启用了代理，并记下 URL 路径。  
  
-   验证 Web 应用程序代理服务器上的证书是否适用于你要发布的应用程序。  
  
#### <a name="to-publish-an-oauth2-app"></a>发布 OAuth2 应用  
  
1.  在 Web 应用程序代理服务器上的远程访问管理控制台的**导航**窗格中，单击 " **Web 应用程序代理**"，然后在 "**任务**" 窗格中单击 "**发布**"。  
  
2.  在“发布新应用程序向导”上的“欢迎”页面上单击“下一步”。  
  
3.  在 "**预身份验证**" 页上，单击 " **Active Directory 联合身份验证服务（AD FS）** "，然后单击 "**下一步**"。  
  
4.  在 "**支持的客户端**" 页上，选择**OAuth2** ，然后单击 "**下一步**"。  
  
5.  在“信赖方”页面上的信赖方列表中，选择要发布的应用程序的信赖方，然后单击“下一步”。  
  
6.  在“发布设置” 页面上执行以下操作，然后单击“下一步”。  
  
    -   在“名称”框中输入应用程序的友好名称。  
  
        此名称仅在远程访问管理控制台的已发布应用程序列表中使用。  
  
    -   在“外部 URL”框中输入此应用程序的外部 URL，例如 https://server1.contoso.com/app1/ 。  
  
    -   在“外部证书”列表中，选择其使用者包含外部 URL 的证书 。  
  
        为了确保用户可以访问你的应用程序，即使他们忽视在 URL 中键入 HTTPS，也请选择 "**启用 HTTP 到 HTTPS 重定向**" 框。  
  
    -   在“后端服务器 URL”框中，输入后端服务器的 URL。 请注意，当你输入外部 URL 时，会自动输入此值，仅当后端服务器 URL 不同时才应更改该值;例如， https://sp/app1/ 。  
  
        > [!NOTE]  
        > Web 应用程序代理可以将主机名转换为 Url，但无法转换路径名。 因此，你可以输入不同的主机名，但必须输入相同的路径名。 例如，你可以输入 https://apps.contoso.com/app1/ 的外部 URL 和 https://app-server/app1/ 的后端服务器 URL。 但是，不能输入 https://apps.contoso.com/app1/ 的外部 URL 和 https://apps.contoso.com/internal-app1/ 的后端服务器 URL。  
  
7.  在“确认” 页面上复查设置，然后单击“发布”。 你可以复制 PowerShell 命令来设置其他发布的应用程序。  
  
8.  在“结果”页面上，确保已成功发布该应用程序，然后单击“关闭”。  
  
在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要为 fs.contoso.com 的联合服务器地址设置 OAuth 身份验证 URL，并将 URL 路径设置为/adfs/oauth2/：  
  
```  
Set-WebApplicationProxyConfiguration -OAuthAuthenticationURL 'https://fs.contoso.com/adfs/oauth2/'  
```  
  
若要发布应用程序，请运行以下命令：  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://storeapp.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://storeapp.contoso.com/'  
    -Name 'Microsoft Store app Server'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Store_app_Relying_Party'  
    -UseOAuthAuthentication  
```  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [Web 应用程序代理疑难解答](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   [通过 Web 应用程序代理发布应用程序](https://technet.microsoft.com/library/dn383659.aspx)  
  
-   [规划使用 Web 应用程序代理发布应用程序](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Web 应用程序代理操作实例指南](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Windows PowerShell 中的 Web 应用程序代理 Cmdlet](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Set-webapplicationproxyapplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-webapplicationproxyconfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  



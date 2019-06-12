---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: 使用 AD FS 预身份验证发布应用程序
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: ca4d8661f8f0252334bdecbde85603d8af5e2d2a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446810"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>使用 AD FS 预身份验证发布应用程序

>适用于：Windows Server 2016

**此内容是相关的本地版本的 Web 应用程序代理。若要支持通过云对本地应用程序的安全访问，请参阅[Azure AD 应用程序代理内容](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/)。**  
  
本主题介绍如何通过使用 Active Directory 联合身份验证服务 (AD FS) 预身份验证的 Web 应用程序代理发布应用程序。  
  
对于所有类型的应用程序可以发布使用 AD FS 预身份验证，必须添加 AD FS 信赖方信任联合身份验证服务。  
  
常规的 AD FS 预身份验证流如下所示：  
  
> [!NOTE]  
> 此身份验证流不适用于使用 Microsoft Store 应用的客户端。  
  
1.  客户端设备尝试访问特定资源 URL; 上的已发布的 web 应用程序例如 https://app1.contoso.com/。  
  
    资源 URL 是 Web 应用程序代理侦听传入 HTTPS 请求的公共地址。  
  
    如果启用 HTTP 到 HTTPS 重定向，Web 应用程序代理还将侦听传入的 HTTP 请求。  
  
2.  Web 应用程序代理将 HTTPS 请求重定向到 AD FS 服务器使用编码的 URL 参数，包括资源 URL 和 appRealm （一个信赖方标识符）。  
  
    用户进行身份验证使用 AD FS 服务器; 所需的身份验证方法例如，用户名和密码、 结合一次性密码等等的双因素身份验证。  
  
3.  用户进行身份验证后，AD FS 服务器颁发安全令牌，即边缘令牌，其中包含以下信息和重定向回 Web 应用程序代理服务器的 HTTPS 请求：  
  
    -   用户尝试访问的资源标识符。  
  
    -   为用户主体名称 (UPN) 的用户的标识。  
  
    -   访问授权审批过期时间；也就是说，将向用户授予有限时间的访问权限，在此期限过后，需要重新对用户进行身份验证。  
  
    -   边缘令牌中的信息签名。  
  
4.  Web 应用程序代理从具有边缘令牌的 AD FS 服务器接收重定向的 HTTPS 请求并验证和使用该令牌，如下所示：  
  
    -   验证边缘令牌签名来自 Web 应用程序代理配置中配置联合身份验证服务。  
  
    -   验证令牌是否是针对正确的应用程序颁发的。  
  
    -   验证令牌是否未过期。  
  
    -   根据需要使用用户标识；例如，如果后端服务器配置为使用集成 Windows 身份验证，则获取 Kerberos 票证。  
  
5.  如果边缘令牌有效，Web 应用程序代理将转发到已发布的 web 应用程序使用 HTTP 或 HTTPS 将 HTTPS 请求。  
  
6.  现在，客户端可以访问发布的 Web 应用程序；但是，可将发布的应用程序配置为要求用户执行其他身份验证。 例如，如果发布的 Web 应用程序是 SharePoint 站点且不需要执行其他身份验证，则用户将在浏览器中看到该 SharePoint 站点。  
  
7.  Web 应用程序代理客户端设备上保存 cookie。 Cookie 是 Web 应用程序代理用于标识，此会话已经过预身份验证，且没有更多的预身份验证需要。  
  
> [!IMPORTANT]  
> 配置外部 URL 和后端服务器 URL 时，请确保输入完全限定的域名 (FQDN) 而不是 IP 地址。  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_1.1"></a>为 Web 浏览器客户端发布基于声明的应用程序  
若要发布使用声明进行身份验证的应用程序，必须向联合身份验证服务添加该应用程序的信赖方信任。  
  
在发布基于声明的应用程序以及从浏览器访问应用程序时，要执行的常规身份验证流如下：  
  
1.  客户端尝试访问基于声明的应用程序使用 web 浏览器;例如， https://appserver.contoso.com/claimapp/。  
  
2.  Web 浏览器到 Web 应用程序代理服务器将请求重定向到 AD FS 服务器发送 HTTPS 请求。  
  
3.  AD FS 服务器对用户和设备进行身份验证，并将请求重定向回 Web 应用程序代理。 现在，该请求包含边缘令牌。 AD FS 服务器将单一登录 (SSO) cookie 添加到请求，因为用户已执行了针对 AD FS 服务器进行身份验证。  
  
4.  Web 应用程序代理验证令牌，添加其自身的 cookie，并将其转发给后端服务器。  
  
5.  后端服务器将请求重定向到 AD FS 服务器，以获取应用程序安全令牌。  
  
6.  请求重定向到后端服务器的 AD FS 服务器。 现在，该请求包含应用程序令牌和 SSO Cookie。 已授予用户对应用程序的访问权限，并且用户无需输入用户名或密码。  
  
此过程描述如何发布 Web 浏览器客户端将要访问的基于声明的应用程序，例如 SharePoint 站点。 在开始之前，请确保已完成以下操作：  
  
-   在 AD FS 管理控制台中创建应用程序的信赖方信任。  
  
-   已验证 Web 应用程序代理服务器上的证书适用于你想要发布应用程序。  
  

  
#### <a name="to-publish-a-claims-based-application"></a>发布基于声明的应用程序的步骤  
  
1.  在 Web 应用程序代理服务器上，在远程访问管理控制台中，在**导航**窗格中，单击**Web 应用程序代理**，然后在**任务**窗格中，单击**发布**。  
  
2.  在“发布新应用程序向导”  上的“欢迎”  页面上单击“下一步”  。  
  
3.  上**预身份验证**页上，单击**Active Directory 联合身份验证服务 (AD FS)** ，然后单击**下一步**。  
  
4.  在“支持的客户端”  页面上选择“Web 和 MSOFBA”  ，然后单击“下一步”  。  
  
5.  在“信赖方”  页面上的信赖方列表中，选择要发布的应用程序的信赖方，然后单击“下一步”  。  
  
6.  在“发布设置”  页面上执行以下操作，然后单击“下一步”  。  
  
    -   在“名称”框中输入应用程序的友好名称  。  
  
        此名称仅在远程访问管理控制台的已发布应用程序列表中使用。  
  
    -   在“外部 URL”  框中输入此应用程序的外部 URL，例如 https://sp.contoso.com/app1/。  
  
    -   在“外部证书”列表中，选择其使用者包含外部 URL 的证书  。  
  
    -   在“后端服务器 URL”框中，输入后端服务器的 URL  。 请注意当你输入外部 URL 和后端服务器 URL 是不同的; 如果只应将其更改时将自动输入此值例如， https://sp/app1/。  
  
        > [!NOTE]  
        > Web 应用程序代理可将转换为 Url，主机名，但无法转换路径名。 因此，你可以输入不同的主机名，但必须输入相同的路径名。 例如，可以输入的外部 URL https://apps.contoso.com/app1/和后端服务器 URL 的 https://app-server/app1/。 但是，不能输入的外部 URL https://apps.contoso.com/app1/和后端服务器 URL 的 https://apps.contoso.com/internal-app1/。  
  
7.  在“确认”  页面上复查设置，然后单击“发布”  。 你可以复制 PowerShell 命令来设置其他发布的应用程序。  
  
8.  在“结果”  页面上，确保已成功发布该应用程序，然后单击“关闭”  。  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
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
  
## <a name="BKMK_1.2"></a>将集成 Windows 基于身份验证的应用程序发布为 Web 浏览器客户端  
可以使用 web 应用程序代理发布应用程序使用集成 Windows 身份验证;即，Web 应用程序代理执行预身份验证为必需的并可以使用集成 Windows 身份验证的已发布应用程序执行 SSO。 若要发布使用集成 Windows 身份验证的应用程序，必须向联合身份验证服务添加该应用程序的非声明感知信赖方信任。  
  
若要允许 Web 应用程序代理执行单一登录 (SSO) 以及委派执行凭据使用 Kerberos 约束委派，Web 应用程序代理服务器必须加入到域。 请参阅[规划 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
若要允许用户访问使用集成 Windows 身份验证的应用程序，Web 应用程序代理服务器必须能够提供已发布的应用程序用户委派。 可以在域控制器上针对任何应用程序执行此操作。 您可以还执行此操作后端服务器上运行时将其在 Windows Server 2012 R2 或 Windows Server 2012 上。 有关详细信息，请参阅 [Kerberos 身份验证的新增功能](https://technet.microsoft.com/library/hh831747.aspx)。  
  
有关如何配置 Web 应用程序代理发布使用集成 Windows 身份验证的应用程序的演练，请参阅[将站点配置为使用集成 Windows 身份验证](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3)。  
  
当使用向后端服务器的集成 Windows 身份验证，Web 应用程序代理与发布的应用程序之间的身份验证不是基于声明的而是使用 Kerberos 约束委派给应用程序的最终用户进行身份验证。 下面描述了常规流：  
  
1.  客户端尝试访问非基于声明的应用程序使用 web 浏览器;例如， https://appserver.contoso.com/nonclaimapp/。  
  
2.  Web 浏览器到 Web 应用程序代理服务器将请求重定向到 AD FS 服务器发送 HTTPS 请求。  
  
3.  AD FS 服务器对用户进行身份验证，并将请求重定向回 Web 应用程序代理。 现在，该请求包含边缘令牌。  
  
4.  Web 应用程序代理验证该令牌。  
  
5.  如果令牌有效，Web 应用程序代理的 Kerberos 票证从域控制器获取代表用户。  
  
6.  Web 应用程序代理将 Kerberos 票证作为简单和受保护 GSS-API 协商机制 (SPNEGO) 令牌的一部分添加到请求，并将其转发到后端服务器。 该请求包含 Kerberos 票证；因此，已向用户授予对应用程序的访问权限，且无需用户进一步进行身份验证。  
  
此过程描述如何发布 Web 浏览器客户端将要访问的、使用集成 Windows 身份验证的应用程序，例如 Outlook Web App。 在开始之前，请确保已完成以下操作：  
  
-   在 AD FS 管理控制台中创建应用程序的非声明感知信赖方信任。  
  
-   已在域控制器上或者通过将 Set-ADUser cmdlet 与 -PrincipalsAllowedToDelegateToAccount 参数结合使用，将后端服务器配置为支持 Kerberos 约束委派。 请注意，是否后端服务器正在运行 Windows Server 2012 R2 或 Windows Server 2012 上，您可以也运行此 PowerShell 命令后端服务器上。  
  
-   已确保，将 Web 应用程序代理服务器配置为委派给服务主体名称的后端服务器。  
  
-   已验证 Web 应用程序代理服务器上的证书适用于你想要发布应用程序。  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>发布非基于声明的应用程序的步骤  
  
1.  在 Web 应用程序代理服务器上，在远程访问管理控制台中，在**导航**窗格中，单击**Web 应用程序代理**，然后在**任务**窗格中，单击**发布**。  
  
2.  在“发布新应用程序向导”  上的“欢迎”  页面上单击“下一步”  。  
  
3.  上**预身份验证**页上，单击**Active Directory 联合身份验证服务 (AD FS)** ，然后单击**下一步**。  
  
4.  在“支持的客户端”  页面上选择“Web 和 MSOFBA”  ，然后单击“下一步”  。  
  
5.  在“信赖方”  页面上的信赖方列表中，选择要发布的应用程序的信赖方，然后单击“下一步”  。  
  
6.  在“发布设置”  页面上执行以下操作，然后单击“下一步”  。  
  
    -   在“名称”框中输入应用程序的友好名称  。  
  
        此名称仅在远程访问管理控制台的已发布应用程序列表中使用。  
  
    -   在“外部 URL”  框中输入此应用程序的外部 URL，例如 https://owa.contoso.com/。  
  
    -   在“外部证书”列表中，选择其使用者包含外部 URL 的证书  。  
  
    -   在“后端服务器 URL”框中，输入后端服务器的 URL  。 请注意当你输入外部 URL 和后端服务器 URL 是不同的; 如果只应将其更改时将自动输入此值例如， https://owa/。  
  
        > [!NOTE]  
        > Web 应用程序代理可将转换为 Url，主机名，但无法转换路径名。 因此，你可以输入不同的主机名，但必须输入相同的路径名。 例如，可以输入的外部 URL https://apps.contoso.com/app1/和后端服务器 URL 的 https://app-server/app1/。 但是，不能输入的外部 URL https://apps.contoso.com/app1/和后端服务器 URL 的 https://apps.contoso.com/internal-app1/。  
  
    -   在“后端服务器 SPN”  框中输入后端服务器的服务主体名称，例如 HTTP/owa.contoso.com。  
  
7.  在“确认”  页面上复查设置，然后单击“发布”  。 你可以复制 PowerShell 命令来设置其他发布的应用程序。  
  
8.  在“结果”  页面上，确保已成功发布该应用程序，然后单击“关闭”  。  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
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
  
## <a name="BKMK_1.3"></a>发布使用 MS OFBA 的应用程序  
Web 应用程序代理在后端服务器上支持从 Microsoft Word 等 Microsoft Office 客户端访问该访问文档和数据。 这些应用程序与标准浏览器之间的唯一区别是，重定向到 STS 已不是通过普通的 HTTP 重定向，但使用特殊的 MS OFBA 标头中指定： [ https://msdn.microsoft.com/library/dd773463(v=office.12).aspx ](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx)。 后端应用程序可以是声明或 IWA。   
若要使用 MS OFBA 的客户端发布的应用程序，必须添加到联合身份验证服务应用程序的信赖方信任。 根据具体的应用程序，你可以使用基于声明的身份验证或集成 Windows 身份验证。 因此，必须根据应用程序添加相关的信赖方信任。  
  
若要允许 Web 应用程序代理执行单一登录 (SSO) 以及委派执行凭据使用 Kerberos 约束委派，Web 应用程序代理服务器必须加入到域。 请参阅[规划 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
如果应用程序使用基于声明的身份验证，则没有其他规划步骤。 如果应用程序使用集成 Windows 身份验证，请参阅[集成 Windows 基于身份验证的应用程序发布为 Web 浏览器客户端](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2)。  
  
下面描述了使用 MS OFBA 协议使用基于声明的身份验证的客户端的身份验证流。 对于此方案，身份验证可以使用 URL 中或正文中的应用程序令牌。  
  
1.  用户正在操作一个 Office 程序，并从“最近使用的文档”列表中打开了 SharePoint 站点上的某个文件  。  
  
2.  该 Office 程序显示了一个窗口，其中包含的浏览器控件要求用户输入凭据。  
  
    > [!NOTE]  
    > 在某些情况下，由于客户端已经过身份验证，因此可能不显示该窗口。  
  
3.  Web 应用程序代理将请求重定向到 AD FS 服务器，后者将执行身份验证。  
  
4.  AD FS 服务器将请求重定向回 Web 应用程序代理。 现在，该请求包含边缘令牌。  
  
5.  AD FS 服务器将单一登录 (SSO) cookie 添加到请求，因为用户已执行了针对 AD FS 服务器进行身份验证。  
  
6.  Web 应用程序代理验证令牌，并将其转发给后端服务器。  
  
7.  后端服务器将请求重定向到 AD FS 服务器，以获取应用程序安全令牌。  
  
8.  请求已重定向到后端服务器。 现在，该请求包含应用程序令牌和 SSO Cookie。 已授予用户对 SharePoint 站点的访问权限，并且用户无需输入用户名或密码就能查看文件。  
  
若要发布使用 MS OFBA 的应用程序的步骤是基于声明的应用程序或非基于声明的应用程序的步骤相同。 对于基于声明的应用程序，请参阅[为 Web 浏览器客户端发布基于声明的应用程序](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1)，对于非基于声明的应用程序，请参阅[集成 Windows 基于身份验证的应用程序的 Web 发布浏览器客户端](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2)。 Web 应用程序代理会自动检测客户端，并将根据需要对用户进行身份验证。  
  
## <a name="publish-an-application-that-uses-http-basic"></a>发布使用 HTTP 基本的应用程序  

HTTP 基本身份验证是许多协议，用来连接丰富客户端，包括智能手机，与您的 Exchange 邮箱的授权协议。 HTTP 基本的详细信息，请参阅[RFC 2617](https://www.ietf.org/rfc/rfc2617.txt)。 Web 应用程序代理与 AD FS 使用重定向; 传统上进行交互大多数的丰富客户端不支持 cookie 或状态管理。 在这种方式中 Web 应用程序代理允许 HTTP 应用程序接收非声明联合身份验证服务应用程序的信赖方信任。 请参阅[规划 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
下方，并在此关系图描述了使用 HTTP 基本的客户端的身份验证流：  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  用户尝试访问已发布的 web 应用程序电话客户端。  
  
2.  应用程序将 HTTPS 请求发送到 Web 应用程序代理发布的 URL。  
  
3.  如果请求不包含凭据，Web 应用程序代理将返回到应用，其中包含身份验证的 AD FS 服务器的 URL 的 HTTP 401 响应。  
  
4.  用户将发送到应用的 HTTPS 请求再次使用授权设置为基本和用户名称和 Base 64 加密 www 中用户的密码的身份验证请求标头。  
  
5.  因为设备不能被重定向到 AD FS，Web 应用程序代理身份验证将请求发送到 AD FS 使用的凭据，它可以包括用户名和密码。 代表设备获取令牌。  
  
6.  为了最小化到的 AD FS 发送的请求数，Web 应用程序代理验证使用缓存的令牌，前提令牌的有效期的后续客户端请求。 Web 应用程序代理定期清除缓存。 您可以查看缓存使用的性能计数器的大小。  
  
7.  如果令牌有效，Web 应用程序代理将请求转发到后端服务器和用户被授予访问权限已发布的 web 应用程序。  
  
以下过程说明如何发布 HTTP 基本应用程序。  
  
#### <a name="to-publish-an-http-basic-application"></a>HTTP 基本应用程序发布  
  
1.  在 Web 应用程序代理服务器上，在远程访问管理控制台中，在**导航**窗格中，单击**Web 应用程序代理**，然后在**任务**窗格中，单击**发布**。  
  
2.  在“发布新应用程序向导”  上的“欢迎”  页面上单击“下一步”  。  
  
3.  上**预身份验证**页上，单击**Active Directory 联合身份验证服务 (AD FS)** ，然后单击**下一步**。  
  
4.  上**支持的客户端**页上，选择**HTTP Basic** ，然后单击**下一步**。  
  
    如果你想要启用对 Exchange 的访问，只能从已加入工作区的设备，选择**工作区启用访问已加入设备**框。 有关详细信息请参阅[加入工作区以从任一设备实现 SSO 和无缝第二因素身份验证跨公司应用程序](https://technet.microsoft.com/library/dn280945.aspx)。  
  
5.  在“信赖方”  页面上的信赖方列表中，选择要发布的应用程序的信赖方，然后单击“下一步”  。 请注意，此列表仅包含在声明的信赖方。  
  
6.  在“发布设置”  页面上执行以下操作，然后单击“下一步”  。  
  
    -   在“名称”框中输入应用程序的友好名称  。  
  
        此名称仅在远程访问管理控制台的已发布应用程序列表中使用。  
  
    -   在中**外部 URL**框中，输入外部 URL 为此应用程序; 例如，mail.contoso.com  
  
    -   在“外部证书”列表中，选择其使用者包含外部 URL 的证书  。  
  
    -   在“后端服务器 URL”框中，输入后端服务器的 URL  。 请注意当你输入外部 URL 和后端服务器 URL 是不同的; 如果只应将其更改时将自动输入此值例如，mail.contoso.com。  
  
7.  在“确认”  页面上复查设置，然后单击“发布”  。 你可以复制 PowerShell 命令来设置其他发布的应用程序。  
  
8.  在“结果”  页面上，确保已成功发布该应用程序，然后单击“关闭”  。  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
此 Windows PowerShell 脚本启用预身份验证的所有设备，而不仅仅是已加入工作区的设备。  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
以下预身份验证仅 workplace join 的设备：  
  
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
  
## <a name="BKMK_1.4"></a>发布使用 OAuth2 如 Microsoft Store 应用的应用程序  
若要发布的 Microsoft Store 应用的应用程序，必须添加到联合身份验证服务应用程序的信赖方信任。  
  
若要允许 Web 应用程序代理执行单一登录 (SSO) 以及委派执行凭据使用 Kerberos 约束委派，Web 应用程序代理服务器必须加入到域。 请参阅[规划 Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD)。  
  
> [!NOTE]  
> Web 应用程序代理发布只支持使用 OAuth 2.0 协议的 Microsoft Store 应用。  
  
在 AD FS 管理控制台中，必须确保为 OAuth 终结点是启用的代理。 若要检查是否为 OAuth 终结点启用了代理，请打开 AD FS 管理控制台，展开“服务”  ，单击“终结点”  ，在“终结点”  列表中找到 OAuth 终结点，并确保“已启用代理”  列中的值为“是”  。  
  
使用 Microsoft Store 应用的客户端的身份验证流如下所述：  
  
> [!NOTE]  
> Web 应用程序代理重定向到 AD FS 服务器进行身份验证。 由于 Microsoft Store 应用不支持重定向，如果你使用 Microsoft Store 应用，因此有必要，若要设置 AD FS 服务器使用 Set-webapplicationproxyconfiguration cmdlet 和 OAuthAuthenticationURL 参数的 URL。  
>   
> 可以仅使用 Windows PowerShell 发布 Microsoft Store 应用。  
  
1.  客户端尝试访问已发布的 web 应用程序使用的 Microsoft Store 应用。  
  
2.  应用程序将 HTTPS 请求发送到 Web 应用程序代理发布的 URL。  
  
3.  Web 应用程序代理返回到应用，其中包含身份验证的 AD FS 服务器的 URL 的 HTTP 401 响应。 此过程被称为发现。  
  
    > [!NOTE]  
    > 如果应用程序知道身份验证的 AD FS 服务器的 URL，并且已具有包含 OAuth 令牌和边缘令牌的组合令牌，则步骤 2 和 3 会将跳过此身份验证流中。  
  
4.  应用程序将 HTTPS 请求发送到 AD FS 服务器。  
  
5.  该应用使用 web 身份验证代理生成一个对话框，在其中用户输入的凭据进行身份验证到 AD FS 服务器。 有关 Web 身份验证代理的信息，请参阅 [Web 身份验证代理](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx)。  
  
6.  完成后成功进行身份验证，AD FS 服务器创建的组合令牌，包含 OAuth 令牌和边缘令牌并将该令牌发送到应用程序。  
  
7.  应用会发送包含组合令牌向 Web 应用程序代理发布的 URL 的 HTTPS 请求。  
  
8.  Web 应用程序代理将组合令牌拆分成两个部分，并验证边缘令牌。  
  
9. 如果边缘令牌有效，Web 应用程序代理将转发到后端服务器只使用 OAuth 令牌请求。 已授予用户对发布的 Web 应用程序的访问权限。  
  
此过程描述如何为 OAuth2 发布应用程序。 可以仅使用 Windows PowerShell 发布这种类型的应用程序。 在开始之前，请确保已完成以下操作：  
  
-   在 AD FS 管理控制台中创建应用程序的信赖方信任。  
  
-   确保为 OAuth 终结点是在 AD FS 管理控制台中启用的代理，并记下 URL 路径。  
  
-   已验证 Web 应用程序代理服务器上的证书适用于你想要发布应用程序。  
  
#### <a name="to-publish-an-oauth2-app"></a>若要发布 OAuth2 应用  
  
1.  在 Web 应用程序代理服务器上，在远程访问管理控制台中，在**导航**窗格中，单击**Web 应用程序代理**，然后在**任务**窗格中，单击**发布**。  
  
2.  在“发布新应用程序向导”  上的“欢迎”  页面上单击“下一步”  。  
  
3.  上**预身份验证**页上，单击**Active Directory 联合身份验证服务 (AD FS)** ，然后单击**下一步**。  
  
4.  上**支持的客户端**页上，选择**OAuth2** ，然后单击**下一步**。  
  
5.  在“信赖方”  页面上的信赖方列表中，选择要发布的应用程序的信赖方，然后单击“下一步”  。  
  
6.  在“发布设置”  页面上执行以下操作，然后单击“下一步”  。  
  
    -   在“名称”框中输入应用程序的友好名称  。  
  
        此名称仅在远程访问管理控制台的已发布应用程序列表中使用。  
  
    -   在“外部 URL”  框中输入此应用程序的外部 URL，例如 https://server1.contoso.com/app1/。  
  
    -   在“外部证书”列表中，选择其使用者包含外部 URL 的证书  。  
  
        若要确保你的用户可以访问你的应用，即使他们忽略键入 HTTPS URL 中，选择**启用 HTTP 到 HTTPS 重定向**框。  
  
    -   在“后端服务器 URL”框中，输入后端服务器的 URL  。 请注意当你输入外部 URL 和后端服务器 URL 是不同的; 如果只应将其更改时将自动输入此值例如， https://sp/app1/。  
  
        > [!NOTE]  
        > Web 应用程序代理可将转换为 Url，主机名，但无法转换路径名。 因此，你可以输入不同的主机名，但必须输入相同的路径名。 例如，可以输入的外部 URL https://apps.contoso.com/app1/和后端服务器 URL 的 https://app-server/app1/。 但是，不能输入的外部 URL https://apps.contoso.com/app1/和后端服务器 URL 的 https://apps.contoso.com/internal-app1/。  
  
7.  在“确认”  页面上复查设置，然后单击“发布”  。 你可以复制 PowerShell 命令来设置其他发布的应用程序。  
  
8.  在“结果”  页面上，确保已成功发布该应用程序，然后单击“关闭”  。  
  
在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要设置联合身份验证服务器的 OAuth 身份验证 URL 地址 fs.contoso.com 和 /adfs/oauth2/的 URL 路径：  
  
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
  
-   [在 Windows PowerShell 中的 web 应用程序代理 Cmdlet](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Add-WebApplicationProxyApplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-WebApplicationProxyConfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  



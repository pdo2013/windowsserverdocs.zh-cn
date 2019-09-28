---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Windows Server 2016 的 Active Directory 联合身份验证服务的新增功能
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/23/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6294c7b6ead0a9fa338f8b2cc8134b750f7e3e8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385547"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Active Directory 联合身份验证服务的新增功能


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Windows Server 2019 Active Directory 联合身份验证服务的新增功能

### <a name="protected-logins"></a>受保护登录
下面简要概述了 AD FS 2019 中提供的受保护登录：
- **作为主要客户的外部身份验证提供程序**，客户现在可以使用第三方身份验证产品作为第一个因素，而不是将密码公开为第一个因素。 如果外部身份验证提供程序可以证明2个因素，则它可以声明 MFA。 
- **作为附加身份验证的密码身份验证**-客户有一个完全受支持的收件箱选项，只需使用密码较少的选项作为第一个因素。 这提高了客户在 ADFS 2016 的体验，客户必须下载 github 适配器，该适配器受支持。 
- 可**插入风险评估模块**-客户现在可以构建自己的插件模块，在预身份验证阶段阻止特定类型的请求。 这样，客户就可以更轻松地使用云智能（如标识保护）来阻止有风险用户或风险事务的登录名。  有关详细信息，请参阅[用 AD FS 2019 风险评估模型生成插件](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **ESL 改进**-通过添加以下功能改进了2016的 ESL QFE
    - 允许客户处于审核模式，同时受 ADFS 2012R2 提供的 "经典" extranet 锁定功能保护。 目前，2016客户在审核模式下没有任何保护。 
    - 为熟悉的位置启用独立锁定阈值。 这样一来，使用常见服务帐户运行的多个应用程序实例就可以滚动使用最小影响的密码。 

### <a name="additional-security-improvements"></a>其他安全性改进
AD FS 2019 中提供了以下附加安全改进：
- **使用智能卡登录的远程 PSH** -客户现在可以通过 PSH 使用智能卡远程连接到 ADFS，并使用它来管理所有 PSH 函数（包括多节点 PSH cmdlet）。
- **HTTP 标头自定义**-客户现在可以自定义 ADFS 响应期间发出的 HTTP 标头。 这包括以下标头
     - HSTS这表明，ADFS 终结点仅可用于兼容浏览器的 HTTPS 终结点，以便强制执行
     - x 框架-选项：允许 ADFS 管理员允许特定的信赖方嵌入 ADFS 交互式登录页的 Iframe。 应该谨慎使用这一点，只需在 HTTPS 主机上使用。 
     - 将来的标头：还可以配置其他将来的标头。 

有关详细信息，请参阅[自定义 HTTP security response 标头与 AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>身份验证/策略功能
以下身份验证/策略功能位于 AD FS 2019：
- **指定针对每个 RP 的其他身份验证的身份验证方法**-客户现在可以使用声明规则来决定为其他身份验证提供程序调用的其他身份验证提供程序。 这适用于两个用例
    - 客户从一个附加的身份验证提供程序转换到另一个身份验证提供程序。 这样，当他们将用户加入到较新的身份验证提供程序时，他们可以使用组来控制调用哪个附加身份验证提供程序。
    - 对于某些应用程序，客户需要使用特定的附加身份验证提供程序（例如证书）。 
- **仅限基于 TLS 的设备身份验证仅限需要它的应用程序**-客户现在可以将基于客户端 TLS 的设备身份验证限制为仅执行基于设备的条件性访问的应用程序。 这会阻止不需要基于 TLS 的设备身份验证的应用程序进行设备身份验证（如果客户端应用程序无法处理）的任何不需要的提示。
- **MFA 新鲜度支持**-AD FS 现在支持根据第二因素凭据的新鲜度重新执行第二因素凭据。 这样，客户就可以使用2个因素执行初始交易，并定期仅提示第二个因素。 这仅适用于可以在请求中提供附加参数的应用程序，并且不是 ADFS 中的可配置设置。 如果在 Azure AD 中配置了 "记住我的 MFA 时间 X 天" 并将 "supportsMFA" 标志设置为 true，则 Azure AD 支持此参数。 

### <a name="sign-in-sso-improvements"></a>登录 SSO 改进
AD FS 2019 中进行了以下登录 SSO 改进：

- [使用居中主题对 UX 进行分页](../operations/AD-FS-paginated-sign-in.md)-现在已移动到分页的 ux 流，它允许 adfs 进行验证并提供更流畅的登录体验。 ADFS 现在使用居中的 UI （而不是屏幕的右侧）。 你可能需要更新的徽标和背景图像以使其符合此体验。 这还会反映 Azure AD 中提供的功能。
- **Bug 修复：执行 PRT auth @ no__t 时，Win10 设备的持久性 SSO 状态。这解决了在使用 Windows 10 设备的 PRT authentication 时，未保留 MFA 状态的问题。 问题的结果是，最终用户经常收到第二因素凭据（MFA）的提示。 通过客户端 TLS 和通过 PRT 机制成功执行设备身份验证时，该修补程序还会使体验保持一致。 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>用于构建现代业务线应用的支持
AD FS 2019 中添加了以下对生成新式 LOB 应用的支持：

 - **Oauth 设备流/配置文件**-AD FS 现在支持 oauth 设备流配置文件，以便在不具有用于支持丰富登录体验的 UI 外围设备的设备上执行登录。 这允许用户在不同的设备上完成登录体验。 此功能是在 Azure Stack 中 Azure CLI 体验所必需的，并且可以在其他情况下使用。 
 - **删除 "Resource" 参数**-AD FS 现在已不再需要指定资源参数，该参数与当前 Oauth 规范的行为相同。 除请求权限外，客户端现在可以将信赖方信任标识符作为作用域参数提供。 
 - **AD FS 响应中的 CORS 标头**-客户现在可以构建单页面应用程序，以允许客户端 JS 库通过在 AD FS 上通过查询 OIDC 发现文档中的签名密钥来验证 id_token 的签名。 
 - **PKCE 支持**-AD FS 添加 PKCE 支持，以在 OAuth 内提供安全的身份验证代码流。 这会为此流额外添加一个安全层，以防止代码被劫持并从其他客户端重播。 
 - **Bug 修复：发送 x5t 和儿童理赔 @ no__t-0-这是一个次要 bug 修复。 AD FS 现在还发送 "儿童" 声明，以表示用于验证签名的密钥 id 提示。 以前 AD FS 仅将其作为 "x5t" 声明发送。

### <a name="supportability-improvements"></a>可支持性改进
以下可支持性改进不属于 AD FS 2019：
- 向**AD FS 管理员发送错误详细信息**-允许管理员将最终用户身份验证中与失败相关的调试日志配置为以压缩方式存储，以方便使用。 管理员还可以配置 SMTP 连接，以将压缩的文件 automail 到会审电子邮件帐户，或根据电子邮件自动创建票证。 

### <a name="deployment-updates"></a>部署更新
AD FS 2019 中现在包含以下部署更新：
- **场行为级别 2019** -与 AD FS 2016 相同，存在启用上述新功能所需的新场行为级别版本。 这允许执行以下操作：
    - 2012 R2-> 2019
    - 2016-> 2019   

### <a name="saml-updates"></a>SAML 更新
以下 SAML 更新位于 AD FS 2019：
- **Bug 修复：修复聚合联合 @ no__t 中的错误-围绕聚合联合身份验证支持（例如 InCommon），有很多 bug 修复。 解决方法如下： 
  - 改善了聚合联合元数据文档中大 # 实体的缩放。以前，这会失败并出现 "ADMIN0017" 错误。 
  - 使用 "ScopeGroupID" 参数通过 AdfsRelyingPartyTrustsGroup PSH cmdlet 进行查询。 
  - 处理有关重复 entityID 的错误条件


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>作用域参数中的 Azure AD 样式资源规范 
之前，AD FS 要求所需的资源和作用域位于任何身份验证请求中的单独参数中。 例如，典型的 oauth 请求如下所示：7 **https：&#47;&#47;fs.contoso.com/adfs/oauth2/authorize？ </br>response_type = code & client_id = claimsxrayclient & 资源 = urn： microsoft： </br>adfs： claimsxray & scope = oauth & redirect_uri = https：&#47; &#47;adfshelp.microsoft.com/</br> ClaimsXray/TokenResponse & prompt = login**
 
通过服务器2019上的 AD FS，现在可以传递嵌入在 scope 参数中的资源值。 这与可以对 Azure AD 进行身份验证的方式一致。 

作用域参数现在可以组织为以空格分隔的列表，其中每个条目都作为资源/作用域的结构。 例如  

**< 创建一个有效的示例请求 >**
> [!NOTE]
> 身份验证请求中只能指定一个资源。 如果请求中包含多个资源，AD FS 将返回错误，并且身份验证不会成功。 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>适用于 oAuth 的代码交换（PKCE）支持的证明密钥 
使用授权代码授予的 OAuth 公用客户端容易受到授权代码拦截攻击。  RFC 7636 中详细介绍了这种攻击。 若要缓解这种攻击，服务器2019中的 AD FS 支持 OAuth 授权代码授予流的代码交换（PKCE）的校验密钥。 
 
若要利用 PKCE 支持，此规范将向 OAuth 2.0 授权和访问令牌请求添加其他参数。

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. 客户端创建并记录名为 "code_verifier" 的机密，并派生转换的版本 "t （code_verifier）" （称为 "code_challenge"），该版本与转换方法 "t_m" 一起在 OAuth 2.0 授权请求中发送。 

B. 授权终结点的响应方式通常为，但记录为 "t （code_verifier）" 和转换方法。 

C. 然后，客户端像往常一样在访问令牌请求中发送授权代码，但包含在（A）生成的 "code_verifier" 机密。 

2-D. AD FS 转换 "code_verifier"，并将其与（B）中的 "t （code_verifier）" 进行比较。  如果访问不相等，则拒绝访问。 

#### <a name="faq"></a>常见问题 
**：.** 能否传递资源值作为范围值的一部分，如如何对 Azure AD 进行请求呢？ 
</br>**的.** 通过服务器2019上的 AD FS，现在可以传递嵌入在 scope 参数中的资源值。 作用域参数现在可以组织为以空格分隔的列表，其中每个条目都作为资源/作用域的结构。 例如  
**< 创建一个有效的示例请求 >**

**：.** AD FS 是否支持 PKCE 扩展？
</br>**的.** 服务器2019中的 AD FS 支持 OAuth 授权代码授予流的代码交换（PKCE）的证明密钥 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Windows Server 2016 的 Active Directory 联合身份验证服务的新增功能   
如果你正在查找有关早期版本的 AD FS 的信息，请参阅以下文章：  
 [Windows Server 2012 或 2012 R2 中的 ADFS](https://technet.microsoft.com/library/hh831502.aspx)和[AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory 联合身份验证服务在各种应用程序（包括 Office 365、基于云的 SaaS 应用程序以及企业网络上的应用程序）中提供访问控制和单一登录。  
* 对于 IT 组织，它使你能够基于相同的一组凭据和策略向本地和在云中的新式应用程序提供登录和访问控制。    
* 对于用户来说，它提供了使用相同的熟悉帐户凭据的无缝登录。  
* 对于开发人员而言，它提供了一种简单的方法来对其身份在组织目录中的用户进行身份验证，以便你可以专注于你的应用程序，而不是身份验证或身份验证。  

本文介绍 Windows Server 2016 （AD FS 2016）中 AD FS 的新增功能。  

## <a name="eliminate-passwords-from-the-extranet"></a>消除 Extranet 中的密码  
AD FS 2016 支持三个新选项，无需密码即可登录，使组织能够避免网络泄露的钓鱼、泄露或被盗的密码。 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>用 Azure 多重身份验证登录
AD FS 2016 基于 Windows Server 2012 R2 中 AD FS 的多重身份验证（MFA）功能，只允许使用 Azure MFA 代码登录，无需首先输入用户名和密码。

* 通过 Azure MFA 作为主要身份验证方法，系统会提示用户输入其用户名和 Azure Authenticator 应用中的 OTP 代码。  
* 使用 Azure MFA 作为辅助或附加身份验证方法，用户可以提供主要身份验证凭据（使用 Windows 集成身份验证、用户名和密码、智能卡、用户或设备证书），然后看到文本提示，基于语音或 OTP 的 Azure MFA 登录名。  
* 利用新的内置 Azure MFA 适配器，与 AD FS 的 Azure MFA 的设置和配置从未简单一些。
* 组织可以利用 Azure MFA，无需本地 Azure MFA 服务器。
* 可为 intranet 或 extranet 配置 Azure MFA，或将其作为任何访问控制策略的一部分进行配置。

有关 Azure MFA 与 AD FS 的详细信息
*  [配置 AD FS 2016 和 Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>来自符合设备的无密码访问
AD FS 2016 基于以前的设备注册功能构建，以便基于设备符合性状态启用登录和访问控制。 用户可以使用设备凭据登录，并在设备属性发生更改时重新评估符合性，使你始终能够确保策略得以强制实施。  这将启用

* 仅从托管和/或符合的设备启用访问权限  
* 仅从托管和/或合规设备启用 Extranet 访问  
* 对于不受管理或不符合的计算机，需要多重身份验证  

AD FS 提供混合方案中条件访问策略的本地组件。 向云资源的条件性访问注册 Azure AD 设备时，还可将设备标识用于 AD FS 策略。

![新增](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 有关在云中使用基于设备的条件性访问的详细信息   
 *  [Azure Active Directory 条件性访问](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

有关将基于设备的条件性访问与 AD FS 一起使用的详细信息
*  [使用 AD FS 规划基于设备的条件性访问](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [AD FS 中的访问控制策略](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>通过 Windows Hello 企业版登录   
Windows 10 设备引入了 Windows Hello 和 Windows Hello 企业版，将用户密码替换为受用户手势保护的强设备绑定用户凭据（PIN、指纹等生物识别手势或面部识别）。 AD FS 2016 支持这些新的 Windows 10 功能，使用户可以从 intranet 或 extranet 登录到 AD FS 应用程序，而无需提供密码。

有关在组织中使用 Microsoft Windows Hello for Business 的详细信息
*  [在组织中启用 Windows Hello 企业版](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>保护对应用程序的访问

### <a name="modern-authentication"></a>新式身份验证
AD FS 2016 支持最新的新式协议，可为 Windows 10 提供更好的用户体验，以及最新的 iOS 和 Android 设备和应用。  

有关详细信息，请参阅面向[开发人员的 AD FS 方案](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>配置访问控制策略，无需了解声明规则语言  
以前，AD FS 管理员必须使用 AD FS 声明规则语言配置策略，这样就难以配置和维护策略。 使用访问控制策略时，管理员可以使用内置模板来应用常见的策略，例如
* 仅允许 intranet 访问
* 允许所有人，并需要来自 Extranet 的 MFA
* 允许每个人并要求来自特定组的 MFA

使用向导驱动的过程可以轻松地自定义模板，以添加异常或其他策略规则，并可应用于一个或多个应用程序以实现一致的策略。

有关详细信息，请参阅[AD FS 中的访问控制策略。](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>使用非 AD LDAP 目录启用登录  
许多组织都有 Active Directory 和第三方目录的组合。 添加了 AD FS 对用于验证 LDAP 兼容目录中存储的用户进行身份验证的支持，现在可以将 AD FS 用于：
* 第三方中的用户、符合 LDAP v3 的目录
* 未配置 Active Directory 双向信任 Active Directory 林中的用户
* Active Directory 轻型目录服务（AD LDS）中的用户

有关详细信息，请参阅[配置 AD FS 以便对存储在 LDAP 目录中的用户进行身份验证。](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>更好的登录体验
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>为 AD FS 应用程序自定义登录体验  
我们听说，你可以自定义每个应用程序的登录体验，这是一项很好的可用性改进，特别是对于提供代表多个不同公司或品牌的应用程序的组织。  

以前，Windows Server 2012 R2 中的 AD FS 为所有信赖方应用程序提供了一种通用的登录体验，并能够基于每个应用程序自定义基于文本的内容子集。 对于 Windows Server 2016，你可以自定义每个应用程序的消息，但不包括图像、徽标和 web 主题。 此外，还可以创建新的自定义 web 主题，并按依赖方应用这些主题。  

有关详细信息，请参阅[AD FS 用户登录自定义。](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>可管理性和操作增强  
以下部分介绍了 Windows Server 2016 中 Active Directory 联合身份验证服务引入的改进操作方案。  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>简化的审核，便于管理管理  
在 Windows Server 2012 R2 的 AD FS 中，为单个请求生成了大量审核事件，有关登录或令牌颁发活动的相关信息可能不存在（在某些版本的 AD FS 中）或分布于多个审核事件。 默认情况下，AD FS 审核事件处于关闭状态，原因是它们的详细特性。  
随着 AD FS 2016 的发布，审核变得越来越简单，更不详细。  

有关详细信息，请参阅[Windows Server 2016 中的审核增强功能 AD FS。](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>提高了与 SAML 2.0 的互操作性，以参与 confederations  
AD FS 2016 包含附加的 SAML 协议支持，包括支持基于包含多个实体的元数据导入信任。 这使你可以将 AD FS 配置为参与 confederations，例如 InCommon 联合身份验证和其他符合 eGov 2.0 标准的实现。  

有关详细信息，请参阅[与 SAML 2.0 的互操作性改进。](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>针对联合 O365 用户的简化密码管理  
可以将 Active Directory 联合身份验证服务（AD FS）配置为向受 AD FS 保护的信赖方信任（应用程序）发送密码过期声明。 如何使用这些声明取决于应用程序。 例如，使用 Office 365 作为你的信赖方时，已将更新部署到 Exchange 和 Outlook，以通知联合用户即将过期的密码。  

有关详细信息，请参阅[Configure AD FS to send password 过期声明。](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>在 windows server 2012 R2 中从 AD FS 移动到 Windows Server 2016 中的 AD FS 更简单  
以前，迁移到新版本的 AD FS 需要从旧的场导出配置，并将其导入全新的并行场。  

现在，从 Windows Server 2012 R2 上的 AD FS 迁移到 Windows Server 2016 上的 AD FS 已变得更加简单。 只需将新的 Windows Server 2016 服务器添加到 Windows Server 2012 R2 场，场将作用于 Windows Server 2012 R2 场行为级别，因此它的外观和行为就像 Windows Server 2012 R2 场。  

然后，将新的 Windows Server 2016 服务器添加到场，验证该功能并从负载均衡器中删除旧服务器。 所有场节点运行 Windows Server 2016 后，便可以将场行为级别升级到2016，并开始使用新功能。  

有关详细信息，请参阅[升级到 Windows Server 2016 中的 AD FS。](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  

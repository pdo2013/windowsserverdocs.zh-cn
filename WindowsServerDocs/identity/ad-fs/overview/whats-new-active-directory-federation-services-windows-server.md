---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Windows Server 2016 的 Active Directory 联合身份验证服务的新增功能
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fbb289c16d82da79aded49e3af4134ac7f6df325
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188704"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Active Directory 联合身份验证服务的新增功能


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>什么是 Active Directory 联合身份验证服务的 Windows Server 2019 中的新增功能

### <a name="protected-logins"></a>受保护的登录名
以下是更新到受保护的登录名在 AD FS 2019 中可用的简短摘要：
- **外部身份验证提供程序作为主要**-客户可以使用第三方身份验证产品作为第一个因素并不公开的密码作为第一个因素。 在可以证明外部身份验证提供程序的 2 个因素的情况下，它可以声明 MFA。 
- **作为附加身份验证的密码身份验证**-客户现在有完全受支持的收件箱选项，若要使用的密码，仅有的其他因素后密码更少选项使用作为第一个因素。 这提高了从客户不得不下载 github 适配器的支持，是因为 ADFS 2016 的客户体验。 
- **可插入的风险评估模块**-客户现在可以构建他们自己即插即用在模块在预身份验证阶段期间阻止某些类型的请求。 这使得客户更轻松地使用例如标识保护的云智能来阻止有风险的用户或有风险的事务的登录名。  有关详细信息请参阅[构建插件与 AD FS 2019 风险评估模型](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **ESL 改进**-改进了 ESL QFE 2016 中通过添加以下功能
    - 使客户能够由自 ADFS 2012R2 起可用的经典 extranet 锁定功能保护的同时处于审核模式。 当前 2016年客户已在审核模式下的任何保护。 
    - 启用独立的锁定阈值的熟悉的位置。 这样，可以使用常见的服务帐户来滚动更新的影响最小密码运行的应用程序的多个实例。 

### <a name="additional-security-improvements"></a>其他安全改进
AD FS 2019 中提供了以下其他安全改进：
- **使用智能卡登录名的远程 PSH** -客户可以立即使用智能卡远程连接到通过 PSH ADFS 和使用，来管理所有 PSH 函数包括多节点 PSH cmdlet。
- **自定义 HTTP 标头**-客户现在可以自定义 ADFS 响应在发出的 HTTP 标头。 这包括以下标头
     - HSTS:这会告知，ADFS 终结点可仅用于为兼容的浏览器的 HTTPS 终结点上强制实施
     - x 帧选项：允许 ADFS 管理员允许特定信赖方嵌入 Iframe ADFS 交互式登录页。 这应谨慎，并且只能 HTTPS 主机上使用。 
     - 将来的标头：还可以配置未来的其他标头。 

有关详细信息请参阅[AD FS 2019 的自定义 HTTP 安全响应标头](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>身份验证/策略功能
以下身份验证/策略功能是在 AD FS 2019 中：
- **指定每个 RP 的其他身份验证的身份验证方法**-客户可以现在使用声明规则以确定要为附加身份验证提供程序调用的其他身份验证提供程序。 这是有用的对于 2 用例
    - 客户正在从一种其他身份验证提供程序转换为另一个。 这种方式为它们载入用户到较新的身份验证提供程序，他们可以使用组，以控制哪些额外的身份验证提供程序调用。
    - 客户对于某些应用程序有特定的附加身份验证提供程序 （例如证书） 的需求。 
- **将 TLS 基于设备身份验证限制为需要它的应用程序**-客户现在可以限制客户端 TLS 基于设备的身份验证，仅执行设备的应用程序基于条件性访问。 这不需要基于 TLS 设备身份验证的应用程序可以防止任何不需要会提示你输入设备身份验证 （或如果客户端应用程序不能处理故障）。
- **MFA 新鲜度支持**-AD FS 现在支持的功能到重新执行基于最新的第二个身份凭据的第二个身份凭据。 这允许客户执行具有 2 个因素并仅提示定期更新的第二个因素的初始事务。 此选项仅适用于应用程序可以提供附加参数在请求中的并不是在 ADFS 中的可配置设置。 此参数支持的 Azure AD 时配置"记住我的 X 天的 MFA"和 supportsMFA 标志设置为在 Azure AD 中的联合的域信任设置，则返回 true。 

### <a name="sign-in-sso-improvements"></a>登录 SSO 改进
已在 AD FS 2019 中进行了以下单一登录 SSO 改进：

- [分页用户体验与中心主题](../operations/AD-FS-paginated-sign-in.md)-ADFS 现在已移至分页的 UX 流允许 ADFS 验证并提供更多更流畅的登录体验。 ADFS 现在使用居中的 UI （而不是屏幕的右侧）。 你可能需要较新的徽标和背景图像，以便与这种体验保持一致。 这也反映在 Azure AD 中提供的功能。
- **Bug 修复：Win10 设备执行 PRT 身份验证时的持久 SSO 状态**这解决了其中 MFA 状态未保留的 Windows 10 设备使用 PRT 身份验证时出现问题。 此问题的结果是，将获取最终用户经常提示为第二个身份凭据 (MFA)。 解决方法也使体验一致设备身份验证已成功执行通过客户端 TLS 和 PRT 机制时。 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>用于构建现代业务线应用程序的支持
用于构建现代 LOB 应用了以下支持已添加到 AD FS 2019:

 - **Oauth 设备流/配置文件**-AD FS 现在支持 OAuth 流设备配置文件不具有 UI 的表面区域，以支持丰富的登录体验的设备上执行的登录名。 这允许用户完成其他设备上的登录体验。 此功能需要 Azure CLI 在 Azure Stack 中的体验，并可以在其他情况下使用。 
 - **Resource 参数的删除**-AD FS 现在已删除指定资源参数符合最新的 Oauth 规范的要求。 客户端现在可以提供信赖方信任标识符作为作用域参数此外到请求的权限。 
 - **在 AD FS 响应中的 CORS 标头**-客户现在可以生成允许端 JS 库来通过查询中的 AD FS 上的 OIDC 发现文档的签名密钥验证 id_token 的签名的客户端的单页面应用程序。 
 - **PKCE 支持**-AD FS 添加 PKCE 的支持来提供内 OAuth 的安全身份验证代码流。 这会额外的安全层添加到此流，以防止劫持代码和重播从不同的客户端。 
 - **Bug 修复：发送 x5t 和 kid 声明**-这是一个小 bug 修复。 现在，AD FS 此外发送 kid 声明来表示用于验证签名的密钥 id 提示。 以前 AD FS 仅发送此 x5t 声明。

### <a name="supportability-improvements"></a>可支持性的改进
以下可支持性改进不属于 AD FS 2019:
- **将错误详细信息发送到 AD FS 管理员**-允许管理员配置最终用户能够发送与在最终用户身份验证失败，如方便使用的压缩存档存储相关的调试日志。 管理员还可以配置的 automail 压缩的文件到会审电子邮件帐户，或自动创建票证基于电子邮件的 SMTP 连接。 

### <a name="deployment-updates"></a>部署更新
在 AD FS 2019 中现在包含以下部署更新：
- **场行为级别 2019年**-如 ad FS 2016 中，没有启用上文所述的新功能所需的新场行为级别版本。 这允许从：
    - 2012 R2-> 2019
    - 2016 -> 2019   

### <a name="saml-updates"></a>SAML 更新
以下 SAML 更新是在 AD FS 2019 中：
- **Bug 修复：聚合的联合身份验证中修复的 bug** -发生了大量 bug 修复，关于聚合联合身份验证支持 (例如 InCommon)。 修复了围绕以下： 
  - 改进了大型 # 聚合的联合身份验证元数据文档中的实体的扩展。以前，这会失败，"ADMIN0017"错误。 
  - 使用通过 Get AdfsRelyingPartyTrustsGroup PSH cmdlet ScopeGroupID 参数进行查询。 
  - 处理重复的 entityID 周围的错误条件


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>作用域参数中的 azure AD 样式资源规范 
以前，AD FS 要求的所需的资源和作用域中的任何身份验证请求中的单独参数。 例如，典型的 oauth 请求会看到如下所示：7 **https:&#47;&#47;fs.contoso.com/adfs/oauth2/authorize？</br>response_type = 代码 & client_id = claimsxrayclient 资源 = urn: microsoft:</br>adfs:claimsxray 和作用域 = oauth 和 redirect_uri = https:&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray /TokenResponse & prompt = 登录名**
 
与 AD FS 服务器 2019年中，您现在可以传递作用域参数中嵌入的资源值。 这是与一个可以如何针对 Azure AD 的身份验证还保持一致。 

作用域参数现在可以按以空格分隔的列表，其中每个条目是与资源/作用域的结构。 例如  

**< 创建有效的示例请求 >**
> [!NOTE]
> 身份验证请求中，可以指定只有一个资源。 如果在请求中包含多个资源，AD FS 将返回错误，身份验证将不会成功。 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>校验密钥的 for Code Exchange (PKCE) 支持 oAuth 
OAuth 公共客户端使用授权代码授予将易受授权代码拦截攻击。  RFC 7636 中也描述了攻击。 若要缓解这种攻击，服务器 2019年中的 AD FS 支持证明密钥，为 Code Exchange (PKCE) 用于 OAuth 授权代码授予流。 
 
若要利用对 PKCE 的支持，此规范将其他参数添加到在 OAuth 2.0 授权和访问令牌请求中。

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. 客户端创建和记录名为"code_verifier"机密派生的转换后的版本"t(code_verifier)"（称为"code_challenge"），其中发送 OAuth 2.0 授权请求中以及转换方法"t_m"。 

B. 授权终结点但记录"t(code_verifier)"和转换方法来响应像往常一样。 

C. 然后，客户端像往常一样访问令牌请求中发送的授权代码，但包括在 (A) 生成的"code_verifier"机密。 

D. AD FS 转换"的 code_verifier"，并将它与"t(code_verifier)"进行比较 (B) 中。  如果它们不相等，则拒绝访问。 

#### <a name="faq"></a>常见问题 
**Q.** 可以像对 Azure AD 中如何执行请求的范围值的一部分传递资源值？ 
</br>**A.** 与 AD FS 服务器 2019年中，您现在可以传递作用域参数中嵌入的资源值。 作用域参数现在可以按以空格分隔的列表，其中每个条目是与资源/作用域的结构。 例如  
**< 创建有效的示例请求 >**

**Q.** AD FS 是否支持 PKCE 扩展？
</br>**A.** 服务器 2019年中的 AD FS 支持证明密钥，为 Code Exchange (PKCE) 用于 OAuth 授权代码授予流 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Windows Server 2016 的 Active Directory 联合身份验证服务的新增功能   
如果您要查找有关早期版本的 AD FS 的信息，请参阅以下文章：  
 [Windows Server 2012 或 2012 R2 中的 ad FS](https://technet.microsoft.com/library/hh831502.aspx)和[AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory 联合身份验证服务提供访问控制和上跨多种应用程序包括 Office 365，云的单一登录基于 SaaS 应用程序和企业网络上的应用程序。  
* 为 IT 组织，它可以提供登录和访问控制现代和传统应用程序在本地和云中，基于相同的凭据和策略集。    
* 对于用户，它使用熟悉的相同帐户凭据提供无缝登录。  
* 对于开发人员，它提供了其标识位于组织的目录，以便你可以专注于你的工作应用程序、 不身份验证或标识的用户进行身份验证的简单方法。  

本文介绍什么是 Windows Server 2016 (AD FS 2016) 中的 AD FS 中的新增功能。  

## <a name="eliminate-passwords-from-the-extranet"></a>来自 Extranet 消除密码  
AD FS 2016 启用三个新选项以进行无密码，从而使组织能够避免网络风险登录破坏从被骗泄漏或被盗的密码。 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>使用 Azure 多重身份验证登录
AD FS 2016 基于多重身份验证 (MFA) 的 Windows Server 2012 R2 中的 AD FS 功能，从而仅使用 Azure MFA 代码，而无需输入用户名和密码登录。

* 与 Azure MFA 的主要身份验证方法，其用户名和 OTP 代码从 Azure 验证器应用程序提示用户。  
* 使用 Azure MFA 的辅助或其他身份验证方法，用户提供主要身份验证凭据 （使用 Windows 集成身份验证、 用户名和密码、 智能卡或用户或设备证书），则会看到的提示的文本，语音或 OTP 基于 Azure MFA 登录名。  
* 使用新的内置 Azure MFA 适配器，安装和配置 Azure mfa 与 AD FS 从未如此简单。
* 组织可以充分利用 Azure MFA 无需本地 Azure MFA 服务器。
* 为 intranet 或 extranet 或任何访问控制策略的一部分，可以配置 azure MFA。

有关 Azure MFA AD FS 的详细信息
*  [配置 AD FS 2016 和 Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>从合规的设备的密码无访问权限
AD FS 2016 基于以前的设备注册功能，以启用登录和访问控制基于设备符合性状态。 用户可以使用设备凭据，在登录和法规遵从性重新计算时更改设备属性，以便您可以始终确保强制实施策略。  启用策略例如

* 只能从托管和/或符合的设备启用访问权限  
* 仅从托管和/或符合的设备启用的 Extranet 访问  
* 对于不是管理或不符合的计算机需要多重身份验证  

AD FS 提供混合方案中的条件性访问策略上的本地的组件。 与条件性访问云资源的 Azure AD 注册设备时，可以为 AD FS 的策略以及使用的设备标识。

![新的新增功能](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 有关使用设备的详细信息基于在云中的条件性访问   
 *  [Azure Active Directory 条件性访问](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

有关使用设备的详细信息基于 AD FS 的条件性访问
*  [规划设备基于 AD FS 的条件性访问](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [在 AD FS 访问控制策略](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>使用 Windows Hello for Business 登录   
Windows 10 设备引入 Windows Hello 和 Windows hello 企业版，用户的密码替换为强设备绑定用户凭据保护的用户的手势 （PIN、 指纹或面部识别等生物识别手势）。 AD FS 2016 支持这些新的 Windows 10 功能，使用户可以登录到 AD FS 应用程序从 intranet 或 extranet 而无需提供一个密码。

有关 Microsoft Windows Hello 企业版在组织中使用的详细信息
*  [启用 Windows Hello for Business 中你的组织](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>对应用程序的安全访问

### <a name="modern-authentication"></a>新式身份验证
AD FS 2016 支持最新的新式协议为 Windows 10，以及最新的 iOS 和 Android 设备和应用程序提供更好的用户体验。  

有关详细信息请参阅[面向开发人员的 AD FS 方案](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>配置访问控制策略，而无需了解声明规则语言  
以前，AD FS 管理员必须配置策略使用 AD FS 声明规则语言，因此很难配置和维护的策略。 使用访问控制策略，管理员可以使用内置的模板将应用通用策略，例如
* 允许 intranet 访问权限
* 授权所有人并要求进行 MFA 从 Extranet
* 授权所有人并要求从特定的组进行 MFA

模板轻松地自定义使用向导驱动的过程中添加异常或更多策略规则，可应用于一个或多个应用程序来实施一致的策略。

有关详细信息请参阅[AD FS 中的访问控制策略。](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>与非 AD LDAP 目录上启用登录  
许多组织的 Active Directory 和第三方目录的组合。 LDAP v3 兼容目录中存储的用户进行身份验证的 AD FS 支持的添加后，现在为使用 AD FS:
* 第三方 LDAP v3 兼容目录中的用户
* 未向其配置 Active Directory 双向信任关系的 Active Directory 林中的用户
* Active Directory 轻型目录服务 (AD LDS) 中的用户

有关详细信息请参阅[配置 AD FS 进行身份验证 LDAP 目录中存储的用户。](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>更好地登录体验
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>自定义登录体验的 AD FS 应用程序  
我们收到您自定义每个应用程序的登录体验的功能会很好的可用性改进，特别是对于表示多个不同的公司或品牌的应用程序提供登录的组织。  

以前，Windows Server 2012 R2 中的 AD FS 提供了常见的登录体验的所有信赖方应用程序，自定义的文本子集的功能与基于每个应用程序的内容。 使用 Windows Server 2016 中，可以自定义不仅消息，但映像，每个应用程序的徽标和 web 主题。 此外，可以创建新的自定义 web 主题，并应用这些按信赖方。  

有关详细信息请参阅[AD FS 用户登录自定义。](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>可管理性和操作的增强功能  
以下部分介绍与 Active Directory 联合身份验证服务在 Windows Server 2016 中引入的改进操作方案。  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>简化为更轻松地管理管理审核  
在 AD FS 有 Windows Server 2012 R2 的已生成用于单个请求与日志中的相关信息的大量审核事件或令牌颁发活动是不存在 （在某些版本的 AD FS） 或跨多个审核事件。 默认情况下，AD FS 审核事件处于关闭状态由于其详细的特性。  
AD FS 2016 的版本中，审核变得更简单和不太详细。  

有关详细信息请参阅[审核 Windows Server 2016 中的 AD FS 的增强功能。](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>改进了针对参与 confederations 与 SAML 2.0 互操作性  
AD FS 2016 包含其他 SAML 协议的支持，包括支持导入基于包含多个实体的元数据的信任关系。 这使你可以配置 AD FS 参与 confederations 如 InCommon 联合身份验证并符合 eGov 2.0 标准其他实现。  

有关详细信息请参阅[改进了与 SAML 2.0 互操作性。](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>为简化的密码管理联合 O365 用户  
到受 AD FS 的信赖方信任 （应用程序），可以配置 Active Directory 联合身份验证服务 (AD FS) 来发送密码过期声明。 如何使用这些声明取决于应用程序。 例如，与 Office 365 作为信赖方，更新了对 Exchange 和 Outlook 以通知他们即将-到--已过期的密码的联合的用户。  

有关详细信息请参阅[配置 AD FS 以发送密码过期声明。](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>从 Windows Server 2012 R2 中的 AD FS 迁移到 Windows Server 2016 中的 AD FS 是更轻松  
以前，迁移到新版本的 AD FS 需要从旧场中导出配置和导入到全新的并行场。  

现在，从 AD FS Windows Server 2012 R2 上转移到 Windows Server 2016 上的 AD FS 变得容易。 只需将新的 Windows Server 2016 服务器添加到 Windows Server 2012 R2 场，并在 Windows Server 2012 R2 场行为级别中，执行操作场，因此它的外观和行为就像 Windows Server 2012 R2 场。  

然后，将新的 Windows Server 2016 服务器添加到场、 验证功能和从负载均衡器中删除较旧服务器。 一旦所有场节点都运行 Windows Server 2016，你现可将场行为级别升级到 2016年并开始使用新功能。  

有关详细信息请参阅[升级到 Windows Server 2016 中的 AD FS。](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  

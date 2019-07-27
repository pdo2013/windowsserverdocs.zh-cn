---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: 配置备用登录 ID
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 220b409b2e0bcc5e5a01aeb9f244ebaa55ac0e1e
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544608"
---
# <a name="configuring-alternate-login-id"></a>配置备用登录 ID


## <a name="what-is-alternate-login-id"></a>什么是备用登录 ID？
在大多数情况下, 用户使用其 UPN (用户主体名称) 登录到其帐户。 但在某些环境中, 由于公司策略或本地业务线应用程序依赖关系, 用户可能正在使用某种其他形式的登录。 

>[!NOTE]
>Microsoft 推荐的最佳做法是将 UPN 与主 SMTP 地址匹配。 本文解决了不能修正 UPN 以匹配的少量客户。

例如, 他们可以使用其电子邮件 id 进行登录, 并且该 id 可以不同于其 UPN。 这在其 UPN 不可路由的情况下尤其常见。 请考虑使用 UPN jdoe@contoso.local和电子邮件地址jdoe@contoso.com的用户 Jane Doe。 Jane 可能甚至不知道 UPN, 因为她始终使用其电子邮件 id 进行登录。 使用任何其他登录方法, 而不是 UPN 构成备用 ID。 有关如何创建 UPN 的详细信息, 请参阅[Azure AD UserPrincipalName 填充](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname)。

Active Directory 联合身份验证服务 (AD FS) 允许使用 AD FS 的联合应用程序使用备用 ID 登录。 这使管理员可以指定用于登录的默认 UPN 的替代方法。 AD FS 已使用 Active Directory 域服务 (AD DS) 接受的任意形式的用户标识符支持。 当配置为备用 ID 时, AD FS 允许用户使用配置的备用 ID 值 (如电子邮件 ID) 登录。使用备用 ID 可以采用 SaaS 提供程序 (如 Office 365), 而无需修改本地 Upn。 它还使你能够使用使用者预配的标识支持业务线服务应用程序。

## <a name="alternate-id-in-azure-ad"></a>Azure AD 中的替代 id
组织可能需要在以下情况下使用备用 ID:
1. 本地域名称不可路由, 例如 Contoso. local 和, 因此默认的用户主体名称是不可路由的 (jdoe@contoso.local)。 由于本地应用程序依赖项或公司策略, 无法更改现有 UPN。 Azure AD 和 Office 365 要求与 Azure AD 目录关联的所有域后缀可以完全通过 internet 路由。 
2. 本地 UPN 与用户的电子邮件地址不相同, 若要登录到 Office 365, 用户使用电子邮件地址和 UPN 不能使用, 因为组织受到限制。
   在上述方案中, 具有 AD FS 的备用 ID 使用户能够登录到 Azure AD, 而无需修改本地 Upn。 

## <a name="end-user-experience-with-alternate-login-id"></a>具有备用登录 ID 的最终用户体验
最终用户体验根据用于备用登录 id 的身份验证方法而有所不同。目前有三种方法可以实现使用备用登录 id。  它们分别是：

- **常规身份验证 (旧)** -使用基本身份验证协议。
- **新式身份验证**-基于 ACTIVE DIRECTORY 身份验证库 (ADAL) 登录到应用程序。 这将启用多因素身份验证 (MFA)、基于 SAML 的第三方标识提供程序与 Office 客户端应用程序、智能卡和基于证书的身份验证等登录功能。
- **混合新式身份验证**-提供新式身份验证的所有优点, 并使用户能够使用从云获取的授权令牌访问本地应用程序。

>[!NOTE]
> 为了获得最佳体验, Microsoft 强烈建议混合新式身份验证。



## <a name="configure-alternate-logon-id"></a>配置备用登录 ID
使用 Azure AD Connect 建议使用 Azure AD Connect 为环境配置备用登录 ID。

- 有关 Azure AD Connect 的新配置, 请参阅连接到 Azure AD, 了解有关如何配置备用 ID 和 AD FS 场的详细说明。
- 有关现有 Azure AD Connect 安装的详细说明, 请参阅更改用户登录方法, 了解有关将登录方法更改为 AD FS 的说明

当 Azure AD Connect 提供 AD FS 环境的详细信息时, 它会自动在 AD FS 上检查是否存在正确的 KB, 并为备用 ID 配置 AD FS, 其中包括 Azure AD 联合身份验证信任的所有必需的正确声明规则。 在向导外无需执行其他步骤来配置备用 ID。

>[!NOTE]
> Microsoft 建议使用 Azure AD Connect 来配置备用登录 ID。

### <a name="manually-configure-alternate-id"></a>手动配置备用 ID
若要配置备用登录 ID, 必须执行以下任务:将 AD FS 声明提供程序信任配置为启用备用登录 ID

1.  如果你有服务器 2012R2, 请确保在所有 AD FS 服务器上都安装了 KB2919355。 可以通过 Windows 更新服务获取此服务或直接下载。 

2.  通过在服务器场中的任何联合服务器上运行以下 PowerShell cmdlet 来更新 AD FS 配置 (如果你有 WID 场, 则必须在场中的主 AD FS 服务器上运行此命令):

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID**是要用于登录的属性的 LDAP 名称。

**LookupForests**是用户所属的林 DNS 的列表。

若要启用备用登录 ID 功能, 必须使用一个非 null 的有效值来配置-AlternateLoginID 和-LookupForests 参数。

在下面的示例中, 你将启用备用登录 ID 功能, 使具有 contoso.com 和 fabrikam.com 林中的帐户的用户可以通过其 "mail" 特性登录到支持 AD FS 的应用程序。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3. 若要禁用此功能, 请将这两个参数的值设置为 null。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>具有替代 ID 的混合新式身份验证

>[!IMPORTANT]
>以下仅针对 AD FS 而不是第三方标识提供者进行过测试。

### <a name="exchange-and-skype-for-business"></a>Exchange 和 Skype for Business
如果在 Exchange 和 Skype for Business 中使用备用登录 id, 则根据是否使用 HMA, 用户体验会有所不同。

>[!NOTE]
>为了获得最佳的最终用户体验, Microsoft 建议使用混合新式身份验证。

有关详细信息, 请参阅[混合新式身份验证概述](https://support.office.com/en-us/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Exchange 和 Skype for Business 的先决条件
下面是用备用 ID 实现 SSO 的先决条件。

- Exchange Online 应启用新式身份验证。
- Skype for Business (SFB) Online 应启用新式身份验证。
- Exchange 内部部署应启用新式身份验证。  所有 Exchange 服务器都需要 exchange 2013 CU19 或 Exchange 2016 CU18 和更高。 环境中没有 Exchange 2010。
- 本地 Skype for Business 应启用新式身份验证。
- 必须使用启用了新式身份验证的 Exchange 和 Skype 客户端。 所有服务器都必须运行 SFB Server 2015 CU5。
- 支持新式身份验证的 Skype for Business 客户端
   - iOS、Android、Windows Phone
   - SFB 2016 (默认情况下, MA 处于启用状态, 但请确保它未被禁用。)
   - SFB 2013 (MA 默认情况下处于关闭状态, 因此请确保 MA 已打开。)
   - SFB Mac 桌面
- 支持新式身份验证并支持 AltID regkeys 的 Exchange 客户端
    - 仅限 Office Pro Plus 2016





#### <a name="supported-office-version"></a>支持的 Office 版本

如果其他配置未完成, 则使用替代 id 为 SSO 配置包含备用 id 的你的目录可能会导致额外的身份验证提示。 请参阅一文, 了解可能对备用 id 的用户体验产生的影响。

通过以下附加配置, 用户体验得到了重大改进, 你可以为组织中的备用 id 用户提供接近零的身份验证提示。

##### <a name="step-1-update-to-required-office-version"></a>步骤 1： 更新为所需 Office 版本
Office 版本 1712 (build no 8827.2148) 和更高版本已更新身份验证逻辑来处理备用 id 方案。 若要利用新的逻辑, 需要将客户端计算机更新为 Office 版本 1712 (不生成 8827.2148) 和更高版本。

##### <a name="step-2-update-to-required-windows-version"></a>步骤 2： 更新为所需的 Windows 版本
Windows 版本1709及更高版本已更新身份验证逻辑来处理备用 id 方案。 为了利用新的逻辑, 需要将客户端计算机更新到 Windows 版本1709及更高版本。

##### <a name="step-3-configure-registry-for-impacted-users-using-group-policy"></a>步骤 3： 使用组策略为受影响的用户配置注册表
Office 应用程序依赖于目录管理员推送的信息来标识备用 id 环境。 需要将以下注册表项配置为帮助 office 应用程序通过备用 id 对用户进行身份验证, 而不显示任何额外的提示

|要添加的 Regkey|Regkey 数据名称、类型和值|Windows 7/8|Windows 10|描述|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER\Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|必填|必需|此 regkey 的值是组织的租户中已验证的自定义域名。 例如, 如果 Contoso.com 是租户 Contoso.onmicrosoft.com 中某个已验证的自定义域名, 则 Contoso corp 可以在此 regkey 中提供值 Contoso.com。|
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|对于 Outlook 2016 ProPlus 是必需的|对于 Outlook 2016 ProPlus 是必需的|此 regkey 的值可以为 1/0, 以指示 Outlook 应用程序是否应参与改进后的备用 id 身份验证逻辑。|
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|必填|必填|此 regkey 可用于将 STS 设置为 internet 设置中的受信任区域。 标准 ADFS 部署建议将 ADFS 命名空间添加到 Internet Explorer 的本地 Intranet 区域|

## <a name="new-authentication-flow-after-additional-configuration"></a>其他配置之后的新身份验证流

![身份验证流](media/Configure-Alternate-Login-ID/alt1a.png)

1. 的使用替代 id 在 Azure AD 中预配用户
   </br>b目录管理员将所需的 regkey 设置推送到受影响的客户端计算机
2. 用户在本地计算机上进行身份验证并打开 office 应用程序
3. Office 应用程序采用本地会话凭据
4. Office 应用程序使用管理员和本地凭据推送的域提示 Azure AD 进行身份验证
5. Azure AD 通过定向到正确的联合领域并颁发令牌来成功对用户进行身份验证

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>其他配置之后的应用程序和用户体验

### <a name="non-exchange-and-skype-for-business-clients"></a>非 Exchange 和 Skype for Business 客户端

|客户端|支持语句|备注|
| ----- | -----|-----|
|Microsoft Teams|支持|<li>Microsoft 团队支持 AD FS (SAML-P、WS 送、WS 信任和 OAuth) 以及新式身份验证。</li><li> 核心 Microsoft 团队 (如渠道、聊天和文件功能) 可用于备用登录 ID。</li><li>第一个和第三方应用必须由客户单独调查。 这是因为每个应用程序都有自己的可支持性身份验证协议。</li>|     
|OneDrive for Business|支持的-客户端注册表项建议 |配置备用 ID 后, 会在验证字段中预先填充本地 UPN。 需要将其更改为所使用的备用标识。 建议使用本文中提到的客户端注册表项:Office 2013 和 Lync 2013 会定期提示输入 SharePoint Online、OneDrive 和 Lync Online 的凭据。|
|OneDrive for business Mobile 客户端|支持|| 
|Office 365 Pro Plus 激活页|支持的-客户端注册表项建议|配置备用 ID 后, 会在验证字段中预先填充本地 UPN。 需要将其更改为所使用的备用标识。 建议使用本文中提到的客户端注册表项:Office 2013 和 Lync 2013 会定期提示输入 SharePoint Online、OneDrive 和 Lync Online 的凭据。|

### <a name="exchange-and-skype-for-business-clients"></a>Exchange 和 Skype for Business 客户端

|客户端|支持声明-包含 HMA|支持声明-无 HMA|
| ----- |----- | ----- |
|Outlook|支持, 无额外提示|支持</br></br>对于 Exchange Online 的**新式验证**:支持</br></br>对于 Exchange Online 的**常规身份验证**:支持, 但有以下注意事项:</br><li>你必须在已加入域的计算机上, 并且已连接到公司网络 </li><li>只能在不允许对邮箱用户进行外部访问的环境中使用备用 ID。 这意味着, 如果用户已连接到企业网络、VPN 或通过直接访问计算机连接到其邮箱, 则用户只能通过受支持的方式对其邮箱进行身份验证, 但在配置 Outlook 配置文件时, 你会收到几个额外的提示。| 
|混合公用文件夹|支持, 无额外提示。|对于 Exchange Online 的**新式验证**:支持</br></br>对于 Exchange Online 的**常规身份验证**:不支持</br></br><li>如果使用备用 ID, 则不能扩展混合公用文件夹, 因此目前不应使用常规身份验证方法。|
|跨界委托|请参阅[配置 Exchange 以支持混合部署中的委派邮箱权限](https://technet.microsoft.com/library/mt784505.aspx)|请参阅[配置 Exchange 以支持混合部署中的委派邮箱权限](https://technet.microsoft.com/library/mt784505.aspx)|
|存档邮箱访问权限 (本地邮箱)|支持, 无额外提示|支持-在访问存档时, 用户会收到额外的凭据提示, 在出现提示时, 他们必须提供其备用 ID。| 
|Outlook Web Access|支持|支持|
|适用于 Android、IOS 和 Windows Phone 的 Outlook 移动应用|支持|支持|
|Skype for Business/Lync|支持, 无需额外提示|受支持 (除了所述), 但有可能导致用户混淆。</br></br>在移动客户端上, 仅当 SIP 地址 = 电子邮件地址 = 备用 ID 时才支持备用 Id。</br></br> 用户可能需要两次登录到 Skype for business 桌面客户端, 首先使用本地 UPN, 然后使用备用 ID。 (请注意, "登录地址" 实际上是 SIP 地址, 它可能与 "用户名" 不同, 但通常为)。 首次提示输入用户名时, 用户应输入 UPN, 即使使用备用 ID 或 SIP 地址错误地预填充了该 UPN。 用户通过 UPN 单击 "登录" 后, 将再次出现用户名提示, 此时间已预填充 UPN。 这次, 用户必须将此替换为备用 ID, 并单击 "登录" 以完成登录过程。 在移动客户端上, 用户应使用 SAM 样式格式 (域 \ 用户名), 而不是 UPN 格式在 "高级" 页中输入本地用户 ID。</br></br>成功登录后, 如果 Skype for Business 或 Lync 显示 "Exchange 需要你的凭据", 则需要提供对邮箱所在位置有效的凭据。 如果邮箱在云中, 则需要提供备用 ID。 如果邮箱位于本地, 则需要提供本地 UPN。| 

## <a name="additional-details--considerations"></a>& 注意事项的其他详细信息

-   备用登录 ID 功能适用于已部署 AD FS 的联合环境。  不支持在以下方案中执行此操作:
    -   无法 Azure AD 验证的不可路由域 (例如 Contoso)。
    -   未部署 AD FS 的托管环境。


-   启用后, "备用登录 ID" 功能仅可用于在 AD FS 支持的所有用户名/密码身份验证协议 (SAML-P、WS 送、WS 信任和 OAuth) 之间进行用户名/密码身份验证。


-   当执行 Windows 集成身份验证 (WIA) 时 (例如, 当用户尝试从 intranet 访问已加入域的计算机上的企业应用程序, 并且 AD FS 管理员已将身份验证策略配置为使用 WIA 进行 intranet), UPN isused用于身份验证。 如果已为信赖方配置了 "备用登录 ID" 功能的任何声明规则, 则应确保这些规则在 WIA 情况下仍然有效。

-   启用备用登录 ID 功能后, 需要为 AD FS 支持的每个用户帐户林从 AD FS 服务器访问至少一个全局编录服务器。 如果无法访问用户帐户林中的全局编录服务器, 则会导致 AD FS 回退为使用 UPN。 默认情况下, 所有域控制器都是全局编录服务器。

-   启用后, 如果 AD FS 服务器发现在所有配置的用户帐户林中指定了相同备用登录 ID 值的多个用户对象, 则该登录将失败。

-   当启用备用登录 ID 功能时, AD FS 首先尝试使用备用登录 ID 对最终用户进行身份验证, 然后在找不到可由备用登录 ID 标识的帐户时回退以使用 UPN。 如果要仍支持 UPN 登录, 则应确保备用登录 ID 和 UPN 之间没有冲突。 例如, 如果将一个邮件属性设置为另一个 UPN, 则会阻止其他用户通过其 UPN 进行登录。

-   如果管理员配置的其中一个林已关闭, AD FS 将继续使用已配置的其他林中的备用登录 ID 查找用户帐户。 如果 AD FS server 在搜索的林中查找唯一的用户对象, 则用户将成功登录。

-   你可能还需要自定义 AD FS 登录页面, 以便向最终用户授予有关备用登录 ID 的一些提示。 可以通过添加自定义登录页说明来执行此操作 (有关详细信息, 请参阅在用户名字段上[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)或自定义 "使用组织帐户登录" 字符串 (有关详细信息, 请参阅[AD FS 登录页的高级自定义](https://technet.microsoft.com/library/dn636121.aspx)。

-   包含备用登录 ID 值的新声明类型为**http: schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>事件和性能计数器
当启用备用登录 ID 时, 添加了以下性能计数器来测量 AD FS 服务器的性能:

-   备用登录 Id 身份验证: 使用备用登录 ID 执行的身份验证次数

-   备用登录 Id 身份验证数/秒: 每秒使用备用登录 ID 执行的身份验证次数

-   备用登录 ID 的平均搜索延迟: 管理员为备用登录 ID 配置的林的平均搜索延迟

以下是针对用户登录体验的各种错误情况和对 AD FS 所记录事件的相应影响:



|                       **错误事例**                        | **对登录体验的影响** |                                                              **引发**                                                              |
|--------------------------------------------------------------|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| 无法获取 user 对象的 SAMAccountName 值 |          登录失败           |                  事件 ID 364, 出现异常消息 MSIS8012:找不到用户 "{0}" 的 samAccountName。                   |
|        CanonicalName 属性不可访问         |          登录失败           |               事件 ID 364, 出现异常消息 MSIS8013:CanonicalName: 用户{0}{1}"" 的 "" 的格式不正确。                |
|        在一个林中找到多个用户对象        |          登录失败           | 事件 ID 364, 出现异常消息 MSIS8015:在林 "{1}" 中找到具有{0}标识 "" 的多个用户帐户, 标识:{2} |
|   跨多个林找到多个用户对象    |          登录失败           |           事件 ID 364, 出现异常消息 MSIS8014:在林中找到多个标识为{0}"" 的用户帐户:{1}            |

## <a name="see-also"></a>请参阅
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)



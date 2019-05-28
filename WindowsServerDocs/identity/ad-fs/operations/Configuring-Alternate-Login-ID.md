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
ms.openlocfilehash: 5bc43717f37fb3b14ac7f384a061ee64c734222d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189661"
---
# <a name="configuring-alternate-login-id"></a>配置备用登录 ID


## <a name="what-is-alternate-login-id"></a>备用登录 ID 是什么？
在大多数情况下，用户使用其登录到其帐户的 UPN （用户主体名称）。 但是，在某些环境中由于公司政策或本地业务线应用程序依赖关系，用户可能使用其他形式的单一登录。 

>[!NOTE]
>Microsoft 建议的最佳做法是在以使 UPN 与主 SMTP 地址相匹配。 此文章地址无法修正 UPN 的客户的小百分比的匹配。

例如，他们可以登录使用其电子邮件 id，这可能不同于其 UPN。 这是特别是一种常见的情况，在其 UPN 不可路由的情况下。 请考虑用户与 UPN Jane Doejdoe@contoso.local和电子邮件地址jdoe@contoso.com。 Jane 不可能甚至知道的 UPN 因为她具有始终用于其电子邮件 id 登录。 使用任何其他登录方法而不是 UPN 即表示备用 id。 For UPN 的方式的详细信息创建，请参阅[Azure AD UserPrincipalName 填充](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname)。

Active Directory 联合身份验证服务 (AD FS) 启用联合应用程序使用 AD FS 以使用备用登录 id。 这使管理员可以指定为默认值要用于登录的 UPN 的替代方法。 AD FS 已支持使用任何形式的接受的 Active Directory 域服务 (AD DS) 的用户标识符。 为备用 ID 配置时，AD FS，用户可以登录使用的已配置的备用 ID 值，例如电子邮件 id。使用替代 ID 可以采用 SaaS 提供商，而无需修改本地 Upn 的 Office 365 等。 它还可以以支持与使用者设置标识的业务线服务应用程序。

## <a name="alternate-id-in-azure-ad"></a>Azure AD 中的备用 id
组织可能需要在以下方案中使用备用 ID:
1.  在本地的域名称是不可路由，例如。 Contoso.local 和结果是不可路由的默认用户主体名称 (jdoe@contoso.local)。 由于本地应用程序依赖关系或公司策略，无法更改现有的 UPN。 Azure AD 和 Office 365 需要与完全是 internet 可路由的 Azure AD 目录相关联的所有域后缀。 
2.  本地 UPN 与不同用户的电子邮件地址登录到 Office 365，用户使用电子邮件地址和 UPN 不能使用，因为如果组织限制。
在上述情况下，使用 AD FS 备用 ID 使用户能够登录到 Azure AD 而无需修改本地 Upn。 

## <a name="end-user-experience-with-alternate-login-id"></a>使用备用登录 ID 的最终用户体验
最终用户体验与备用登录 id 一起使用的身份验证方法而异。目前那里三种不同的方式在其中可以实现使用备用登录 id。  它们分别是：

- **正则身份验证 （旧版）** -使用基本身份验证协议。
- **新式身份验证**-提供基于 Active Directory 身份验证库 (ADAL） 的单一登录对应用程序。 这使单一登录功能如多重身份验证 (MFA)，基于 SAML 的第三方标识提供程序与 Office 客户端应用程序，智能卡和基于证书的身份验证。
- **混合新式身份验证**-提供所有新式身份验证的优点，并为用户提供访问本地应用程序使用从云中获取的授权令牌的能力。

>[!NOTE]
> 有关可能的最佳体验，Microsoft 强烈建议混合新式身份验证。



## <a name="configure-alternate-logon-id"></a>配置备用登录 ID
使用 Azure AD 连接我们建议使用 Azure AD 连接来配置你的环境的备用登录 ID。

- Azure AD connect 的新配置，请参阅有关如何配置备用 ID 和 AD FS 场的连接到 Azure AD 详细的说明。
- 对于现有的 Azure AD Connect 安装，请参阅更改用户登录方法为将登录方法更改为 AD FS 的说明

如果 Azure AD Connect 提供了有关 AD FS 环境的详细信息，则会自动检查存在的 AD FS 上的正确 KB 和包括 Azure AD 联合身份验证信任的所有必要的正确声明规则的备用 id 配置 AD FS。 没有任何额外的步骤所需外部向导来配置备用 id。

>[!NOTE]
> Microsoft 建议使用 Azure AD Connect 配置备用登录 id。

### <a name="manually-configure-alternate-id"></a>手动配置备用 ID
若要配置备用登录 ID，必须执行以下任务：配置你的 AD FS 声明提供程序信任启用备用登录 ID

1.  如果你有 Server 2012R2，确保您有所有 AD FS 服务器上安装 KB2919355。 可以通过 Windows Update 服务获取，或直接下载。 

2.  通过在任何场中联合身份验证服务器上运行以下 PowerShell cmdlet 更新 AD FS 配置 （如果有 WID 场，您必须运行此命令在您的服务器场中的主 AD FS 服务器上）：

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID**是你想要使用的登录名的属性的 LDAP 名称。

**LookupForests**是林 DNS 属于你的用户的列表。

若要启用备用登录 ID 功能，必须具有非 null，则有效的值配置-AlternateLoginID 和-LookupForests 参数。

在以下示例中，要启用备用登录 ID 的功能，以便使用用户在 contoso.com 和 fabrikam.com 林中的帐户可以登录到 AD FS 启用的应用程序使用其"邮件"属性。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3.  若要禁用此功能，设置为两个参数为 null 的值。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>使用备用 ID 的混合新式身份验证

>[!IMPORTANT]
>以下仅针对测试 AD FS 和未第三方标识提供程序。

### <a name="exchange-and-skype-for-business"></a>Exchange 和 Skype for Business
如果您使用备用登录 id 带 Exchange 和 Skype for Business，用户体验不同而异，具体取决于你使用 HMA。

>[!NOTE]
>为获得最佳的最终用户体验，Microsoft 建议使用混合新式身份验证。

或详细信息，请参阅[混合新式身份验证概述](https://support.office.com/en-us/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Exchange 和 Skype for Business 的必备组件
以下是系统必备组件实现 SSO 使用备用 id。

- 应具有 Exchange Online 启用新式身份验证。
- 对于业务 (SFB) Online Skype 应已启用新式身份验证。
- Exchange 内部部署应已新式身份验证打开。  Exchange 2013 CU19 或 Exchange 2016 CU18 和最多需要所有 Exchange 服务器上。 在环境中没有 Exchange 2010。
- Skype for Business 本地应具有新式身份验证启用。
- 必须使用已启用新式身份验证的 Exchange 和 Skype 客户端。 所有服务器必须都运行 SFB Server 2015 CU5。
- 有关业务的客户端支持新式身份验证的 Skype
   - iOS、 Android、 Windows Phone
   - SFB 2016 （MA 默认情况下，设置为 ON，但请确保未禁用。）
   - SFB 2013 (MA 默认处于禁用状态，因此请确保已开启 MA。)
   - SFB Mac 桌面
- Exchange 客户端支持新式身份验证并支持 AltID regkeys
    - Office Pro Plus 2016 仅





#### <a name="supported-office-version"></a>受支持的 Office 版本

如果未完成这些其他的配置，使用备用 id 使用备用 id 用于 SSO 配置你的目录可能会导致身份验证的额外提示。 请参阅使用备用 id 的用户体验可能会影响的文章。

使用以下其他配置，用户体验已得到明显改进，并可以在你的组织实现接近零会提示你输入的备用 id 用户的身份验证。

##### <a name="step-1-update-to-required-office-version"></a>步骤 1： 对更新所需的 Office 版本
Office 版本 1712年 （内部没有 8827.2148） 和更高版本已更新身份验证逻辑来处理的备用 id 方案。 若要利用新的逻辑，客户端计算机需要更新到 （内部版本没有 8827.2148） 的 Office 版本 1712年及更高版本。

##### <a name="step-2-update-to-required-windows-version"></a>步骤 2： 对更新所需的 Windows 版本
Windows 1709 版和更高版本已更新身份验证逻辑来处理的备用 id 方案。 若要利用新的逻辑，客户端计算机需要更新到 Windows 版本 1709年及更高版本。

##### <a name="step-3-configure-registry-for-impacted-users-using-group-policy"></a>步骤 3： 配置为使用组策略的受影响用户的注册表
Office 应用程序依赖于标识备用 id 环境推送到由目录管理员的信息。 以下注册表项需要进行配置以便使用备用 id 用户而不显示任何额外的提示进行身份验证的 office 应用程序

|若要添加的注册密钥|Regkey 数据名称、 类型和值|Windows 7/8|Windows 10|描述|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER\Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|必需|必需|此注册密钥的值是组织的租户中的已验证自定义域名称。 例如，Contoso 公司可以提供的 Contoso.com 中此注册密钥的值，如果 Contoso.com 租户 Contoso.onmicrosoft.com 中的已验证自定义域名称之一。|
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|所需的专业增强版 Outlook 2016|所需的专业增强版 Outlook 2016|此注册密钥的值可以是 1 / 0 到 Outlook 应用程序指示它是否应该咨询改进的备用 id 身份验证逻辑。|
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|必需|必需|此注册密钥可用于设置作为受信任区域中的 internet 设置的 STS。 标准 ADFS 部署建议将 ADFS 命名空间添加到本地 Intranet 区域的 Internet 资源管理器|

## <a name="new-authentication-flow-after-additional-configuration"></a>其他配置后的新身份验证流

![身份验证流](media/Configure-Alternate-Login-ID/alt1a.png)

1. 答：在使用备用 id 的 Azure AD 中预配用户
   </br>B:目录管理员将需要的 regkey 设置推送到受影响的客户端计算机
2. 用户在本地计算机上进行身份验证，并打开 office 应用程序
3. Office 应用程序采用本地会话凭据
4. Office 应用程序到 Azure AD 使用域提示推送管理员和本地凭据进行身份验证
5. Azure AD 成功验证用户身份通过指示更正联合身份验证领域并颁发的令牌

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>应用程序和用户体验的其他配置后

### <a name="non-exchange-and-skype-for-business-clients"></a>非 Exchange 和 Skype 的业务客户端
|客户端|支持声明|备注|
| ----- | -----|-----|
|Microsoft Teams|支持|<li>Microsoft Teams 支持 AD FS (SAML-P WS 联合身份验证、 WS 信任和 OAuth) 和现代身份验证。</li><li> 如通道、 聊天和文件功能的核心 Microsoft Teams，需使用备用登录 id。</li><li>第 1 个和第三方应用必须由客户单独调查。 这是因为每个应用程序具有其自己的可支持性身份验证协议。</li>|     
|OneDrive for Business|支持的客户端的注册表项推荐 |配置备用 ID 与您看到验证字段中的本地 UPN 是预先填充。 这需要为其更改为正在使用的备用标识。 我们建议使用本文中所述的客户端注册表项：Office 2013 和 Lync 2013 定期提示输入到 SharePoint Online、 OneDrive 和 Lync Online 的凭据。|
|移动客户端 OneDrive for Business|支持|| 
|Office 365 Pro Plus 激活页|支持的客户端的注册表项推荐|配置备用 ID 与您看到验证字段中的本地 UPN 是预先填充。 这需要为其更改为正在使用的备用标识。 我们建议使用本文中所述的客户端的注册表项：Office 2013 和 Lync 2013 定期提示输入到 SharePoint Online、 OneDrive 和 Lync Online 的凭据。|

### <a name="exchange-and-skype-for-business-clients"></a>Exchange 和 Skype 的业务客户端

|客户端|支持声明-与 HMA|支持声明-而无需 HMA|
| ----- |----- | ----- |
|Outlook|受支持、 不需要额外的提示|支持</br></br>与**新式身份验证**对于 Exchange Online:支持</br></br>与**常规身份验证**为 Exchange Online:支持以下注意事项：</br><li>你必须是加入域的计算机上并连接到企业网络 </li><li>只能在对邮箱用户不允许外部访问的环境中使用备用 ID。 这意味着，用户可以仅时进行身份验证到他们的邮箱中受支持的方式，但无论连接和已加入到企业网络，在 VPN 上，通过直接访问计算机连接配置 Outlook 配置文件时获取几个额外提示。| 
|混合公用文件夹|受支持的、 不额外提示。|与**新式身份验证**对于 Exchange Online:支持</br></br>与**常规身份验证**为 Exchange Online:不支持</br></br><li>混合公用文件夹不能进行扩展备用 ID 使用，因此应不能立即与正则身份验证方法。|
|跨内部部署委派|请参阅[配置 Exchange 混合部署中支持委派的邮箱权限](https://technet.microsoft.com/library/mt784505.aspx)|请参阅[配置 Exchange 混合部署中支持委派的邮箱权限](https://technet.microsoft.com/library/mt784505.aspx)|
|存档邮箱访问 （邮箱的本地-在云中的存档）|受支持、 不需要额外的提示|支持-用户获取额外提示输入凭据，访问存档时，他们必须提供其备用 ID 出现提示时。| 
|Outlook Web Access|支持|支持|
|Outlook 移动适用于 Android、 IOS 和 Windows Phone 应用|支持|支持|
|Skype for Business / Lync|受支持，无需额外提示|支持 （注明的除外） 但可能出现的用户混淆。</br></br>移动客户端上备用 Id 才支持 SIP 地址 = 电子邮件地址 = 备用 id。</br></br> 用户可能需要登录两次到 Skype for Business 桌面客户端，首先使用本地 UPN，然后使用的备用 id。 （请注意，"登录地址"与实际的 SIP 地址可能不是"用户名"相同，但通常是）。 首次出现提示时输入用户名，用户应输入 UPN，即使未正确预填充的备用 ID 或 SIP 地址。 以用户单击使用 UPN 名称提示符再次显示的用户，这一次预填充了 UPN 登录。 此时，用户必须将此替换使用备用 ID 并单击登录以完成登录过程。 在移动客户端，用户应输入的本地用户 ID 在高级页，使用 SAM 样式格式 （域 \ 用户名），不是 UPN 格式。</br></br>成功登录后，如果 Skype for Business 或 Lync 显示为"Exchange 需要你的凭据"，需要提供有效的邮箱所在的位置的凭据。 如果邮箱处于云中，你需要提供的备用 id。 如果邮箱在本地需要提供本地 UPN。| 
 
## <a name="additional-details--considerations"></a>更多详细信息和注意事项

-   备用登录 ID 功能是可用于联合的环境与 AD FS 部署。  它不支持以下方案：
    -   非路由的域 (例如 Contoso.local)，不能由 Azure AD 验证。
    -   托管的环境不具有部署的 AD FS。


-   启用时，备用登录 ID 功能功能仅适用于用户名/密码身份验证在所有用户/密码身份验证协议受 AD FS (SAML-P WS 联合身份验证、 WS 信任和 OAuth)。


-   Windows 集成身份验证 (WIA) 执行操作时 （例如，当用户尝试从 intranet 访问已加入域的计算机上的企业应用程序，并且 AD FS 管理员已配置为 intranet 使用 WIA 的身份验证策略），UPN 寻址表示身份验证。 如果配置任何声明规则了信赖方为备用登录 ID 功能，则应确保这些规则是在 WIA 的情况下仍然有效。

-   启用时，备用登录 ID 功能要求至少一个全局编录服务器都可以从 AD FS 支持每个用户帐户林 AD FS 服务器访问。 无法到达中用户帐户林的全局编录服务器会导致 AD FS 回退为使用 UPN。 默认情况下的所有域控制器都是全局编录服务器。

-   启用时，如果 AD FS 服务器具有相同的备用登录 ID 值以指定在所有配置的用户帐户林中找到多个用户对象，它将失败的登录名。

-   启用备用登录 ID 功能后，AD FS 尝试备用登录 ID 的最终用户先进行身份验证并会回退为使用 UPN，如果它找不到可由备用登录 ID 标识的帐户 应确保如果你想要的 UPN 登录名仍支持，则备用登录 ID 和 UPN 之间没有冲突。 例如，设置一个值的 mail 属性与其他的 UPN 阻止其他用户使用其 UPN 登录。

-   如果由管理员配置的林中的一个已关闭，AD FS 将继续查找中配置其他林的备用登录 id 的用户帐户。 如果 AD FS 服务器跨林具有搜索找到一个唯一的用户对象，用户登录成功。

-   另外要自定义 AD FS 登录页后，可以为最终用户提供一些提示有关备用登录 id。 可以通过添加自定义登录页说明操作即可 (有关详细信息，请参阅[自定义 AD FS 登录页](https://technet.microsoft.com/library/dn280950.aspx)或自定义用户名字段的上面"使用组织帐户登录"字符串 （的详细信息信息，请参阅[Advanced Customization of AD FS 登录页](https://technet.microsoft.com/library/dn636121.aspx)。

-   包含的备用登录 ID 值的新声明类型是**http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>事件和性能计数器
已添加了以下性能计数器来测量性能的 AD FS 服务器时启用备用登录 ID:

-   备用登录 Id 身份验证： 通过使用备用登录 ID 来执行的身份验证次数

-   备用登录 Id 的身份验证次数/秒： 身份验证使用备用登录 ID，每秒执行的次数

-   备用登录 id 的平均搜索延迟： 跨林的管理员已配置备用登录 id 的平均搜索延迟

以下是各种错误情况和对用户的登录体验与 AD FS 所记录的事件的相应影响：



**错误案例**|**对单一登录体验的影响**|**事件**|
---------|---------|---------
无法获取用户对象的 SAMAccountName 的值|登录失败|事件 ID 364 与异常消息 MSIS8012:找不到用户的 samAccountName:{0}。|
CanonicalName 属性不可访问|登录失败|事件 ID 364 与异常消息 MSIS8013:CanonicalName: '{0}的用户:{1}的格式不正确。|
在一个林中找到多个用户对象|登录失败|事件 ID 364 与异常消息 MSIS8015:找到具有标识的多个用户帐户{0}在林中{1}与标识： {2}|
跨多个林发现多个用户对象|登录失败|事件 ID 364 与异常消息 MSIS8014:找到具有标识的多个用户帐户{0}林中： {1}|

## <a name="see-also"></a>请参阅
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)



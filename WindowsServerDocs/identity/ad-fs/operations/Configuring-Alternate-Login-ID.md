---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: "配置替代登录 ID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb4c98ff1090a9d3c35654614a43e12db99d691d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-alternate-login-id"></a>配置替代登录 ID

>适用于：Windows Server 2016，Windows Server 2012 R2

用户可以使用任何形式的用户标识符接受的 Active Directory 域服务 (广告 DS) 的 Active Directory 联合身份验证服务 (广告 FS) 启用应用程序进行登录。 其中包括 (Upn) 的用户主要名称 (johndoe@contoso.com) 或域限定三千帐户名称（contoso\johndoe 或 contoso.com\johndoe）。

在某些环境，由于企业策略或本地业务线应用的依赖关系，最终用户只可能注意到他们的电子邮件地址不及其 UPN 或三千帐户名称。 在某些情况下，UPN 也是非路由 (jdoe@contoso.local) 和仅用于验证到公司的网络上的应用程序。

自从非路由域的 （例如。 无法验证 Contoso.local) 所有权、 Office 365 需要所有用户登录 Id，以将完全路由 internet。 如果本地 UPN 使用非路由域 （例如。 Contoso.local)，或现有 UPN 不能更改由于本地应用的依赖关系，我们建议设置替代登录 id。 备用登录 ID 允许你在体验配置符号用户可以登录之外其 UPN，如邮件特性。

此功能的优势是它使你可以采用 SaaS 提供商，如 Office 365，而不会修改本地 Upn。 它还可以支持服务的业务线应用程序的消费者-预配身份。

> [!IMPORTANT]
> 用备用 ID 混合环境 Exchange 和/或 Skype for Business 支持，但不是建议这样做。 有关本地中和网上使用凭据 (如 UPN) 的同一组提供最佳混合环境中的用户体验。  Microsoft 建议客户更改其 Upn 如果可能，以避免备用 id。 Lync 或 Skype ID 备用使用适用于企业需要 Lync Server 2013 或更高版本。 使用备用 ID 的客户都应该考虑启用[现代身份验证](https://support.office.com/article/using-office-365-modern-authentication-with-office-clients-776c0036-66fd-41cb-8928-5495c0f9168a)改进的用户体验的 Office 365 中的更换费用。 此外，使用 Skype for Business 与移动版的客户端的客户必须确保 SIP 地址与用户的邮件地址 （和备用 ID） 相同。 

请参阅下表中的用户体验备用 id 为使用不同的 Office 365 客户端与常规身份验证、 现代身份验证和证书基于身份验证 （需要启用现代身份验证）。

|**客户端类型**|**其他信息**|**支持声明的常规和现代身份验证**|**描述**|
|--------------------|------------------------------|------------------------------|------------------------------|
|Outlook|域加入的计算机上必须常规身份验证： 并已连接到公司的网络<br /><br />现代身份验证： 支持|只能在不允许邮箱用户外部访问的环境中使用备用 ID。 这意味着，用户可以仅身份验证在该处邮箱到受支持的方式在连接和连接到 VPN，公司网络，或通过直接访问进行连接时。 如果您选择用来配置现代身份验证 （称为 ADAL），可以使用非域加入连接的计算机上的 Outlook 但配置你的 Outlook 个人资料时，你将收到了几个额外的提示。<br /><br />请参阅下表的用户体验演示的第一个映像。|[Office 2013 中的现代身份验证](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|混合公共文件夹|不支持常规身份验证：<br /><br />现代身份验证： 支持|混合公共文件夹不能以展开如果习惯备用 ID，并因此应不能用于今天常规身份验证方法。 如果你想要将无法使用公共文件夹中混合你将需要配置现代身份验证 （称为 ADAL）。<br /><br />请参阅下表的用户体验演示的第一个映像。|[Office 2013 中的现代身份验证](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|交叉本地委派|不受支持|当前交叉的本地权限均不支持在混合配置中，但它们也会如果不起作用使用 AltID。||
|存档邮箱访问 （邮箱本地-在云中存档）|受支持|访问存档时，用户将获取额外提示输入凭据，它们将需要提供那里备用在出现提示时 ID。<br /><br />请参阅下表的用户体验演示的第一个映像。||
|Office 365 Pro 以及激活页面|受支持的客户端注册表项建议|备用 id 配置中，你将看到本地 UPN 了较密集的预验证字段。 这需要更改为用于备用身份。 我们建议使用客户端注册表项链接列所示。<br /><br />请参阅下表的用户体验演示第二个图像。|[Office 2013 和 Lync 2013 定期提示输入凭据到 SharePoint Online、 OneDrive 和 Lync 在线](https://support.microsoft.com/en-us/kb/2913639)|
|Microsoft 团队|受支持|Microsoft 团队支持广告 FS (SAML P、WS-进 WS 信任和 OAuth) 和现代身份验证。<br/><br/> 如频道、聊天和文件的功能的核心 Microsoft 团队适用于 Altnernate 登录 id。 </br></br>第一个和第三方应用需要单独的客户调查。 这是因为每个应用具有其自己性身份验证的协议。| 
|Skype for Business / Lync|受支持 （除外所述） 但用户混乱潜在。  在移动版的客户端上备用支持 Id 仅当 SIP 地址 = 电子邮件地址 = 备用 id。|用户可能需要进行登录到 Skype 两次的业务桌面客户端，首先使用本地 UPN，然后备用 id。 （注意"签入地址"是实际上的可能不会"用户名"相同，但通常是 SIP 地址）。  在首次系统 User name 提示，用户应该输入 UPN，，即使它预不正确填充的备用 ID 或 SIP 地址。 之后用户登录的 UPN 将再出现提示，用户，这一次填入 UPN 可单击。 用户必须这替换备用 ID，并单击这一次登录来完成注册过程。 在移动版的客户端，用户应该输入的本地用户 ID 在高级页，使用三千样式格式域 （\ 用户名），不 UPN 格式。<br /><br />后成功登录，如果 Skype 企业或 Lync 显示"Exchange 需要你的凭据"，你需要提供适用于邮箱所在位置的凭据。 你在云中邮箱是否需要提供备用 id。 如果邮箱本地您需要提供本地 UPN。|[Office 2013 中的现代身份验证](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Outlook 访问 Web|受支持|||
|适用于 Android、 IOS 和 Windows Phone 的 outlook 移动应用|受支持|||
|OneDrive for Business|受支持的客户端注册表项建议|备用 id 配置中，你将看到本地 UPN 了较密集的预验证字段。 这需要更改为用于备用身份。 我们建议使用客户端注册表项链接列所示。<br /><br />请参阅下表的用户体验演示第二个图像。|[Office 2013 和 Lync 2013 定期提示输入凭据到 SharePoint Online、 OneDrive 和 Lync 在线](https://support.microsoft.com/en-us/kb/2913639)|
|OneDrive for Business Mobile 客户端|受支持|||

![替代登录](media/Configure-Alternate-Login-ID/ADFS_Alt_ID1.png)

![替代登录](media/Configure-Alternate-Login-ID/ADFS_Alt_ID2.png)

![替代登录](media/Configure-Alternate-Login-ID/ADFS_Alt_ID3.png)

下面的屏幕截图是使用 Skype for Business 的其他示例。  在示例使用以下信息


- SIP:userA@contoso.com 
- UPN:userA@contoso.local
- 电子邮件：userA@contoso.com
- AltId:userA@contoso.com 

在登录字段中输入 SIP 地址。

![Skype](media/Configure-Alternate-Login-ID/SkypeA.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeB.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeC.png)

## <a name="to-configure-alternate-login-id"></a>若要配置替代登录 ID
为了配置替代登录 ID，必须执行以下任务：

将配置你的广告 FS 索赔提供商信任启用替代登录 ID

1.  安装[KB2919355](https://go.microsoft.com/fwlink/?LinkID=396590)。  你可以通过 Windows 更新服务获取它或将其直接下载。

2.  通过运行以下 PowerShell cmdlet 联合服务器你电场的日落中的任何更新的广告 FS 配置 （如果你有 WID 电场的日落，你必须运行此命令场中的广告 FS 主服务器上）：

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>

    ```

    **AlternateLoginID**是你想要用于登录的特性 LDAP 名称。

    **LookupForests**森林 DNS 属于你的用户的列表。

    若要启用替代登录 ID 功能，你必须将-AlternateLoginID 和-LookupForests 参数配置非空的有效的值。

    在以下示例中，您要启用替代登录 ID 功能，以便在 contoso.com 和 fabrikam.com 林帐户与您的用户可以登录到他们的"邮件"特性与广告 FS 启用应用程序。

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
    ```

3.  若要禁用此功能，设置的值为两个参数为 null。

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
    ```

4.  若要启用替代登录 ID，Azure AD 时，使用 Azure AD 连接时，则需要没有其他配置步骤。   直接从该向导，可配置备用 ID。  请参阅唯一标识您的部分下的用户[连接到 Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/#connect-to-azure-ad)。

## <a name="additional-details--considerations"></a>更多详细信息和注意事项

-   使用部署广告 FS 替代登录 ID 功能才可用的联合环境。  它不支持在以下情况中：
    -   非路由域 (如 Contoso.local) 无法通过 Azure AD 验证。
    -   没有广告 FS 部署的环境进行管理。


-   启用后替代登录 ID 功能功能仅适用于用户名/密码验证跨所有用户/密码身份验证协议受广告 FS (SAML P、 WS-进 WS 信任和 OAuth)。


-   执行 Windows 集成身份验证 (WIA) 时 （例如，当用户尝试访问的公司的应用在已加入域的计算机上的 intranet 和广告 FS 管理员的身份验证策略 WIA 用于 intranet 配置），则 UPN 将用于身份验证。 如果你有任何索赔规则配置信赖方替代登录 ID 功能，你应确保这些规则 WIA 以防万一仍然有效。

-   启用时，替代登录 ID 功能需要至少一个全球目录服务器可到达从广告 FS 服务器每个用户帐户林 FS 广告支持。 广告 FS 回退使用 UPN 会失败联系用户帐户森林中的全局目录服务器。 默认情况下的所有域控制器将都是全球目录服务器。

-   当启用，如果广告 FS 服务器发现相同替代登录 ID 值跨所有配置的用户帐户林指定多个用户对象，它将无法登录。

-   广告 FS 启用替代登录 ID 功能后，将尝试首先通过替代登录 id 为最终用户身份验证，然后再回退使用 UPN 找不到备用登录 ID 通过可以识别的帐户 你应确保有没有替代登录 ID 和 UPN 之间的冲突如果你想要仍支持 UPN 登录。 例如，将设置一个的邮件特性与其他人 UPN 将阻止其他用户登录他 UPN。

-   如果一个由管理员配置林已关闭，将继续广告 FS 查找配置其他林具有替代登录的用户帐户。 如果广告 FS 服务器找到一个唯一的用户对象跨它具有搜索林，用户将成功登录。

-   此外希望自定义广告 FS 登录页面，以向最终用户提供一些提示有关替代登录 id。 可以通过添加自定义登录页面说明进行操作 (有关详细信息，请参阅[自定义的广告 FS 登录页面](https://technet.microsoft.com/library/dn280950.aspx)或自定义"使用组织帐户登录"用户名字段上面的字符串 (有关详细信息，请参阅[高级的自定义的广告 FS 登录页面](https://technet.microsoft.com/library/dn636121.aspx)。

-   包含备用登录 ID 值的索赔新的类型是**http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>事件和性能计数器
添加了以下性能计数器以启用替代登录 ID 测量广告 FS 服务器的性能：

-   通过使用替代登录 ID 执行的身份验证的备用登录 Id 身份验证： 数

-   备用登录 Id 身份验证/秒： 身份验证使用每秒替代登录 ID 执行数

-   平均搜索延迟备用登录 id： 跨管理员已经配置了替代登录 ID 林平均搜索延迟

以下是各种的错误情况和对用户登录体验由广告 FS 记录的事件，相应的影响：



**错误情况**|**在登录且体验的影响**|**事件**|
---------|---------|---------
无法获取 SAMAccountName 的值为用户的对象|登录失败|事件 ID 364 与异常消息 MSIS8012： 找不到用户 samAccountName: ' {0} '。|
不能访问 CanonicalName 属性|登录失败|事件 ID 364 与异常消息 MSIS8013: CanonicalName: '{0}' 的用户:' {1} ' 处于格式不正确。|
一个森林中找到的多个用户对象|登录失败|事件 ID 364 与异常消息 MSIS8015： 位于森林 '{1}' 身份使用多个用户帐户的身份 ' {0} ': {2}|
多个用户对象找到跨多个林|登录失败|事件 ID 364 与异常消息 MSIS8014： 在森林中找到多个用户帐户的身份 ' {0} ': {1}|

## <a name="see-also"></a>请参阅
[广告 FS 操作](../../ad-fs/AD-FS-2016-Operations.md)



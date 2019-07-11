---
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: Active Directory 联合身份验证服务 (AD FS) 的所需的更新
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 3/29/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2336847825cfb3f232674a1e39d3bab7953a32c0
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792299"
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>有关 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理 (WAP) 所需的更新


截至 2016 年 10 月 Windows Server 的所有组件的所有更新都发布仅通过 Windows Update (WU)。  有没有更多修补程序或单独下载。
这适用于 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 SP1。

此页列出了 AD FS 和 WAP，以及建议用于 AD FS 和 WAP 的修补程序更新的历史列表特别感兴趣的汇总包。

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>为 AD FS 和 WAP Windows Server 2016 中的更新
Windows Server 2016 的更新通过 Windows Update 每月会传递，并且具有累积性。 下面列出的更新包建议用于 AD FS 和 WAP 2016 的所有服务器，并包括所有以前所需的更新，以及最新修补程序。

|KB # |描述|发布日期
|----- | ----- |-----
|[CVE-2019-1126](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1126) | 此安全更新解决了 Active Directory 联合身份验证服务 (AD FS) 这可能允许攻击者绕过 extranet 锁定策略中的漏洞。 |2019 年 7 月|
|[4489889 （OS 内部版本 14393.2879）](https://support.microsoft.com/help/4489889/windows-10-update-kb4489889) | 解决了在 Active Directory 联合身份验证服务 (AD FS) 会导致重复信赖方信任 AD FS 管理控制台中显示的问题。 发生这种情况时创建或查看信赖方信任使用的 AD FS 管理控制台。 |2019 年 3 月|
|[4487006 （OS 内部版本 14393.2828）](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) | 解决了导致更新失败时使用 PowerShell 或 Active Directory 联合身份验证服务 (AD FS) 管理控制台的信赖方信任的问题。 如果配置为使用发布多个 PassiveRequestorEndpoint 联机元数据 URL 的信赖方信任，会发生此问题。 错误是，"MSIS7615:信赖方信任中指定的受信任终结点必须是唯一的该信赖方信任。"  </br></br>解决了因 Azure 密码保护策略而显示的特定错误消息的复杂性外部密码更改的问题。 |2019 年 2 月|
|[4462928 （OS 内部版本 14393.2580）](https://support.microsoft.com/help/4462928/windows-10-update-kb4462928)|解决了 Active Directory 联合身份验证服务 (ADFS) Extranet 智能锁定 (ESL) 和备用登录 ID 之间的互操作问题 启用备用登录 ID 后，调用到 AD FS Powershell cmdlet、 Get AdfsAccountActivity 和重置 AdfsAccountLockout，返回"找不到帐户"错误。 当调用集 AdfsAccountActivity 时，而不是编辑一个现有添加新条目。|2018 年 10 月|
|[4343884 （OS 内部版本 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)|解决了 Active Directory 联合身份验证服务 (AD FS) 的问题： 多重身份验证不会无法正常工作与使用自定义区域性定义的移动设备。 </br></br>解决了在 Windows hello 企业版的新用户注册会导致较长的延迟 （15 秒） 的问题。 硬件安全模块使用 ADFS 注册机构 (RA) 证书存储时，会发生此问题。|2018 年 8 月|
|[4338822 （OS 内部版本 14393.2395）](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822)|解决了在显示时创建或查看从控制台的信赖方信任的 AD FS 管理控制台中的重复信赖方信任的 AD FS 中的问题。</br></br>解决了导致 Windows hello 企业版失败的 ADFS 中的问题。 有两个声明提供程序时，将发生此问题。 PIN 注册将失败，并"400 内部服务器错误：无法获取设备标识符。"</br></br> 解决了与永远不会结束的非活动连接相关的 WAP 问题。 这会导致系统资源泄漏 （例如，内存泄漏） 和它不再响应的 WAP 服务。 解决了 AD FS 的问题，以防止用户选择不同的登录选项。 当用户选择使用基于证书身份验证进行登录，但尚未配置时，将发生这种情况。 如果用户选择基于证书的身份验证，然后尝试选择另一个登录名选项也会发生这种情况。 如果发生这种情况，用户将重定向到基于证书的身份验证页直至将其关闭浏览器。|2018 年 7 月|
|[4103720 （OS 内部版本 14393.2273）](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)|解决了使用 ADFS 对 SAML 信赖方时启用 PreventTokenReplays 失败将导致 IdP 发起的登录名的问题。 </br></br>解决了从设备或浏览器应用程序的 OAUTH 进行身份验证时，会发生了 ADFS 的问题。 更改用户密码生成失败，并要求用户退出应用程序或浏览器进行登录。 </br></br>解决了的问题，启用 Extranet 智能锁定 utc + 1 和更高版本 （欧洲和亚洲） 不起作用。 此外，它会导致正常的 Extranet 锁定失败并出现以下错误：Get-AdfsAccountActivity:大于 DateTime.MaxValue 或小于 DateTime.MinValue 时转换为 UTC 日期时间值不能序列化为 JSON。</br></br>解决了 ADFS Windows hello 企业中的新用户不能预配其 PIN 的业务问题。 配置无 MFA 提供程序时，将发生这种情况。|2018 年 5 月|
|[4093120 （OS 内部版本 14393.2214）](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120)| 解决了未经处理的刷新令牌验证问题。 它会生成以下错误：“Microsoft.IdentityServer.Web.Protocols.OAuth.Exceptions.OAuthInvalidRefreshTokenException:MSIS9312:接收到无效的 OAuth 刷新令牌。 刷新令牌已收到早于令牌中允许的时间。" |2018 年 4 月|
|[4077525 （OS 内部版本 14393.2097）](https://support.microsoft.com/help/4077525/windows-10-update-kb4077525)|解决了 HTTP 500 错误发生时的 ADFS 场有使用 Windows 内部数据库 (WID) 在至少两台服务器的问题。 在此方案中，HTTP 基本预身份验证 Web 应用程序代理 (WAP) 服务器上的无法对某些用户进行身份验证。 发生错误时，可能还会看到警告 WAP 事件日志中的事件 ID 13039 Microsoft Windows Web 应用程序代理。 说明读取，"Web 应用程序代理无法对用户进行身份验证。 预身份验证是 ADFS 的富客户端。 给定的用户无权访问给定信赖方。 目标信赖方或 WAP 信赖方的授权规则所需进行修改。"</br></br>解决 AD FS 可以不再忽略提示的问题在身份验证期间 = 登录名。 已禁用选项被为了支持在密码中不使用身份验证的方案。 有关详细信息，请参阅 AD FS 在 Windows Server 2016 RTM 中的身份验证过程中忽略"prompt = 登录名"参数。</br></br>在 AD FS 中解决问题，其中授权客户 （以及信赖方） 人员选择证书身份验证选项将无法进行连接。 使用提示符时，出现故障 = 登录名，如果已启用 Windows 集成身份验证 (WIA)，并请求怎样 WIA。</br></br>解决 AD FS 错误地显示在那里主页领域发现 (HRD) 页面与 OAuth 组中的信赖方 (RP) 相关联的标识提供者 (IDP) 时的问题。 除非多个 Idp 都与 RP OAuth 组中，用户将不会显示 HRD 页。 相反，用户将直接转到相关联的 IDP 进行身份验证。|2018 年 2 月|
|[4041688 （OS 内部版本 14393.1794）](https://support.microsoft.com/kb/4041688)|这一修补解决了间歇性 misdirects AD 颁发机构请求到错误的标识提供程序由于不正确的缓存行为的问题。 这可以影响身份验证功能，如多重身份验证。 </br></br>添加了 AAD 连接运行状况到 ADFS 服务器运行状况报表，以正确保真度 （使用详细审核） 上混合 WS2012R2 和 WS2016 ADFS 场。</br></br>其中 2012年的升级过程 R2 ADFS 场到 ADFS 2016 解决了问题，用于将场行为级别提升的 powershell cmdlet 失败具有超时值，当有多个信赖方信任时。</br></br>解决 AD FS 通过联合到其他安全令牌服务器 (STS) 的请求时修改 wct 参数值导致身份验证失败的其中一个问题。|2017 年 10 月|
|[4038801 （OS 内部版本 14393.1737）](https://support.microsoft.com/kb/4038801)|添加了对使用联合的 LDPs OIDC 注销的支持。 这将允许"网亭方案"其中多个用户可能会按顺序记录到一个设备没有使用 LDP 联合身份验证。</br></br>修复的问题 CEP/CES 基于证书的位置不使用 gMSA 帐户 WinHello。</br></br>Windows 内部数据库 (WID) 在 Windows Server 2016 ADFS 服务器上无法同步某些设置，如 IdentityServerPolicy.Scopes 和 IdentityServerPolicy.Clients 表中的 ApplicationGroupId 列可以解决问题） 由于外键约束。 此类的同步故障可以导致不同的声明，声明提供程序和应用程序的体验，到辅助主 ADFS 服务器之间。 此外，如果 WID 主角色移到辅助节点中，应用程序组将不再可管理在 ADFS 管理的用户体验。</br></br>此更新修复了问题： 多重身份验证不会无法正常工作与使用自定义区域性定义的移动设备|2017 年 9 月|
|[4034661 （OS 内部版本 14393.1613）](https://support.microsoft.com/kb/4034661)|其中，调用方 IP 地址是由 411 事件 ADFS 4.0 在安全事件日志中记录的 nog 可以解决问题 \ 即使在启用"成功审核"和"失败审核"后的 Windows Server 2016 RS1 ADFS 服务器。</br></br>ADFX 服务器配置为使用 HTTP 代理时，这一修补解决了问题与 Azure 多重身份验证 (MFA)。</br></br>"解决其中呈现到 ADFS 代理服务器的已过期或已吊销证书不返回错误给用户的问题。"|2017 年 8 月|
|[4034658 （OS 内部版本 14393.1593）](https://support.microsoft.com/kb/4034658)|修复 2016 ad FS 服务器以便在本地部署上支持 Windows Hello For Business 的 MFA 证书注册|2017 年 8 月|
|[4025334 （OS 内部版本 14393.1532）](https://support.microsoft.com/kb/4025334)|解决了其中 PkeyAuth 令牌处理程序可能失败身份验证，如果 pkeyauth 请求包含不正确的数据的问题。 身份验证应仍继续而不执行设备身份验证|2017 年 7 月|
|[4022723 （OS 内部版本 14393.1378）](https://support.microsoft.com/kb/4022723)|[Web 应用程序代理]DisableHttpOnlyCookieProtection 配置属性的值将不提取 WAP 2016 2012R2/2016年混合部署中 </br></br>[Web 应用程序代理]无法从 EAS 预身份验证方案中的 AD FS 获取用户访问令牌。</br></br>AD FS 2016:异常会导致注销 WSFED|2017 年 6 月
|[3213986](https://support.microsoft.com/kb/3213986)|对于基于 x64 的系统 (KB3213986) 适用于 Windows Server 2016 累积更新| 2017 年 1 月

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>为 AD FS 和 WAP Windows Server 2012 R2 中的更新
下面是 Windows Server 2012 R2 中 Active Directory 联合身份验证服务 (AD FS) 中发布的修补程序和更新汇总的列表。

|KB # |描述|发布日期
|----- | ----- |-----
|[4507448](https://support.microsoft.com/help/4507448/windows-8-1-update-kb4507448)| 此安全更新解决了 Active Directory 联合身份验证服务 (AD FS) 这可能允许攻击者绕过 extranet 锁定策略中的漏洞。 |2019 年 7 月
|[4041685](https://support.microsoft.com/kb/4041685)|解决 AD FS 问题其中 MSISConext 请求标头中的 cookie 可以最终溢出的标头大小限制并导致未能通过身份验证与 HTTP 状态代码 400"错误请求-标头太长"。</br></br>解决了问题，ADFS 无法再忽视"prompt = 登录名"身份验证过程。 添加了"已禁用"选项，以还原的方案使用非密码身份验证的位置。|2017 年 10 月更新汇总预览
|[4019217](https://support.microsoft.com/kb/4019217)|工作的文件夹使用 Server 2012 R2 AD FS 服务器将使用令牌代理的客户端不起作用|2017 年 5 月预览版的更新汇总
|[4015550](https://support.microsoft.com/kb/4015550)|解决了与 AD FS 不进行身份验证的外部用户和 AD FS WAP 随机失败的请求转发问题|2017 年 4 月更新汇总
|[4015547](https://support.microsoft.com/kb/4015547)|解决了与 AD FS 不进行身份验证的外部用户和 AD FS WAP 随机失败的请求转发问题|2017 年 4 月安全更新
|[4012216](https://support.microsoft.com/kb/4009970)|MS17 019 此安全更新解决的漏洞在 Active Directory 联合身份验证服务 (ADFS)。 如果攻击者将特别是特制的请求发送到 AD FS 服务器，从而允许攻击者能够读取有关目标系统的机密信息的漏洞可能导致信息泄露。|2017 年 3 月更新汇总
|[3179574](https://support.microsoft.com/kb/3179574)|与 AD FS extranet 密码更新中修复的问题。 |2016 年 8 月更新汇总
|[3172614](https://support.microsoft.com/kb/3172614)|引入了的提示符 = 登录名[支持](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-faq#BKMK_7)，使用 AD FS 管理控制台和 AlwaysRequireAuthentication 设置修复的问题。 |2016 年 7 月更新汇总
|[3163306](https://support.microsoft.com/kb/3163306)|Active Directory 联合身份验证服务 (AD FS) 3.0 无法连接到配置为使用安全套接字层 (SSL) 端口 636 或 3269 连接字符串中的轻型目录访问协议 (LDAP) 属性存储。 |2016 年 6 月更新汇总
|[3148533](https://support.microsoft.com/kb/3148533)|通过 Windows Server 2012 R2 中的 ADFS 代理的 MFA 回退身份验证失败 |2016 年 5 月
|[3134787](https://support.microsoft.com/kb/3134787)|AD FS 日志不包含 Windows Server 2012 R2 中的帐户锁定方案的客户端 IP 地址 |2016 年 2 月
|[3134222](https://support.microsoft.com/kb/3134222)|MS16-020:Active Directory 联合身份验证服务地址的拒绝服务对安全更新：2016 年 2 月 9日日|2016 年 2 月
|[3105881](https://support.microsoft.com/kb/3105881)|在基于 Windows Server 2012 R2 的 AD FS 服务器中启用设备身份验证的情况下无法访问应用程序|2015 年 10 月
|[3092003](https://support.microsoft.com/kb/3092003)|重复加载页面和身份验证失败时用户在 Windows Server 2012 R2 AD FS 中使用 MFA|2015 年 8 月
|[3080778](https://support.microsoft.com/kb/3080778)|Windows Server 2012 R2 中的 MFA 适配器引发异常时，AD FS 不会调用 OnError|2015 年 7 月
|[3075610](https://support.microsoft.com/kb/3075610)|信任关系会丢失辅助 AD FS 服务器上后添加或删除 Windows Server 2012 R2 中的声明提供程序|2015 年 7 月
|[3070080](https://support.microsoft.com/kb/3070080)|无法为非声明感知信赖方信任正常工作的主领域发现|2015 年 6 月
|[3052122](https://support.microsoft.com/kb/3052122)|更新在 Windows Server 2012 R2 中的 AD FS 令牌中添加支持复合 ID 声明|2015 年 5 月
|[3045711](https://support.microsoft.com/kb/3045711)|MS15-040:Active Directory 联合身份验证服务中的漏洞可能导致信息泄露|2015 年 4 月
|[3042127](https://support.microsoft.com/kb/3042127)|"HTTP 400-错误请求"错误在 Windows Server 2012 R2 中打开通过 WAP 共享的邮箱时|2015 年 3 月
|[3042121](https://support.microsoft.com/kb/3042121)|对 Web 应用程序代理身份验证令牌，在 Windows Server 2012 R2 的 AD FS 令牌重放保护|2015 年 3 月
|[3035025](https://support.microsoft.com/kb/3035025)|更新密码的修补程序功能，以便用户不需要在 Windows Server 2012 R2 中使用已注册的设备|2015 年 1 月
|[3033917](https://support.microsoft.com/kb/3033917)|AD FS 无法处理 Windows Server 2012 R2 中的 SAML 响应|2015 年 1 月
|[3025080](https://support.microsoft.com/kb/3025080)|操作失败时尝试将 Web 应用程序代理通过将 Office 文件保存在 Windows Server 2012 R2|2015 年 1 月
|[3025078](https://support.microsoft.com/kb/3025078)|您不会提示输入用户名再次时使用的不正确的用户名登录到 Windows Server 2012 R2|2015 年 1 月
|[3020813](https://support.microsoft.com/kb/3020813)|Windows Server 2012 R2 AD FS 中运行的 web 应用程序时，会提示进行身份验证|2015 年 1 月
|[3020773](https://support.microsoft.com/kb/3020773)|Windows Server 2012 R2 中的设备注册服务的初始部署后的超时故障|2015 年 1 月
|[3018886](https://support.microsoft.com/kb/3018886)|提示您输入用户名和密码两次时从 intranet 访问 Windows Server 2012 R2 AD FS 服务器|2015 年 1 月
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 更新汇总|2014 年 12 月
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 更新汇总|2014 年 11 月
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 更新汇总|2014 年 8 月
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 更新汇总|2014 年 7 月
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 更新汇总|2014 年 6 月
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 更新汇总|2014 年 5 月
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 更新汇总|2014 年 4 月

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>更新适用于 AD FS 中 Windows Server 2012 (AD FS 2.1) 和 AD FS 2.0
下面是已发布适用于 AD FS 2.0 和 2.1 的修补程序和更新汇总的列表。

|KB # |描述|发布日期|适用于：
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|在 Windows Server 2012 （这是常规发布的修补程序 3094446） 通过代理身份验证失败|2016 年 11 月质量汇总更新|AD FS 2.1
|[3197869](https://support.microsoft.com/kb/3197869)|在 Windows Server 2008 R2 SP1 （这是常规发布的修补程序 3094446） 通过代理身份验证失败|2016 年 11 月质量汇总更新|AD FS 2.0
|[3094446](https://support.microsoft.com/kb/3094446)|在 Windows Server 2012 或 Windows Server 2008 R2 SP1 通过代理身份验证失败|2015 年 9 月|AD FS 2.0 和 2.1
|[3070078](https://support.microsoft.com/kb/3070078)|AD FS 2.1 将引发异常时对 Windows Server 2012 中的加密证书进行身份验证|2015 年 7 月|AD FS 2.1
|[3062577](https://support.microsoft.com/kb/3062577)|MS15-062 所述：Active Directory 联合身份验证服务中的漏洞可能允许特权提升|2015 年 6 月|AD FS 2.0 / 2.1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077:Active Directory 联合身份验证服务中的漏洞可能导致信息泄露：2015 年 4 月 14日日|2014 年 11 月|AD FS 2.0 / 2.1
|[2987843](https://support.microsoft.com/kb/2987843)|许多用户登录 Windows Server 2012 中的 web 应用程序时，持续增加的 AD FS 联合身份验证服务器的内存使用情况|2014 年 7 月|AD FS 2.1
|[2957619](https://support.microsoft.com/kb/2957619)|在 AD FS 的信赖方信任已停止时请求委派令牌的 AD FS|2014 年 5 月|AD FS 2.1
|[2926658](https://support.microsoft.com/kb/2926658)|如果您没有 SQL 权限 ADFS SQL 场部署将失败|2014 年 10 月|AD FS 2.1
|[2896713](https://support.microsoft.com/kb/2896713)或[2989956](https://support.microsoft.com/kb/2989956)|有可用更新来解决的 AD FS 服务器上安装安全更新 2843638 后的几个问题|2013 年 11 月</br></br>2014 年 9 月|AD FS 2.0 / 2.1
|[2877424](https://support.microsoft.com/kb/2877424)|更新，您可以使用一个证书，为多个信赖方信任 AD FS 中 2.1 场|2013 年 10 月|AD FS 2.1
|[2873168](https://support.microsoft.com/kb/2873168)|解决方法：使用第三方的 CSP 和 HSM，然后为 Windows Server 2008 R2 Service Pack 1 上的 AD FS 2.0 配置更新汇总 3 中的声明提供程序信任时出错|2013 年 9 月|AD FS 2.0
|[2861090](https://support.microsoft.com/kb/2861090)|在加密证书的使用者名称中的逗号会导致 Windows Server 2008 R2 SP1 中的异常|2013 年 8 月|AD FS 2.0
|[2843639](https://support.microsoft.com/kb/2843639)|[Security]Active Directory 联合身份验证服务中的漏洞可能导致信息泄露|2013 年 11 月|AD FS 2.1
|[2843638](https://support.microsoft.com/kb/2843638)|MS13 066:Active Directory 联合身份验证服务 2.0 的安全更新的说明：2013 年 8 月 13日日|2013 年 8 月|AD FS 2.0
|[2827748](https://support.microsoft.com/kb/2827748)|Federationmetadata.xml 文件不包含 Windows Server 2012 中的 WS 信任和 WS 联合身份验证终结点的 MEX 终结点信息|2013 年 5 月|AD FS 2.1
|[2790338](https://support.microsoft.com/kb/2790338)|更新汇总 3 说明 Active Directory 联合身份验证服务 (AD FS) 2.0|2013 年 3 月|AD FS 2.0

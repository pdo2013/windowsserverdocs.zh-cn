---
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: "所需的更新 Active Directory 联合身份验证服务 (广告 FS)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 434bf65c9f5ec977318b5efcdeebd19a5f1ed475
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>所需的更新的 Active Directory 联合身份验证服务 (广告 FS) 和 Web 应用程序代理 (WAP)

>适用于： Windows Server 2016，Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 SP1

截止到 2016 年 10 月 Windows Server 的所有组件的所有更新都发布仅通过 Windows 更新 (WU)。  有没有多个修复程序或个别下载。
这适用于 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 SP1。

此页列出广告 FS WAP，以及座历史悠久的列表中的广告金融服务和 WAP 建议的修补程序更新汇总包特别关注。

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>广告金融服务和 WAP 在 Windows Server 2016 的更新
Windows Server 2016 的更新每月通过 Windows 更新提供，并且是累积性。 下面列出的更新软件包适用于所有广告金融服务和 WAP 2016 服务器，并包含之前所需的所有更新，以及最新修复程序。

|知识库编号 |描述|发布日期
|----- | ----- |-----
|[4041688（操作系统内部版本 14393.1794）](https://support.microsoft.com/kb/4041688)|此修补程序解决间歇性由于错误缓存行为 misdirects 广告颁发机构请求向错误的身份提供了以下问题。 这会影响等多因素身份验证的身份验证功能。 </br></br>报告中添加功能 AAD 连接健康 ADFS 服务器健康以正确保真度（使用详述审核）上混合 WS2012R2 和 WS2016 ADFS 场。</br></br>解决了 2012 年升级的过程 R2 ADFS 场 ADFS 2016 问题，提升电场的日落行为级别 powershell cmdlet 失败，超时，有许多信赖的方信任时。</br></br>解决了广告 FS 会导致某些身份验证失败时联盟请求到其他安全令牌服务器 (STS) 修改 wct 参数值。|10 月 2017 年终止|
|[4038801（操作系统内部版本 14393.1737）](https://support.microsoft.com/kb/4038801)|使用联盟的 LDPs OIDC 日志添加的支持。 这将使"展台方案"了多个用户可能会串行登录到一台设备具有 LDP 联盟的位置。</br></br>解决了消费电子产品 CEP/展基于证书的问题将不起作用 gMSA 帐户与 WinHello。</br></br>解决了 Windows 在 Windows Server 2016 ADFS 服务器上内部数据库 (WID) 无法同步某些设置，如 IdentityServerPolicy.Scopes 从 ApplicationGroupId 列的问题和 IdentityServerPolicy.Clients 表）由于外的关键约束。 此类同步失败可以导致其他索赔，声称主要到辅助 ADFS 服务器之间的提供商和应用程序的体验。 此外，如果 WID 的主要角色移至辅助节点，应用程序组将不再 ADFS 管理体验在易于管理</br></br>此更新可修复问题了多因素身份验证无法正常工作的移动设备使用自定义文化定义|9 月 2017 年终止|
|[4034661（操作系统内部版本 14393.1613）](https://support.microsoft.com/kb/4034661)|解决问题的调用方 IP 地址所在 nog 由 411 事件在安全事件日志 ADFS 4.0 的记录 \ Windows Server 2016 RS1 ADFS 服务器甚至后启用"成功审核"和"失败审核".</br></br>ADFX 服务器配置为使用 HTTP 代理时，此修复解决了对 Azure 多重身份验证 (MFA) 的问题。</br></br>"解决了以下问题：呈现到 ADFS 代理服务器过期或吊销证书不返回错误向用户"。|8 月 2017 年终止|
|[4034658（操作系统内部版本 14393.1593）](https://support.microsoft.com/kb/4034658)|修复的 2016 年广告 FS 服务器以的 Windows Hello 企业版本地部署上的支持 MFA 证书的注册|8 月 2017 年终止| 
|[4025334（操作系统内部版本 14393.1532）](https://support.microsoft.com/kb/4025334)|解决了以下问题：PkeyAuth 令牌处理程序可能无法身份验证，如果 pkeyauth 请求包含错误的数据。 身份验证应仍继续无需执行设备身份验证|7 月 2017 年终止|
|[4022723（操作系统内部版本 14393.1378）](https://support.microsoft.com/kb/4022723)|[Web 应用程序代理]DisableHttpOnlyCookieProtection 配置属性的价值不由获取 WAP 2016 中 2012R2/2016 年混合部署 </br></br>[Web 应用程序代理]无法从广告 FS EAS 的预验证方案中获得用户访问标记。</br></br>注销 WSFED 导致例外广告 FS 2016:|6 月 2017 年终止 
|[3213986](https://support.microsoft.com/kb/3213986)|对于基于 x64 的系统 (KB3213986) 适用于 Windows Server 2016 的累积更新| 2017 年 1 月

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>广告金融服务和 WAP 在 Windows Server 2012 R2 的更新
下面是在 Windows Server 2012 R2 的 Active Directory 联合身份验证服务 (广告 FS) 中发布修补程序和更新汇总的列表。

|知识库编号 |描述|发布日期
|----- | ----- |-----
|[4041685](https://support.microsoft.com/kb/4041685)|解决了在请求标头 MSISConext cookie 可以最终溢出标题大小限制并且 HTTP 状态代码 400 导致失败进行身份验证广告 FS 问题"错误请求-标题过长"。</br></br>解决了问题，其中 ADFS 可以不会再忽略"提示 = 登录"身份验证过程。 "禁用"选项添加还原方案使用非密码身份验证的位置。|2017 10 月更新汇总预览|
|[4019217](https://support.microsoft.com/kb/4019217)|工作的文件夹使用标记的经纪人的客户端使用 Server 2012 R2 广告 FS 服务器时不起作用|2017 年 5 月预览更新汇总| 
|[4015550](https://support.microsoft.com/kb/4015550)|解决了以下问题广告 FS 未进行身份验证的外部用户和广告 FS WAP 随机无法向前请求|2017 4 月更新汇总| 
|[4015547](https://support.microsoft.com/kb/4015547)|解决了以下问题广告 FS 未进行身份验证的外部用户和广告 FS WAP 随机无法向前请求|4 月 2017 年安全更新| 
|[4012216](https://support.microsoft.com/kb/4009970)|MS17 019 此安全更新将解决 Active Directory 联合身份验证服务 (ADFS) 中的漏洞。 如果攻击发送特制的请求到广告 FS 服务器，使得攻击者阅读有关在目标系统的敏感信息，的漏洞可能允许信息泄露。|3 月 2017 年更新汇总| 
|[3179574](https://support.microsoft.com/kb/3179574)|解决了广告 FS 外部网络密码的更新问题。 |2016 年 8 月更新汇总
|[3172614](https://support.microsoft.com/kb/3172614)|引入的提示 = 登录[支持](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/ad-fs-faq#BKMK_7)，与广告 FS 管理控制台和设置 alwaysrequireauthentication 时修复问题。 |2016 年 7 月更新汇总
|[3163306](https://support.microsoft.com/kb/3163306)|Active Directory 联合身份验证服务 (广告 FS) 3.0 无法连接到便捷目录访问协议 (LDAP) 特性官方商城配置为使用 636 或 3269 连接字符串中的安全套接字层 (SSL) 端口。 |2016 年 6 月更新汇总
|[3148533](https://support.microsoft.com/kb/3148533)|通过在 Windows Server 2012 R2 的 ADFS 代理失败 MFA 回退身份验证 |2016 年 5 月
|[3134787](https://support.microsoft.com/kb/3134787)|广告 FS 日志中不包含在 Windows Server 2012 R2 的帐户锁定方案的客户端 IP 地址 |2016 年 2 月
|[3134222](https://support.microsoft.com/kb/3134222)|MS16-020： 安全更新的 Active Directory 联合身份验证服务地址拒绝服务： 2016 年 2 月 9 日|2016 年 2 月
|[3105881](https://support.microsoft.com/kb/3105881)|基于 Windows Server 2012 R2 的广告 FS 服务器中启用的设备身份验证，则无法访问的应用程序|设置 2015 2015年 10 月
|[3092003](https://support.microsoft.com/kb/3092003)|页面加载反复和验证失败用户在 Windows Server 2012 R2 广告 FS 使用 MFA 时|2015 年 8 月
|[3080778](https://support.microsoft.com/kb/3080778)|广告 FS 不调用出错当 MFA 适配器引发在 Windows Server 2012 R2 的例外|2015 年 7 月
|[3075610](https://support.microsoft.com/kb/3075610)|信任关系辅助广告 FS 服务器上添加或删除在 Windows Server 2012 R2 的索赔提供程序后丢失|2015 年 7 月
|[3070080](https://support.microsoft.com/kb/3070080)|不能正常工作的非索赔感知依赖方信任发现家庭领域|2015 年 6 月
|[3052122](https://support.microsoft.com/kb/3052122)|更新添加了支持复合 ID 索赔，以在 Windows Server 2012 R2 的广告 FS 标记|2015 年 5 月
|[3045711](https://support.microsoft.com/kb/3045711)|MS15-040: Active Directory 联合身份验证服务中的漏洞可能允许信息泄露|2015 年 4 月
|[3042127](https://support.microsoft.com/kb/3042127)|"HTTP 400-错误请求"当你在 Windows Server 2012 R2 打开通过 WAP 共享的邮箱错误|2015 年 3 月
|[3042121](https://support.microsoft.com/kb/3042121)|在 Windows Server 2012 R2 的 Web 应用程序代理身份验证令牌广告 FS 令牌重播保护|2015 年 3 月
|[3035025](https://support.microsoft.com/kb/3035025)|修复更新密码的功能，以便用户不需要在 Windows Server 2012 R2 使用注册的设备|2015 年 1 月
|[3033917](https://support.microsoft.com/kb/3033917)|广告 FS 无法处理 SAML 响应，在 Windows Server 2012 R2|2015 年 1 月
|[3025080](https://support.microsoft.com/kb/3025080)|当你尝试将 Web 应用程序代理通过 Office 文件保存在 Windows Server 2012 R2 操作将会失败|2015 年 1 月
|[3025078](https://support.microsoft.com/kb/3025078)|未提示你输入用户名再次时不正确的用户名用于登录到 Windows Server 2012 R2 上|2015 年 1 月
|[3020813](https://support.microsoft.com/kb/3020813)|在运行 Windows Server 2012 R2 广告 FS 中的 web 应用程序时提示进行身份验证|2015 年 1 月
|[3020773](https://support.microsoft.com/kb/3020773)|超时失败后初始部署 Windows Server 2012 R2 中的设备注册服务|2015 年 1 月
|[3018886](https://support.microsoft.com/kb/3018886)|系统提示你输入用户名和密码两次访问来自 intranet Windows Server 2012 R2 广告 FS 服务器时|2015 年 1 月
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 更新汇总|2014 年 12 月
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 更新汇总|2014 年 11 月
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 更新汇总|2014 年 8 月
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 更新汇总|7 月 2014 2014年
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 更新汇总|2014 年 6 月
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 更新汇总|5 月 2014 2014年
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 更新汇总|2014 年 4 月

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>在 Windows Server 2012 (广告 FS 2.1) 和 AD 广告 fs 更新 FS 2.0
下面是修复程序和更新汇总广告 fs 2.0 和 2.1 已发布的列表。

|知识库编号 |描述|发布日期|适用于：
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|通过的代理身份验证 （这是修补程序 3094446 公开发布） 的 Windows Server 2012 中无法正常工作|2016 年 11 月质量汇总|广告 FS 2.1
|[3197869](https://support.microsoft.com/kb/3197869)|通过代理服务器的身份验证失败在 Windows Server 2008 R2 SP1 （这是修补程序 3094446 通用版本）|2016 年 11 月质量汇总|广告 FS 2.0
|[3094446](https://support.microsoft.com/kb/3094446)|在 Windows Server 2012 或 Windows Server 2008 R2 SP1 的身份验证的代理通过无法正常工作|2015 年 9 月|广告 FS 2.0 和 2.1
|[3070078](https://support.microsoft.com/kb/3070078)|广告 FS 2.1 引发异常，当您在 Windows Server 2012 加密证书针对进行身份验证|2015 年 7 月|广告 FS 2.1
|[3062577](https://support.microsoft.com/kb/3062577)|MS15 062 的： Active Directory 联合身份验证服务中的漏洞可能允许特权提升|2015 年 6 月|广告 FS 2.0 / 2.1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077: Active Directory 联合身份验证服务中的漏洞可能允许信息泄露： 2015 年 4 月 14 日|2014 年 11 月|广告 FS 2.0 / 2.1
|[2987843](https://support.microsoft.com/kb/2987843)|当很多用户登录 web 应用程序在 Windows Server 2012，保留增加广告 FS 联盟服务器的内存使用情况|7 月 2014 2014年|广告 FS 2.1
|[2957619](https://support.microsoft.com/kb/2957619)|当委派令牌用于广告 FS 请求时停止在广告 FS 信赖的方信任|5 月 2014 2014年|广告 FS 2.1
|[2926658](https://support.microsoft.com/kb/2926658)|如果你没有 SQL 权限 ADFS SQL 电场的日落部署无法正常工作|2014 年 10 月|广告 FS 2.1
|[2896713](https://support.microsoft.com/kb/2896713)或[2989956](https://support.microsoft.com/kb/2989956)|更新为可供你安装广告 FS 服务器上的安全更新 2843638 后修复几个问题|11 月 2013</br></br>2014 年 9 月|广告 FS 2.0 / 2.1
|[2877424](https://support.microsoft.com/kb/2877424)|更新使你使用一个证书的多个信赖方信任中广告 FS 2.1 电场的日落|2013 年 10 月|广告 FS 2.1
|[2873168](https://support.microsoft.com/kb/2873168)|当您使用第三方 CSP 和 HSM，然后广告 fs 2.0 于 Windows Server 2008 R2 Service Pack 1 配置更新汇总 3 中的索赔提供商信任错误发生修复：|9 月 2013|广告 FS 2.0
|[2861090](https://support.microsoft.com/kb/2861090)|加密证书的对象名称在输入逗号导致在 Windows Server 2008 R2 SP1 的例外|8 月 2013|广告 FS 2.0
|[2843639](https://support.microsoft.com/kb/2843639)|安全Active Directory 联合身份验证服务中的漏洞可能允许信息泄露|11 月 2013|广告 FS 2.1
|[2843638](https://support.microsoft.com/kb/2843638)|安全更新的 Active Directory 联合身份验证服务 2.0 MS13-066： 说明： 2013 年 8 月 13 日|8 月 2013|广告 FS 2.0
|[2827748](https://support.microsoft.com/kb/2827748)|Federationmetadata.xml 文件不包含在 Windows Server 2012 WS 信任和 WS 联盟端点 MEX 端点信息|5 月 2013|广告 FS 2.1
|[2790338](https://support.microsoft.com/kb/2790338)|描述累积更新 3 的 Active Directory 联合身份验证服务 (广告 FS) 2.0|3 月 2013|广告 FS 2.0





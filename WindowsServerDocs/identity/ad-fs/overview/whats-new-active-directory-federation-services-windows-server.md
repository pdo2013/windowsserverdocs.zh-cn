---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: 什么是 Windows Server 2016 的 Active Directory 联合身份验证服务中的新增功能
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8dd9174a869110d98ba1e65cbf2c2b6ec000b71
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
---
# <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>什么是 Windows Server 2016 的 Active Directory 联合身份验证服务中的新增功能

>适用于：Windows Server 2016

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>什么是 Windows Server 2016 的 Active Directory 联合身份验证服务中的新增功能   
如果你在查找有关广告 FS 的较早版本的信息，请参阅下文：  
 [在 Windows Server 2012 ADFS 或 2012 R2](https://technet.microsoft.com/library/hh831502.aspx)和[广告 FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory 联合身份验证服务提供跨各种包括 Office 365、基于云的 SaaS 应用和应用程序公司的网络上的应用程序访问控制和单一登录。  
* 对于 IT 组织中，它使您能够登录提供有关和访问控制用于现代和传统应用程序在本地或在云中进行构建，根据凭据和策略的同一组。    
* 对于的用户，使用相同的熟悉帐户凭据它提供无缝登录。  
* 开发人员，它将提供简单的方式进行身份验证其身份 live 组织目录中，以便你可以将精力应用程序、不身份验证或身份的用户。  

本文介绍了在广告 FS（广告 FS 2016）的 Windows Server 2016 的新增功能。  

## <a name="eliminate-passwords-from-the-extranet"></a>从联网消除密码  
从 phished，泄漏或被盗密码危及广告 FS 2016 启用三个新选项不启用组织为避免网络密码的情况下登录。 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>登录 Azure 多重身份验证
广告 FS 2016 基础多重身份验证 (MFA) 的广告 FS 在 Windows Server 2012 R2 的功能，从而仅使用 Azure MFA 代码，而无需输入用户名和密码登录。

* Azure MFA 作为主要身份验证方法、与他们的用户名和 OTP 代码 Azure 验证器应用从提示用户。  
* Azure MFA 辅助或其他身份验证方法为，用户提供了主要身份验证凭据（使用 Windows 的集成身份验证、用户名和密码、智能卡或用户或设备证书），然后将看到一条提示输入文本，语音，或 OTP 基于 Azure MFA 登录。  
* 使用新的内置 Azure MFA 适配器安装和配置为与广告 FS Azure MFA 从未如此简单。
* 组织可以充分利用 Azure MFA 而无需使用本地 Azure MFA 服务器上。
* Azure MFA 可以的 intranet 或外部网时，或配置作为任何访问控制策略的一部分。

有关 Azure AD FS 与的 MFA 详细信息
*  [广告金融服务 2016 年和 Azure MFA 配置](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>从兼容设备的密码不太访问
广告 FS 2016 版本上以前以启用登录的设备注册功能和访问控制基于设备合规性状态。 用户可以使用设备的凭据登录和合规性时，将重新计算设备属性更改，以便你始终可以确保强制策略。  启用策略如

* 仅从托管和/或兼容的设备中启用访问  
* 仅从托管和/或兼容的设备启用外部网络的访问权限  
* 计算机不是托管或不兼容，需要多重身份验证  

广告 FS 提供在本地组成部分混合方案中的条件访问策略。 条件访问云资源使用 Azure AD 注册设备，当设备身份可用于广告 FS 策略还。

![新增功能](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 有关使用设备的详细信息基于在云中的条件访问   
 *  [Azure Active Directory 条件访问](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)

有关使用设备的详细信息基于与广告 FS 条件访问
*  [设备规划基于与广告 FS 条件访问](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [广告 FS 中访问控制策略](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>使用 Windows Hello 企业版登录   
Windows Hello 和 Windows Hello 企业版，用户密码替换为强大的设备绑定用户凭据引入了 Windows 10 设备受用户手势 (PIN，生物识别动作指纹或面部识别等)。 广告 FS 2016 支持这些新这些新的 Windows 10 功能，以便用户可以登录到广告 FS 应用程序的 intranet 或外部而无需输入密码。

有关 Microsoft Windows Hello 企业版在使用你的组织的详细信息
*  [启用 Windows Hello 企业版在你的组织](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>安全于应用程序访问

### <a name="modern-authentication"></a>现代身份验证
广告 FS 2016 支持的最新的现代协议为 Windows 10，以及新 iOS 和 Android 设备和应用中提供更好的用户体验。  

有关详细信息，请参阅[广告 FS 方案面向开发人员](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>无需知道配置访问控制策略声称规则语言  
以前，广告 FS 管理员必须配置策略使用该广告 FS 索赔规则语言，因此很难配置和维护策略。 通过访问控制策略，管理员可以使用内置的模板应用常见策略，如
* 允许 intranet 访问权限
* 许可的每个人和需要 MFA 从外联网
* 许可的每个人和需要 MFA 特定组中

模板轻松自定义使用向导驱动进程添加例外或其他策略规则其中可以一个或多个的应用程序一致的策略实施的应用。

有关详细信息，请参阅[广告 FS 中的访问控制策略。](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>启用与广告 LDAP 目录登录  
许多组织都有 Active Directory 和第三方目录的组合。 新增的广告 FS LDAP v3 兼容目录中存储的用户身份验证的支持，与广告 FS 现在可用于：
* 第三方，LDAP v3 兼容目录中的用户
* Active Directory 双向信任不配置 Active Directory 森林中的用户
* Active Directory 轻型目录服务 (广告 LDS) 中的用户

有关详细信息，请参阅[配置广告 FS LDAP 目录中存储的用户进行身份验证。](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>更好地登录体验
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>自定义日志中有关广告 FS 应用程序的体验  
我们收到了你可以自定义每个应用的登录体验，将会是出色可用性改进，特别是对于组织提供的应用程序代表的多个不同的公司或品牌登录。  

以前，在 Windows Server 2012 R2 的广告 FS 信赖的方应用程序的体验提供常用的符号，能够自定义的文本子集基于每个应用程序的内容。 与 Windows Server 2016 中，你可以自定义不仅的消息，但图像，每个应用程序徽标和 web 主题。 此外，你可以创建新的自定义 web 主题和应用依赖每这些方。  

有关详细信息，请参阅[广告 FS 用户登录自定义。](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>可管理性和运营增强功能  
以下部分介绍使用 Active Directory 联合身份验证服务 Windows Server 2016 中引入的改进操作情况。  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>简化更易于管理的审核  
广告有 Windows Server 2012 R2 的 FS 已生成一个请求许多审核事件和有关登录或令牌的发布活动的相关信息是不存在（在某些版本的广告 FS）或分布跨多个审核事件。 默认情况下广告 FS 审核事件均处于关闭状态由于其详述自然。  
广告 FS 2016 的发布，与审核变得更流畅，并且不太详述。  

有关详细信息，请参阅[审核广告 FS Windows Server 2016 中的增强功能。](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>改进了参与 confederations SAML 2.0 互操作性  
广告 FS 2016 包含其他 SAML 协议支持，包括导入信任根据所包含的多个实体元数据的支持。 这将使您可以配置广告 FS 参与 confederations 如 InCommon 联盟和符合 eGov 2.0 标准其他实现。  

有关详细信息，请参阅[改进了与 SAML 2.0 互操作性。](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>简化的密码管理联合 O365 用户  
您可以配置了 Active Directory 联合身份验证服务 (AD FS) 发送到信赖的方信任（应用）中受广告 FS 密码到期索赔。 这些声明的使用方式取决于应用程序。 例如，与作为你依赖方 Office 365、更新已经实现了对通知的其很快到-会-已过期密码联合的用户的 Exchange and Outlook。  

有关详细信息，请参阅[配置广告 FS 发送密码到期索赔。](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>将在 Windows Server 2012 R2 的广告 FS 移动到在 Windows Server 2016 的广告 FS 就能更易于  
以前，迁移到新版本的广告 FS 要求从旧场中导出配置和导入到新、并行场。  

现在，从广告 FS 在 Windows Server 2012 R2 上移至 Windows Server 2016 上广告 FS 变得更加容易。 只需将一个新的 Windows Server 2016 服务器添加到 Windows Server 2012 R2 电场的日落，并在 Windows Server 2012 R2 电场的日落行为级别，采取措施电场的日落，因此它的外观和就像 Windows Server 2012 R2 场行为。  

然后，将新的 Windows Server 2016 服务器添加到电场的日落，验证功能负载平衡从删除较早的服务器。 所有电场的日落节点运行的 Windows Server 2016，现在可以升级到 2016 年电场的日落行为级别并开始使用的新功能。  

有关详细信息，请参阅[升级到 Windows Server 2016 中的广告 FS。](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  

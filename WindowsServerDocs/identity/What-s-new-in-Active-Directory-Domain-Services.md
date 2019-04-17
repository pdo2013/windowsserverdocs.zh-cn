---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: "什么 & #39; s Active Directory 域服务中的新增功能"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: 
ms.suite: na
ms.technology: active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: 56716e2e0d633a19e9c6e02bc829874bbe863833
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services"></a>什么 & #39; s Active Directory 域服务中的新增功能 

>适用于：Windows Server 2016

以下 Active Directory 域服务 (广告 DS) 中的新功能来提升组织安全 Active Directory 环境，并帮助他们仅限云的部署和混合部署，了在云中托管某些应用程序和服务，并且其他人在本地承载迁移的功能。 改进包括：  
  
-   [管理访问的权限](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [通过加入 Azure Active Directory 的 Windows 10 设备的扩展云功能](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [连接到 Azure AD 已加入域的设备，Windows 10 的体验](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [在你的组织中启用用于工作 Microsoft Passport](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [Deprecation 的文件复制服务 (FRS) 和 Windows Server 2003 功能级别](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>管理访问的权限  
访问权限管理 (PAM) 可以帮助减少安全关注的 Active Directory 环境引起的凭据技术被盗此类 pass 哈希、矛网络钓鱼和类似类型的攻击。 它提供一个新的管理访问解决方案，方法是使用 Microsoft 身份管理器 (MIM) 配置。 引入 PAM:  
  
-   新堡垒 Active Directory 的森林由 MIM 预配。 堡垒森林具有特殊的与现有的森林 PAM 信任。 它提供一个已知的任何恶意的活动，并隔离现有林的使用权限帐户的免费的新 Active Directory 环境。  
  
-   MIM 请求管理权限，以及新的基于批准请求的工作流中的新进程。  
  
-   新阴影安全主体（组）的由预配堡垒林中 MIM 管理权限请求的响应。 阴影安全主体有引用的现有的树林中管理组 SID 属性。 这样将现有的树林中访问资源卷影组而不会更改任何访问 acl)。  
  
-   过期的通知链接功能，使时间限制卷影组中的成员。 用户可以添加到组中，只需足够时间，需要执行的管理任务中。 时间限制的会员表示通过传播到 Kerberos 票证生命周期内的时间 live (TTL) 值。  
  
    > [!NOTE]  
    > 过期的链接都适用于所有链接属性。 但成员/memberOf 链接的属性一组与用户之间的关系是仅示例了预完整的解决方案，如 PAM 使用到期链接功能。  
  
-   KDC 增强功能内置于 Active Directory 域控制器，以便最低可能 live 时间 (TTL) 中的值用户具有多个时间限制会员身份管理组中的情况下限制 Kerberos 票证生命周期。 例如，如果你添加时间限制组 A 到时，然后当你登录时 Kerberos 票证许可票证 (TGT) 整个使用期限内等于 A.组中的剩余的时间如果你还组成员的其他时间限制 B，具有较低比组 A TTL，然后 TGT 整个使用期限内等于组 b。在剩余的时间  
  
-   监控的新功能来帮助你轻松地确定谁可以请求访问权限、允许哪些访问，并执行哪些操作。  
  
**要求**  
  
-   Microsoft 标识管理器  
  
-   Active Directory 林功能级别的 Windows Server 2012 R2 或更高版本。  
  
## <a name="BKMK_AzureADJoin"></a>Azure AD 加入  
Azure Active Directory 加入增强了身份难忘企业版、企业和 EDU 客户的用于企业和个人设备的改进功能。  
  
优点：  
  
-   **现代设置的可用性**corp 所拥有的 Windows 设备上。 氧气的服务不再需要的个人 Microsoft 帐户： 它们现在关闭用户现有工作帐户，以确保合规性运行。 氧气服务也适用于本地上的 Windows 域加入的电脑和 Pc 和将"加入"你 Azure AD 租户 ("云 domain") 的设备。 这些设置包括：  
  
    -   漫游或个性化、辅助功能设置和凭据  
  
    -   备份和还原  
  
    -   访问 Microsoft 官方商城通过工作帐户  
  
    -   动态磁贴和通知  
  
-   **访问组织资源**移动设备 （台式机手机） 不能加入 Windows 某个域中，它们是否 corp 拥有或 BYOD  
  
-   **上的单一登录**Office 365 其他组织的应用、网站和资源。  
  
-   **BYOD 设备上**、（从本地域或 Azure AD）的工作帐户添加到个人所拥有的设备，并尽情享受 SSO 工作资源，通过应用并在 web 上，有助于确保符合条件的帐户控件和设备健康审计等新功能的方式。  
  
-   **MDM 集成**允许您自动注册设备到你的 MDM（Intune 或第三方）  
  
-   **设置"展台"模式下，并共享设备**为在你的组织的多个用户  
  
-   **开发人员体验**允许您在生成适应企业和个人上下文使用共享的编程堆栈中的应用。  
  
-   **成像**选项允许你选择之间映像并使你的用户，若要在首次运行体验期间直接配置 corp 拥有的设备。  
  
有关详细信息，请参阅[适用于企业的 Windows 10：使用设备的工作方式](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1)。  
  
## <a name="BKMK_IDLocker"></a>MicrosoftPassport  
MicrosoftPassport 是新密钥基于身份验证方法组织并消费者而言，超出密码。 在违反、被盗和网络钓鱼溅凭据依赖于这种形式的身份验证。  
  
用户在登录到设备使用生物识别或 PIN 登录链接到证书或对称的关键配对的信息。 身份提供商 (IDPs) 验证通过映射到 IDLocker 公钥的用户的用户，并通过一次密码 (OTP)、Phonefactor 或其他通知机制信息提供日志。  
  
有关详细信息，请参阅[进行身份验证身份没有通过 Microsoft Passport 的密码](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport/)  
  
## <a name="BKMK_FRSDeprecation"></a>Deprecation 的文件复制服务 (FRS) 和 Windows Server 2003 功能级别  
尽管文件复制服务 (FRS) 和 Windows Server 2003 功能级别已弃用在以前版本的 Windows Server，它携带重复 Windows Server 2003 的操作系统不再受支持。 因此，应从域中删除任何运行 Windows Server 2003 的域控制器。 到应该至少提升域和森林功能级别 Windows Server 2008 以防止被添加到环境中运行较早版本的 Windows Server 的域控制器。  
  
在 Windows Server 2008 和更高版本的域功能级别，分布式文件服务 (DFS) 复制用于域控制器之间复制 SYSVOL 文件夹的内容。 如果你创建一个新域，Windows Server 2008 域级别正常工作或更高版本，已自动使用 DFS 复制复制 SYSVOL。 如果你在较低的功能级别创建域，你将需要使用的 SYSVOL DFS 复制到 FRS 迁移。 对于迁移步骤中，您可以按照[TechNet 上的过程](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)，或者你可以参考[优化的步骤存储团队文件柜博客上的一套](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。  
  
Windows Server 2003 域和森林功能级别继续受支持，但组织应提升与 Windows Server 2008 （或如果可能，更高版本） 功能级别以确保 SYSVOL 复制兼容性，并在将来的支持。 此外，有许多其他权益和功能可在更高版本的功能级别更高版本。 请参阅以下详细信息资源：  
  
-   [了解 Active Directory 域服务 (广告 DS) 功能级别](ad-ds/active-directory-functional-levels.md)  
  
-   [提升域功能级别](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [提升森林功能级别](https://technet.microsoft.com/library/cc730985.aspx)  
  

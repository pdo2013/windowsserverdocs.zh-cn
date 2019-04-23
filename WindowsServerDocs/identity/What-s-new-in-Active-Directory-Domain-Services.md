---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: 什么&#39;s Active Directory 域服务中的新增功能
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: ffa8bcb43b17ae8779c70d499bff27a8f77cce75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841328"
---
# <a name="what39s-new-in-active-directory-domain-services"></a>什么&#39;s Active Directory 域服务中的新增功能 

>适用于：Windows Server 2016

Active Directory 域服务 (AD DS) 中的以下新功能改进组织保护 Active Directory 环境并帮助他们迁移到仅限云的部署和混合部署，则一些应用程序和服务的功能托管在云中和其他的则托管在本地。 这些改进包括：  
  
-   [特权的访问管理](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [将云功能扩展到 Windows 10 设备通过 Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [连接到 Azure AD 适用于 Windows 10 的已加入域的设备体验](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [在组织中启用 Microsoft Passport for Work](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [不推荐使用的文件复制服务 (FRS) 和 Windows Server 2003 功能级别](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>特权的访问管理  
Privileged access management (PAM) 可帮助缓解安全问题而引起的 Active Directory 环境的凭据盗窃技术，这种传递哈希、 鱼叉式网络钓鱼和类似类型的攻击。 它提供了一个新的管理访问权限解决方案，通过使用 Microsoft Identity Manager (MIM) 配置。 PAM 引入了：  
  
-   新的堡垒 Active Directory 林中，由 MIM 预配。 堡垒林之间具有特殊的 PAM 信任的现有林。 它提供了新的 Active Directory 环境已知的任何恶意活动，并从现有林使用特权帐户的隔离免费的。  
  
-   在 MIM 中请求管理权限，以及基于请求的批准的新工作流的新进程。  
  
-   新卷影安全主体 （组） 中预配在堡垒林中由 MIM 管理权限请求的响应。 卷影安全主体具有的属性的引用是管理组在现有林中的 SID。 这允许在现有林中访问资源的卷影组而无需更改任何访问控制列表 (Acl)。  
  
-   到期的链接功能，它使卷影组中的限时成员身份。 用户可以添加到组中，执行管理任务所需的足够时间中。 通过一个生存时间 (TTL) 值，该值将传播到 Kerberos 票证生存期来表示时限的成员身份。  
  
    > [!NOTE]  
    > 链接的所有属性上提供了即将到期的链接。 但成员/memberOf 链接的属性组和用户之间的关系是唯一的示例，如 PAM 完整的解决方案预配置为使用即将到期的链接功能。  
  
-   若要将 Kerberos 票证生存期限制为最小可能生存时间 (TTL) 值在用户在其中具有多个时限的成员身份的管理组中的情况下的 Active Directory 域控制器内置了 KDC 增强功能。 例如，如果您添加到的限时组 A，则当你登录，Kerberos 票证授予票证 (TGT) 生存期是等于时间必须保留在组 a。如果你还有另一个限时组具有较低的 TTL 比组 A，B 的成员则等于具有组 B 中的剩余的时间的 TGT 生存期  
  
-   新的监视功能，帮助你轻松识别人员请求访问权限、 哪些访问权限授予，以及已执行的活动。  
  
**要求**  
  
-   Microsoft 标识管理器  
  
-   Active Directory 林功能级别的 Windows Server 2012 R2 或更高版本。  
  
## <a name="BKMK_AzureADJoin"></a>Azure AD Join  
Azure Active Directory Join 增强了对 enterprise、 business 和 EDU 客户-使用适用于公司和个人设备的改进功能的标识体验。  
  
优势：  
  
-   **现代设置可用性**公司拥有的 Windows 设备上。 氧气服务无需再使用个人 Microsoft 帐户： 它们现在运行关闭用户的现有工作帐户，以确保符合性。 氧气服务将适用于加入到的本地 Windows 域，电脑和 Pc 和"联接"到 Azure AD 租户 （"云域"） 的设备。 这些设置包括：  
  
    -   漫游或个性化设置、 辅助功能设置和凭据  
  
    -   备份和还原  
  
    -   使用工作帐户访问 Microsoft Store  
  
    -   动态磁贴和通知  
  
-   **访问组织资源**BYOD 或无法加入 Windows 域中，无论它们是公司拥有的移动设备 （手机、 平板手机） 上  
  
-   **单一登录**对 Office 365 和其他组织的应用、 网站和资源。  
  
-   **BYOD 设备上**、 工作帐户 （从本地域或 Azure AD） 将添加到个人拥有的设备和享受 SSO 以资源、 通过应用和 web 上的工作方式有助于确保符合性等条件的帐户的新功能控制和设备运行状况证明。  
  
-   **MDM 集成**可以自动注册到 MDM （Intune 或第三方） 的设备  
  
-   **设置"展台"模式和共享设备**为你的组织中的多个用户  
  
-   **开发人员体验**允许您生成应用，它们适用于企业和个人上下文具有共享的编程堆栈。  
  
-   **图像处理**选项允许您选择映像，并允许你的用户之间直接在首次运行体验过程中配置公司拥有的设备。  
  
有关详细信息，请参阅[适用于企业的 Windows 10:使用设备的工作方式](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1)。  
  
## <a name="BKMK_IDLocker"></a>Microsoft Passport  
Microsoft Passport 是新的基于密钥的身份验证方法组织和消费者而言，，超越了密码。 这种形式的身份验证依赖于漏洞、 盗窃和网络钓鱼诈骗抵御凭据。  
  
在用户登录到该设备使用生物识别或 PIN 登录链接到证书或非对称密钥对的信息。 标识提供者 (Idp) 通过将用户的公钥映射到 IDLocker 验证用户，并提供有关通过一次性密码 (OTP)、 Phonefactor 或不同的通知机制的信息的日志。  
  
有关详细信息，请参阅[而无需通过 Microsoft Passport 的密码进行身份验证](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport/)  
  
## <a name="BKMK_FRSDeprecation"></a>不推荐使用的文件复制服务 (FRS) 和 Windows Server 2003 功能级别  
尽管在以前版本的 Windows Server 中已弃用文件复制服务 (FRS) 和 Windows Server 2003 功能级别，但还是要重复提醒不再支持 Windows Server 2003 操作系统。 因此，应从域中删除任何运行 Windows Server 2003 的域控制器。 应至少幂域和林功能级别 Windows Server 2008，以防止从添加到环境中运行 Windows Server 的早期版本的域控制器。  
  
在 Windows Server 2008 和更高版本的域功能级别，分布式文件服务 (DFS) 复制用于域控制器之间复制 SYSVOL 文件夹的内容。 如果在 Windows Server 2008 域功能级别或更高版本创建一个新的域，自动使用 DFS 复制来复制 SYSVOL。 如果在较低功能级别创建域，您将需要使用 FRS 的 SYSVOL 的 DFS 复制进行迁移。 有关迁移的步骤，你可以按照[TechNet 上的过程](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)也可以引用[简化组的 Cab 文件的存储团队博客上的步骤](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。  
  
Windows Server 2003 域和林功能级别继续受到支持，但组织应提升功能级别为 Windows Server 2008 （或更高，如果可能） 以确保 SYSVOL 复制的兼容性和将来支持。 此外，有许多其他权益和功能在更高版本的功能级别更高版本。 有关详细信息，请参阅以下资源：  
  
-   [了解 Active Directory 域服务 (AD DS) 功能级别](ad-ds/active-directory-functional-levels.md)  
  
-   [提升域功能级别](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [提升林功能级别](https://technet.microsoft.com/library/cc730985.aspx)  
  

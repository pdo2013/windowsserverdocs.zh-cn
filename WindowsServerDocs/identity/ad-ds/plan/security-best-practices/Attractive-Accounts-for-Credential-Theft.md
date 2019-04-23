---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: 凭据容易被盗的帐户
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e5d2b01f73127f84f700dab3518b66687f0294f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863098"
---
# <a name="attractive-accounts-for-credential-theft"></a>凭据容易被盗的帐户

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

凭据被盗攻击是攻击者最初获取最高特权 （根、 管理员或系统，具体取决于正在使用的操作系统） 到上一个网络，然后使用任意可用工具中提取凭据的计算机的访问权限从其他登录的帐户的会话。 根据系统配置中，可以在哈希、 票证或甚至是纯文本密码的窗体中提取这些凭据。 如果已获取任何的凭据可能存在 （例如，Windows 中的管理员帐户或在 OSX、 UNIX 或 Linux 根帐户） 在网络上其他计算机上的本地帐户，攻击者凭据提供给其他计算机上网络传播到其他计算机的危害，并尝试获取两个特定类型的帐户的凭据：  

1.  使用各种广泛而深入的权限 （即，在多台计算机上并在 Active Directory 中具有管理员级别特权的帐户） 的特权的域帐户。 这些帐户可能不是任何 Active Directory 中的最高特权组的成员，但它们可能已被授予管理员级别的特权在许多服务器和工作站中的域或林，这使它们作为有效地强大在 Active Directory 中的特权组的成员。 在大多数情况下，已被广泛的 Windows 基础结构负责跨授予高级别的特权的帐户是特权的服务帐户，因此服务帐户应始终进行评估的广度和深度。  

2.  "非常重要 Person"(VIP) 的域帐户。 在本文档的上下文，VIP 帐户是有权访问信息 （知识产权和其他敏感信息），攻击者想要的任何帐户或任何可用于授予对该信息的攻击者访问权限的帐户。 这些用户帐户的示例包括：  

    1.  高级管理人员的帐户有权访问敏感的公司信息  

    2.  技术支持人员，他们负责维护计算机和使用高级管理人员的应用程序的帐户  

    3.  有权访问组织的 bid 和协定的文档，无论文档是用于其自己的组织或客户端组织的法律部门员工的帐户  

    4.  产品规划师有权访问计划和规范的产品公司的开发中的管道，而不考虑类型的产品的公司开始使用  

    5.  研究人员使用的帐户来访问产品表达式或攻击者感兴趣的任何其他研究研究数据  

传播泄漏以及处理 VIP 帐户或可以访问的数据，可以使用 Active Directory 中的高特权的帐户，因为凭据被盗攻击的最有用帐户是 Enterprise Admins 的成员的帐户域管理员和管理员组在 Active Directory 中。  

域控制器的 AD DS 数据库的存储库，并且域控制器在 Active Directory 中拥有完全访问权限的所有数据，因为域控制器还针对受损，是否以并行凭据被盗攻击，或一个或多个高特权的 Active Directory 帐户已被损坏。 尽管多家出版物 （和许多攻击者） 时，应注意的 Domain Admins 组成员身份描述传递哈希和其他凭据被盗攻击 (如中所述[减少 Active Directory 攻击面](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md))该帐户是任何此处列出的组的成员可用于危及整个 AD DS 安装。  

> [!NOTE]  
> 有关传递哈希和其他凭据被盗攻击的全面信息，请参阅[缓解传递的哈希 (PTH) 攻击和其他凭据盗窃技术](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf)中列出的白皮书[附录 M:文档链接和建议的读物](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)。 有关确定攻击被攻击的详细信息，这有时称为"高级持久性威胁"(Apt)，请参阅[确定的攻击者和目标攻击](https://www.microsoft.com/download/details.aspx?id=34793)。  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>增加遭到破坏的可能性的活动  
凭据被盗的目标通常是高特权的域帐户和 VIP 帐户，因为它是管理员必须是有意识的提高成功的凭据被盗攻击的可能性的活动。 如果 Vip 不提供高级别的特权系统或域中，攻击者还会以 VIP 帐户，尽管其凭据被盗需要其他类型的攻击，例如社交工程 VIP 以提供机密信息。 或者，攻击者必须首先获取特权的访问系统的 VIP 会缓存凭据。 因此，增加此处所述的凭据被盗的可能性的活动是重点主要放在阻止高特权的管理凭据获取。 这些活动是常见的攻击者能够入侵系统以获取特权的凭据的机制。  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>登录到具有权限的帐户的未受保护计算机  
允许凭据被盗攻击成功的核心漏洞是登录到不是使用帐户广泛的安全和深度特权在整个环境的计算机的行为。 这些登录可以是此处所述的各种配置错误的结果。  

#### <a name="not-maintaining-separate-administrative-credentials"></a>不维护单独的管理凭据  
虽然这是相对较少出现，在评估各种 AD DS 安装，但我们发现 IT 员工使用其工作的所有单个帐户。 是至少一个 Active Directory 中的最高特权组的成员，并且是相同的帐户的员工用于登录到他们的工作站中第二天早上、 检查其电子邮件、 浏览 Internet 站点和下载内容到其计算机。 当用户使用被授予本地管理员权利和权限的帐户运行时，它们公开本地计算机以完成泄漏。 当这些帐户也是 Active Directory 中的最高特权组的成员时，它们公开整个林泄漏，使攻击者能够完全控制的 Active Directory 和 Windows 环境十分容易。  

同样，在某些环境中，我们发现，相同的用户名和密码用于非 Windows 计算机上的根帐户都使用在 Windows 环境中，使得攻击者能够将泄漏从 UNIX 或 Linux 系统扩展到 Windows 系统反之亦然。

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>登录到遭到入侵的工作站或成员服务器使用特权帐户  
当高特权的域帐户用于以交互方式登录到遭到入侵的工作站或成员服务器时，该攻击的计算机可能收集并从登录到系统的任何帐户的凭据。  

#### <a name="unsecured-administrative-workstations"></a>不安全管理工作站  
在许多组织而言，IT 人员使用多个帐户。 一个帐户用于登录到员工的工作站，因为这些是 IT 人员，他们通常具有本地管理员权限在他们的工作站上。 在某些情况下，UAC 是保持启用，以便在用户至少收到拆分访问令牌登录时，必须进行提升时所需的特权。 当这些用户正在执行维护活动时，它们通常使用本地安装的管理工具并通过选择其域权限的帐户，提供的凭据**以管理员身份运行**选项或通过提供当系统提示的凭据。 尽管此配置可能都不适合，它公开侵扰，因为环境：  

-   员工使用登录到其工作站上的"常规"用户帐户具有本地管理员权限，计算机容易受到[偷渡式下载](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx)说服用户来安装恶意软件的攻击。  

-   恶意软件已安装的管理帐户的上下文中，现在可以使用计算机来捕获击键、 剪贴板内容、 屏幕截图和驻留在内存中的凭据，其中的任何一可能会导致暴露的功能强大的域凭据帐户。  

在此方案中的问题在两方面。 首先，尽管使用单独的帐户来进行本地和域管理，计算机是不安全，并不会保护以免被盗的帐户。 其次，普通用户帐户和管理帐户已被授予过多的权限和权限。  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>浏览 Internet 使用高特权帐户  
登录到帐户的计算机上，本地管理员组的成员或 Active Directory 中的特权组的成员并且人员然后浏览 Internet （或已泄露的 intranet） 的计算机的用户公开本地计算机和要破坏的目录。  

使用具有管理权限运行的浏览器访问恶意的网站可以允许攻击者的特权用户的上下文中在本地计算机上存放恶意代码。 如果用户在计算机上具有本地管理员权限，攻击者可以欺骗用户下载恶意代码或打开利用应用程序漏洞，并利用用户的权限来提取本地缓存的电子邮件附件在计算机上的所有活动用户的的凭据。 如果用户在目录中 Enterprise Admins、 Domain Admins 或 Active Directory 中的 Administrators 组的成员资格中具有管理权限，攻击者可提取的域凭据，使用它们来损害整个 AD DS 域或林，而无需破坏林中的任何其他计算机。  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>跨系统的相同凭据配置本地特权的帐户  
在多个或所有计算机启用凭据被盗时从一台计算机以用来破坏使用相同的凭据的所有其他计算机上的 SAM 数据库上配置相同的本地管理员帐户名称和密码。 至少应为本地管理员帐户使用不同的密码，在每个已加入域的系统。 本地管理员帐户也可能唯一名称，但对每个系统的特权的本地帐户使用不同的密码已足以确保不能在其他系统上使用凭据。  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation 和过度使用的特权的域组  
在域中的 EA、 DA 或 BA 组的成员身份授予为攻击者创建的目标。 这些组，更高的特权的用户可能无意滥用凭据并公开这些凭据被盗的可能性攻击的成员数越大。 每个工作站或服务器到特权的域用户登录提供可能机制特权的用户的凭据可能搜集和用于入侵的 AD DS 域和林。  

### <a name="poorly-secured-domain-controllers"></a>这么不安全的域控制器  
域控制器可存放域的 AD DS 数据库的副本。 在只读域控制器的情况下该数据库的本地副本包含在目录中，这些都默认情况下的特权的域帐户的帐户的一个子集的凭据。 在读写域控制器上每个域控制器维护 AD DS 数据库，包括不仅适用于类似于域管理员，但如的域控制器帐户或域的 Krbtgt 特权的帐户的特权用户的凭据的完整副本帐户是与域控制器上的 KDC 服务相关联的帐户。 如果不是域控制器功能所需的其他应用程序安装在域控制器上，或者如果域控制器不严格地完成修补和保护，攻击者可能会危及它们通过未修补的漏洞，或它们可以利用其他直接在其上安装恶意软件的攻击媒介。  

## <a name="privilege-elevation-and-propagation"></a>特权提升和传播  
无论使用的攻击方法，在 Windows 环境受到攻击时，始终为目标 Active Directory，因为它最终控制访问到任何攻击者。 这并不意味着针对整个目录，但是。 特定帐户、 服务器和基础结构组件通常是 Active Directory 对攻击的主要目标。 这些帐户的说明，如下所示。  

### <a name="permanent-privileged-accounts"></a>永久特权的帐户  
因为 Active Directory 的介绍，就能使用极高的特权帐户生成 Active Directory 林，再指向委托权限和执行日常管理到较低权限帐户所需的权限。 在 Active Directory 中 Enterprise Admins、 Domain Admins 或管理员组的成员资格才需要暂时和很少实现最低特权方法到日常管理的环境中。  

永久特权的帐户是已在特权组中置于并留有一天到一天的帐户。 如果你的组织域的 Domain Admins 组中置于五个帐户，这些五个帐户可以是目标的 24 小时一天，一周七天。 但是，实际需要使用具有域管理员权限的帐户通常是仅为特定的全域性配置，以及一段较短的时间。  

### <a name="vip-accounts"></a>VIP 帐户  
在 Active Directory 违规中的经常被忽略的目标是在组织中的"非常重要的人员"（或 Vip） 的帐户。 特权的帐户被目标，因为这些帐户可以授予访问权限给攻击者，使他们能够破坏或甚至会破坏目标的系统，如在本部分中前面所述。  

### <a name="privilege-attached-active-directory-accounts"></a>"特权附加"Active Directory 帐户  
"特权附加"Active Directory 帐户是未进行任何有权限在 Active Directory 中的最高级别，但改为已被授予的权限在多台服务器上的高级别的组的成员的域帐户和在环境中的工作站。 这些帐户是配置为在已加入域的系统，通常用于在大段的基础结构上运行的应用程序上运行服务的大多数通常基于域的帐户。 如果它们已被授予在大量的系统上的高特权，这些帐户在 Active Directory 中不具有任何权限，尽管它们可用于入侵或甚至破坏大段的基础结构，实现相同的效果的受到安全威胁Active Directory 帐户特权。  

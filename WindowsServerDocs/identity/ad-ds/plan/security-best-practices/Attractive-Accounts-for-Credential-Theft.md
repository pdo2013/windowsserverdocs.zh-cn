---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: 凭据容易被盗的帐户
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2d6fdeaef6e8c90d5e723bdd3ee334369ef73c68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367740"
---
# <a name="attractive-accounts-for-credential-theft"></a>凭据容易被盗的帐户

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

凭据被盗攻击是指攻击者最初获得最高特权（根、管理员或系统，具体取决于所使用的操作系统）访问网络上的计算机，然后使用自由可用的工具提取凭据从其他登录帐户的会话中。 根据系统配置，可以采用哈希、票证甚至纯文本密码形式提取这些凭据。 如果任何搜集凭据适用于可能存在于网络上的其他计算机上的本地帐户（例如，Windows 中的管理员帐户或 OSX、UNIX 或 Linux 中的根帐户），则攻击者会向中的其他计算机提供凭据网络传播到其他计算机，并尝试获取两种特定类型帐户的凭据：  

1.  具有广泛和深层权限的特权域帐户（即，在多台计算机和 Active Directory 中具有管理员级别权限的帐户）。 这些帐户可能不是 Active Directory 中的任何最高特权组的成员，但在域或林中的许多服务器和工作站上，它们可能已被授予管理员级别的权限，这使他们能够有效地与Active Directory 中的特权组的成员。 在大多数情况下，在 Windows 基础结构的广泛大量中被授予高级权限的帐户是服务帐户，因此应始终评估服务帐户的广度和深度。  

2.  "非常重要的人员（VIP）域帐户"。 在本文档的上下文中，VIP 帐户是指有权访问攻击者所需的信息的任何帐户（知识产权和其他敏感信息）或可用于向攻击者授予该信息访问权限的任何帐户。 这些用户帐户的示例包括：  

    1.  其帐户有权访问敏感企业信息的高级管理人员  

    2.  负责维护管理人员使用的计算机和应用程序的技术支持人员帐户  

    3.  有权访问组织的投标和合同文档的法律人员帐户，无论文档是用于其自己的组织还是客户组织  

    4.  有权访问公司开发管道中产品的计划和规范的产品规划人员，不管公司所做的产品类型如何  

    5.  其帐户用于访问调查数据、产品表达式或对攻击者感兴趣的任何其他调查的研究人员  

由于 Active Directory 中的高特权帐户可用于传播泄露，并可以操作 VIP 帐户或它们可以访问的数据，因此凭据窃取攻击最有用的帐户是企业管理员成员的帐户，Active Directory 中的 "域管理员" 和 "管理员" 组。  

由于域控制器是 AD DS 数据库的存储库，并且域控制器对 Active Directory 中的所有数据都具有完全访问权限，因此，无论是并行处理凭据盗窃攻击还是在一个或多个高度特权的 Active Directory 帐户已泄露。 尽管许多发布（和许多攻击者）都在描述传递哈希和其他凭据盗窃攻击（如[降低 Active Directory 攻击面](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)）时专注于域管理员组成员身份，但此处列出的任何组的成员均可用于破坏整个 AD DS 安装。  

> [!NOTE]  
> 有关传递哈希和其他凭据被盗攻击的全面信息，请参阅 PTH-1Appendix M @no__t 中列出的[缓解哈希传递（）攻击和其他凭据盗窃技术](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf)白皮书：文档链接和建议阅读 @ no__t-0。 有关通过确定攻击者（有时称为 "高级持久性威胁" （Apt））的攻击的详细信息，请参阅确定的[攻击者和目标攻击](https://www.microsoft.com/download/details.aspx?id=34793)。  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>增加危害可能性的活动  
由于凭据被盗的目标通常是高度特权的域帐户和 VIP 帐户，因此管理员一定要了解提高凭据盗窃攻击成功的可能性。 尽管攻击者还以 VIP 帐户为目标，但是，如果在系统或域中没有为 Vip 提供高级别的权限，则他们的凭据被盗需要其他类型的攻击，如社交工程来提供机密信息。 或者，攻击者必须首先获取缓存了 VIP 凭据的系统的特权访问权限。 因此，增加此处所述凭据被盗的可能性的活动主要是为了防止获取高度特权的管理凭据。 这些活动是常见的机制，攻击者可以利用这些机制来损害系统，以获取特权凭据。  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>登录到具有特权帐户的不安全计算机  
允许凭据窃取攻击成功的核心漏洞是登录到计算机的操作，这些计算机不是安全的，并且在整个环境中具有广泛的权限。 这些登录可能是此处所述的各种错误配置的结果。  

#### <a name="not-maintaining-separate-administrative-credentials"></a>不维护单独的管理凭据  
虽然这比较罕见，但在评估各种 AD DS 安装时，我们发现 IT 员工使用单个帐户来完成所有工作。 此帐户是 Active Directory 中的至少一个最特权组的成员，并且是员工在早上登录到其工作站所使用的帐户，请检查其电子邮件、浏览 Internet 站点，并将内容下载到计算机. 当用户运行的帐户被授予了本地管理员权限时，它们会公开本地计算机以完成安全。 如果这些帐户也是 Active Directory 中大多数特权组的成员，则它们将公开整个林以进行破坏，使攻击者可以轻松地获得对 Active Directory 和 Windows 环境的完全控制。  

同样，在某些环境中，我们发现，在 Windows 环境中，在非 Windows 计算机上使用的是相同的用户名和密码，这使得攻击者能够从 UNIX 或 Linux 系统扩展到 Windows 系统。反之亦然。

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>具有特权帐户的受攻击工作站或成员服务器的登录  
当使用高度特权的域帐户以交互方式登录到遭到入侵的工作站或成员服务器时，该计算机遭受攻击可能会从登录系统的任何帐户获取凭据。  

#### <a name="unsecured-administrative-workstations"></a>不安全的管理工作站  
在许多组织中，IT 人员使用多个帐户。 一个帐户用于登录到员工的工作站，因为这些是 IT 人员，他们通常在其工作站上具有本地管理员权限。 在某些情况下，UAC 处于启用状态，以便用户在登录时至少收到一个拆分访问令牌，并且必须在需要权限时提升。 当这些用户执行维护活动时，他们通常使用本地安装的管理工具并通过选择 "以**管理员身份运行**" 选项或通过提供在出现提示时提供凭据。 尽管此配置可能适用，但它会公开环境，因为：  

-   员工用于登录到其工作站的 "常规" 用户帐户具有本地管理员权限，这种[情况](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx)下，该计算机容易受到攻击，导致用户相信要安装恶意软件。  

-   恶意软件安装在管理帐户的上下文中，现在可以使用计算机来捕获击键、剪贴板内容、屏幕快照和内存驻留凭据，其中任何一个都可能导致公开强大域的凭据账号.  

此方案中的问题是双重问题。 首先，尽管单独的帐户用于本地和域管理，但计算机不安全，并且不保护帐户免遭盗窃。 其次，普通用户帐户和管理帐户被授予过多的权限。  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>使用高特权帐户浏览 Internet  
使用计算机上的本地 Administrators 组成员的帐户登录计算机的用户，或 Active Directory 中特权组的成员，然后浏览 Internet （或受攻击的 intranet）的用户将公开本地计算机和要泄露的目录。  

通过使用管理权限运行的浏览器访问恶意制作的网站，攻击者可以在特权用户的上下文中将恶意代码放在本地计算机上。 如果用户在计算机上具有本地管理员权限，攻击者可能会欺骗用户下载恶意代码或打开利用应用程序漏洞的电子邮件附件，并利用用户的权限提取本地缓存的计算机上所有活动用户的凭据。 如果用户在 Active Directory 的 "企业管理员"、"域管理员" 或 "管理员" 组的成员身份中具有管理权限，则攻击者可以提取域凭据，并使用它们来损害整个 AD DS 域或林。无需破坏林中的任何其他计算机。  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>跨系统配置具有相同凭据的本地特权帐户  
在多台计算机或所有计算机上配置相同的本地管理员帐户名和密码，可以在一台计算机上从 SAM 数据库中盗取凭据，以使使用相同凭据的所有其他计算机泄露。 至少应在每个已加入域的系统中为本地管理员帐户使用不同的密码。 本地管理员帐户还可以唯一命名，但对每个系统的特权本地帐户使用不同的密码足以确保凭据无法在其他系统上使用。  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation 和滥用特权域组  
向域中的 EA、DA 或 BA 组授予成员身份会为攻击者创建目标。 这些组中的成员越多，特权用户可能会无意间滥用凭据并向其公开凭据被盗攻击的可能性就越大。 具有特权的域用户登录到的每个工作站或服务器都提供一种可能的机制，通过该机制，特权用户的凭据可能会被搜集并用于危害 AD DS 域和林。  

### <a name="poorly-secured-domain-controllers"></a>安全可靠的域控制器  
域控制器承载域的 AD DS 数据库的副本。 如果是只读域控制器，则数据库的本地副本只包含目录中帐户子集的凭据，默认情况下不包含任何权限域帐户。 在读写域控制器上，每个域控制器都维护 AD DS 数据库的完整副本，其中不仅包括域管理员之类的特权用户的凭据，而且还包括诸如域控制器帐户或域的 Krbtgt 这样的特权帐户。帐户，这是与域控制器上的 KDC 服务关联的帐户。 如果域控制器上安装了域控制器功能不需要的其他应用程序，或者如果域控制器未进行得到修补和保护，攻击者可能会通过未修补的漏洞或可以利用其他攻击媒介直接安装恶意软件。  

## <a name="privilege-elevation-and-propagation"></a>权限提升和传播  
无论使用何种攻击方法，在 Windows 环境受到攻击时，Active Directory 始终是目标，因为它最终控制对攻击者所需的任何访问。 但这并不意味着整个目录的目标是。 特定帐户、服务器和基础结构组件通常是攻击的主要目标 Active Directory。 这些帐户的描述如下所示。  

### <a name="permanent-privileged-accounts"></a>永久特权帐户  
由于引入了 Active Directory，因此可以使用高特权帐户来构建 Active Directory 林，然后将执行日常管理所需的权限委派给不太特权的帐户。 Active Directory 中的 Enterprise Admins、Domain Admins 或 Administrators 组中的成员身份仅在实现了日常管理的最低特权方法的环境中才是必需的。  

永久特权帐户是指已放入特权组中的帐户，并留在每天。 如果你的组织将五个帐户放入域的域管理员组，则这五个帐户可以是每周7天、每天24小时。 但是，通常只需要使用具有域管理员权限的帐户才能进行特定的全域性配置。  

### <a name="vip-accounts"></a>VIP 帐户  
Active Directory 违例中经常被忽略的目标是组织中 "非常重要的人员" （或 Vip）的帐户。 特权帐户的目标是因为这些帐户可以向攻击者授予访问权限，这使他们能够破坏甚至破坏目标系统，如本部分前面所述。  

### <a name="privilege-attached-active-directory-accounts"></a>"特权附加" Active Directory 帐户  
"特权附加" Active Directory 帐户是指尚未成为 Active Directory 中具有最高级别的权限的任何组的成员，但在多个服务器上授予了高级别权限环境中的工作站。 这些帐户通常是基于域的帐户，这些帐户配置为在已加入域的系统上运行服务，通常适用于在基础结构的大型分区上运行的应用程序。 尽管这些帐户在 Active Directory 中没有特权，但如果他们在大量系统上被授予了高权限，则可以使用它们来损害甚至销毁基础结构的大段，实现与泄露特权 Active Directory 帐户。  

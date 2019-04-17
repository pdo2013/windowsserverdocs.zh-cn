---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: "吸引帐户凭据盗用"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae1bfe017e8b21c3abfcdc137153b0fd379053fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="attractive-accounts-for-credential-theft"></a>吸引帐户凭据盗用

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

凭据盗窃攻击那些攻击最初获得最高权限 （根、 管理员或系统，具体取决于正在使用的操作系统） 在网络，然后选择工具从其他登录帐户的会话中提取凭据免费提供使用计算机的访问。 根据的系统配置，这些凭据可以将其解压缩哈希、 门票或密码甚至纯文本的形式。 如果任何已获取凭据的本地帐户，可能存在其他计算机上 （例如，管理员帐户，在 Windows 中，或者根帐户 OSX、 UNIX 或 Linux） 该网络，攻击者提供的凭据到其他计算机上传播到其他计算机的危害，并尝试以获取的两个特定类型的帐户凭据，该网络:  

1.  特权的域帐户，以广泛和深度权限 （，即在多台计算机和 Active Directory 中有管理员级别权限的帐户）。 这些帐户可能的 Active Directory，在最高权限组中的任何成员，但它们可能已被授予管理员级别权限跨多服务器和域或使它们在 Active Directory 有效地一样功能强大的权限的组成员的森林工作站。 在大多数情况下，已在主要负责 Windows 基础结构授予权限的高级别帐户是服务帐户、，因此服务帐户始终应该广泛和特权提升深度评估。  

2.  "非常重要的人"(VIP) 域帐户。 在本文档中的上下文，VIP 帐户是有权访问攻击想 （知识财产和其他重要信息） 的信息的任何帐户或用于授予攻击访问权限，该信息的任何帐户。 这些用户帐户的示例包括：  

    1.  主管其帐户有权访问敏感公司的信息  

    2.  技术支持人员，负责维护的计算机和应用程序使用的主管他们的帐户  

    3.  法律的人员是否有权访问组织叫合同文档他们的帐户的文档是自己组织或企业客户端  

    4.  无论为不同类型的产品公司进行产品规划有权访问套餐并规范产品在公司的开发人员管道  

    5.  使用帐户的访问产品配方或任何其他研究感兴趣的攻击研究数据研究人员  

因为传播危害和 VIP 帐户或的数据，他们都可以访问的操作，可以使用 Active Directory 中的高权限的帐户，最有用的凭据盗窃攻击帐户将是企业管理员，域管理员管理员 Active Directory 中的组成员的帐户。  

因为域控制器广告 DS 数据库存储库维护，并且域控制器具有 Active Directory 中的完全访问权限的所有数据，域控制器是还面向受损，是否并排使用凭据盗窃攻击，或在一个或多个高权限 Active Directory 后帐户已受到威胁。 虽然发布了大量 （和许多攻击者） 重点关注域管理员组成员描述 pass 希代码以及其他凭据盗窃攻击时 (如中所述[减少 Active Directory 攻击 Surface](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md))，任何在此处列出的组成员的帐户可用于损害整个广告 DS 安装。  

> [!NOTE]  
> 有关完整 pass 希代码以及其他凭据盗窃攻击，请参阅[Mitigating Pass--哈希 (PTH) 攻击和其他凭据盗窃技术](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf)白皮书中列出[附录 m： 文档链接和建议阅读](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)。 有关确定对手通过攻击的详细信息，其中有时称为"高级永久性威胁"(APTs)，请参阅[确定对手和定向攻击](https://www.microsoft.com/download/details.aspx?id=34793)。  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>增加损害的可能性的活动  
由于目标的凭据被盗通常是高权限的域帐户和 VIP 帐户、 务必管理员意识增加成功的凭据被盗攻击的可能性的活动。 尽管还攻击者可 VIP 帐户，如果 VIPs 不具备高的特权系统上或在域中，盗取其凭据将需要其他类型的社交工程 VIP 机密信息来提供如的攻击。 或者攻击者必须首先获取对系统缓存的 VIP 凭据特权的访问。 出于此原因，增加凭据被盗此处所述的可能性的活动被重点主要阻止的高权限管理的凭据获取。 这些活动是攻击者的能够危害系统以获取特权的凭据常见机制。  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>在登录到特权帐户的计算机不安全  
允许凭据盗窃攻击成功核心漏洞是登录到不使用广泛有的帐户安全和深特权整个环境计算机的操作。 这些登录可以此处所述的各种错误配置的结果。  

#### <a name="not-maintaining-separate-administrative-credentials"></a>未保持单独管理凭据  
虽然这评估各种广告 DS 安装相当少见，但我们发现了 IT 员工使用一个帐户的所有工作。 帐户的至少一个中 Active Directory 的最高权限的组成员并且员工使用早上，登录到其工作站查看他们的电子邮件的同一帐户、 浏览 Internet 站点和内容下载到他们的计算机。 当用户运行时的本地管理员权限授予的帐户时，它们公开完成危害在本地计算机。 这些帐户的同时也是最特权组 Active Directory 中的成员，当他们公开整个林危害，进行易如反掌攻击者可获得完全控制 Active Directory 和 Windows 环境。  

同样，在一些环境中，我们已发现的相同用户名和密码用于根非 Windows 的计算机上的帐户作为 Windows 环境，使得攻击者从 UNIX 或 Linux 的 Windows 系统的想法，反之亦然系统延长危害使用。

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>登录到受到威胁的工作站或通过特权帐户成员服务器  
当交互式登录到受到威胁的工作站或成员服务器用于高权限的域帐户时，该受到威胁的计算机可能收集并从登录到系统的任何帐户的凭据。  

#### <a name="unsecured-administrative-workstations"></a>不安全管理工作站  
在许多组织 IT 人员使用多个帐户。 一个帐户用于登录到该员工工作站，如下所示 IT 人员，因为它们通常都本地管理员权限在他们的工作站上。 在某些情况下，将 UAC 保持启用，以便用户在至少接收拆分访问标记登录时，并且必须提升时所需的权限。 在这些用户执行维护活动时，他们通常使用本地安装的管理工具并提供其域权限的帐户，方法是依次选择的凭据**以管理员身份运行**选项或顺便提供在出现提示时的凭据。 尽管此配置听起来可能很相应，它会公开环境侵扰，因为：  

-   员工用于登录到他们的工作站上的"常规"用户帐户已的本地管理员权限、 计算机受到[驱动器通过下载](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx)确信用户安装恶意软件的攻击。  

-   恶意软件安装在管理的帐户的上下文，现在可以使用计算机捕获按键、 剪贴板上的内容、 屏幕截图和内存常驻凭据，其中任一可能会导致强大域帐户凭据泄露。  

在此情况下的问题将在两方面。 首先，尽管本地和域管理使用了不同的帐户，计算机为不安全，并且不能保护针对被盗帐户。 其次，普通用户帐户和管理的帐户已被授予过多的权限。  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>浏览 Internet 高权限帐户  
用户在登录到的计算机上本地成员或特权 Active Directory，以及其他人中的组成员的帐户的计算机，然后浏览 Internet （或受到威胁的 intranet） 暴露在本地计算机和危害的目录。  

使用具有管理员权限的运行的浏览器中访问恶意的网站可以允许特权用户上下文中本地计算机上存入恶意代码的攻击。 如果用户可以在计算机上的本地管理员权限，攻击者可能欺骗用户下载恶意的代码或打开电子邮件附件利用的漏洞应用程序，利用用户的权限，若要解压缩本地缓存的计算机上的所有活动用户的凭据。 如果用户的企业管理员、 域管理或管理员 Active Directory 中的组成员资格目录中具有管理员权限，可以解压缩域凭据攻击者，并使用危害整个广告 DS 域或森林，而无需危害森林中的其他计算机。  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>使用相同的凭据跨系统配置本地特权的帐户  
从一台计算机以用于危害使用相同的凭据的所有其他计算机上三千数据库被盗许多或所有计算机启用凭据配置相同的本地管理员帐户名称和密码。 至少，你应在每个已加入域的系统的本地管理员帐户使用不同的密码。 此外可能唯一命名本地管理员帐户，但针对每个系统特权本地帐户使用不同的密码是不足以确保凭据，不能用于其他系统上。  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation 和特权的域组滥用  
攻击者为授予某个域中 EA、 DA 或栏组中的成员创建的目标。 更多的这些组中，更高版本的可能性权限的用户可能会无意中滥用凭据和公开他们凭据盗窃攻击的成员。 每个工作站或的特权的域用户登录的服务器提供可能的机制特权的用户的凭据可获取并使用危害的广告 DS 域和森林。  

### <a name="poorly-secured-domain-controllers"></a>这么不安全的域控制器  
域控制器存放某个域广告 DS 数据库的副本。 仅阅读域控制器在本地数据库副本包含指针子集目录中，都不是默认特权的域帐户的帐户的凭据。 在读写域控制器上每个域控制器维护广告 DS 数据库中，包括不仅特权用户等域管理员凭据完全副本，但如域控制器帐户或的域 Krbtgt 帐户，该帐户关联的 kdc 特权的帐户域控制器上的服务。 如果上域控制器安装了其他应用程序不必要的功能，域控制器，或域控制器未严格的修补程序并安全，攻击者可能未安装修补程序的安全漏洞，通过危害它们或他们可能会利用其他直接在其上安装的恶意软件的攻击方法。  

## <a name="privilege-elevation-and-propagation"></a>特权提升和传播  
无论为使用攻击方法，Active Directory 始终旨在当 Windows 环境被攻击，因为它最终控制访问权限到任何攻击者。 这并不意味着针对整个目录，但是。 特定的帐户、 服务器和基础结构组件通常是攻击 Active Directory 主要目标。 这些帐户的说明，如下所示。  

### <a name="permanent-privileged-accounts"></a>永久性特权的帐户  
因为引入 Active Directory 后的，它已被可能到版本 Active Directory 森林高度特权使用帐户，然后委托的权利和执行日常管理不太权限帐户到所需的权限。 企业管理员、 域管理或管理员组 Active Directory 中的成员资格，才能仅暂时和很少实现最低权限管理日常方法的环境中。  

永久性特权的帐户都已放置在特权组和左有一天的帐户。 如果你的组织放入某个域管理员组的五个帐户，这些五个帐户可能会有针对性的 24 小时一周的七天的一天。 但是，利用域管理员权限的帐户的实际需要通常是只对特定整个域配置，和段较短的时间。  

### <a name="vip-accounts"></a>VIP 帐户  
Active Directory 违规中经常被忽视的目标是某个组织中的"非常重要的人"（或 VIPs） 的帐户。 因为这些帐户可以授予访问攻击，允许他们危害或甚至销毁有针对性的系统，述早些时候在此部分中进行针对性投放特权的帐户。  

### <a name="privilege-attached-active-directory-accounts"></a>"特权附加"Active Directory 帐户  
"特权附加"Active Directory 帐户都是权限的不所做的任何有特权在 Active Directory 的最高级别但而被授予高的许多服务器和环境中的工作站上的组成员域帐户。 这些帐户会配置为通常运行的应用程序的基础结构大部分上已加入域的系统上运行的服务通常是基于域的大多数帐户。 尽管这些帐户 Active Directory，在有没有权限，如果他们会被授予高特权大量的系统上，他们可以使用危害或甚至销毁大段基础结构，实现特权 Active Directory 帐户危害相同的效果。  

---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: 保护域控制器免受攻击
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b91164e14f3ae94f6f7d01c62125f45124df0907
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367619"
---
# <a name="securing-domain-controllers-against-attack"></a>保护域控制器免受攻击

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

@no__t 0Law 第三号：如果攻击者对您的计算机具有不受限制的物理访问权限，则不是您的计算机。 *@no__t[10 个安全永恒定律（版本2.0）](https://technet.microsoft.com/security/hh278941.aspx)  
  
域控制器提供了 AD DS 数据库的物理存储，还提供了允许企业有效管理其服务器、工作站、用户和应用程序的服务和数据。 如果恶意用户获取了对域控制器的特权访问，则该用户可以修改、损坏或销毁 AD DS 数据库，并通过扩展 Active Directory 管理的所有系统和帐户。  
  
由于域控制器可以读取和写入 AD DS 数据库中的任何内容，因此危及域控制器的安全意味着 Active Directory 林永远不会被视为可信的，除非你能够使用已知的良好备份进行恢复并关闭进程中的漏洞。  
  
在几分钟到几小时内（而不是几天或几周），可能会在几分钟内完成对 AD DS 数据库的修改，甚至会导致对数据库的修改甚至无法修复。 重要的是，攻击者有权访问 Active Directory 的时间，但攻击者在获取特权访问权限时计划了多少时间。 损害域控制器可以提供访问的大规模传播的最有利路径，或成员服务器、工作站和 Active Directory 的最直接的路径。 因此，域控制器应单独进行保护，更得到于常规 Windows 基础结构。  

## <a name="physical-security-for-domain-controllers"></a>域控制器的物理安全性

本部分提供有关物理保护域控制器的信息，无论域控制器是物理计算机还是虚拟机，在数据中心位置，分支机构，甚至只包含基本基础结构控制的远程位置。  
  
### <a name="datacenter-domain-controllers"></a>Datacenter 域控制器  
  
#### <a name="physical-domain-controllers"></a>物理域控制器

在数据中心，物理域控制器应安装在独立于常规服务器总体的专用安全机架或 cages 中。 如果可能，应将域控制器配置为受信任的平台模块（TPM）芯片，并通过 BitLocker 驱动器加密保护域控制器服务器中的所有卷。 BitLocker 通常会在一位数字的百分比中增加性能开销，但即使从服务器中删除了磁盘，也会防止目录泄露。 BitLocker 还可以帮助系统防范攻击，如 rootkit，因为修改启动文件将导致服务器启动到恢复模式，以便可以加载原始二进制文件。 如果将域控制器配置为使用软件 RAID、串行连接 SCSI、SAN/NAS 存储或动态卷，则无法实现 BitLocker，因此，在使用本地连接的存储（有或没有硬件 RAID）时，应在域控制器中使用可以.  
  
#### <a name="virtual-domain-controllers"></a>虚拟域控制器 

如果实现虚拟域控制器，应确保域控制器与环境中的其他虚拟机在不同的物理主机上运行。 即使使用第三方虚拟化平台，也请考虑在 Windows Server 2012 或 Windows Server 2008 R2 中的 Hyper-v Server 上部署虚拟域控制器，这将提供最小的攻击面并可使用其托管的域控制器进行管理而不是与其他虚拟化主机一起进行管理。 如果实现了用于管理虚拟化基础结构的 System Center Virtual Machine Manager （SCVMM），则可以委派域控制器虚拟机所在的物理主机和域控制器的管理本身。 还应考虑分离虚拟域控制器的存储，以防止存储管理员访问虚拟机文件。  
  
### <a name="branch-locations"></a>分支位置  
  
#### <a name="physical-domain-controllers-in-branches"></a>分支中的物理域控制器

在多个服务器所在的位置，但未以物理方式保护数据中心服务器的安全程度，物理域控制器应配置 TPM 芯片，并为所有服务器卷 BitLocker 驱动器加密。 如果域控制器无法存储在分支机构的锁定聊天室中，则应考虑在这些位置部署 Rodc。  
  
#### <a name="virtual-domain-controllers-in-branches"></a>分支中的虚拟域控制器

如果可能，应在不同物理主机上的分支机构中运行虚拟域控制器，而不是在站点中的其他虚拟机上运行。 在虚拟域控制器不能在独立于虚拟服务器总体的物理主机上运行的分支机构中，你应在至少运行虚拟域控制器的主机上实施 TPM 芯片和 BitLocker 驱动器加密，并所有主机（如果可能）。 你应考虑在分支机构中部署 Rodc，具体取决于分支机构的大小和物理主机的安全性。  
  
### <a name="remote-locations-with-limited-space-and-security"></a>空间和安全性有限的远程位置

如果你的基础结构包含只能安装单个物理服务器的位置，则应在远程位置安装能够运行虚拟化工作负载的服务器，并将 BitLocker 驱动器加密配置为保护所有服务器中的卷。 服务器上的一个虚拟机应运行 RODC，而其他服务器则作为主机上的独立虚拟机运行。 [只读域控制器规划和部署指南](https://go.microsoft.com/fwlink/?LinkID=135993)中提供了有关部署 RODC 的规划的信息。 有关部署和保护虚拟化域控制器的详细信息，请参阅 TechNet 网站上的[在 hyper-v 中运行域控制器](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx)。 有关强化 Hyper-v、委派虚拟机管理和保护虚拟机的更多详细指南，请参阅 Microsoft 网站上的[Hyper-v 安全指南](https://www.microsoft.com/download/details.aspx?id=16650)解决方案加速器。  
  
## <a name="domain-controller-operating-systems"></a>域控制器操作系统

你应在你的组织中支持的最新版本的 Windows Server 上运行所有域控制器，并确定域控制器填充中旧版操作系统的取消授权。 通过使域控制器保持最新并删除旧的域控制器，你通常可以利用在域或林中可能不可用的新功能和安全性，域控制器运行旧版操作系统。 应全新安装并升级域控制器，而不是从以前的操作系统或服务器角色升级：也就是说，不会执行域控制器的就地升级，也不能在未安装操作系统的服务器上运行 AD DS 安装向导。 通过实施全新安装的域控制器，你可以确保旧的文件和设置不会无意中留给域控制器，并且你可以简化一致的安全域控制器配置的强制实施。  
  
## <a name="secure-configuration-of-domain-controllers"></a>保护域控制器的配置

许多免费使用的工具（默认情况下安装在 Windows 中）可用于创建域控制器的初始安全配置基线，这些基线随后可由 Gpo 强制执行。 此处描述了这些工具。  
  
### <a name="security-configuration-wizard"></a>安全配置向导  

在初始生成时，所有域控制器都应被锁定。 这可以通过使用 Windows Server 中本机附带的安全配置向导来在 "基本生成" 域控制器上配置服务、注册表、系统和 WFAS 设置。 可以将设置保存并导出到 GPO 中，该 GPO 可以链接到林中每个域中的域控制器 OU，以强制域控制器的配置一致。 如果你的域包含多个版本的 Windows 操作系统，则可以将 Windows Management Instrumentation （WMI）筛选器配置为仅将 Gpo 应用于运行相应操作系统版本的域控制器。  
  
### <a name="microsoft-security-compliance-toolkit"></a>Microsoft 安全合规性工具包

[Microsoft 安全符合性工具包](https://www.microsoft.com/download/details.aspx?id=55319)域控制器设置可以与 "安全配置向导" 设置结合使用，以便为部署在Active Directory 中的域控制器 OU。  
  
### <a name="rdp-restrictions"></a>RDP 限制

链接到林中所有域控制器 Ou 的组策略对象应配置为仅允许来自经过授权的用户和系统（例如，跳转服务器）的 RDP 连接。 这可以通过组合用户权限设置和 WFAS 配置来实现，并且应在 Gpo 中实现，以便一致地应用策略。 如果它被绕过，则下一个组策略刷新会将系统恢复为其正确的配置。  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>域控制器的修补和配置管理

尽管可能看起来不够直观，但你应考虑从常规 Windows 基础结构中单独修补域控制器和其他关键的基础结构组件。 如果你将企业配置管理软件用于基础结构中的所有计算机，则可以使用系统管理软件的危害来损害或破坏该软件管理的所有基础结构组件。 通过将域控制器的修补和系统管理与常规总体分开，你可以减少安装在域控制器上的软件的数量，并严格控制其管理。
  
### <a name="blocking-internet-access-for-domain-controllers"></a>阻止域控制器的 Internet 访问  

作为 Active Directory 安全评估的一部分执行的一项检查是在域控制器上使用和配置 Internet Explorer。 Internet Explorer （或任何其他 web 浏览器）不应在域控制器上使用，但对数千个域控制器的分析已揭示了很多情况下，特权用户使用 Internet Explorer 浏览组织的 intranet 或互联.  
  
如前面所述，该[途径](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)的 "配置错误" 一节中所述，使用高特权帐户从 Windows 基础结构中最强大的计算机中浏览 Internet （或受感染的 intranet）。默认情况下，允许本地登录域控制器的帐户会给组织的安全带来特别的风险。 无论是通过下载还是下载恶意软件感染的 "实用程序" 通过驱动器，攻击者都可以获得完全破坏或销毁 Active Directory 环境所需的所有内容。  
  
尽管 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 和当前版本的 Internet Explorer 提供了许多针对恶意下载的保护，但在大多数情况下，使用域控制器和特权帐户来浏览 Internet，域控制器运行的是 Windows Server 2003，或者是特意禁用了更高版本的操作系统和浏览器提供的保护。  
  
在域控制器上启动 web 浏览器应禁止策略，但不能通过技术控制来禁止，而且不应允许域控制器访问 Internet。 如果域控制器需要跨站点进行复制，则应该在站点之间实施安全连接。 虽然详细的配置说明不在本文档的讨论范围内，但你可以实现许多控制措施来限制域控制器被滥用或配置错误并随后泄露的能力。  
  
### <a name="perimeter-firewall-restrictions"></a>外围防火墙限制

应将外围防火墙配置为阻止来自域控制器的出站连接到 Internet。 尽管域控制器可能需要在站点边界之间进行通信，但可以根据[如何为域和信任关系配置防火墙](https://support.microsoft.com/kb/179442)中提供的指导原则，将外围防火墙配置为允许站点间通信。Microsoft 支持部门网站。  
  
### <a name="dc-firewall-configurations"></a>DC 防火墙配置  

如前文所述，你应使用安全配置向导捕获域控制器上具有高级安全性的 Windows 防火墙的配置设置。 应查看安全配置向导的输出，以确保防火墙配置设置满足组织的要求，然后使用 Gpo 来强制执行配置设置。  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>阻止从域控制器进行 Web 浏览

可以结合使用 AppLocker 配置、"黑洞" 代理配置和 WFAS 配置来防止域控制器访问 Internet 和阻止在域控制器上使用 web 浏览器。

---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: 保护域控制器免受攻击
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6be2899e85b68578518d9de1805c287367608163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863088"
---
# <a name="securing-domain-controllers-against-attack"></a>保护域控制器免受攻击

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

*定律第三步：如果攻击者对您的计算机具有无限制物理访问，它就不再您的计算机了。* - [十个永恒定律的安全 （版本 2.0）](https://technet.microsoft.com/security/hh278941.aspx)  
  
域控制器为 AD DS 数据库，此外还提供服务和数据，可让企业能够有效地管理其服务器、 工作站、 用户和应用程序提供的物理存储。 如果恶意用户获得特权的访问域控制器，则该用户可以修改、 已损坏，或破坏的 AD DS 数据库和通过扩展，所有的系统和管理的 Active Directory 的帐户。  
  
泄漏的域控制器的域控制器可以读取和写入到 AD DS 数据库中的任何内容，因为意味着的 Active Directory 林可以永远不会被认为是高信度再次除非能够使用已知完好的备份进行恢复和关闭进程中允许这种损害的间隙。  
  
具体取决于攻击者的准备、 工具和技能，修改或甚至无法挽回损失的 AD DS 数据库可以为小时、 不天或数周几分钟内完成。 问题不是什么时间攻击者具有特权访问 Active Directory，但获得多少，攻击者已计划于特权访问时的时刻。 影响的域控制器可以提供大规模的访问权限，传播的最快速路径或析构的成员服务器、 工作站和 Active Directory 与最直接的路径。 因此，域控制器应单独和比常规 Windows 基础结构更严格地保护。  

## <a name="physical-security-for-domain-controllers"></a>域控制器的物理安全性

本部分提供有关以物理方式保护域控制器、 域控制器是否是物理或虚拟机，数据中心位置、 分支机构和与仅基本基础结构控件甚至远程位置中的信息。  
  
### <a name="datacenter-domain-controllers"></a>数据中心域控制器  
  
#### <a name="physical-domain-controllers"></a>物理域控制器

在数据中心，物理域控制器都应安装在专用的安全机架或独立于常规服务器填充 cages。 如果可能，应使用受信任的平台模块 (TPM) 芯片配置域控制器和域控制器服务器中的所有卷应通过 BitLocker 驱动器加密都保护。 BitLocker 通常会增加性能开销在个位数的百分比，但保护免受威胁的目录，即使从服务器中删除磁盘。 BitLocker 还有助于保护系统免受攻击，例如 rootkit，因为启动文件的修改将导致服务器启动进入恢复模式，以便可以加载原始二进制文件。 如果域控制器配置为使用软件 RAID、 串行连接 SCSI SAN/NAS 存储或动态卷，BitLocker 不能实现，因此本地附加的存储 （有或无需任何硬件 RAID） 应在域控制器中使用时可能。  
  
#### <a name="virtual-domain-controllers"></a>虚拟域控制器 

如果你实现虚拟域控制器，应确保域控制器在环境中的其他虚拟机比单独的物理主机上运行。 即使使用第三方虚拟化平台，请考虑部署在 Windows Server 2012 或 Windows Server 2008 R2，它提供了一个小的受攻击面，并且可与它托管的域控制器管理的 HYPER-V 服务器上的虚拟域控制器而不是管理与虚拟化主机的其余部分。 如果为虚拟化基础结构管理实现 System Center Virtual Machine Manager (SCVMM)，可以委派的域控制器虚拟机驻留和域控制器上的物理主机的管理本身经过授权的管理员。 此外应考虑将分离的虚拟域控制器来防止存储管理员访问虚拟机文件存储。  
  
### <a name="branch-locations"></a>分支机构位置  
  
#### <a name="physical-domain-controllers-in-branches"></a>在分支中的物理域控制器

在多个服务器驻留，但不是保证物理安全程度上受保护数据中心服务器的位置，物理域控制器应使用 TPM 芯片和 BitLocker 驱动器加密为配置服务器的所有卷。 如果域控制器不能存储在分支机构中加锁的机房内，则应考虑部署这些位置中的 Rodc。  
  
#### <a name="virtual-domain-controllers-in-branches"></a>在分支中的虚拟域控制器

只要有可能，应在与站点中的其他虚拟机的单独物理主机上分支机构中运行的虚拟域控制器。 在分支机构虚拟域控制器的虚拟服务器填充其余部分的单独物理主机不能运行，则应实现 TPM 芯片和 BitLocker 驱动器加密的虚拟域控制器最小值，在其运行的主机上和如有可能的所有主机。 具体取决于分支机构的大小和物理主机的安全性，应考虑部署 Rodc 分支机构中。  
  
### <a name="remote-locations-with-limited-space-and-security"></a>具有有限的空间和安全的远程位置

如果您的基础结构包括可以在其中安装一台物理服务器的位置，能够运行虚拟化工作负载的服务器应安装在远程位置，并应配置 BitLocker 驱动器加密来保护所有在服务器中的卷。 在服务器上的一台虚拟机应运行 RODC，与作为单独的虚拟机在主机上运行的其他服务器。 有关规划中提供了部署 RODC 的信息[只读域控制器计划和部署指南](https://go.microsoft.com/fwlink/?LinkID=135993)。 有关部署和保护虚拟化的域控制器的详细信息，请参阅[的 HYPER-V 中运行域控制器](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx)TechNet 网站上。 有关更多详细指南用于强化的 HYPER-V，将虚拟机管理委派和保护的虚拟机，请参阅[HYPER-V 安全指南](https://www.microsoft.com/download/details.aspx?id=16650)Solution Accelerator，Microsoft 网站上。  
  
## <a name="domain-controller-operating-systems"></a>域控制器操作系统

应在最新版本的 Windows Server 的支持在组织内，并划分其优先级解除授权的域控制器填充内容中的旧版操作系统上运行的所有域控制器。 通过保留你的域控制器的当前和消除旧版域控制器，您通常可以充分利用新功能，并且可能不在提供的域或林具有运行旧版操作系统的域控制器的安全。 域控制器应该全新安装和升级而不是从早期版本的操作系统或服务器角色; 升级也就是说，不要执行就地升级的域控制器或在其操作系统未全新安装的服务器上运行 AD DS 安装向导。 通过实现全新安装的域控制器，可确保旧文件和设置不会无意中保留在域控制器上，并简化强制执行一致的、 安全的域控制器配置。  
  
## <a name="secure-configuration-of-domain-controllers"></a>域控制器的安全配置

许多免费工具，其中一些安装在 Windows 中默认情况下，可以用于创建域控制器，随后可通过 Gpo 强制实施初始安全配置基线。 此处介绍了这些工具。  
  
### <a name="security-configuration-wizard"></a>安全配置向导  

在初始生成后，应锁定的所有域控制器。 这可以实现使用在 Windows Server"基生成"域控制器上配置服务、 注册表、 系统和 WFAS 设置中本机随附的安全配置向导。 可以保存和导出到一个可以链接到每个林中的域中域控制器 OU，以强制实施的域控制器配置的 GPO 设置。 如果你的域包含多个版本的 Windows 操作系统，可以配置要 Gpo 仅应用于运行的操作系统的相应版本的域控制器的 Windows Management Instrumentation (WMI) 筛选器。  
  
### <a name="microsoft-security-compliance-toolkit"></a>Microsoft 安全遵从性工具包

[Microsoft 安全遵从性工具包](https://www.microsoft.com/download/details.aspx?id=55319)域控制器设置可以组合使用以生成全面的配置基线的部署和强制实施的 Gpo 的域控制器的安全配置向导设置部署在 Active Directory 中域控制器 OU。  
  
### <a name="rdp-restrictions"></a>RDP 限制

组策略对象链接到 Ou 在林中的所有域控制器的应配置为允许仅从经过授权的用户和系统 （例如，跳转服务器） 的 RDP 连接。 这可通过用户权限设置和 WFAS 配置的组合，以便该策略一致地应用应在 Gpo 中实现。 如果绕过下, 一次组策略刷新返回系统对其正确配置。  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>修补程序和域控制器的配置管理

尽管它可能看起来有悖常理，但应考虑修补域控制器和其他关键基础设施组件独立于常规的 Windows 基础结构。 如果您在您的基础结构中利用企业配置管理软件的所有计算机，可以使用的系统管理软件受到安全威胁损坏或破坏该软件管理的所有基础结构组件。 通过将域控制器的修补程序和系统管理与总体的分离，可以减少除了严密控制其管理的域控制器上安装的软件。
  
### <a name="blocking-internet-access-for-domain-controllers"></a>阻止 Internet 访问的域控制器  

作为 Active Directory 安全性评估的一部分执行的检查之一是使用和 Internet Explorer 的域控制器上的配置。 Internet Explorer （或任何其他 web 浏览器） 不应在域控制器上，但数千个域控制器的分析表明许多情况下，在其中的特权的用户使用 Internet Explorer 浏览组织的 intranet 或Internet。  
  
如前面所述的"错误配置"部分[攻击途径](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)，从一个使用高特权的 Windows 基础结构中最强大的计算机浏览 Internet （或受感染的 intranet）（这是仅允许本地登录到域控制器的默认帐户） 的帐户会带来特别到组织的安全风险。 无论是通过下载或下载恶意软件感染"实用程序"的驱动器，攻击者可以访问完全损坏或破坏 Active Directory 环境所需一切内容。  
  
尽管 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008 和 Internet Explorer 的当前版本提供了大量的保护，以应对恶意下载，多数情况下在哪个域控制器和特权的帐户已使用到浏览 Internet、 域控制器正在运行 Windows Server 2003 或更高版本的操作系统和浏览器所提供的保护有故意禁用。  
  
启动域控制器上的 web 浏览器应通过策略，不仅能通过技术控制禁止并且域控制器不应允许访问 Internet。 如果你的域控制器需要多个站点之间复制，应实现在站点之间的安全连接。 虽然本文档的讨论范围内的详细的配置说明，可以实现控件以限制滥用或配置错误并随后泄露的域控制器的功能的数。  
  
### <a name="perimeter-firewall-restrictions"></a>外围防火墙限制

应将外围防火墙配置为阻止从域控制器到 Internet 的出站连接。 尽管域控制器可能需要跨站点边界进行通信，但可以将外围防火墙配置为允许站点间通信中提供的准则[如何为域和信任关系配置防火墙](https://support.microsoft.com/kb/179442) Microsoft 支持网站上。  
  
### <a name="dc-firewall-configurations"></a>DC 防火墙配置  

如前文所述，应使用安全配置向导来捕获具有高级安全性的 Windows 防火墙的域控制器上的配置设置。 您应查看安全配置向导，以确保防火墙配置设置满足你组织的要求，然后使用 Gpo 强制实施配置设置的输出。  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>阻止从域控制器浏览网络

可以使用 AppLocker 配置、"黑洞"代理配置和 WFAS 配置的组合，以防止域控制器访问 Internet，并以防止域控制器上的 web 浏览器的使用。

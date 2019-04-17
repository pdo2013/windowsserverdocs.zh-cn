---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: "保护免受攻击域控制器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc40d0ac536b128b799006214e360fa4d991ee44
ms.sourcegitcommit: 06a84f5caeab49b8480d6eed037aebbb59c77a9f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2018
---
# <a name="securing-domain-controllers-against-attack"></a>保护免受攻击域控制器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

*法律第三步： 如果你的计算机攻击者已无限制物理的访问，它不是你的计算机不再。* - [十变法律规定，，安全 （2.0 版本）](https://technet.microsoft.com/security/hh278941.aspx)  
  
域控制器提供有关广告 DS 数据库中，除了提供数据，以允许企业实际上管理其服务器、 工作站、 用户，以及应用程序和服务物理存储。 如果特权的访问域控制器恶意用户获取，该用户可以修改、 已损坏、 或破坏广告 DS 数据库和的扩展的所有系统和由 Active Directory 的帐户。  
  
因为域控制器可以从读取和写入广告 DS 数据库中的所有内容，域控制器危害意味着的 Active Directory 森林可以永远不会被认为是可信再次除非你将无法恢复使用已知的正常备份并关闭了允许危害进程中的缺陷。  
  
根据攻击者的准备工具，，技能、 修改或广告 DS 甚至不能修复损坏可在几分钟时间、 不天或数周到完成数据库。 Matters 的未攻击有特权访问 Active Directory，但量攻击者已计划的访问权限时当前获取多长时间。 危及域控制器可以提供的访问权限，大规模传播到最合适的路径或最直接销毁成员服务器、 工作站和 Active Directory 的路径。 出于此原因，域控制器应单独和严格相比常规 Windows 基础结构的安全。  

  
## <a name="physical-security-for-domain-controllers"></a>对于域控制器物理安全  
此部分中提供了有关物理保护域控制器，域控制器是否物理或数据中心位置、 分支机构和甚至遥远与仅的基础结构控件中的虚拟机的信息。  
  
### <a name="datacenter-domain-controllers"></a>Datacenter 域控制器  
  
#### <a name="physical-domain-controllers"></a>物理域控制器  
在数据中心，应安装物理域控制器在专用安全机架或护罩从常规服务器人口不同的。 如果可能，应带受信任的平台模块 (TPM) 芯片配置域控制器和域控制器服务器上的所有卷应该都保护通过 BitLocker 驱动器加密。 BitLocker 通常增加以一位百分比头顶上飞过时的性能，但即使磁盘卸下服务器保护免受威胁目录。 BitLocker 还有助于保护 rootkit 等攻击的系统，因为的启动文件修改将导致该服务器以启动到恢复模式，这样，可加载原始二进制文件。 如果域控制器配置为使用软件 RAID 串行附加 SCSI，圣/NAS 存储或无法实现动态卷，BitLocker，因此本地连接的存储 （和不带硬件 RAID） 应使用域控制器尽可能中。  
  
#### <a name="virtual-domain-controllers"></a>虚拟域控制器  
如果你实现虚拟域控制器，你应确保在单独的物理主机比环境中的其他虚拟机上运行域控制器。 即使您使用第三方虚拟化平台，考虑部署 Windows Server 2012 或 Windows Server 2008 R2，它可以提供最少攻击 surface 中的 HYPER-V 服务器上的虚拟域控制器，并且可以与它承载而不是管理虚拟化主机的其余部分的域控制器进行管理。 如果你对您的虚拟化基础结构进行管理实现系统中心虚拟机管理器 (SCVMM)，可以委派物理主机上的域控制器虚拟机驻留和域控制器自行授权管理员进行管理。 你还应该考虑虚拟域控制器，以便防止存储管理员访问虚拟机文件存储的分隔。  
  
### <a name="branch-locations"></a>位置 branch  
  
#### <a name="physical-domain-controllers"></a>物理域控制器  
在多个服务器驻留，但不是物理安全 datacenter 服务器某种程度的位置，物理域控制器应配置 TPM 芯片和 BitLocker 驱动器加密所有服务器卷。 如果不能域控制器存储在锁定房间中的位置 branch 中，您应该考虑部署在这些位置 Rodc。  
  
#### <a name="virtual-domain-controllers"></a>虚拟域控制器  
尽可能，你应该在分支机构运行虚拟域控制器，该站点中的其他虚拟机比单独物理主机上。 在虚拟域控制器独立虚拟服务器人口的其余部分的物理主机无法运行的分支机构，，您应该实现 TPM 芯片和 BitLocker 驱动器加密虚拟域控制器至少，其运行的主机和尽可能的所有主机上。 根据我们的大小分支机构和物理主机安全，您应该考虑部署 Rodc 位置 branch 中。  
  
### <a name="remote-locations-with-limited-space-and-security"></a>空间和有限安全遥远  
如果您的基础结构包含在其中可以安装的单个物理服务器的位置，在远程的位置，应安装能够运行虚拟化工作负载的服务器和 BitLocker 驱动器加密应配置为服务器中所有的卷的保护。 在服务器上的一个虚拟机应运行 RODC，与其他主机上为不同的虚拟机运行的服务器。 有关计划部署 RODC 中提供的信息[只读域控制器计划和部署指南](https://go.microsoft.com/fwlink/?LinkID=135993)。 有关部署和保护措施虚拟化的域控制器的详细信息，请参阅[HYPER-V 中运行的域控制器](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx)TechNet 网站上。 有关详细强化 HYPER-V 指南，委派虚拟机管理和保护虚拟机，请参阅[HYPER-V 安全指南](https://www.microsoft.com/download/details.aspx?id=16650)Microsoft 网站上的解决方案加速器。  
  
## <a name="domain-controller-operating-systems"></a>域控制器操作系统  
在 Windows Server，在你的组织中均受支持，并且来优先考虑停止使用域控制器的总体旧操作系统的最新版本，应运行所有域控制器。 通过保持域控制器当前和消除旧版域控制器，你通常可以充分利用新功能，并且可能不可用域或森林中的运行传统操作系统域控制器的安全。 域控制器应该新鲜安装和升级而不是从以前的操作系统或服务器的角色： 升级也就是说，不要执行的域控制器就地升级或在服务器操作系统不新鲜安装该上运行广告 DS 安装向导。 通过实现刚安装的域控制器，您将确保，旧的文件和设置将不无意中处于域控制器上，并简化一致、 安全域控制器配置实施。  
  
## <a name="secure-configuration-of-domain-controllers"></a>安全的域控制器配置  
大量的免费工具，其中一些安装在 Windows 中的默认情况下，可用于创建后来通过 Gpo 强制执行的域控制器初始安全配置基线。 下面介绍了这些工具。  
  
### <a name="security-configuration-wizard"></a>安全配置向导  

在初始版本后，应锁定所有域控制器。 这可以实现使用 Windows Server"基本版本"该域控制器上配置服务、 注册表、 系统和 WFAS 设置附带本地安全配置向导。 可以保存设置，并将其导出到一个 GPO 域控制器森林中的每个域中可以增强的域控制器一致配置为可链接。 如果你的域中包含 Windows 操作系统的多个版本，你可以配置 Windows Management Instrumentation (WMI) 的筛选器来 Gpo 仅适用于运行的操作系统版本对应的域控制器。  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft 安全合规性管理器  
[Microsoft 安全合规性管理器](https://technet.microsoft.com/library/cc677002.aspx)可与安全配置向导设置，以生成的域控制器的部署和由 Gpo Active Directory 中域控制器在部署实施的完整配置基线结合使用域控制器设置。  
  
### <a name="applocker"></a>AppLocker  
应使用 AppLocker 或第三方应用程序白工具配置获准域控制器上运行的应用程序和服务，这些许可的范围内的应用程序和服务应组成仅的内容，才能进行主机广告 DS 可能 DNS，以及所有系统安全软件，例如，防病毒软件到计算机。 允许应用使用域控制器上的白，通过添加额外的安全层了，这样即使上域控制器安装未经授权的应用，应用程序不能运行。  
  
### <a name="rdp-restrictions"></a>RDP 限制  
应配置链接到所有域控制器华丽绚烂森林中的组策略对象允许 RDP 连接仅从授权的用户和系统（例如，跳转服务器）。 这可通过组合的用户版权设置和 WFAS 配置，以便策略始终应用应该 Gpo 中实现。 如果跳过下, 一组策略刷新将返回到其正确配置的系统。  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>修补程序和配置管理域控制器  
尽管这听起来可能很起来不够直观，您应该考虑修补域控制器和其他基础结构的关键组件单独从常规的 Windows 基础结构。 如果您在您的基础结构利用企业配置管理所有计算机的新软件，危害系统管理软件可用于威胁或破坏由该软件的所有基础结构组件。 通过从常规的总体分离修补程序和系统管理域控制器，可以减少域控制器，除了紧密控制其管理上安装的软件。  
  
### <a name="blocking-internet-access-for-domain-controllers"></a>对于域控制器阻止 Internet 访问权限  

作为一 Active Directory 安全评估执行检查之一是使用和域控制器上的 Internet Explorer 的配置。 Internet Explorer （或任何其他 web 浏览器） 应该不会域控制器上使用，但分析成千上万的域控制器表明很多情况下，在其中特权的用户使用 Internet Explorer 浏览公司的 intranet 或 Internet。  
  
如上文所述的"错误配置"部分中[危害的途径](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)，其中一台计算机最强大中使用的高权限的帐户 Windows 基础结构 （它们仅允许在本地登录到域控制器默认情况下的帐户） 从浏览 Internet （或受感染的 intranet） 提供了种不平凡的组织的安全风险。 无论是通过驱动器下载或下载恶意软件感染"工具"，攻击者可以访问所有内容他们需要完全威胁或破坏 Active Directory 环境。  
  
虽然 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008 和当前版本的 Internet Explorer 提供了大量抵御恶意下载的保护，域控制器在大多数情况下在哪一个域控制器和特权的帐户已被用于浏览 Internet、 运行 Windows Server 2003，或更高版本操作系统和浏览器通过提供的保护有有意禁用。  
  
启动 web 浏览器的域控制器上应不仅策略，而且通过 technical 控件，禁止，域控制器应不允许访问 Internet。 如果需要复制站点跨域控制器，你应实现安全的站点之间的连接。 尽管本文的范围之外配置详细说明进行操作，可以实现的大量控件限制域控制器，以便滥用或错误配置的并随后威胁的功能。  
  
### <a name="perimeter-firewall-restrictions"></a>周边防火墙限制  
若要阻止域控制器站连接到 Internet，应配置外围防火墙。 尽管域控制器可能需要站点边界之间进行通信，可以配置外围防火墙为允许通过遵循以下指南中提供的站点间通信[如何配置为域和信任防火墙](https://support.microsoft.com/kb/179442)Microsoft 支持网站上。  
  
### <a name="dc-firewall-configurations"></a>DC 防火墙配置  

如上文所述的那样，你应使用安全配置向导捕获具有高级安全的 Windows 防火墙域控制器上配置设置。 您应查看安全配置向导以确保防火墙配置设置符合你的组织的要求，然后使用 Gpo 强制配置设置输出。  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>阻止从域控制器 Web 浏览  
若要阻止访问互联网域控制器，，以免的 web 浏览器的域控制器上使用时，可以使用"黑洞的信息"代理配置，并且 WFAS 配置和 AppLocker 配置组成。  
  



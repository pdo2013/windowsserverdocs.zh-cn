---
title: DFS 复制：常见问题 (FAQ)
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 92fe505c3ae7d76f7a8d5bd9d2ed0ce845159fde
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71940751"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>DFS 复制：常见问题 (FAQ)


更新日期：2019年4月30日

适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

此常见问题回答了有关 Windows Server 分布式文件系统（DFS）复制（也称为 DFS 或 DFSR）的问题。

有关 dfs 命名空间的信息， [请参阅 dfs 命名空间：](https://technet.microsoft.com/library/ee404780)常见问题解答。

有关 DFS 复制中的新增功能的信息，请参阅以下主题：

  - [DFS 命名空间和 DFS 复制概述](https://technet.microsoft.com/library/jj127250)（在 Windows Server 2012 中）  
      
  - [Windows server 2008 到 Windows server 2008 R2 中功能更改](https://technet.microsoft.com/library/dd391932)的分布式文件系统主题中[的新增](https://technet.microsoft.com/library/ee307957)功能  
      
  - [Windows server 2003 SP1 到 Windows server 2008 的功能更改](https://technet.microsoft.com/library/cc753208)中的[分布式文件系统](https://technet.microsoft.com/library/cc753479)主题  
      

有关本主题最近更改的列表，请参阅本主题的[更改历史记录](#change-history)部分。

      

## <a name="interoperability"></a>互操作性

### <a name="can-dfs-replication-communicate-with-frs"></a>能否 DFS 复制与 FRS 通信？

否。 DFS 复制不与文件复制服务（FRS）通信。 DFS 复制和 FRS 可以同时在同一服务器上运行，但绝不能将它们配置为复制相同的文件夹或子文件夹，因为这样做可能会导致数据丢失。

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>可以 DFS 复制将 FRS 替换为 SYSVOL 复制

是的，DFS 复制可以替换运行 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的服务器上的 SYSVOL 复制的 FRS。 运行 Windows Server 2003 R2 的服务器不支持使用 DFS 复制来复制 SYSVOL 文件夹。

有关使用 DFS 复制复制 sysvol 的详细信息，请参阅[sysvol 复制迁移指南：要 DFS 复制](https://technet.microsoft.com/library/dd640019)的 FRS。

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>能否在不丢失配置设置的情况下从 FRS 升级到 DFS 复制？

是。 若要将复制从 FRS 迁移到 DFS 复制，请参阅以下文档：

  - 若要迁移 SYSVOL 文件夹之外的文件夹复制，请参阅[DFS 操作指南：从 frs 迁移到 DFS 复制](http://go.microsoft.com/fwlink/?linkid=192776)和[FRS2DFSR – FRS 到 DFSR 迁移实用程序](http://go.microsoft.com/fwlink/?linkid=195437)（ http://go.microsoft.com/fwlink/?LinkID=195437) ）。  
      
  - 若要将 sysvol 文件夹的复制迁移到 DFS 复制， [请参阅 SYSVOL 复制迁移指南：要 DFS 复制](https://technet.microsoft.com/library/dd640019)的 FRS。  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>能否在混合的 Windows/UNIX 环境中使用 DFS 复制？

是。 尽管 DFS 复制仅支持在运行 Windows Server 的服务器之间复制内容，但 UNIX 客户端可以访问 Windows Server 上的文件共享。 为此，请在 DFS 复制服务器上安装网络文件系统（NFS）服务。

你还可以使用许多 UNIX 客户端中包含的 SMB/CIFS 客户端功能直接访问 Windows 文件共享，不过此功能通常是有限的，或者需要对 Windows 环境进行修改（如使用组策略）。

DFS 复制与运行 Windows Server 操作系统的服务器上的 NFS 互操作，但不能复制 NFS 装入点。

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>是否可以将卷影复制服务用于 DFS 复制？

是。 DFS 复制在卷影复制服务（VSS）卷上受支持，并且以前的版本客户端可以成功还原以前的快照。

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>能否使用 Windows 备份（al.exe）远程备份已复制文件夹？

不可以，在运行 windows server 2003 或更早版本的计算机上使用 Windows 备份（al.exe）来备份运行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的计算机上的已复制文件夹的内容。

若要备份存储在已复制文件夹中的文件，请使用 Windows Server 备份或 Microsoft® System Center Data Protection Manager。 有关 Windows Server 2008 R2 和 Windows Server 2008 中的备份和恢复功能的信息，请参阅[备份和恢复](https://technet.microsoft.com/library/Cc754097)。 有关详细信息，请参阅[System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) （ http://go.microsoft.com/fwlink/?LinkId=182261) 。

### <a name="do-file-system-policies-impact-dfs-replication"></a>文件系统策略是否会影响 DFS 复制？

是。 不要在已复制文件夹上配置文件系统策略。 文件系统策略会在每个组策略刷新间隔重新应用 NTFS 权限。 这可能会导致共享冲突，因为在文件关闭前，不会复制打开的文件。

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>是否 DFS 复制复制 Microsoft Exchange Server 上托管的邮箱？

否。 DFS 复制不能用于复制 Microsoft Exchange Server 上托管的邮箱。

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>DFS 复制是否支持文件服务器资源管理器创建的文件屏蔽？

是。 但是，在复制的两端，文件服务器资源管理器（FSRM）文件屏蔽设置必须匹配。 此外，DFS 复制具有自己的文件和文件夹筛选器机制，可用于从复制中排除某些文件和文件类型。

下面是实现文件屏蔽或配额的最佳方案：

  - 隐藏的 DfsrPrivate 文件夹不得服从配额或文件屏蔽。  
      
  - 在启用了滤入功能之前，屏蔽文件不得存在于任何已复制的文件夹中。  
      
  - 启用配额之前，任何文件夹都可能超出配额。  
      
  - 必须谨慎使用硬配额。 复制组的各个成员可能会在复制前保持在配额范围内，但在复制文件时将超出该配额。 例如，如果用户将10兆字节（MB）文件复制到服务器 A （即硬限制），而另一个用户将 5 MB 文件复制到服务器 B，则在下一次复制发生时，这两台服务器的配额将超过 5 mb。 这可能会导致 DFS 复制不断地重试复制文件，从而导致版本向量和可能的性能问题。  
      

### <a name="is-dfs-replication-cluster-aware"></a>DFS 复制群集是否可识别？

是的，在 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 中 DFS 复制包括将故障转移群集添加为复制组的成员的功能。 有关详细信息，请参阅[将故障转移群集添加到复制组](http://go.microsoft.com/fwlink/?linkid=155085)（ http://go.microsoft.com/fwlink/?LinkId=155085) 。 Windows Server 2008 R2 之前的 Windows 版本上的 DFS 复制服务并未设计为与故障转移群集协调，并且该服务不会故障转移到另一个节点。


> [!NOTE]
> DFS 复制不支持复制群集共享卷上的文件。 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>与重复数据删除 DFS 复制是否兼容？

是的，DFS 复制可以复制使用 Windows Server 中的重复数据删除的卷上的文件夹。

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>DFS 复制与 RIS 和 WDS 兼容吗？

是。 DFS 复制复制启用了单实例存储（SIS）的卷。 SIS 由远程安装服务（RIS）、Windows 部署服务（WDS）和 Windows Storage Server 使用。

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>是否可以将 DFS 复制用于脱机文件？

当一次只有一个用户写入文件时，可以安全地使用 DFS 复制和脱机文件。 这适用于在两个分支机构之间旅行并希望能够在任一分支或脱机时访问其文件的用户。 脱机文件在本地缓存文件以供脱机使用，DFS 复制在每个分支机构之间复制数据。

不要在多用户环境中使用与脱机文件 DFS 复制，因为 DFS 复制不提供任何分布式锁定机制或文件签出功能。 如果两个用户在不同服务器上同时修改同一文件，DFS 复制在下一次复制过程中，将\\较旧的文件移到 DfsrPrivate ConflictandDeleted 文件夹（位于已复制文件夹的本地路径下）。

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>哪些防病毒应用程序与 DFS 复制兼容？

如果防病毒应用程序的扫描活动更改了复制文件夹中的文件，则可能会导致复制过多。 有关详细信息，请[与 DFS 复制](http://go.microsoft.com/fwlink/?linkid=73990)（ http://go.microsoft.com/fwlink/?LinkId=73990) 。

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>使用 DFS 复制而不是 Windows SharePoint Services 的好处是什么？

Windows® SharePoint®服务以 DFS 复制不会的文件签出功能提供了严格的一致性。 如果你担心多人编辑同一个文件，则建议使用 Windows SharePoint Services。 Windows Server 2003 R2 中提供了 windows SharePoint Services 2.0 Service Pack 2。 可从 Microsoft 网站下载 Windows SharePoint Services;它未包含在 Windows Server 的较新版本中。 但是，如果要跨多个站点复制数据，并且用户将不会同时编辑相同的文件，DFS 复制提供更大的带宽和更简单的管理。

## <a name="limitations-and-requirements"></a>限制和要求

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>是否可以在没有 VPN 连接的分支机构之间 DFS 复制复制？

是—假设有一个专用的广域网（WAN）链接（而不是 Internet）连接到分支机构。 但是，你必须在外部防火墙中打开适当的端口。 DFS 复制使用 RPC 终结点映射程序（端口135）和超过1024的随机分配的临时端口。 可以使用**dfsrdiag.exe**命令行工具指定静态端口而不是临时端口。 有关如何指定 RPC 终结点映射器的详细信息，请参阅 Microsoft 知识库中的[文章 154596](http://go.microsoft.com/fwlink/?linkid=73991) （ http://go.microsoft.com/fwlink/?LinkId=73991) 。

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>能否 DFS 复制复制加密文件系统加密的文件？

否。 DFS 复制不会复制使用加密文件系统（EFS）加密的文件或文件夹。 如果用户对先前复制的文件进行加密，DFS 复制会从复制组的所有其他成员中删除该文件。 这可确保文件的唯一可用副本是服务器上的加密版本。

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>能否 DFS 复制复制 Outlook .pst 或 Microsoft Office Access 数据库文件？

只有出于存档目的存储了 Microsoft Outlook 个人文件夹文件（.pst）和 Microsoft Access 文件时，DFS 复制才能安全地复制这些文件，并且使用客户端（如 Outlook 或 Access文件，请首先将文件复制到本地存储设备。 这样做的原因如下：

  - 通过网络连接打开 .pst 文件可能导致 .pst 文件中的数据损坏。 有关无法从网络上安全地访问 .pst 文件的原因的详细信息，请参阅 Microsoft 知识库中的[文章 297019](http://go.microsoft.com/fwlink/?linkid=125363) （ http://go.microsoft.com/fwlink/?LinkId=125363) 。  
      
  - .pst 和 Access 文件往往会长时间保持打开状态，而客户端（如 Outlook 或 Office Access）正在访问这些文件。 这会阻止 DFS 复制复制这些文件，直到它们关闭。  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>能否在工作组中使用 DFS 复制？

否。 DFS 复制依赖于 Active Directory®域服务进行配置。 它仅在域中有效。

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>是否可以在单个服务器上复制多个文件夹？

是。 DFS 复制可以在服务器之间复制多个文件夹。 确保每个已复制文件夹都具有唯一的根路径，并且它们不会重叠。 例如，d：\\sales 和 d：\\Accounting 可以是两个已复制文件夹的根路径，但 d：\\sales 和 d：\\sales\\报表不能是两个已复制文件夹的根路径。

### <a name="does-dfs-replication-require-dfs-namespaces"></a>DFS 复制是否需要 DFS 命名空间？

否。 DFS 复制和 DFS 命名空间可以单独使用，也可以一起使用。 此外，还可以使用 DFS 复制来复制独立的 DFS 命名空间，这是不可能使用 FRS 实现的。

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>DFS 复制在服务器之间是否需要时间同步？

否。 DFS 复制不会显式要求服务器之间的时间同步。 但 DFS 复制要求服务器时钟严格匹配。 服务器时钟必须设置为五分钟（默认值），Kerberos 身份验证才能正常工作。 例如，DFS 复制使用时间戳来确定在发生冲突时哪个文件优先。 准确时间对于垃圾回收、计划和其他功能也很重要。

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>DFS 复制是否支持复制整个卷？

是。 但是，必须首先安装 Windows Server 2003 Service Pack 2 或修补程序。 有关详细信息，请参阅 Microsoft 知识库中的[文章 920335](http://go.microsoft.com/fwlink/?linkid=76776) （ http://go.microsoft.com/fwlink/?LinkId=76776) 。 此外，复制整个卷可能会导致以下问题：

  - 如果卷包含 Windows 分页文件，则复制将失败，并在系统事件日志中记录 DFSR 事件4312。  
      
  - DFS 复制设置目标服务器上已复制文件夹的系统属性和隐藏属性。 出现这种情况的原因是，默认情况下，Windows 会将系统和隐藏属性应用于卷根文件夹。 如果目标服务器上已复制文件夹的本地路径也是卷根路径，则不会对文件夹属性进行进一步的更改。  
      
  - 在复制包含 Windows 系统文件夹的卷时，DFS 复制会识别% WINDIR% 文件夹，不会复制该文件夹。 但是，DFS 复制会复制非 Microsoft 应用程序使用的文件夹，如果应用程序具有 DFS 复制的互操作性问题，则可能导致应用程序在目标服务器上失败。  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>DFS 复制是否支持 RPC over HTTP？

否。

### <a name="does-dfs-replication-work-across-wireless-networks"></a>DFS 复制在无线网络上工作吗？

是。 DFS 复制与连接类型无关。

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>DFS 复制是在 ReFS 还是 FAT 卷上工作？

否。 DFS 复制仅支持 NTFS 文件系统格式化的卷;不支持复原文件系统（ReFS）和 FAT 文件系统。 DFS 复制需要 NTFS，因为它使用 ntfs 文件系统的 NTFS 更改日志和其他功能。

### <a name="does-dfs-replication-work-with-sparse-files"></a>DFS 复制是否处理稀疏文件？

是。 可以复制稀疏文件。 **Sparse**属性保留在接收成员上。

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>是否需要以管理员身份登录来复制文件？

否。 DFS 复制是在本地系统帐户下运行的服务，因此你不需要以管理员身份登录以进行复制。 但是，您必须是受影响的文件服务器的域管理员或本地管理员才能对 DFS 复制配置进行更改。

有关详细信息，请参阅[委派管理 DFS 复制的能力](http://go.microsoft.com/fwlink/?linkid=182294)（ http://go.microsoft.com/fwlink/?LinkId=182294) ）中的 "DFS 复制安全要求和委派"。

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>如何升级或替换 DFS 复制成员？

若要升级或替换 DFS 复制成员，请参阅 "询问目录服务团队博客：正在[替换 DFSR 成员硬件或操作系统](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx)。

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>DFS 复制适用于复制漫游配置文件吗？

是。 复制漫游用户配置文件时，支持某些方案。 有关支持的方案的详细信息，请参阅[Microsoft 针对复制的用户配置文件数据的支持声明](http://go.microsoft.com/fwlink/?linkid=201282)（ http://go.microsoft.com/fwlink/?LinkId=201282) 。

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>文件夹深度是否有文件字符限制或限制？

Windows 和 DFS 复制最多支持32000个字符的文件夹路径。 DFS 复制不限制为260个字符的文件夹路径。

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>复制组的成员是否必须位于同一个域中？

否。 复制组可以跨越单个林中的域，但不能跨不同的林。

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>DFS 复制支持的限制是什么？

以下列表提供了一组可由 Microsoft 测试并适用于 Windows Server 2012 R2、Windows Server 2016 和 Windows Server 2019 的可伸缩性指南

  - 服务器上所有复制文件的大小：100 tb。  
      
  - 卷上的已复制文件数：70000000。  
      
  - 最大文件大小：250 gb。  
      


> [!IMPORTANT]
> 创建包含大量文件的复制组时，我们建议导出数据库克隆，并使用预种子设定技术来最大程度地缩短初始复制的持续时间。 有关详细信息，请[参阅 Windows Server 2012 R2 中的 DFS 复制初始同步：克隆](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/DFS-Replication-Initial-Sync-in-Windows-Server-2012-R2-Attack-of/ba-p/424877)的攻击。 
<br>


下面的列表提供了一组在 Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008 上经过 Microsoft 测试的可伸缩性指导原则：

  - 服务器上所有复制文件的大小：10 tb。  
      
  - 卷上的已复制文件数：11000000。  
      
  - 最大文件大小：64 gb。  
      


> [!NOTE]
> 复制组、已复制文件夹、连接或复制组成员的数量不再有限制。 
<br>


有关 Microsoft for Windows Server 2003 R2 测试的可伸缩性准则列表，请参阅[DFS 复制可伸缩性准则](http://go.microsoft.com/fwlink/?linkid=75043)（ http://go.microsoft.com/fwlink/?LinkId=75043) 。

### <a name="when-should-i-not-use-dfs-replication"></a>何时应不使用 DFS 复制？

不要在多个用户在不同服务器上同时更新或修改相同文件的环境中使用 DFS 复制。 这样做可能会导致 DFS 复制将文件的冲突副本移到隐藏的 DfsrPrivate\\ConflictandDeleted 文件夹。

当多个用户需要在不同服务器上同时修改相同的文件时，请使用 Windows SharePoint Services 的 "文件签出" 功能，以确保只有一个用户在处理文件。 Windows Server 2003 R2 中提供了 windows SharePoint Services 2.0 Service Pack 2。 可从 Microsoft 网站下载 Windows SharePoint Services;它未包含在 Windows Server 的较新版本中。

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>为什么 DFS 复制需要架构更新？

DFS 复制使用 Active Directory 域服务的域命名上下文中的新对象来存储配置信息。 这些对象是在更新 Active Directory 域服务架构时创建的。 有关详细信息，请参阅[查看 DFS 复制的要求](http://go.microsoft.com/fwlink/?linkid=182264)（ http://go.microsoft.com/fwlink/?LinkId=182264) 。

## <a name="monitoring-and-management-tools"></a>监视和管理工具

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>能否自动运行运行状况报告以接收警告？

是。 可以通过三种方式自动运行运行状况报告：

  - 将 Windows Server 2012 R2 或 DfsrAdmin 中包含的 DFSR Windows PowerShell 模块与计划任务结合使用，以便定期生成运行状况报告。 有关详细信息，请参阅[自动 DFS 复制运行状况报告](http://go.microsoft.com/fwlink/?linkid=74010)（ http://go.microsoft.com/fwlink/?LinkId=74010) ）。  
      
  - 使用 System Center Operations Manager 的 DFS 复制管理包创建基于指定条件的警报。  
      
  - 使用 DFS 复制 WMI 提供程序为警报编写脚本。  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>能否使用 Microsoft System Center Operations Manager 来监视 DFS 复制？

是。 有关详细信息，请参阅 Microsoft 下载中心中的[System Center Operations Manager 2007 的 DFS 复制管理包](http://go.microsoft.com/fwlink/?linkid=182265)（ http://go.microsoft.com/fwlink/?LinkId=182265) 。

### <a name="does-dfs-replication-support-remote-management"></a>DFS 复制是否支持远程管理？

是。 DFS 复制使用 DFS 管理控制台和 "**添加复制组**" 命令支持远程管理。 例如，在服务器 A 上，你可以连接到林中定义的复制组，其中服务器 A 和 B 作为成员。

Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 和 Windows Server 2003 R2 随附 DFS 管理。 若要管理其他版本的 Windows DFS 复制，请使用远程桌面或[windows 7 的远程服务器管理工具](https://technet.microsoft.com/library/Ee449475)。


> [!IMPORTANT]
> 若要查看或管理包含只读复制文件夹或故障转移群集成员的复制组，必须使用 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 随附的 DFS 管理版本、<a href="http://go.microsoft.com/fwlink/p/?linkid=238560">远程适用于 Windows 8 的服务器管理工具</a>或<a href="https://technet.microsoft.com/library/ee449475">适用于 windows 7 的远程服务器管理工具</a>。 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound 和 Sonar.projectname 是否适用 DFS 复制？

否。 DFS 复制有自己的一组监视和诊断工具。 Ultrasound 和 Sonar.projectname 仅能监视 FRS。

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>如何从 ConflictAndDeleted 或预先存在的文件夹恢复文件？

若要恢复丢失的文件，请使用 "文件历史记录"、"在文件资源管理器中**还原以前的版本**" 命令或通过从备份还原文件来还原文件系统文件夹或共享文件夹中的文件。 若要从 ConflictAndDeleted 或预先存在的文件夹中直接恢复文件`Get-DfsrPreservedFiles` ， `Restore-DfsrPreservedFiles`请使用和 windows PowerShell cmdlet （包含在 Windows Server 2012 R2 中的 DFSR 模块中）或 MSDN 中的[RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr)示例脚本代码库。 此脚本仅用于灾难恢复，按原样提供，无需担保。

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>是否有方法知道复制状态？

是。 可以通过多种方式监视复制：

  - DFS 复制具有用于提供主动监视的 System Center Operations Manager 管理包。  
      
  - DFS 管理为复制积压工作（backlog）、复制效率以及给定复制组中的文件和文件夹数量提供内置的诊断报告。  
      
  - Windows Server 2012 R2 中的 DFSR Windows PowerShell 模块包含用于启动传播测试和编写传播和运行状况报告的 cmdlet。 有关详细信息，请参阅[Windows PowerShell 中的分布式文件系统复制 cmdlet](https://technet.microsoft.com/library/dn296601.aspx)。  
      
  - Dfsrdiag.exe 是一个命令行工具，可生成积压工作（backlog）计数或触发传播测试。 两者都显示复制的状态。 传播向你显示是否将文件复制到所有节点。 积压工作（Backlog）显示了在两台计算机同步之前仍需要复制的文件数。积压工作计数是指未处理复制组成员的更新数。 在运行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的计算机上，Dfsrdiag.exe 还可以显示当前正在复制 DFS 复制的更新。  
      
  - 脚本可以使用 WMI 手动或通过 MOM 收集积压工作（backlog）信息。  
      

## <a name="performance"></a>性能

### <a name="does-dfs-replication-support-dial-up-connections"></a>DFS 复制是否支持拨号连接？

尽管 DFS 复制可以在拨号速度下工作，但如果有大量要复制的更改，则可能会出现累积的情况。 如果对现有文件进行了小的更改，使用远程差分压缩（RDC） DFS 复制将提供比直接复制文件更高的性能。

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>DFS 复制是否执行带宽检测？

否。 DFS 复制不会执行带宽检测。 可以将 DFS 复制配置为在每个连接基础上使用有限的带宽量（带宽限制）。 但是，如果网络接口变得饱和，则 DFS 复制不会进一步降低带宽利用率，并且 DFS 复制可以在短时间内使链接变得饱和。 使用 DFS 复制的带宽限制并不完全准确，因为 DFS 复制会限制 RPC 调用，从而限制带宽。 因此，网络堆栈中较低级别的各种缓冲区（包括 RPC）可能会干扰，导致网络流量激增。

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>DFS 复制限制每个计划、每个服务器还是每个连接的带宽？

如果在指定计划时配置带宽限制，则该复制组的所有连接都将使用该设置进行带宽限制。 带宽限制还可以使用 DFS 管理设置为连接级设置。

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>DFS 复制使用 Active Directory 域服务来计算站点链接和连接成本吗？

否。 DFS 复制使用管理员定义的拓扑，该拓扑与 Active Directory 域服务站点成本无关。

### <a name="how-can-i-improve-replication-performance"></a>如何提高复制性能？

若要了解优化复制性能的不同方法，请参阅[咨询目录服务团队博客](http://blogs.technet.com/b/askds/)[中的在 DFSR 中优化复制性能](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx)。

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>DFS 复制如何避免连接饱和？

在 DFS 复制设置要在连接上使用的最大带宽，并且服务将维护该网络使用级别。 这不同于后台智能传输服务（BITS），如果正确设置，则 DFS 复制不会对连接进行饱和。

尽管如此，带宽限制并不是精确到 100%，DFS 复制可能会在短时间内使链接饱和。 这是因为 DFS 复制通过限制 RPC 调用来限制带宽。 由于此过程依赖于网络堆栈的较低级别（包括 RPC）中的各种缓冲区，因此复制流量倾向于突发，这可能会使网络链接饱和。

Windows Server 2008 中的 DFS 复制包含了多个性能增强功能，如[分布式文件系统](https://technet.microsoft.com/library/Cc753479)中的 " [WINDOWS server 2003 SP1 到 Windows Server 2008" 功能的变化](https://technet.microsoft.com/library/cc753208)主题中所述。

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>DFS 复制性能与 FRS 相比如何？

DFS 复制比 FRS 快得多，尤其是在对大型文件进行少量更改，且已启用 RDC 的情况下。 例如，使用 RDC，对 2 MB PowerPoint®演示的小更改可能导致在网络中发送 60 kb （传输字节节省 97%）。

RDC 不用于小于 64 KB 的文件，并且在不会争用网络带宽的高速 Lan 上可能不会有好处。 可以使用 DFS 管理在每个连接基础上禁用 RDC。

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>DFS 复制复制数据的频率如何？

数据根据您设置的计划进行复制。 例如，你可以将计划设置为每周7天，每隔15分钟一次。 在这些时间间隔内，将启用复制。 检测到文件更改后（通常在几秒钟内），将立即开始复制。

当连接计划设置为接收成员的本地时间时，可以将复制组计划设置为通用时间协调（UTC）。 当复制组跨多个时区时，请考虑到这一点。 本地时间表示托管入站连接的成员的时间。 当计划设置为本地时间时，入站连接和相应的出站连接的显示计划将反映时区差异。

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>我的服务器的系统资源将 DFS 复制使用多少？

DFS 复制所使用的磁盘、内存和 CPU 资源取决于许多因素，包括文件的数量和大小、更改的速率、复制组成员的数目以及已复制文件夹的数目。 此外，某些资源更难以估算。 例如，用于 DFS 复制数据库的可扩展存储引擎（ESE）技术可能会消耗大量可用内存，它会根据需要释放。 除 DFS 复制以外的应用程序可以在同一台服务器上承载，具体取决于服务器配置。 但是，在单个服务器上承载多个应用程序或服务器角色时，请务必在生产环境中实现此配置，然后再对其进行测试。

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>如果 WAN 链接在复制过程中失败，会发生什么情况？

如果连接中断，DFS 复制将在计划打开时继续尝试复制。 DFS 复制事件日志中还会提到一些连接错误，这些错误可以使用 MOM （主动通过警报）和 DFS 复制运行状况报告（被动，如管理员运行时）进行搜集。

## <a name="remote-differential-compression-details"></a>远程差分压缩详细信息

### <a name="are-changes-compressed-before-being-replicated"></a>在复制之前是否压缩了更改？

是。 在为除以下文件类型之外的所有文件类型（已压缩）发送之前，已对文件的更改部分进行了压缩：。 wma，.wmv，.zip，.jpg，. mpg，mpeg-2，. .m1v，. mp2，. mp3，mpa，.cab，.wav，. snd，. z，. gz，. tgz，. frx。 无法在 Windows Server 2003 R2 中配置这些文件类型的压缩设置。

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>管理员可以关闭 RDC 或更改阈值吗？

是。 可以通过给定连接的属性页关闭 RDC。 禁用 RDC 可以减少没有带宽限制的快速局域网（LAN）链接或主要包含小于 64 KB 的文件的复制组的 CPU 利用率和复制延迟。 如果选择在连接上禁用 RDC，请在更改之前和之后测试复制效率，以验证是否已提高复制性能。

您可以通过使用**Dfsradmin 连接设置**命令、DFS 复制 WMI 提供程序或通过手动编辑配置 XML 文件来更改 RDC 大小阈值。

### <a name="does-rdc-work-on-all-file-types"></a>RDC 是否适用于所有文件类型？

是。 RDC 在块级别计算差异，而不考虑文件数据类型。 但是，在某些文件类型（如 Word 文档、PST 文件和 VHD 映像）上，RDC 的工作效率更高。

### <a name="how-does-rdc-work-on-a-compressed-file"></a>RDC 如何处理压缩文件？

DFS 复制使用 RDC，它计算文件中已更改的块，并只通过网络发送这些块。 DFS 复制无需了解有关文件内容的任何信息，只需更改已更改的块。

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>升级到 Windows Server Enterprise Edition 或 Datacenter Edition 时是否已启用跨文件 RDC？

Windows Server Standard Edition 不支持跨文件 RDC。 但是，当你升级到支持跨文件 RDC 的版本或复制连接的成员正在运行受支持的版本时，它会自动启用。 有关支持跨文件 RDC 的版本列表，请参阅 Windows 操作系统支持跨文件 RDC 的版本？

### <a name="is-rdc-true-block-level-replication"></a>是否为 RDC true 块级复制？

否。 RDC 是用于压缩文件传输的通用协议。 DFS 复制在文件级别使用块，而不是在磁盘块级别使用。 RDC 将文件分成块。 对于文件中的每个块，它将计算一个签名，该签名是可表示较大块的少量字节。 一组签名将从服务器传输到客户端。 客户端将服务器签名进行比较。 然后，客户端请求服务器只发送不在客户端上的签名的数据。

### <a name="what-happens-if-i-rename-a-file"></a>重命名文件会发生什么情况？

DFS 复制在下一次复制过程中重命名复制组的所有其他成员上的文件。 使用唯一 ID 跟踪文件，因此，重命名文件并将文件移动到副本中并不会影响 DFS 复制复制文件的能力。

### <a name="what-is-cross-file-rdc"></a>什么是跨文件 RDC？

即使客户端不存在具有相同名称的文件，跨文件 RDC 也允许 DFS 复制使用 RDC。 跨文件 RDC 使用试探法来确定与需要复制的文件类似的文件，并使用与复制文件相同的类似文件块来最大程度地减少通过 WAN 传输的数据量。 跨文件 RDC 可以在此过程中使用多达五个相似文件的块。

若要使用跨文件 RDC，复制连接的一个成员必须运行支持跨文件 RDC 的 Windows 版本。 有关支持跨文件 RDC 的版本列表，请参阅 Windows 操作系统支持跨文件 RDC 的版本？

### <a name="what-is-rdc"></a>什么是 RDC？

远程差分压缩（RDC）是一种客户端-服务器协议，可用于通过带宽有限的网络有效更新文件。 RDC 检测文件中数据的插入、删除和重新排列，使 DFS 复制仅在更新文件时才复制更改。 RDC 仅用于默认情况下为 64 KB 或更大的文件。 RDC 可以在已复制文件夹或 DfsrPrivate\\ConflictandDeleted 文件夹中（位于已复制文件夹的本地路径下）使用具有相同名称的较旧版本的文件。

### <a name="when-is-rdc-used-for-replication"></a>RDC 何时用于复制？

如果文件超过最小大小阈值，则使用 RDC。 默认情况下，此大小阈值为 64 KB。 复制超过该阈值的文件后，文件的更新版本将始终使用 RDC，除非该文件的很大部分发生了更改或 RDC 处于禁用状态。

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Windows 操作系统的哪些版本支持跨文件 RDC？

要使用跨文件 RDC，复制连接的一个成员必须运行支持跨文件 RDC 的 Windows 操作系统版本。 下表显示了哪些版本的 Windows 操作系统支持跨文件 RDC。

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>Windows 操作系统版本中的跨文件 RDC 可用性

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>操作系统版本</th>
<th>Standard Edition</th>
<th>企业版</th>
<th>Datacenter Edition</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>是<em></p></td>
<td><p>未推出</p></td>
<td><p>是</em></p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>是</p></td>
<td><p>未推出</p></td>
<td><p>是</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2008 R2</p></td>
<td><p>否</p></td>
<td><p>是</p></td>
<td><p>是</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2008</p></td>
<td><p>否</p></td>
<td><p>是</p></td>
<td><p>否</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2003 R2</p></td>
<td><p>否</p></td>
<td><p>是</p></td>
<td><p>否</p></td>
</tr>
</tbody>
</table>

\*您可以选择在 Windows Server 2012 R2 上禁用跨文件 RDC。

## <a name="replication-details"></a>复制详细信息

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>创建已复制文件夹后，是否可以更改该文件夹的路径？

否。 如果需要更改已复制文件夹的路径，则必须在 DFS 管理中将其删除，并将其作为新的已复制文件夹添加回去。 然后 DFS 复制使用远程差分压缩（RDC）来执行同步，以确定发送和接收成员上的数据是否相同。 它不会再次复制文件夹中的所有数据。

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>能否配置要复制的文件属性？

不可以，不能配置 DFS 复制复制哪些文件属性。

有关属性值及其说明的列表，请参阅 MSDN 上的[文件属性](http://go.microsoft.com/fwlink/?linkid=182268)（ http://go.microsoft.com/fwlink/?LinkId=182268) 。

以下属性值通过使用`SetFileAttributes dwFileAttributes`函数进行设置，并 DFS 复制进行复制。 对这些属性值的更改会触发属性复制。 如果内容也发生更改，则不会复制该文件的内容。 有关详细信息，请参阅 MSDN library 中的[SetFileAttributes 函数](http://go.microsoft.com/fwlink/?linkid=182269)（ http://go.microsoft.com/fwlink/?LinkId=182269) 。

  - 文件\_属性\_已隐藏  
      
  - 文件\_属性\_READONLY  
      
  - 文件\_属性\_系统  
      
  - 未\_\_为内容\_编制索引的文件属性\_  
      
  - 文件\_属性\_脱机  
      

以下属性值由 DFS 复制进行复制，但不会触发复制。

  - 文件\_属性\_存档  
      
  - 文件\_属性\_正常  
      

尽管不能使用`SetFileAttributes`函数（ `GetFileAttributes`使用函数查看属性值）设置以下文件属性值，但它们也会触发复制。

  - 文件\_属性\_重新分析\_点  
      

> [!NOTE]
> DFS 复制不会复制重新分析点属性值，除非重新分析标记为 IO_REPARSE_TAG_SYMLINK。 具有 IO_REPARSE_TAG_DEDUP、IO_REPARSE_TAG_SIS 或 IO_REPARSE_TAG_HSM 重新分析标记的文件作为普通文件复制。 但是，重新分析标记和重新分析数据缓冲区不会复制到其他服务器，因为重新分析点仅适用于本地系统。 
<br>

  - 文件\_属性\_已压缩  
      
  - 文件\_属性\_已加密  
      

> [!NOTE]
> DFS 复制不会复制使用加密文件系统（EFS）加密的文件。 DFS 复制会复制使用非 Microsoft 软件加密的文件，但前提是它不在文件上设置 FILE_ATTRIBUTE_ENCRYPTED 属性值。 
<br>

  - 文件\_属性\_稀疏文件\_  
      
  - 文件\_属性\_目录  
      

DFS 复制不会复制文件\_属性\_临时值。

### <a name="can-i-control-which-member-is-replicated"></a>能否控制复制的成员？

是。 创建复制组时，可以选择拓扑。 或者，你可以选择 "**无拓扑**"，并在创建复制组后手动配置连接。

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>在初始复制之前是否可以使用数据播种复制组成员？

是。 DFS 复制支持在初始复制之前将文件复制到复制组成员。 这种 "预留" 可以显著减少在初始复制期间复制的数据量。

如果文件只是真实的属性或时间戳，则初始复制不需要复制内容。 Real 特性是一个可以由 Win32 函数`SetFileAttributes`设置的特性。 有关详细信息，请参阅 MSDN library 中的[SetFileAttributes 函数](http://go.microsoft.com/fwlink/?linkid=182269)（ http://go.microsoft.com/fwlink/?LinkId=182269) 。 如果两个文件不同于其他属性（如压缩），则会复制该文件的内容。

若要预留复制组成员，请将文件复制到目标服务器上的适当文件夹中，创建复制组，然后选择主成员。 选择具有您要复制的最新文件的成员，因为主成员的内容被视为 "权威"。 这意味着在初始复制期间，主成员的文件将始终覆盖复制组的其他成员上的其他版本的文件。

有关预播种和克隆 DFSR 数据库的信息，请参阅[在 Windows Server 2012 R2 中 DFS 复制初始同步：克隆](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx)的攻击。

有关初始复制的详细信息，请参阅[创建复制组](https://technet.microsoft.com/library/cc725893)。

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>DFS 复制是否克服了常见文件复制服务问题？

是。 DFS 复制克服了三个常见的 FRS 问题：

  - 日志包装：DFS 复制从日记本动态进行恢复。 在再次启用复制之前，每个现有的文件或文件夹都将标记为 journalWrap 并验证文件系统。 在恢复期间，此卷不可用于双向复制。  
      
  - 过多复制：若要防止过多复制，DFS 复制使用信用额度。  
      
  - 仿型文件夹：若要防止仿型文件夹名称，DFS 复制将冲突数据存储在\\隐藏的 DfsrPrivate ConflictandDeleted 文件夹中（位于已复制文件夹的本地路径下）。 例如，使用 FRS 在复制的不同服务器上同时创建多个具有相同名称的文件夹会导致 FRS 重命名旧文件夹。 DFS 复制将旧文件夹移动到本地冲突和已删除文件夹。  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>DFS 复制按时间顺序复制文件吗？

否。 可以不按顺序复制文件。

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>DFS 复制复制其他应用程序正在使用的文件吗？

如果某个应用程序打开一个文件并在该文件上创建文件锁（防止其他应用程序在它打开时使用它），则在关闭文件之前，DFS 复制不会复制该文件。 如果应用程序使用读共享访问权限打开文件，则仍可以复制文件。

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>DFS 复制是否复制 NTFS 文件权限、备用数据流、硬链接和重新分析点？

  - DFS 复制复制 NTFS 文件权限和备用数据流。  
      
  - Microsoft 不支持与已复制文件夹中的文件创建 NTFS 硬链接，这样做会导致受影响文件的复制问题。 硬链接文件被 DFS 复制忽略，并且不会被复制。 联接点也不会被复制，并且 DFS 复制日志事件4406。  
      
  - DFS 复制复制的唯一重新分析点是使用 IO\_重新分析\_标记\_符号标记的重新分析点; 但是，DFS 复制不保证还会复制链接目标。 有关详细信息，请参阅 "[询问目录服务团队博客](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx)"。  
      
  - 复制带有\_io 重新分析\_标记\_、\_\_io\_重新分析标记\_SIS 或 io重新分析标记的文件。\_\_作为普通文件。 重新分析标记和重新分析数据缓冲区不会复制到其他服务器，因为重新分析点仅适用于本地系统。 因此，DFS 复制可以在 Windows Server 2012 或单实例存储（SIS）中使用重复数据删除的卷上复制文件夹，但是，每个启用了角色服务的服务器将单独维护重复数据删除信息。  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>如果未对文件进行其他更改，是否 DFS 复制复制时间戳更改？

不，DFS 复制不会复制仅更改为时间戳的更改的文件。 此外，更改的时间戳不会复制到复制组的其他成员，除非对文件进行了其他更改。

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>DFS 复制复制对文件或文件夹的更新的权限吗？

是。 DFS 复制复制文件和文件夹的权限更改。 仅复制与访问控制列表（ACL）关联的文件部分，不过 DFS 复制仍必须将整个文件读入过渡区域。


> [!NOTE]
> 更改大量文件上的 Acl 可能会影响复制性能。 但是，在使用 RDC 时，传输的数据量与 Acl 的大小成正比，而不是整个文件的大小。 磁盘流量的大小仍与文件大小成正比，因为文件必须在暂存文件夹中进行读取。 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>发生冲突时 DFS 复制是否支持合并文本文件？

如果存在冲突，DFS 复制不会合并文件。 但是，它会尝试在检测到冲突的计算机上的隐藏 DfsrPrivate\\ConflictandDeleted 文件夹中保留该文件的旧版本。

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>传输数据时 DFS 复制使用加密吗？

是。 DFS 复制通过加密使用远程过程调用（RPC）连接。

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>是否可以禁用加密 RPC 的使用？

否。 DFS 复制服务使用 TCP 上的远程过程调用（RPC）来复制数据。 为了确保跨 Internet 的数据传输安全，DFS 复制服务旨在始终使用身份验证级别的常量`RPC_C_AUTHN_LEVEL_PKT_PRIVACY`。 这可确保始终对 Internet 上的 RPC 通信进行加密。 因此，不能禁用 DFS 复制服务使用加密 RPC。

有关详细信息，请参阅以下 Microsoft 网站：

  - [RPC 技术参考](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [关于远程差分压缩](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [身份验证级别常量](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>如何处理同步复制？

每个已复制文件夹有一个更新管理器。 更新管理器彼此独立地运行。

默认情况下，在所有连接和复制组之间共享最多16（Windows Server 2003 R2 中的四个）并发下载。 由于未对连接和复制组更新进行序列化，因此不会接收到任何特定的更新顺序。 如果打开了两个计划，则通常会同时从两个连接接收和安装更新。

### <a name="how-do-i-force-replication-or-polling"></a>如何实现强制复制还是轮询？

你可以使用 DFS 管理立即强制复制，如[编辑复制计划](https://technet.microsoft.com/library/Cc732278)中所述。 你还可以使用`Sync-DfsReplicationGroup` cmdlet 强制执行复制，该模块包含在 Windows Server 2012 R2 引入的 DFSR PowerShell 模块中，或**dfsrdiag.exe SyncNow**命令。 可以使用`Update-DfsrConfigurationFromAD` cmdlet 或**dfsrdiag.exe PollAD**命令强制轮询。

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>是否可以为经常更改的文件配置副本间的安静时间？

否。 如果计划处于打开状态，DFS 复制会在其通知时复制更改。 无法为文件配置安静时间。

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>是否可以通过 DFS 复制配置单向复制？

是。 如果使用的是 Windows Server 2012 或 Windows Server 2008 R2，则可以创建一个只读的已复制文件夹，通过单向连接来复制内容。 有关详细信息，请参阅[在特定成员上使已复制文件夹为只读](http://go.microsoft.com/fwlink/?linkid=156740)（ http://go.microsoft.com/fwlink/?LinkId=156740) 。

我们不支持在 Windows Server 2008 或 Windows Server 2003 R2 中与 DFS 复制创建单向复制连接。 这样做可能会导致许多问题，包括运行状况检查拓扑错误、暂存问题以及 DFS 复制数据库的问题。

如果使用的是 Windows Server 2008 或 Windows Server 2003 R2，则可以通过执行下列操作来模拟单向连接：

  - 训练管理员仅对要指定为主服务器的服务器进行更改。 然后，将更改复制到目标服务器。  
      
  - 在目标服务器上配置共享权限，使最终用户不具有写入权限。 如果不允许在分支服务器上进行任何更改，则不会复制回任何内容，模拟单向连接并保持 WAN 利用率低。  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>是否有办法强制执行所有文件（包括未更改的文件）的完整复制？

否。 如果 DFS 复制认为文件相同，则不会复制这些文件。 如果未复制已更改的文件，则在配置时，DFS 复制会自动复制这些文件。 若要覆盖配置的计划，请使用 WMI 方法**ForceReplicate （）** 。 但是，这只是一个计划替代，不会强制复制未更改或相同的文件。

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>如果在初始复制期间主成员出现数据库丢失，会发生什么情况？

在初始复制期间，如果接收成员在主成员上具有不同版本的文件，则主成员的文件将始终优先于冲突解决。 主成员指定存储在 Active Directory 域服务中，并在主成员准备好复制之后但在复制组的所有成员复制之前，清除指定。

如果初始复制失败或 DFS 复制服务在复制过程中重启，则主成员会看到本地 DFS 复制数据库中的主要成员指定，并重试初始复制。 如果主成员的 DFS 复制数据库在清除了 Active Directory 域服务中的主标识之后丢失，但在复制组的所有成员完成了初始复制之前，复制组的所有成员都无法复制文件夹，因为没有任何服务器被指定为主成员。 如果发生这种情况，请在主成员服务器上使用**Dfsradmin 成员/set/isprimary： true**命令手动还原主成员指定。

有关初始复制的详细信息，请参阅[创建复制组](https://technet.microsoft.com/library/cc725893)。


> [!WARNING]
> 主成员指定仅在初始复制过程中使用。 如果在复制完成后使用<STRONG>Dfsradmin</STRONG>命令指定已复制文件夹的主要成员，DFS 复制不会将该服务器指定为 Active Directory 域服务中的主要成员。 但是，如果服务器上的 DFS 复制数据库随后会导致不可恢复的损坏或数据丢失，服务器将尝试将初始复制作为主成员执行，而不是从复制的另一个成员中恢复其数据组. 实质上，服务器会成为恶意的主服务器，这可能会导致冲突。 因此，仅当你确定初始复制便失败时，才手动指定主成员。 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>如果复制计划在文件复制过程中关闭，会发生什么情况？

如果在连接时启用了远程差分压缩（RDC），则在计划打开（或不更改为 "**无带宽**"）之前开始复制的64文件的入站复制在计划打开（或对**不是带宽**的更改）。 复制停止时，将从它所处的状态继续。

如果 RDC 处于关闭状态，DFS 复制会完全重新启动文件传输。 当文件在接收成员上可用时，这可能会延迟。

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>当两个用户同时更新不同服务器上的同一文件时会发生什么情况？

当 DFS 复制检测到冲突时，它将使用最后保存的文件的版本。 它将另一个文件移到 DfsrPrivate\\ConflictandDeleted 文件夹（在解决冲突的计算机上的已复制文件夹的本地路径下）。 冲突和已删除文件夹清除（当冲突和已删除文件夹超过配置的大小或 DFS 复制遇到磁盘空间不足错误时）。 不会复制冲突和已删除文件夹，并且这种冲突解决方法可避免在 FRS 中可能出现的 "仿型" 目录的问题。

发生冲突时，DFS 复制会将信息性事件记录到 DFS 复制事件日志中。 此事件不需要用户操作，原因如下：

  - 它对用户是不可见的（它仅对服务器管理员可见）。  
      
  - DFS 复制将冲突和已删除文件夹视为缓存。 当达到配额阈值时，它会清除某些文件。 不保证会保存冲突的文件。  
      
  - 冲突可能驻留在与冲突起源不同的服务器上。  
      

## <a name="staging"></a>分步

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>在按计划或带宽限制配额禁用复制或手动禁用连接时，是否 DFS 复制继续暂存文件？

否。 如果超出了带宽限制配额，或者在禁用连接时，DFS 复制不会继续在计划复制时间之外暂存文件。

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>DFS 复制防止其他应用程序在暂存期间访问文件？

否。 DFS 复制以不阻止用户或应用程序打开复制文件夹中的文件的方式打开文件。 此方法称为 "机会锁定"。

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>是否可以用 DFS 管理工具更改暂存文件夹的位置？

是。 在复制组的每个成员的 "**属性**" 对话框的 "**高级**" 选项卡上配置暂存文件夹位置。

### <a name="when-are-files-staged"></a>何时暂存文件？

当接收成员请求文件时，文件会暂存到发送成员（除非文件为 64 KB 或更小），如下表所示。 如果在连接上禁用远程差分压缩（RDC），则会暂存该文件，除非该文件的大小为 256 KB 或更小。 文件也会在接收成员上暂存，因为如果这些文件的大小小于 64 KB，则它们也会暂存，尽管可以在 16 KB 到 1 MB 之间配置此设置。 如果计划已关闭，则不会暂存文件。

### <a name="the-minimum-file-sizes-for-staging-files"></a>暂存文件的最小文件大小

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>已启用 RDC</th>
<th>已禁用 RDC</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>正在发送成员</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>接收成员</p></td>
<td><p>默认为 64 KB</p></td>
<td><p>默认为 64 KB</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>如果文件在暂存之后但在完全传输到远程站点之前发生了更改，会发生什么情况？

如果文件的任何部分正在传输，DFS 复制将继续传输。 如果在 DFS 复制开始传输文件之前更改了文件，则将发送该文件的较新版本。

## <a name="change-history"></a>更改历史记录


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Date</th>
<th>描述</th>
<th>Reason</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>2018 年 11 月 15 日</p></td>
<td><p>已更新 Windows Server 2019。</p></td>
<td><p>新操作系统。</p></td>
</tr>
<tr class="even">
<td><p>2013 年 10 月 9 日</p></td>
<td><p>已更新 DFS 复制支持的限制是什么？包含来自 Windows Server 2012 R2 的测试结果的部分。</p></td>
<td><p>Windows Server 最新版本的更新</p></td>
</tr>
<tr class="odd">
<td><p>2013年1月30日</p></td>
<td><p>添加了在按计划或带宽限制配额禁用复制或手动禁用连接时，是否 DFS 复制继续暂存文件？条目.</p></td>
<td><p>客户问题</p></td>
</tr>
<tr class="even">
<td><p>2012年10月31日</p></td>
<td><p>编辑了 DFS 复制支持的限制是什么？用于增加卷上已测试的已复制文件数的条目。</p></td>
<td><p>客户反馈</p></td>
</tr>
<tr class="odd">
<td><p>2012年8月15日</p></td>
<td><p>编辑 DFS 复制是否复制 NTFS 文件权限、备用数据流、硬链接和重新分析点？用于进一步阐明 DFS 复制如何处理硬链接和重分析点的条目。</p></td>
<td><p>客户支持服务反馈</p></td>
</tr>
<tr class="even">
<td><p>2012年6月13日</p></td>
<td><p>编辑 DFS 复制对 ReFS 或 FAT 卷进行处理吗？用于添加对 ReFS 的讨论的条目。</p></td>
<td><p>客户反馈</p></td>
</tr>
<tr class="odd">
<td><p>2012年4月25日</p></td>
<td><p>编辑 DFS 复制是否复制 NTFS 文件权限、备用数据流、硬链接和重新分析点？用于阐明 DFS 复制如何处理硬链接的条目。</p></td>
<td><p>减少潜在的混乱</p></td>
</tr>
<tr class="even">
<td><p>2011年3月30日</p></td>
<td><p>编辑后 DFS 复制可以复制 Outlook .pst 或 Microsoft Office Access 数据库文件？用于更正将 DFS 复制与 .pst 和 Access 文件一起使用的潜在影响的条目。</p>
<p>添加了如何提高复制性能的方法？</p></td>
<td><p>有关前一项的客户问题，错误地指出复制 .pst 文件或访问文件可能会损坏 DFS 复制数据库。</p></td>
</tr>
<tr class="odd">
<td><p>2011年1月26日</p></td>
<td><p>添加了如何从 ConflictAndDeleted 或预先存在的文件夹恢复文件？</p></td>
<td><p>客户反馈</p></td>
</tr>
<tr class="even">
<td><p>2010年10月20日</p></td>
<td><p>添加了如何升级或替换 DFS 复制成员？</p></td>
<td><p>客户反馈</p></td>
</tr>
</tbody>
</table>


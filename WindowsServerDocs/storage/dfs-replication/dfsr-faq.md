---
Title: DFS 复制：常见问题 (FAQ)
ms.date: 06/18/2014
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 00bb3dfd79096e28f9752053152571ea9919edcf
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284268"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>DFS 复制：常见问题 (FAQ)


更新日期：2019 年 4 月 30日日

适用于：Windows Server 2019，Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

此 FAQ 解答有关 Windows Server 的分布式文件系统 (DFS) 复制 （也称为 DFS-R 或 DFSR） 的问题。

有关 DFS 命名空间的信息，请参阅[DFS 命名空间：Frequently Asked Questions](https://technet.microsoft.com/library/ee404780)。

有关 DFS 复制中新增的信息，请参阅以下主题：

  - [DFS 命名空间和 DFS 复制概述](https://technet.microsoft.com/library/jj127250)（在 Windows Server 2012)  
      
  - [什么是分布式文件系统中的新增功能](https://technet.microsoft.com/library/ee307957)中的主题[从 Windows Server 2008 到 Windows Server 2008 R2 的功能更改](https://technet.microsoft.com/library/dd391932)  
      
  - [分布式文件系统](https://technet.microsoft.com/library/cc753479)中的主题[从 Windows Server 2003 SP1 到 Windows Server 2008 的功能更改](https://technet.microsoft.com/library/cc753208)  
      

有关本主题最近更改的列表，请参阅本主题的[更改历史记录](#change-history)部分。

      

## <a name="interoperability"></a>互操作性

### <a name="can-dfs-replication-communicate-with-frs"></a>DFS 复制可以与 FRS 通信？

否。 DFS 复制不使用文件复制服务 (FRS) 进行通信。 DFS 复制和 FRS 可以运行在同一服务器上在同一时间，但必须永远不会将它们配置为复制相同的文件夹或子文件夹，因为这样做会导致数据丢失。

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>DFS 复制可以进行 SYSVOL 复制取代 FRS

是的 DFS 复制可以替换 FRS 用于 SYSVOL 复制运行 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 的服务器上。 运行 Windows Server 2003 R2 的服务器不支持使用 DFS 复制来复制 SYSVOL 文件夹。

有关使用 DFS 复制复制 SYSVOL 的详细信息，请参阅[SYSVOL 复制迁移指南：FRS 到 DFS 复制](https://technet.microsoft.com/library/dd640019)。

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>可以升级从 FRS 到 DFS 复制而不会丢失配置设置？

是。 若要将复制从 FRS 迁移到 DFS 复制，请参阅以下文档：

  - 若要迁移复制的 SYSVOL 文件夹之外的文件夹，请参阅[DFS 操作指南：从 FRS 向 DFS 复制迁移](http://go.microsoft.com/fwlink/?linkid=192776)并[FRS2DFSR – DFSR 迁移实用程序 FRS](http://go.microsoft.com/fwlink/?linkid=195437) (http://go.microsoft.com/fwlink/?LinkID=195437) 。  
      
  - 若要将复制的 SYSVOL 文件夹迁移到 DFS 复制，请参阅[SYSVOL 复制迁移指南：FRS 到 DFS 复制](https://technet.microsoft.com/library/dd640019)。  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>可以在混合的 Windows/UNIX 环境中使用 DFS 复制？

是。 尽管 DFS 复制仅支持运行 Windows Server 的服务器之间复制内容，但 UNIX 客户端可以访问的 Windows 服务器上的文件共享。 为此，请安装 DFS 复制服务器上的网络文件系统 (NFS) 服务。

此外可以使用 SMB/CIFS 客户端功能包括在许多 UNIX 客户端直接访问 Windows 文件共享，尽管此功能通常被限制或需要对 （如通过禁用 SMB 签名的 Windows 环境进行修改组策略）。

运行 Windows Server 操作系统的服务器上 DFS 复制 NFS 与互操作，但不能复制 NFS 装入点。

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>可以使用 DFS 复制使用卷影复制服务？

是。 卷影复制服务 (VSS) 卷上支持 DFS 复制和使用以前版本的客户端已成功还原以前的快照。

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>可以使用 Windows Backup (Ntbackup.exe) 已复制文件夹的远程备份？

否，使用 Windows Backup (Ntbackup.exe) 的运行 Windows Server 2003 的计算机上或更低，以便备份运行 Windows Server 2012 的计算机上的已复制文件夹的内容，Windows Server 2008 R2 或 Windows Server 2008 不支持。

若要备份存储在已复制文件夹中的文件，请使用 Windows Server Backup 或 Microsoft® System Center Data Protection Manager。 有关 Windows Server 2008 R2 和 Windows Server 2008 中的备份和恢复功能的信息，请参阅[备份和恢复](https://technet.microsoft.com/library/Cc754097)。 有关详细信息，请参阅[System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261) 。

### <a name="do-file-system-policies-impact-dfs-replication"></a>文件系统策略不会影响 DFS 复制？

是。 不要在已复制文件夹上配置文件系统策略。 文件系统策略重新应用在每个组策略刷新间隔的 NTFS 权限。 这可能导致共享冲突，因为在关闭文件之前不会复制打开的文件。

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>DFS 复制是否复制托管在 Microsoft Exchange Server 上的邮箱？

否。 不能使用 DFS 复制来复制托管在 Microsoft Exchange Server 上的邮箱。

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>DFS 复制不支持通过文件服务器资源管理器中创建文件屏蔽？

是。 但是，文件服务器资源管理器 (FSRM) 文件屏蔽设置必须复制两个边界匹配。 此外，DFS 复制具有其自身的文件和文件夹，可用于从复制中排除某些文件和文件类型的筛选器机制。

以下是实现文件屏蔽或配额的最佳做法：

  - 隐藏的 DfsrPrivate 文件夹不能受配额或文件屏蔽。  
      
  - 启用筛选之前外围的文件必须将不存在于任何已复制文件夹。  
      
  - 没有文件夹可能超过配额，然后才能启用配额。  
      
  - 必须谨慎使用硬配额。 可能的单个成员的复制组，以便不超出配额之前复制，但文件会在复制时，它超过它。 例如，如果用户将复制到服务器 A 上的 10 兆字节 (MB) 文件 （这是然后达到硬限制） 和另一个用户将复制到服务器 B 上, 一个 5 MB 的文件下, 一次复制时，这两个服务器将按 5 兆字节超出配额。 这会导致 DFS 复制以持续重试复制的文件，在版本向量和可能的性能问题导致漏洞。  
      

### <a name="is-dfs-replication-cluster-aware"></a>为 DFS 复制群集识别？

是，Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 中的 DFS 复制包括能够将故障转移群集添加为复制组的成员。 有关详细信息，请参阅[添加到复制组的故障转移群集](http://go.microsoft.com/fwlink/?linkid=155085)(http://go.microsoft.com/fwlink/?LinkId=155085) 。 在 Windows Server 2008 R2 之前的 Windows 版本上的 DFS 复制服务不旨在与故障转移群集，协调和该服务将无法故障转移到另一个节点。


> [!NOTE]
> DFS 复制不支持在群集共享卷上的复制文件。 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>是 DFS 复制兼容的重复数据删除？

是的 DFS 复制可以复制在 Windows Server 中使用重复数据删除的卷上的文件夹。

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>是 DFS 复制兼容的 RIS 和 WDS？

是。 DFS 复制将在其中启用了单实例存储 (SIS) 的卷。 远程安装服务 (RIS)、 Windows 部署服务 (WDS) 和 Windows Storage Server 使用 SIS。

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>是否可以使用 DFS 复制的脱机文件？

您可以安全地使用 DFS 复制和脱机文件一起在方案中时在写入到文件的时间只有一个用户。 这可用于两个分支机构之间往返穿行并想要能够访问他们在任一分支的文件或 while 用户脱机。 脱机文件缓存本地供脱机使用的文件和 DFS 复制不同的每个分支机构之间复制数据。

不要使用 DFS 复制的脱机文件在多用户环境中由于 DFS 复制不提供任何分布式的锁定机制或文件签出功能。 如果两个用户同时在不同的服务器上修改相同的文件，DFS 复制会将较早的文件移动到 DfsrPrivate\\ConflictandDeleted 文件夹 （位于已复制文件夹的本地路径下） 在下一步复制过程。

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>哪些防病毒应用程序是否与 DFS 复制兼容？

防病毒应用程序会导致过多的复制，如果其扫描活动更改已复制文件夹中的文件。 有关详细信息[测试防病毒应用程序的互操作性与 DFS 复制](http://go.microsoft.com/fwlink/?linkid=73990)(http://go.microsoft.com/fwlink/?LinkId=73990) 。

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>而不 Windows SharePoint Services 使用 DFS 复制的优点是什么？

Windows® SharePoint® Services 形式的文件签出功能 DFS 复制不提供紧密的一致性。 如果担心编辑同一个文件的多个人员，我们建议使用 Windows SharePoint Services。 Windows SharePoint Services 2.0 Service Pack 2 是可作为 Windows Server 2003 R2 的一部分。 可以从 Microsoft 网站; 下载 Windows SharePoint Services它不包含在较新版本的 Windows Server。 但是，如果将数据复制跨多个站点，并且用户将不在同一时间编辑相同的文件，DFS 复制提供更大的带宽和管理更为简单。

## <a name="limitations-and-requirements"></a>限制和要求

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>可以不使用 VPN 连接的分支机构之间复制 DFS 复制？

是的假定将分支机构连接的专用广域网 (WAN) 链接 (而不是 Internet)。 但是，您必须在外部防火墙中打开的适当端口。 DFS 复制使用 RPC 端点映射程序 （端口 135） 以及 1024年上随机分配的临时端口。 可以使用**Dfsrdiag**命令行工具，可指定静态端口而不是临时端口。 有关如何指定 RPC 端点映射程序的详细信息，请参阅[一文 154596](http://go.microsoft.com/fwlink/?linkid=73991)在 Microsoft 知识库文章 (http://go.microsoft.com/fwlink/?LinkId=73991) 。

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>DFS 复制可以复制使用加密文件系统加密的文件？

否。 DFS 复制不会复制文件或使用加密文件系统 (EFS) 加密的文件夹。 如果用户对先前复制的文件进行加密，DFS 复制的复制组的所有其他成员中删除文件。 这可确保文件的唯一可用的副本是在服务器上的加密的版本。

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>DFS 复制可以复制 Outlook.pst 或 Microsoft Office Access 数据库文件？

DFS 复制都可以安全地复制 Microsoft Outlook 个人文件夹的文件 (.pst) 和 Microsoft Access 文件仅当它们用于存档存储，且不访问整个网络的使用如 Outlook 或访问客户端 （若要打开.pst 或访问文件，首先将文件复制到本地存储设备）。 这样做的原因是，如下所示：

  - 通过网络连接打开的.pst 文件可能导致数据损坏.pst 文件中。 有关为什么.pst 文件不能安全地访问从跨网络的详细信息，请参阅[一文 297019](http://go.microsoft.com/fwlink/?linkid=125363)在 Microsoft 知识库文章 (http://go.microsoft.com/fwlink/?LinkId=125363) 。  
      
  - .pst 文件和访问文件往往会保持打开状态的时间，同时正在访问的客户端，例如 Outlook 或 Office Access 长时间。 这可以防止 DFS 复制复制这些文件，直至被关闭。  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>可以在工作组中使用 DFS 复制？

否。 DFS 复制依赖于 Active Directory® 域服务的配置。 它仅在域中工作。

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>可以在一台服务器上复制多个文件夹？

是。 DFS 复制可以在服务器之间复制的很多文件夹。 请确保每个已复制文件夹具有一个唯一根路径和它们不重叠。 例如，d:\\销售和 d:\\记帐可以是两个已复制的文件夹，但 d： 的根路径\\销售和 d:\\销售\\报表不能为两个已复制文件夹的根路径。

### <a name="does-dfs-replication-require-dfs-namespaces"></a>DFS 复制是否需要 DFS 命名空间？

否。 单独或一起，可以使用 DFS 复制和 DFS 命名空间。 此外，可以使用 DFS 复制来复制独立 DFS 命名空间，这是不可能实现 frs

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>DFS 复制是否需要服务器之间的时间同步？

否。 DFS 复制不会明确要求服务器之间的时间同步。 但是，DFS 复制需要服务器时钟与紧密匹配。 Kerberos 身份验证才能正常工作，必须在默认情况下的每个其他五分钟内设置服务器时钟。 例如，DFS 复制使用时间戳来确定哪些文件发生冲突时优先。 准确时间也是重要的垃圾回收、 计划和其他功能。

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>DFS 复制支持将复制整个卷？

是。 但是，您必须首先安装 Windows Server 2003 Service Pack 2 或修补程序。 有关详细信息，请参阅[一文 920335](http://go.microsoft.com/fwlink/?linkid=76776)在 Microsoft 知识库文章 (http://go.microsoft.com/fwlink/?LinkId=76776) 。 此外，复制整个卷，这种情况可能会导致以下问题：

  - 如果卷中包含 Windows 页面文件，复制将失败，并且在系统事件日志中记录 DFSR 事件 4312。  
      
  - DFS 复制的目标服务器上的已复制文件夹上设置的系统和 Hidden 属性。 因为 Windows 适用于在卷根文件夹的系统和 Hidden 属性默认情况下，将发生这种情况。 如果目标服务器上的已复制文件夹的本地路径也的卷根目录，进一步的更改是对文件夹属性。  
      
  - 当复制包含 Windows 系统文件夹的卷时，DFS 复制识别 %WINDIR%文件夹并不会复制它。 但是，DFS 复制 does 复制可能会失败的应用程序导致的目标服务器，如果应用程序具有与 DFS 复制的互操作性问题的非 Microsoft 应用程序所使用的文件夹。  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>DFS 复制支持 RPC over HTTP？

否。

### <a name="does-dfs-replication-work-across-wireless-networks"></a>DFS 复制的工作原理跨无线网络？

是。 DFS 复制是独立于连接类型。

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>DFS 复制是否适用于 ReFS 和 fat？

否。 DFS 复制支持使用; 仅在 NTFS 文件系统格式化的卷不支持弹性文件系统 (ReFS) 和 FAT 文件系统。 DFS 复制需要 NTFS，因为它使用 NTFS 变更日志和其他功能的 NTFS 文件系统。

### <a name="does-dfs-replication-work-with-sparse-files"></a>DFS 复制使用稀疏文件？

是。 可以复制稀疏文件。 **Sparse**属性保留在接收成员上。

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>我是否需要以管理员身份将文件复制？

否。 DFS 复制是一种服务，因此不需要以管理员身份将复制的本地系统帐户下运行。 但是，您必须是域管理员或受影响的文件服务器的本地管理员对 DFS 复制配置进行更改。

有关详细信息，请参阅"DFS 复制的安全要求和委派"中[委派管理 DFS 复制的功能](http://go.microsoft.com/fwlink/?linkid=182294)(http://go.microsoft.com/fwlink/?LinkId=182294) 。

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>如何升级或替换的 DFS 复制成员？

若要升级或替换的 DFS 复制成员时，请参阅此博客文章上询问目录服务团队博客：[替换 DFSR 成员硬件或操作系统](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx)。

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>DFS 复制是适用于复制漫游配置文件？

是。 复制漫游用户配置文件时，支持某些方案。 有关支持的方案的信息，请参阅[Microsoft 的支持语句周围复制用户配置文件数据](http://go.microsoft.com/fwlink/?linkid=201282)(http://go.microsoft.com/fwlink/?LinkId=201282) 。

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>是否存在文件字符的限制或文件夹深度限制？

Windows 和 DFS 复制支持包含最多 32 个字符的文件夹路径。 DFS 复制不局限于为 260 个字符的文件夹路径。

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>复制组的成员必须位于同一域中？

否。 复制组可以跨单个林中跨域但不是能跨不同的林中。

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>DFS 复制的受支持的限制是什么？

以下列表提供了一套经过了 Microsoft 在 Windows Server 2012 R2 上的可伸缩性指南：

  - 在服务器上所有复制的文件大小：100 兆兆字节。  
      
  - 复制的卷上的文件数：70 万个。  
      
  - 最大文件大小：250 千兆字节为单位。  
      


> [!IMPORTANT]
> 使用大量或文件的大小创建复制组时，我们建议导出的数据库克隆和使用预种子设定的技术来尽量减少初始复制的持续时间。 有关详细信息，请参阅<A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">Windows Server 2012 R2 中的 DFS 复制在初始同步：克隆</A>。 
<br>


以下列表提供了一套经过了 Microsoft 在 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008 上的可伸缩性指南：

  - 在服务器上所有复制的文件大小：10 千吉字节。  
      
  - 复制的卷上的文件数：11 万个。  
      
  - 最大文件大小：64 千兆字节为单位。  
      


> [!NOTE]
> 不再为复制组、 已复制的文件夹、 连接或复制组成员数限制。 
<br>


Microsoft 为 Windows Server 2003 R2 进行了测试的可伸缩性指南的列表，请参阅[DFS 复制可伸缩性指导原则](http://go.microsoft.com/fwlink/?linkid=75043)(http://go.microsoft.com/fwlink/?LinkId=75043) 。

### <a name="when-should-i-not-use-dfs-replication"></a>何时不应使用 DFS 复制？

不要在多个用户更新或修改同时在不同的服务器上的相同文件的位置的环境中使用 DFS 复制。 执行此操作可能会导致 DFS 复制将冲突的文件的副本移动到隐藏 DfsrPrivate\\ConflictandDeleted 文件夹。

当多个用户需要在不同的服务器上同时修改相同的文件时，使用 Windows SharePoint Services 文件签出功能以确保只有一个用户正在运行的文件。 Windows SharePoint Services 2.0 Service Pack 2 是可作为 Windows Server 2003 R2 的一部分。 可以从 Microsoft 网站; 下载 Windows SharePoint Services它不包含在较新版本的 Windows Server。

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>为什么是 DFS 复制所需的架构更新？

DFS 复制使用新对象的 Active Directory 域服务域命名上下文中存储的配置信息。 更新 Active Directory 域服务架构时，会创建这些对象。 有关详细信息，请参阅[DFS 复制的评审要求](http://go.microsoft.com/fwlink/?linkid=182264)(http://go.microsoft.com/fwlink/?LinkId=182264) 。

## <a name="monitoring-and-management-tools"></a>监视和管理工具

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>可以自动执行运行状况报告，以将会收到警告？

是。 有三种方法来自动执行运行状况报告：

  - 与任务计划程序一起包含在 Windows Server 2012 R2 或 DfsrAdmin.exe DFSR Windows PowerShell 模块用于定期生成运行状况报告。 有关详细信息，请参阅[自动执行 DFS 复制运行状况报告](http://go.microsoft.com/fwlink/?linkid=74010)(http://go.microsoft.com/fwlink/?LinkId=74010) 。  
      
  - 使用 System Center Operations Manager 在 DFS 复制管理包创建基于指定条件的警报。  
      
  - 使用 DFS 复制 WMI 提供程序编写警报脚本。  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>可以使用 Microsoft System Center Operations Manager 来监视 DFS 复制？

是。 有关详细信息，请参阅[适用于 System Center Operations Manager 2007 的 DFS 复制管理包](http://go.microsoft.com/fwlink/?linkid=182265)中 Microsoft Download Center (http://go.microsoft.com/fwlink/?LinkId=182265) 。

### <a name="does-dfs-replication-support-remote-management"></a>DFS 复制是否支持远程管理？

是。 DFS 复制支持远程管理使用 DFS 管理控制台和**添加复制组**命令。 例如，在服务器 A 上，你可以连接到服务器 A 和 B 作为成员林中定义的复制组。

包含 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Server 2003 R2 DFS 管理。 若要从其他版本的 Windows 管理 DFS 复制，请使用远程桌面或[远程服务器管理工具为 Windows 7](https://technet.microsoft.com/library/Ee449475)。


> [!IMPORTANT]
> 若要查看或管理包含只读复制的文件夹或故障转移群集的成员的复制组，必须使用随 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、的DFS管理的版本<a href="http://go.microsoft.com/fwlink/p/?linkid=238560">适用于 Windows 8 的远程服务器管理工具</a>，或<a href="https://technet.microsoft.com/library/ee449475">的 Windows 7 远程服务器管理工具</a>。 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound 和 Sonar 适用于 DFS 复制？

否。 DFS 复制具有其自己的监视和诊断工具集。 Ultrasound 和 Sonar 才能够监视 FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>从 ConflictAndDeleted 或 PreExisting 文件夹可以如何恢复文件？

若要恢复丢失的文件，请将文件还原从文件系统文件夹或使用文件历史记录中的共享的文件夹**还原以前的版本**命令在文件资源管理器，或通过从备份中还原文件。 若要直接从 ConflictAndDeleted 或 PreExisting 文件夹中恢复文件，请使用`Get-DfsrPreservedFiles`并`Restore-DfsrPreservedFiles`Windows PowerShell cmdlet （包含在 Windows Server 2012 R2 中的 DFSR 模块），或[RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr)从 MSDN 代码库的示例脚本。 此脚本仅用于灾难恢复，并提供了 AS 的是，不含任何保证。

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>是否有方法可以知道的复制状态？

是。 有多种方法来监视复制：

  - DFS 复制具有可主动监视的 System Center Operations Manager 管理包。  
      
  - DFS 管理具有内置诊断报告复制积压工作、 复制效率和给定的复制组中文件和文件夹的数量。  
      
  - Windows Server 2012 R2 中的 DFSR Windows PowerShell 模块包含用于启动传播测试和写入传播和运行状况报告的 cmdlet。 有关详细信息，请参阅[分发在 Windows PowerShell 文件系统复制 Cmdlet](https://technet.microsoft.com/library/dn296601.aspx)。  
      
  - Dfsrdiag.exe 是一个命令行工具，可以生成积压工作计数或触发器传播测试。 同时显示复制的状态。 传播演示文件会复制到所有节点。 积压工作将显示文件的数量仍需要复制之前两台计算机都保持同步。积压工作计数为复制组成员未处理的更新数。 在计算机上运行 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2，Dfsrdiag.exe 还可以显示当前复制 DFS 复制的更新。  
      
  - 脚本可以使用 WMI 来收集积压工作的信息，手动或通过 MOM。  
      

## <a name="performance"></a>性能

### <a name="does-dfs-replication-support-dial-up-connections"></a>DFS 复制是否支持拨号连接？

虽然 DFS 复制也能以拨号的速度，可以获取积压，如果有大量复制更改。 如果对现有文件进行了较小的更改，DFS 复制使用远程差分压缩 (RDC) 将提供更高性能比直接复制该文件。

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>DFS 复制执行带宽感知？

否。 DFS 复制不会执行带宽感知。 你可以配置 DFS 复制 （带宽限制） 在每个连接基础上使用有限的带宽量。 但是，DFS 复制不会进一步减少带宽利用率如果网络接口变为满负荷状态，DFS 复制可以短时间内耗尽了链接。 带宽限制与 DFS 复制不完全准确，因为 DFS 复制限制带宽受限制的 RPC 调用。 因此，较低级别的网络堆栈 （包括 RPC） 中的各种缓冲区可能会影响，从而导致的网络流量突发状况。

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>DFS 复制是否限制每个计划，每个服务器，或每个连接的带宽？

如果您配置带宽限制时指定的计划，该复制组的所有连接将都使用该设置带宽限制。 带宽限制可以也设置为使用 DFS 管理的连接级设置。

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>不 DFS 复制使用 Active Directory 域服务来计算站点链接和连接成本？

否。 DFS 复制使用由管理员独立于 Active Directory 域服务站点的成本计算定义的拓扑。

### <a name="how-can-i-improve-replication-performance"></a>如何提高复制性能？

若要了解有关优化复制性能的不同方法，请参阅[DFSR 中优化复制性能](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx)上[询问目录服务团队博客](http://blogs.technet.com/b/askds/)。

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>DFS 复制如何避免饱和连接？

在 DFS 复制设置你想要连接时，使用的最大带宽和服务维护的网络使用该级别。 这与不同，从后台智能传输服务 (BITS)，并相应地设置时，DFS 复制不会不使饱和连接。

尽管如此，带宽限制不是 100%准确，DFS 复制可以短时间内耗尽了链接。 这是因为 DFS 复制限制带宽受限制的 RPC 调用。 因为此过程依赖于各种缓冲区中的网络堆栈，包括 RPC、 较低级别的复制流量往往会爆发的可能会使网络链接有时饱和的传输。

Windows Server 2008 中的 DFS 复制包括多项性能增强功能，如中所述[分布式文件系统](https://technet.microsoft.com/library/Cc753479)，在主题[从 Windows Server 2003 SP1 到 Windows Server 的功能更改2008](https://technet.microsoft.com/library/cc753208)。

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>DFS 复制的性能与 FRS 相比如何？

尤其是当对大文件进行少量更改，并且已启用 RDC，DFS 复制是比 FRS，快得多。 例如，使用 RDC，向 2 MB PowerPoint® 演示文稿稍微进行更改可能会导致仅 60 千字节 (KB) 正在跨网络发送，以字节为单位传输 97%的成本。

RDC 未使用的文件小于 64 KB，可能不为有益高速 Lan 上其中不争用网络带宽。 可以使用 DFS 管理在每个连接基础上禁用 RDC。

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>DFS 复制频率复制数据？

数据将根据你所设置的计划复制。 例如，您可以将计划设置为 15 分钟间隔一周七天。 在这些间隔期间启用复制。 （通常在秒内） 检测到文件更改之后立即开始复制。

复制组计划可能会为通用时间坐标 (UTC) 设置时的连接计划设置为接收成员的本地时间。 考虑到这一点复制组跨越多个时区时。 本地时间表示托管的入站的连接的成员的时间。 入站的连接和相应的出站连接的显示的计划的计划设置为本地时间时反映时区差异。

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>我的服务器的系统资源量将 DFS 复制使用？

磁盘、 内存和 CPU 资源由 DFS 复制取决于多种因素，包括的数量和大小的文件、 更改、 复制组成员的数量和已复制文件夹的数的速率。 此外，一些资源，越难以估计。 例如，使用 DFS 复制数据库的可扩展存储引擎 (ESE) 技术可能会占用了大量的可用内存，它按需释放。 可以根据服务器配置在同一服务器上托管 DFS 复制之外的其他应用程序。 但是，当承载多个应用程序或单个服务器上的服务器角色时，是重要测试之前实现在生产环境中的此配置。

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>如果 WAN 链路失败在复制期间，会发生什么情况？

如果连接出现故障，DFS 复制将继续尝试复制计划处于打开状态时。 此外将可以使用 MOM （主动通过警报） 和 DFS 复制运行状况报告 （被动，如当管理员运行它） 获得 DFS 复制事件日志中所述的连接错误。

## <a name="remote-differential-compression-details"></a>远程差分压缩的详细信息

### <a name="are-changes-compressed-before-being-replicated"></a>正在复制之前压缩更改？

是。 除以下内容 （其中已压缩） 之外的类型的所有文件在发送之前压缩的文件已更改的部分：.wma、.wmv、.zip、.jpg、.mpg、.mpeg、.m1v、.mp2、.mp3、.mpa、.cab、.wav、.snd、.au、.asf、.wm、.avi、.z、.gz、.tgz 和.frx。 这些文件类型的压缩设置不是可在 Windows Server 2003 R2 中配置的。

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>管理员可以关闭 RDC 或更改的阈值？

是。 您可以通过给定连接的属性页将关闭 RDC。 禁用 RDC 可以减少 CPU 使用率和复制延迟在快速局域网 (LAN) 链接中不存在任何带宽约束或为主要包含小于 64 KB 的文件的复制组。 如果你选择在连接上禁用 RDC，测试复制效率之前和之后的更改来验证改进复制性能。

可以使用更改 RDC 大小阈值**Dfsradmin 连接设置**命令时，DFS 复制 WMI 提供程序，或通过手动编辑配置 XML 文件。

### <a name="does-rdc-work-on-all-file-types"></a>RDC 是否适用于所有文件类型？

是。 RDC 计算在块级别而不考虑文件数据类型的差异。 但是，RDC 更有效地适用于特定文件类型，例如 Word 文档、 PST 文件和 VHD 映像。

### <a name="how-does-rdc-work-on-a-compressed-file"></a>RDC 如何压缩文件？

DFS 复制使用 RDC，计算该文件的已更改，并且通过网络发送只会将这些块的块。 DFS 复制不需要知道有关该文件的内容的任何信息 — 仅哪些块已更改。

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>是跨文件 RDC 时升级到 Windows Server Enterprise Edition 或 Datacenter Edition，启用？

Windows Server Standard Edition 不支持跨文件 RDC。 但是，它会自动启用时升级到版本支持跨文件 RDC，或复制连接的成员正在运行的受支持的版本。 支持跨文件 RDC 版本的列表，请参阅哪些版本的 Windows 操作系统支持的跨文件 RDC？

### <a name="is-rdc-true-block-level-replication"></a>是 RDC true 块级别的复制？

否。 RDC 是一种通用协议的压缩文件传输。 DFS 复制在块在文件级别，但不能在磁盘块级别上使用 RDC。 RDC 将文件划分为块。 对于文件中每个块，它会计算一个签名，这是少量的可表示更大的块的字节数。 签名集从服务器传输到客户端。 客户端将对其自己的服务器签名进行比较。 客户端然后，此请求服务器发送只尚不在客户端的签名的数据。

### <a name="what-happens-if-i-rename-a-file"></a>如果我重命名文件，会发生什么情况？

DFS 复制将在下一步复制期间重命名的复制组的所有其他成员上的文件。 文件是跟踪使用的唯一 ID，因此重命名文件和移动文件副本中的没有任何影响 DFS 复制能够将复制文件。

### <a name="what-is-cross-file-rdc"></a>什么是跨文件 RDC？

跨文件 RDC 使 DFS 复制使用 RDC，即使在客户端不存在具有相同名称的文件。 跨文件 RDC 使用启发式方法来确定类似于需要复制，该文件的文件，并通过 WAN 传输到复制的文件，以最大程度减少数据量完全相同的类似文件的使用块。 跨文件 RDC 可以使用此过程中最多五个相似的文件块。

若要使用的跨文件 RDC，复制连接的一个成员必须运行 Windows 的一个版本，支持跨文件 RDC。 支持跨文件 RDC 版本的列表，请参阅哪些版本的 Windows 操作系统支持的跨文件 RDC？

### <a name="what-is-rdc"></a>什么是 RDC？

远程差分压缩 (RDC) 是一种客户端-服务器协议，可用于通过带宽有限的网络有效更新文件。 RDC 检测插入、 删除和重新排列的数据文件中，启用 DFS 复制，以更新文件时复制的更改。 默认情况下，RDC 是仅用于为 64 KB 的文件或更大。 RDC 可以使用旧版的文件具有相同名称的已复制文件夹中或在 DfsrPrivate\\ConflictandDeleted 文件夹 （位于已复制文件夹的本地路径下）。

### <a name="when-is-rdc-used-for-replication"></a>RDC 时用于复制？

文件超出了最小大小阈值时，使用 RDC。 默认情况下，此大小阈值为 64 KB。 已复制的文件超过该阈值后，该文件的更新的版本将始终使用 RDC，除非有很大一部分文件已更改，或禁用 RDC。

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>哪些版本的 Windows 操作系统支持的跨文件 RDC？

若要使用的跨文件 RDC，复制连接的一个成员必须运行 Windows 操作系统的一个版本，支持跨文件 RDC。 下表显示了哪些版本的 Windows 操作系统支持的跨文件 RDC。

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>跨文件 RDC 的 Windows 操作系统版本中的可用性

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
<th>数据中心版</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>是的<em></p></td>
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

\* 可以选择性地禁用跨文件 RDC Windows Server 2012 R2 上的。

## <a name="replication-details"></a>复制详细信息

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>在创建后是否可以更改已复制文件夹的路径？

否。 如果需要更改已复制文件夹的路径，必须在 DFS 管理中删除，并将其添加为新的已复制文件夹。 DFS 复制使用远程差分压缩 (RDC) 来执行同步，用于确定数据是否发送和接收成员上相同。 它不会再次复制文件夹中的所有数据。

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>可以配置会复制哪些文件的属性吗？

否，不能配置 DFS 复制可以复制哪些文件属性。

属性值和及其说明的列表，请参阅[文件属性](http://go.microsoft.com/fwlink/?linkid=182268)MSDN 上 (http://go.microsoft.com/fwlink/?LinkId=182268) 。

使用设置下列属性值`SetFileAttributes dwFileAttributes`函数，并且它们会通过 DFS 复制复制。 对这些属性值的更改触发复制的属性。 除非内容更改也不会复制该文件的内容。 有关详细信息，请参阅[SetFileAttributes 函数](http://go.microsoft.com/fwlink/?linkid=182269)MSDN 库中 (http://go.microsoft.com/fwlink/?LinkId=182269) 。

  - 文件\_特性\_隐藏  
      
  - 文件\_特性\_READONLY  
      
  - 文件\_特性\_系统  
      
  - 文件\_特性\_不\_内容\_索引  
      
  - 文件\_特性\_脱机  
      

通过 DFS 复制，复制下列属性值，但它们不会触发复制。

  - 文件\_特性\_存档  
      
  - 文件\_特性\_正常  
      

下列文件属性值也触发复制，但不能使用设置`SetFileAttributes`函数 (使用`GetFileAttributes`函数查看属性值)。

  - 文件\_特性\_重新分析\_点  
      

> [!NOTE]
> DFS 复制不会复制重分析点属性值，除非重分析标记是 IO_REPARSE_TAG_SYMLINK。 具有 io_reparse_tag_dedup 进行复制、 IO_REPARSE_TAG_SIS 或 IO_REPARSE_TAG_HSM 重分析标记的文件将复制为普通文件。 但是，重分析标记和重新分析数据缓冲区将不复制到其他服务器由于重分析点仅适用于本地系统。 
<br>

  - 文件\_特性\_压缩  
      
  - 文件\_特性\_加密  
      

> [!NOTE]
> DFS 复制不会复制使用加密文件系统 (EFS) 加密的文件。 DFS 复制 does 复制加密通过使用非 Microsoft 软件，但只有当不在该文件上设置 FILE_ATTRIBUTE_ENCRYPTED 属性值的文件。 
<br>

  - 文件\_特性\_SPARSE\_文件  
      
  - 文件\_特性\_目录  
      

DFS 复制不会复制该文件\_特性\_临时值。

### <a name="can-i-control-which-member-is-replicated"></a>可以控制复制哪些成员？

是。 创建复制组时，可以选择一种拓扑。 也可以选择**没有拓扑**，并在创建复制组之后手动配置连接。

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>可以设置种子之前的初始复制数据的复制组成员？

是。 DFS 复制支持文件复制到复制组成员之前的初始复制。 此"预留"可以显著减少在初始复制期间复制的数据量。

初始复制不需要文件的差异仅在于实际的属性或时间戳时，复制内容。 实际的属性是可以通过 Win32 函数设置的属性`SetFileAttributes`。 有关详细信息，请参阅[SetFileAttributes 函数](http://go.microsoft.com/fwlink/?linkid=182269)MSDN 库中 (http://go.microsoft.com/fwlink/?LinkId=182269) 。 如果两个文件不同的其他属性，例如压缩，然后复制该文件的内容。

若要预安排复制组成员，将文件复制到目标服务器上相应的文件夹、 创建复制组，然后选择主成员。 选择具有你想要复制，因为主成员的内容被视为"具有权威。"的最新文件的成员 这意味着，在初始复制期间主成员的文件将始终覆盖其他版本的复制组的其他成员上的文件。

有关预先播种和克隆 DFSR 数据库的信息，请参阅[Windows Server 2012 R2 中的 DFS 复制在初始同步：克隆](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx)。

有关初始复制的详细信息，请参阅[创建一个复制组](https://technet.microsoft.com/library/cc725893)。

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>DFS 复制是否解决常见的文件复制服务问题呢？

是。 DFS 复制克服了三个常见的 FRS 问题：

  - 日志包装：从动态日志包装中 DFS 复制恢复。 将标记为 journalWrap 并再次启用复制之前验证文件系统对每个现有的文件或文件夹。 在恢复期间，此卷不是可用于在任一方向的复制。  
      
  - 过多的复制：若要防止过多的复制，DFS 复制使用的信用额度的系统。  
      
  - 变形的文件夹：若要防止变形的文件夹名称，DFS 复制将存储发生冲突的数据中隐藏 DfsrPrivate\\ConflictandDeleted 文件夹 （位于已复制文件夹的本地路径下）。 例如，与使用 FRS 复制的其他服务器上的相同名称的同时创建多个文件夹会导致 FRS 重命名的较旧的文件夹。 DFS 复制而是将较旧的文件夹移动到本地的冲突和已删除文件夹。  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>DFS 复制将按时间顺序的文件复制？

否。 可能不按顺序复制文件。

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>DFS 复制将正在使用的另一个应用程序的文件复制？

如果应用程序打开文件并对其 （阻止它正由其他应用程序打开时） 创建文件锁，DFS 复制不会复制该文件，直到将其关闭。 如果应用程序打开该文件具有读取共享访问权限，仍可以复制该文件。

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>DFS 复制复制 NTFS 文件权限、 备用数据流、 硬链接和重新分析点的？

  - DFS 复制可以复制 NTFS 文件权限和备用数据流。  
      
  - Microsoft 不支持在已复制文件夹中创建 NTFS 硬链接到或从文件，这样会导致复制问题与受影响的文件。 硬链接文件通过 DFS 复制将忽略和不复制。 交接点也不复制，和 DFS 复制日志事件 4406 遇到每个交接点。  
      
  - 仅复制 DFS 复制的重新分析点是指那些使用 IO\_重新分析\_标记\_符号链接标记; 但是，DFS 复制不保证还复制符号链接的目标。 有关详细信息，请参阅[询问目录服务团队博客](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx)。  
      
  - 文件 IO\_重新分析\_标记\_重复数据删除、 IO\_重新分析\_标记\_SIS 或 IO\_重新分析\_标记\_复制 HSM 重分析标记作为常规文件。 因为重新分析点仅适用于本地系统到其他服务器不复制重分析标记和重新分析数据缓冲区。 在这种情况下，DFS 复制可以复制在使用 Windows Server 2012 或单实例存储 (SIS) 中重复数据删除的卷上的文件夹，但是，数据重复删除信息在其上启用了角色服务的每个服务器单独维护。  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>如果对文件不进行任何其他更改，不 DFS 复制复制时间戳的更改？

否，DFS 复制不会复制的文件的唯一更改是对时间戳的更改。 此外，已更改的时间戳不会复制到复制组的其他成员除非对文件进行其他更改。

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>DFS 复制复制更新的权限在文件或文件夹？

是。 DFS 复制可以复制对文件和文件夹的权限更改。 只有关联与访问控制列表 (ACL) 的文件的部分被复制，尽管 DFS 复制仍必须读取整个文件到临时区域。


> [!NOTE]
> 更改上大量的文件的 Acl 可以对复制性能产生影响。 但是，在使用 RDC，传输的数据量是 Acl，而不是整个文件的大小的大小成比例。 因为必须读取文件，与暂存文件夹的磁盘流量仍文件的大小成正比。 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>DFS 复制是否支持合并文本文件发生冲突？

冲突时，DFS 复制不会合并文件。 但是，它会尝试保留隐藏 DfsrPrivate 中的文件的较旧版本\\ConflictandDeleted 文件夹在计算机上的检测到冲突。

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>传输数据时，不 DFS 复制使用加密？

是。 DFS 复制使用远程过程调用 (RPC) 连接使用加密。

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>是否可以禁用加密 RPC 使用？

否。 DFS 复制服务通过 TCP 使用远程过程调用 (RPC) 复制数据。 若要在 Internet 上保护数据传输，DFS 复制服务设计为始终使用身份验证级别的常数， `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`。 这可确保在 Internet 上的 RPC 通信始终加密。 因此，不能禁用加密 RPC 使用的 DFS 复制服务。

有关详细信息，请参阅以下 Microsoft 网站：

  - [RPC 技术参考](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [有关远程差分压缩](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [身份验证级别的常数](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>如何处理同时进行的复制？

没有每个已复制文件夹的一个更新管理器。 更新独立于另一个管理器工作。

默认情况下，最多为 16 (四个在 Windows Server 2003 R2) 之间的所有连接和复制组共享的并发下载。 连接和复制组更新会序列化，因为没有收到更新任何特定顺序。 如果两个计划处于打开状态，更新通常接收和安装在同一时间来自这两个连接。

### <a name="how-do-i-force-replication-or-polling"></a>如何强制复制或轮询？

您可以强制立即复制副本使用 DFS 管理中所述[编辑复制计划](https://technet.microsoft.com/library/Cc732278)。 您也可以使用强制复制`Sync-DfsReplicationGroup`借助 Windows Server 2012 R2，引入了对 DFSR PowerShell 模块中包含的 cmdlet 或**Dfsrdiag SyncNow**命令。 可以使用强制轮询`Update-DfsrConfigurationFromAD`cmdlet，或**Dfsrdiag pollad /** 命令。

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>是否可以配置为频繁更改的文件的复制间隔的安静时间？

否。 如果计划处于打开状态，DFS 复制会复制更改，如发现它们。 没有方法来配置文件的安静时间。

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>是否可以使用 DFS 复制配置单向复制？

是。 如果使用 Windows Server 2012 或 Windows Server 2008 R2，您可以创建只读的已复制的文件夹，以复制通过单向连接的内容。 有关详细信息，请参阅[特定成员上进行复制文件夹设置为只读](http://go.microsoft.com/fwlink/?linkid=156740)(http://go.microsoft.com/fwlink/?LinkId=156740) 。

我们不支持使用 Windows Server 2008 或 Windows Server 2003 R2 中的 DFS 复制创建单向复制连接。 执行此操作可能会导致许多问题，包括运行状况检查拓扑错误、 暂存问题和 DFS 复制数据库中的问题。

如果使用 Windows Server 2008 或 Windows Server 2003 R2，你可以通过执行以下操作来模拟单向连接：

  - 培训管理员仅在你想要指定为主服务器的服务器上进行更改。 然后使所做的更改复制到目标服务器。  
      
  - 目标服务器上配置共享权限，以便最终用户没有写入权限。 如果不允许更改上的分支服务器，则不需要复制回来，模拟单向连接并降低 WAN 利用率。  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>有一种方法来强制完成的所有文件包括未更改的文件复制吗？

否。 如果 DFS 复制将文件完全相同，则它不会复制它们。 如果尚未更改的文件中复制，DFS 复制会自动将它们复制配置为执行此操作时。 若要覆盖配置的计划，请使用 WMI 方法**ForceReplicate()** 。 但是，这是仅计划重写，并不会强制保持不变或相同的文件的复制。

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>如果在初始复制期间主成员出现数据库丢失，则会发生什么情况？

在初始复制期间主成员的文件将始终优先中，如果接收成员包含的不同版本的主成员上的文件，则会发生在解决冲突。 主成员标志存储在 Active Directory 域服务，并指定清除主成员准备就绪，若要复制之后, 但在之前复制的所有成员都组复制。

如果初始复制失败或 DFS 复制服务在复制过程将重新启动，主成员可以看到在本地的 DFS 复制数据库中主成员标志，然后重试初始复制。 如果主成员的 DFS 复制数据库清除 Active Directory 域服务中指定的主后将会丢失，但到复制组的所有成员都完成初始复制之前，失败的复制组的所有成员复制文件夹，因为没有服务器被指定为主成员。 如果发生这种情况，使用**Dfsradmin 成员身份设置/isprimary:true/** 命令手动还原主成员标志的主成员服务器上。

有关初始复制的详细信息，请参阅[创建一个复制组](https://technet.microsoft.com/library/cc725893)。


> [!WARNING]
> 仅在初始复制过程中使用主成员标志。 如果您使用<STRONG>Dfsradmin</STRONG>命令指定的主成员的已复制文件夹复制后已完成，则 DFS 复制不会不将服务器指定为 Active Directory 域服务中的主成员。 但是，如果服务器上的 DFS 复制数据库随后出现不可逆的损坏或数据丢失，则服务器将尝试与主成员而不是从另一个成员的复制恢复其数据执行初始复制组。 从根本上来说，服务器将成为恶意主服务器，这可能导致冲突。 出于此原因，主成员手动指定仅确定如果无法解决该失败的初始复制。 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>如果复制计划关闭复制文件时，会发生什么情况？

如果在连接上启用远程差分压缩 (RDC)，则入站复制大于开始复制之前计划结束的那一刻的 64 KB 的文件 (或者更改为**没有带宽**) 将继续时计划打开 (或者对某些内容而不更改**没有带宽**)。 复制将继续从停止复制时的状态。

如果 RDC 处于关闭状态，DFS 复制完全重新启动文件传输。 这可能会延迟文件时，可在接收成员上。

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>当两个用户同时更新不同的服务器上的相同文件时，会发生什么情况？

当 DFS 复制检测到冲突时，它使用上一次保存该文件的版本。 它将另一个文件移至 DfsrPrivate\\ConflictandDeleted 文件夹 （位于解决了冲突的计算机上的已复制文件夹的本地路径）。 之前仍会保留那里冲突和已删除文件夹清理，时发生的冲突和已删除文件夹超出了配置的大小或 DFS 复制遇到磁盘空间不足错误。 不复制冲突和已删除文件夹，并且这种冲突解决方法可以避免可能在 FRS.变迁目录的问题

当发生冲突时，DFS 复制到 DFS 复制事件日志记录信息性事件。 由于以下原因，此事件不需要用户执行任何操作：

  - 不是对 （它是仅对服务器管理员可见） 的用户可见的。  
      
  - DFS 复制将缓存视为冲突和已删除文件夹。 当达到配额阈值时，该向导会清除这些文件的一些。 则将保存冲突的文件不能保证。  
      
  - 冲突无法驻留在不同的冲突的源服务器上。  
      

## <a name="staging"></a>分步

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>DFS 复制会继续按计划或带宽限制配额，请禁用复制或者连接被手动禁用暂存文件？

否。 DFS 复制或如果尚未超出带宽限制配额，则将禁用的连接时继续计划的复制时间之外的暂存文件。

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>DFS 复制是否防止在临时过程访问的文件的其他应用程序？

否。 DFS 复制中不会阻止用户或应用程序在复制文件夹中打开文件从一种方法打开文件。 此方法称为"伺机锁定"。

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>是否可以更改暂存文件夹使用 DFS 管理工具的位置？

是。 在上配置的临时文件夹位置**高级**选项卡**属性**复制组的每个成员的对话框。

### <a name="when-are-files-staged"></a>何时暂存文件？

文件暂存的发送成员时接收成员请求文件 (除非文件为 64 KB 或更小) 下, 表中所示。 除非它是 256 KB，如果在连接上禁用远程差分压缩 (RDC)，则暂存该文件或更小。 文件还会在接收成员上暂存，在传输它们是否小于 64 KB 的大小，尽管您可以配置此设置 16 KB 和 1 MB 之间。 如果计划已关闭，不会暂存文件。

### <a name="the-minimum-file-sizes-for-staging-files"></a>临时文件的最小文件大小

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
<th>RDC 禁用</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>发送成员</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>接收成员</p></td>
<td><p>默认情况下的 64 KB</p></td>
<td><p>默认情况下的 64 KB</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>如果文件发生更改时，它分阶段之后但尚未完全传输到远程站点之前，会发生什么情况？

如果已传输的文件的任何部分时，DFS 复制将继续传输。 如果 DFS 复制开始传输文件之前，在更改文件，然后发送较新版本的文件。

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
<td><p>更新适用于 Windows Server 2019。</p></td>
<td><p>新的操作系统。</p></td>
</tr>
<tr class="even">
<td><p>2013 年 10 月 9 日</p></td>
<td><p>更新的 DFS 复制受支持的限制是什么？从 Windows Server 2012 R2 上的测试结果的部分。</p></td>
<td><p>最新版本的 Windows Server 更新</p></td>
</tr>
<tr class="odd">
<td><p>2013 年 1 月 30日日</p></td>
<td><p>添加不 DFS 复制继续按计划或带宽限制配额，请禁用复制或者连接被手动禁用暂存文件？项。</p></td>
<td><p>客户的问题</p></td>
</tr>
<tr class="even">
<td><p>2012 年 10 月 31日日</p></td>
<td><p>编辑 DFS 复制的支持的限制是什么？若要增加的卷上复制文件经过测试的数量的条目。</p></td>
<td><p>客户反馈</p></td>
</tr>
<tr class="odd">
<td><p>2012 年 8 月 15日日</p></td>
<td><p>编辑没有 DFS 复制复制 NTFS 文件权限、 备用数据流、 硬链接和重新分析点？条目来进一步阐明 DFS 复制如何处理硬链接和重新分析点。</p></td>
<td><p>从客户支持服务的反馈</p></td>
</tr>
<tr class="even">
<td><p>2012 年 6 月 13日日</p></td>
<td><p>编辑 ReFS 或 FAT 卷上的没有 DFS 复制工作？要添加的引用的讨论的项。</p></td>
<td><p>客户反馈</p></td>
</tr>
<tr class="odd">
<td><p>2012 年 4 月 25日日</p></td>
<td><p>编辑没有 DFS 复制复制 NTFS 文件权限、 备用数据流、 硬链接和重新分析点？若要阐明如何 DFS 复制处理硬链接的项。</p></td>
<td><p>减少潜在混淆</p></td>
</tr>
<tr class="even">
<td><p>2011 年 3 月 30日日</p></td>
<td><p>编辑可 DFS 复制复制 Outlook.pst 或 Microsoft Office Access 数据库文件？若要更正的 DFS 复制使用.pst 文件和访问文件的潜在影响的项。</p>
<p>添加如何提高复制性能？</p></td>
<td><p>客户错误地指示，.pst 或访问的文件复制可能会损坏 DFS 复制数据库的上一项有关的问题。</p></td>
</tr>
<tr class="odd">
<td><p>2011 年 1 月 26日日</p></td>
<td><p>添加如何文件恢复从 ConflictAndDeleted 或预先存在的文件夹？</p></td>
<td><p>客户反馈</p></td>
</tr>
<tr class="even">
<td><p>2010 年 10 月 20日日</p></td>
<td><p>添加如何升级或替换的 DFS 复制成员？</p></td>
<td><p>客户反馈</p></td>
</tr>
</tbody>
</table>


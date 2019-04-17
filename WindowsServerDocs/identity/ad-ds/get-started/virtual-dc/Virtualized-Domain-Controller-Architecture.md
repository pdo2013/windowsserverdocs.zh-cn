---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: "虚拟化的域控制器建筑"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac8b190df065547d82aa431761eb5c00c94a2ad6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-architecture"></a>虚拟化的域控制器建筑

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍虚拟化的域控制器复制安全还原体系的结构。 它显示的进程复制和使用流程图安全还原，，然后提供的详细的说明每个进程中的步骤。  
  
-   [复制体系结构虚拟化的域控制器](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [虚拟化的域控制器安全还原建筑](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="BKMK_CloneArch"></a>复制体系结构虚拟化的域控制器  
  
### <a name="overview"></a>概述  
复制虚拟化的域控制器在虚拟机监控程序平台上以揭示称为标识符依赖于**VM 代 ID**检测创建虚拟机。 广告 DS 最初 (NTDS 与其数据库中存储此标识符值。DIT) 期间域控制器升级。 当虚拟机启动时，从虚拟机 VM 代 ID 的当前值将比较数据库中的值。 如果这两个值不同，域控制器重置调用 ID 和丢弃 RID 池，从而阻止 USN 重复使用或重复安全主体潜在创建。 域控制器，然后查找 DCCloneConfig.xml 文件的位置中称为步骤 3 中[复制详细处理](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails)。 如果发现 DCCloneConfig.xml 文件，它，它可以确定，它正在部署作为副本，以便将其启动重新升级使用现有 NTDS 复制到规定作为额外的域控制器。从源媒体复制 DIT 和 SYSVOL 内容。  
  
其中某些管理程序支持 VM GenerationID 和其他人在未起效混合环境，很可能会意外地部署在不支持 VM GenerationID 虚拟机监控程序克隆媒体。 存在 DCCloneConfig.xml 文件指示管理打算复制 DC。 因此，如果在启动，但 VM GenerationID 过程中发现 DCCloneConfig.xml 文件未提供从主机，克隆 DC 已启动到目录服务还原模式 (DSRM) 以环境的其余员工避免产生任何影响。 克隆媒体随后移动到支持 VM GenerationID 虚拟机监控程序，然后复制可以重试。  
  
如果在虚拟机监控程序支持 VM GenerationID 上部署克隆媒体，但未提供 DCCloneConfig.xml 文件，如 DC 检测到 VM GenerationID 其 DIT 从新 VM 所需的那个之间进行更改，它将触发保护以防止 USN 重新使用，并且避免重复 Sid。 但是，复制将不能启动，因此辅助 DC 将继续运行下源 DC 标识相同。 此辅助 DC 应从网络删除在最早的时间要避免在环境中的任何不一致。 有关如何释放同时确保更新获取复制站，请参阅 Microsoft KB 此辅助 DC 文章[2742970](https://support.microsoft.com/kb/2742970)。  
  
### <a name="BKMK_CloneProcessDetails"></a>复制详细的处理  
下图显示的体系结构对初始克隆操作和克隆重试操作。 这些进程详见按照本主题后面的更多详细信息。  
  
**初始克隆操作**  
  
![虚拟化的 DC 建筑](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**复制重试操作**  
  
![虚拟化的 DC 建筑](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
后续步骤说明的过程中更多详细信息：  
  
1.  现有的虚拟机域控制器启动在虚拟机监控程序支持 VM 代 id。  
  
    1.  此 VM 升级后都有设置任何现有 VM 代 ID 值其广告 DS 计算机对象上。  
  
    2.  即使它为空下, 一步计算机创建将意味着它仍克隆，如新 VM 代-ID 将不匹配。  
  
    3.  VM 代 ID 设置直流，在下一步重新启动后，并且不会复制。  
  
2.  然后，虚拟机读取 VMGenerationCounter 驱动程序提供 VM 代 ID。 它将两个 VM 代 Id 进行比较。  
  
    1.  如果匹配 Id，这不是新的虚拟机和复制不会继续。 如果存在 DCCloneConfig.xml 文件，域控制器重命名时间日期戳以防止复制文件。 服务器仍正常启动。 这是每次重启任何虚拟域控制器在 Windows Server 2012 的运行方式。  
  
    2.  如果不匹配两个 Id，这是一个包含 NTDS 的新虚拟机。从以前的域控制器 DIT （或是还原的快照）。 如果存在 DCCloneConfig.xml 文件，域控制器将继续与复制操作。 如果没有，它将继续快照还原操作。 请参阅[虚拟化域控制器安全还原体系结构](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。  
  
    3.  如果虚拟机监控程序不提供有关比较 VM 代 ID，但没有 DCCloneConfig.xml 文件，访客重命名该文件，然后启动到 DSRM 从重复域控制器保护的网络。 如果没有 dccloneconfig.xml 文件，访客启动通常 （与网络上的重复域控制器可能）。 有关如何释放此重复域控制器，请参阅 Microsoft 知识库文章[2742970](https://support.microsoft.com/kb/2742970)。  
  
3.  NTDS 服务检查的值 （在 HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters) VDCisCloning DWORD 注册表值的名称。  
  
    1.  如果不存在，这是此虚拟机复制在第一次尝试。 来宾操作系统实现直流 V 对象重复保护措施的使无效本地 RID 池，并设置新的复制调用 ID 域控制器  
  
    2.  如果它已设置为 0x1，这是"重试"复制尝试，以前克隆操作失败了。 它们必须已有运行一次之前，并会必要改变访客多次没有采取直流 V 对象重复安全措施。  
  
4.  （下 Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters) 编写 IsClone DWORD 注册表值名称  
  
5.  NTDS 服务更改来宾启动标记，以开始 DS 修复模式中的任何进一步重新启动。  
  
6.  尝试阅读 （DSA 工作目录、 %windir%\ntds 或可移动读取/写入媒体，驱动器号，该驱动器根部的顺序） 的三个接受位置之一 DcCloneConfig.xml NTDS 服务。  
  
    1.  如果在任何有效的位置，该文件不存在，访客检查重复的 IP 地址。 如果不重复的 IP 地址，服务器向上可以正常启动。 如果重复的 IP 地址，计算机启动到 DSRM 从重复域控制器保护的网络。  
  
    2.  如果有效的位置中存在文件，则 NTDS 服务验证其设置。 如果文件是空白 （或任何特定设置为空） NTDS 将配置这些设置为自动值。  
  
    3.  如果 DcCloneConfig.xml 存在，但包含任何无效的条目或无法读取，复制失败和访客启动到目录服务还原模式 (DSRM)。  
  
7.  来宾操作系统禁用所有 DNS 自动注册以避免意外拦截源计算机的名称和 IP 地址。  
  
8.  来宾操作系统停止 Netlogon 服务，以防止任何广告或网络广告 DS 请求的客户端的回答。  
  
9. NTDS 验证没有服务或安装的不是 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 的一部分的程序  
  
    1.  如果有服务或安装的程序未在默认排除项中允许列表或自定义排除项允许列表，复制失败和访客启动到 DSRM 从重复域控制器保护的网络。  
  
    2.  如果有任何不兼容问题，请复制继续。  
  
10. 如果自动 IP 地址将用于由于空白 DCCloneConfig.xml 网络设置，则来宾操作系统上以获取 IP 地址租赁、 网络路由和名称分辨率信息的网络适配器启用 DHCP。  
  
11. 来宾操作系统查找并联系人域控制器角色 FSMO 运行 PDC 仿真器。 这只会消耗 DNS 和 DCLocator 协议。 它使 RPC 连接，并且调用 IDL_DRSAddCloneDC 要复制的域控制器计算机对象的方法。  
  
    1.  如果访客源计算机对象包含域音响扩展的权限的"' 允许 DC 创建自身的副本"然后复制过程。  
  
    2.  如果没有扩展复制失败和访客的权限，启动到 DSRM 来防止网络上是重复的域控制器访客源计算机对象。  
  
12. 广告 DS 计算机的对象名称设置匹配指定中 DCCloneConfig.xml，如果有，否则上 PDCE 自动生成的名称。 NTDS 创建相应的 Active Directory 逻辑站点正确 NTDS 设置对象。  
  
    1.  如果这是一个 PDC 复制，然后访客重命名本地计算机并重新启动。 后重新启动后，它经历步骤 1-10 再次，然后转到步骤 13。  
  
    2.  如果这是一个副本 DC 复制，目前在此阶段无需重新启动。  
  
13. 来宾操作系统提供到 DS 角色服务器服务，该开始升级的促销设置。  
  
14. DS 角色服务器服务停止所有广告 DS 相关服务 NTDS、 NTFRS/DFSR、 KDC （DNS）。  
  
15. 来宾操作系统强制 NT5DS (Windows NTP) 时间同步与另一台域控制器 （在默认 Windows 时间服务分层中，这意味着使用 PDCE）。 来宾操作系统联系人 PDCE。 刷新所有现有 Kerberos 门票。  
  
16. 来宾操作系统配置 DFSR 或 NTFRS 服务自动运行。 来宾操作系统中删除所有现有 DFSR 和 NTFRS 数据库文件 (默认： c:\windows\ntfrs 和 c:\system 音量 information\dfsr\\*< database_GUID >*)，以便旁边启动该服务时强制 SYSVOL 的非权威同步。 来宾操作系统不会删除 SYSVOL 预当更高版本开始同步植入 SYSVOL 该文件的内容。  
  
17. 重命名访客。 来宾操作系统上的 DS 角色服务器服务开始广告 DS 配置 （促销）、 使用现有 NTDS。作为的源代码，而不是模板数据库中 c:\windows\system32 像一种促销通常包含 DIT 数据库文件。  
  
18. 来宾操作系统联系以获取新的 RID 池分配的不用母版 FSMO 角色持有者。  
  
19. 升级过程中创建新的调用 ID，并重新创建该各自的对象复制的域控制器 （无需考虑复制，这是域升级的一部分使用现有的 NTDS 时。DIT 数据库）。  
  
20. NTDS 复制中缺少、 更高版本，或已从合作伙伴域控制器较高版本的对象。 NTDS。DIT 已包含对象从源域控制器脱机的时，以最小化复制流量使用尽可能这些站。 填充全球目录分区。  
  
21. DFSR 或 FRS 服务来启动和因为没有的数据库，SYSVOL 非授权同步入站复制合作伙伴从。 此过程重新使用预现有数据 SYSVOL 文件夹中，以最小化网络复制通信。  
  
22. 来宾操作系统重新启用 DNS 客户端注册既然唯一计算机命名和网络。  
  
23. 来宾操作系统运行 DefaultDCCloneAllowList.xml 由指定的 SYSPREP 模块<SysprepInformation>元素为了将参考之前的计算机名称和 SID 为您去除。  
  
24. 复制升级已完成。  
  
    1.  来宾操作系统中删除 DSRM 启动标志，以便将正常下次重新启动。  
  
    2.  来宾操作系统重命名 DCCloneConfig.xml 附加的日期时间戳，以便它不重新阅读在下次启动。  
  
    3.  来宾操作系统中移除下 HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters VdcIsCloning DWORD 注册表值的名称。  
  
    4.  来宾操作系统设置"VdcCloningDone"DWORD 下 HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 为 0x1 注册表值名称。 Windows 不会使用此值，但相反提供其作为标记的第三方。  
  
25. 来宾操作系统更新中的 msDS GenerationID 属性其自己的复制的域控制器对象匹配当前来宾 VM 代 id。  
  
26. 来宾操作系统重新启动。 现在，它是正常时，域控制器的广告。  
  
## <a name="BKMK_SafeRestoreArch"></a>虚拟化的域控制器安全还原建筑  
  
### <a name="overview"></a>概述  
广告 DS 依赖虚拟机监控程序平台公开称为标识符**VM 代 ID**检测虚拟机的快照还原。 广告 DS 最初 (NTDS 与其数据库中存储此标识符值。DIT) 期间域控制器升级。 当管理员还原以前的快照从虚拟机时，从虚拟机 VM 代 ID 的当前值将比较数据库中的值。 如果这两个值不同，域控制器重置调用 ID 和丢弃 RID 池，从而阻止 USN 重复使用或重复安全主体潜在创建。 有两个安全还原可能发生的方案：  
  
-   还原已关闭时快照后启动时虚拟域控制器  
  
-   快照运行虚拟域控制器上的恢复时  
  
    如果快照中的虚拟化的域控制器处于挂起的状态，而不是关机，然后你需要重新启动广告 DS 服务触发 RID 新池命名请求。 你可以通过使用的服务，或使用 Windows PowerShell 重新启动广告 DS 服务 (NTDS 重启服务的强制)。  
  
以下部分介绍有关每个场景详细安全还原。  
  
### <a name="safe-restore-detailed-processing"></a>安全还原详细的处理  
下面的流程图显示了如何安全恢复虚拟域控制器快照还原已关闭时后启动时发生。  
  
![虚拟化的 DC 建筑](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  虚拟机启动时快照还原后，它也将具有主机提供的虚拟机监控程序由于快照还原新 VM 代 ID。  
  
2.  从虚拟机新 VM 代 ID 进行比较到数据库中 VM 代 ID。 两个 Id 不匹配，因为它会利用虚拟化保护 （参见步骤 3 中的上一个部分）。 还原完应用后，更新设置其广告 DS 计算机对象 VM GenerationID 匹配的新虚拟机监控程序主机 ID 提供。  
  
3.  来宾操作系统采用虚拟化的保护：  
  
    1.  使失效本地 RID 池。  
  
    2.  设置新的调用 ID 域控制器数据库。  
  
> [!NOTE]  
> 与克隆过程中重叠安全还原的这个部分。 此过程介绍安全还原的虚拟域控制器，它在快照恢复启动后，虽然克隆过程中发生情况相同步骤操作。  
  
以下图表将显示虚拟化保护如何防止分歧快照恢复运行虚拟域控制器上时，通过 USN 回退引起。  
  
![虚拟化的 DC 建筑](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> 上面的插图简化以解释的概念。  
  
1.  在时 T1、 虚拟机监控程序管理员进行虚拟 DC1 快照。 这次 dc1 USN 值 (**highestCommittedUsn**实际上) 的 A 100，InvocationId （表示 ID 为在上图中） 值 （在练习这会 GUID）。 SavedVMGID 值为 DIT DC 的文件中的 VM GenerationID (存储在名为属性直流的计算机对象针对**msDS GenerationId**)。 VMGID 是 VM GenerationId 虚拟机驱动程序中可用的当前值。 此值虚拟机监控程序由提供。  
  
2.  在更高版本时 T2，100 用户将添加到此直流 (考虑作为更新无法执行之间该域控制器上的一个示例用户时间 T1 和 T2; 实际上却是用户作品、 组作品、 更新密码、 属性更新等混用这些更新)。 在此示例中，每个更新消耗一个唯一的 USN （尽管在练习创建用户可能会消耗多个 USN）。 提交这些更新之前, DC1 检查的值 VM GenerationID (savedVMGID) 与其数据库中是否可从驱动程序 (VMGID) 的当前值相同。 它们是相同，，因为没有回退发生尚未，以便已提交的更新并 USN 移动达 200 下, 一次更新都可以使用 USN 201 指示。 不没有 InvocationId、 savedVMGID 或 VMGID 中的任何更改。 这些更新出复制到 DC2 在下一步复制周期。 DC2 更新它高水位线 (和**UptoDatenessVector**) 表示简称为 DC1(A) here @USN = 200。 也就是说，DC2 是注意到从 DC1 InvocationId 从 A 到 USN 200 上下文中的所有更新。  
  
3.  在时 T3，次 T1 拍摄快照适用于 DC1。 DC1 已回滚，因此其 USN 回退到 100，表明它可能使用 101 从 Usn 后续更新相关联。 但是，在此情况下，VMGID 值会上管理程序支持 VM GenerationID 不同。  
  
4.  随后时 DC1 执行任何更新，, 将检查的值的 VM GenerationId 它 (savedVMGID) 与其数据库中具有是否值从虚拟机驱动程序 (VMGID) 相同。 在此情况下，它不相同，以便 DC1 推理这为回滚，表明并触发虚拟化保护;换言之，这将其 InvocationId 有重置 (ID = B)，并且丢弃 RID 池 （不显示在上图中）。 然后将保存在其数据库 VMGID 新值，并提交新的 InvocationId b。 上下文中的这些更新 (USN 101-250)在下一个复制循环，DC2 知道执行任何操作从 DC1 InvocationId B 上下文中以便将请求从 DC1 以 InvocationID B 关联的所有内容因此，将安全地汇聚 DC1 快照的应用程序后执行更新。 此外，在 T2 DC1 所执行 （这已后快照还原丢失 DC1 上） 的更新套会复制插回到 DC1 在接下来的计划复制，因为它们已复制到 DC2 （如返回到 DC1 虚线所示）。  
  
来宾操作系统采用了虚拟化保护后，NTDS Active Directory 对象的差异归巢的未授权从复制合作伙伴域控制器。 相应更新是最新的矢量目标目录服务。 然后来宾同步 SYSVOL:  
  
-   如果使用 FRS，访客停止 NTFRS 服务，并设置 D2 BURFLAGS 注册表值。 然后，启动非授权复制站，并重新使用现有不变的 SYSVOL 数据尽可能 NTFRS 服务。  
  
-   如果使用 DFSR，访客停止 DFSR 服务，并删除 DFSR 数据库文件 (默认位置： %systemroot%\system 音量 information\dfsr\\*<database GUID>*)。 然后，启动非授权复制站，并重新使用现有不变的 SYSVOL 数据尽可能 DFSR 服务。  
  
> [!NOTE]  
> -   如果虚拟机监控程序不提供有关比较 VM 代 ID，虚拟机监控程序不支持虚拟化保护和访客将运行 Windows Server 2008 R2 的虚拟化的域控制器等操作或更早版本。 若要开始使用了不高级最后的最高 USN 的合作伙伴 DC 看到过的 Usn 复制有人试图是否，来宾操作系统实现 USN 回退隔离保护。 有关 USN 回退隔离保护的详细信息，请参阅[USN 和 USN 回退](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)  
  



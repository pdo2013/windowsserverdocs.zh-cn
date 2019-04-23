---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: 虚拟化域控制器体系结构
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d69ccfd15004619f890c6f5c1cb630c62e16256b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889188"
---
# <a name="virtualized-domain-controller-architecture"></a>虚拟化域控制器体系结构

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍虚拟化域控制器克隆和安全还原的体系结构。 它将使用流程图显示克隆和安全还原的过程，然后提供过程中每个步骤的详细说明。  
  
-   [虚拟化的域控制器克隆体系结构](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [虚拟化的域控制器安全还原体系结构](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="BKMK_CloneArch"></a>虚拟化的域控制器克隆体系结构  
  
### <a name="overview"></a>概述  
虚拟化域控制器克隆依赖于虚拟机监控程序平台以显示名为 **VM 生成 ID** 的标识符，用于检测虚拟机的创建。 在域控制器升级期间，AD DS 最初将此标识符的值存储在其数据库 (NTDS.DIT) 中。 虚拟机启动时，将比较虚拟机中 VM 生成 ID 的当前值和数据库中的值。 如果两个值不同，则域控制器重置调用 ID 并弃用 RID 池，从而阻止重复使用 USN 或消除创建重复安全主体的可能性。 然后，域控制器将在 [Cloning Detailed Processing](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails)的步骤 3 中标注的位置上查找 DCCloneConfig.xml 文件。 如果找到 DCCloneConfig.xml 文件，则可以确定正将其部署为克隆，因此它通过使用从源媒体复制过来的现有 NTDS.DIT 和 SYSVOL 内容进行重新升级，从而启动克隆以将其本身设置为额外的域控制器。  
  
在混合环境中（某些虚拟机监控程序支持 VM 生成 ID 而其他虚拟机监控程序不支持），可能会在不支持 VM 生成 ID 的虚拟机监控程序上意外地部署克隆媒体。 DCCloneConfig.xml 文件的存在表明了要克隆 DC 的管理意图。 因此，如果在启动期间找到了 DCCloneConfig.xml 文件，但主机未提供 VM 生成 ID，则克隆 DC 将启动进入目录服务还原模式 (DSRM)，以防对环境的其余部分造成任何影响。 随后，可将克隆媒体移动到支持 VM 生成 ID 的虚拟机监控程序，然后可以重试克隆。  
  
如果在支持 VM 生成 ID 的虚拟机监控程序上部署克隆媒体，但未提供 DCCloneConfig.xml 文件，则当 DC 检测到其 DIT 和新 VM 的 DIT 之间的 VM 生成 ID 发生更改时，它将触发安全措施，以阻止重复使用 USN 并避免重复 SID。 但是，将不会启动克隆，因此辅助 DC 将在与源域控制器相同的标识下继续运行。 为了避免环境中出现任何不一致，应及早从网络中删除此辅助 DC。 有关如何在确保更新获取已复制出站的同时回收此辅助 DC 的详细信息，请参阅 Microsoft 知识库文章 [2742970](https://support.microsoft.com/kb/2742970)。  
  
### <a name="BKMK_CloneProcessDetails"></a>克隆详细的处理  
下图显示了初始克隆操作和克隆重试操作的体系结构。 稍后，本主题将对这些过程进行详细说明。  
  
**初始克隆操作**  
  
![虚拟化的 DC 体系结构](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**克隆重试操作**  
  
![虚拟化的 DC 体系结构](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
以下步骤详细介绍了该过程：  
  
1.  现有的虚拟机域控制器在支持 VM 生成 ID 的虚拟机监控程序中启动。  
  
    1.  升级后，此 VM 未在其 AD DS 计算机对象上设置任何现有 VM 生成 ID 值。  
  
    2.  即使它为 null，接下来的计算机创建将意味着它仍可克隆，因为新的 VM 生成 ID 将不匹配。  
  
    3.  将在 DC 下次重新启动后设置 VM 生成 ID，而且不会进行复制。  
  
2.  然后，虚拟机将读取 VMGenerationCounter 驱动程序提供的 VM 生成 ID。 它将比较这两个 VM 生成 ID。  
  
    1.  如果这两个 ID 匹配，则表明它不是新虚拟机，将不会继续克隆。 如果存在 DCCloneConfig.xml 文件，则域控制器将使用时间/日期戳重命名该文件以阻止克隆。 服务器将继续正常启动。 此即 Windows Server 2012 中任一虚拟域控制器每次重新启动的运行方式。  
  
    2.  如果这两个 ID 不匹配，则表示它是新虚拟机，其中包含来自之前域控制器的 NTDS.DIT（或者它是还原的快照）。 如果存在 DCCloneConfig.xml 文件，则域控制器将继续执行克隆操作。 如果不存在，则它将继续执行快照还原操作。 请参阅 [Virtualized domain controller safe restore architecture](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。  
  
    3.  如果虚拟机监控程序没有提供用于比较的 VM 生成 ID，但存在一个 DCCloneConfig.xml 文件，则来宾将重命名该文件，然后启动进入 DSRM 以防网络受重复域控制器的影响。 如果不存在 dccloneconfig.xml 文件，则来宾将正常启动（网络上可能会有重复的域控制器）。 有关如何回收此重复域控制器的详细信息，请参阅 Microsoft 知识库文章 [2742970](https://support.microsoft.com/kb/2742970)。  
  
3.  NTDS 服务会检查 VDCisCloning DWORD 注册表值名称（在 HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters 下）的值。  
  
    1.  如果不存在，则这是此虚拟机的第一次克隆尝试。 来宾将实现 VDC 对象复制安全措施，这些措施使本地 RID 池失效并为域控制器设置新的复制调用 ID。  
  
    2.  如果已将它设置为 0x1，则这是一次“重试”克隆尝试，其中之前的克隆操作已失败。 因为 VDC 对象复制安全措施必须在之前已运行一次且可能会不必要地多次改变来宾，所以不会采取这些措施。  
  
4.  IsClone DWORD 注册表值名称将写入（在 Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 下）  
  
5.  NTDS 服务更改来宾启动标志，以使任何以后的重新启动在 DS 修复模式下启动。  
  
6.  NTDS 服务尝试在三个接受的位置之一（DSA 工作目录、%windir%\ntds 或可移动读/写媒体，按驱动器号的顺序排列，位于驱动器的根目录）读取 DcCloneConfig.xml。  
  
    1.  如果任何有效的位置中不存在该文件，则来宾将检查复制的 IP 地址。 如果 IP 地址不重复，则服务器将正常启动。 如果存在重复的 IP 地址，则计算机将启动进入 DSRM，以防网络受重复域控制器的影响。  
  
    2.  如果文件位于某个有效的位置，则 NTDS 服务将验证其设置。 如果该文件为空（或任何特定的设置为空），则 NTDS 将为这些设置配置自动值。  
  
    3.  如果 DcCloneConfig.xml 存在，但包含任何无效的条目或不可读，则克隆失败，而且来宾将启动进入目录服务还原模式 (DSRM)。  
  
7.  来宾禁用所有 DNS 自动注册，以防止对源计算机名称和 IP 地址的意外劫持。  
  
8.  来宾停止 Netlogon 服务，以阻止来自客户端的网络 AD DS 请求的任何播发或应答。  
  
9. NTDS 将验证安装的服务或程序都是 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 中的一部分  
  
    1.  如果安装的服务或程序没有包含在默认排除允许列表或自定义排除允许列表中，则克隆失败且来宾将启动进入 DSRM，以防止网络受重复域控制器的影响。  
  
    2.  如果没有不兼容问题，则克隆将继续。  
  
10. 如果由于空白 DCCloneConfig.xml 网络设置将使用自动 IP 寻址，则来宾在将网络适配器上启用 DHCP，以获得 IP 地址租用、网络路由和名称解析信息。  
  
11. 来宾定位并联系运行 PDC 模拟器 FSMO 角色的域控制器。 这将使用 DNS 和 DCLocator 协议。 它建立 RPC 连接并调用 IDL_DRSAddCloneDC 方法，以克隆域控制器计算机对象。  
  
    1.  如果来宾的源计算机对象保留“允许 DC 创建其自身的克隆”的域标头扩展权限，则克隆将继续。  
  
    2.  如果来宾的源计算机对象不保留此扩展权限，则克隆失败且来宾将启动进入 DSRM，以防网络受重复域控制器的影响。  
  
12. 将 AD DS 计算机对象名称设置为与 DCCloneConfig.xml 中指定的名称（如果有的话）相匹配，否则将在 PDCE 上自动生成名称。 NTDS 为相应的 Active Directory 逻辑站点创建正确的 NTDS 设置对象。  
  
    1.  如果这是 PDC 克隆，则来宾将重命名本地计算机并重新启动。 重新启动后，它将再次经历步骤 1-10，然后转到步骤 13。  
  
    2.  如果这是副本 DC 克隆，则在此阶段无需重新启动。  
  
13. 来宾向 DS 角色服务器服务提供了升级设置，它将开始升级。  
  
14. DS 角色服务器服务将停止所有 AD DS 的相关服务（NTDS、NTFRS/DFSR、KDC、DNS）。  
  
15. 来宾强制与另一个域控制器进行 NT5DS (Windows NTP) 时间同步（在默认 Windows 时间服务层次结构中，这意味着使用 PDCE）。 来宾与 PDCE 取得联系。 刷新所有现有 Kerberos 票证。  
  
16. 来宾将 DFSR 或 NTFRS 服务配置为自动运行。 来宾将删除所有现有 DFSR 和 NTFRS 数据库文件 (默认： c:\windows\ntfrs 和 c:\system 卷 information\dfsr\\ *< database_GUID >*)，以便强制执行的非权威同步接下来启动该服务时的 SYSVOL。 来宾将不会删除 SYSVOL 的文件内容，以便在稍后同步启动时预植入 SYSVOL。  
  
17. 已重命名来宾。 来宾上的 DS 角色服务器服务开始进行 AD DS 配置（升级），将现有的 NTDS.DIT 数据库文件用作源，而不是同普通升级一样使用 c:\windows\system32 中包含的模板数据库作为源。  
  
18. 来宾将联系 RID 主机 FSMO 角色所有者以获取新的 RID 池分配。  
  
19. 升级过程将创建一个新的调用 ID 并重新创建克隆的域控制器的 NTDS 设置对象（不考虑克隆，这是使用现有 NTDS.DIT 数据库时域升级的一部分）。  
  
20. NTDS 将在伙伴域控制器中缺失、较新或版本更高的对象中进行复制。 NTDS.DIT 中已包含源域控制器脱机时的对象，并且为了最大限度地减少复制流量入站而尽可能使用这些对象。 将填充全局编录分区。  
  
21. DFSR 或 FRS 服务将启动，而且因为没有数据库，所以 SYSVOL 将从复制伙伴进行非权威同步入站。 此过程重新使用 SYSVOL 文件夹中预先存在的数据，以尽量减少网络复制流量。  
  
22. 既然该计算机已具有唯一的名称并已联网，来宾将重新启用 DNS 客户端注册。  
  
23. 来宾运行由 DefaultDCCloneAllowList.xml <SysprepInformation> 元素指定的 SYSPREP 模块，以清理出对之前计算机名称和 SID 的引用。  
  
24. 克隆升级已完成。  
  
    1.  来宾将删除 DSRM 启动标志，以便下次可以正常重新启动。  
  
    2.  来宾将使用追加的日期时间戳重命名 DCCloneConfig.xml，以便下次启动时不会再次读取它。  
  
    3.  来宾将删除 HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 下的 VdcIsCloning DWORD 注册表值名称。  
  
    4.  来宾将 HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 下的“VdcCloningDone”DWORD 注册表值名称设置为 0x1。 Windows 不会使用此值，但会将它提供为用于第三方的标记。  
  
25. 来宾将在其自身的克隆域控制器对象上更新 msDS-GenerationID 属性，以匹配当前来宾 VM 生成 ID。  
  
26. 来宾将重新启动。 现在这是一个正常的、正在进行播发的域控制器。  
  
## <a name="BKMK_SafeRestoreArch"></a>虚拟化的域控制器安全还原体系结构  
  
### <a name="overview"></a>概述  
AD DS 依赖于虚拟机监控程序平台以显示名为 **VM 生成 ID** 的标识符，来检测虚拟机的快照还原。 在域控制器升级期间，AD DS 最初将此标识符的值存储在其数据库 (NTDS.DIT) 中。 当管理员从之前的快照还原虚拟机时，将比较虚拟机中 VM 生成 ID 的当前值和数据库中的值。 如果两个值不同，则域控制器重置调用 ID 并弃用 RID 池，从而阻止重复使用 USN 或消除创建重复安全主体的可能性。 安全还原可能在以下两种应用场景中发生：  
  
-   在虚拟域控制器已关闭的情况下，在还原快照后启动虚拟域控制器  
  
-   在正在运行的虚拟域控制器上恢复快照  
  
    如果快照中的虚拟化域控制器处于已挂起状态，而不是关闭状态，则需要重启 AD DS 服务以触发新的 RID 池请求。 你可以通过使用服务管理单元或使用 Windows PowerShell (Restart-Service NTDS -force) 重新启动 AD DS 服务。  
  
以下部分将针对每个应用场景详细说明安全还原。  
  
### <a name="safe-restore-detailed-processing"></a>安全还原详细处理  
以下流程图显示了在关闭虚拟域控制器时，在还原快照后启动虚拟域控制器的情况下如何发生安全还原。  
  
![虚拟化的 DC 体系结构](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  当虚拟机在快照还原后启动时，由于快照还原，它将具有由虚拟机监控程序主机提供的新 VM 生成 ID。  
  
2.  虚拟机中的新 VM 生成 ID 将与数据库中的 VM 生成 ID 进行比较。 因为这两个 ID 不匹配，所以将它使用虚拟化安全措施（请参阅上一部分中的步骤 3）。 应用完还原后，将更新其 AD DS 计算机对象上设置的 VM 生成 ID，以匹配由虚拟机监控程序主机提供的新 ID。  
  
3.  来宾将通过以下方式使用虚拟化安全措施：  
  
    1.  将本地 RID 池设为无效。  
  
    2.  设置域控制器数据库的新调用 ID。  
  
> [!NOTE]  
> 此部分的安全还原与克隆过程相重叠。 虽然此过程是有关在快照还原后启动虚拟域控制器，然后进行虚拟域控制器的安全还原，但是在克隆过程期间会进行相同的步骤。  
  
下图显示了当快照在正在运行的虚拟域控制器上还原时，虚拟化安全措施如何阻止由 USN 回滚引起的分歧。  
  
![虚拟化的 DC 体系结构](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> 简化了之前的插图以解释这些概念。  
  
1.  在时间 T1，虚拟机监控程序管理员拍摄虚拟 DC1 的快照。 此时 DC1 的 USN 值（实际上为 **highestCommittedUsn**）为 100，InvocationId（在之前的图中表示为 ID）值为 A（实际上它是 GUID）。 SavedVMGID 值是 DC 的 DIT 文件中的 VM 生成 ID（根据 DC 的计算机对象存储在名为 **msDS-GenerationId**的属性中）。 VMGID 是虚拟机驱动程序中可用的 VM 生成 ID 的当前值。 该值由虚拟机监控程序提供。  
  
2.  在稍后的时间 T2，100 位用户将添加到此 DC（将用户视为可能会在时间 T1 和 T2 之间在此 DC 上执行的更新的示例；实际上，这些更新可能是用户创建、组创建、密码更新、属性更新等的组合）。 在此示例中，每个更新使用一个唯一的 USN（但在实际中一个用户创建可能会使用多个 USN）。 在提交这些更新之前，DC1 将检查其数据库中的 VM 生成 ID 值 （savedVMGID） 是否与驱动程序中可用的当前值 (VMGID) 相同。 因为还没有发生回滚，所以它们是相同的，因此将提交更新且 USN 将上移至 200，这指示下一个更新可以使用 USN 201。 InvocationId、savedVMGID 和 VMGID 中没有发生任何更改。 在下一个复制周期，这些更新将复制到 DC2。 DC2 对其进行更新高水印 (和**UptoDatenessVector**) 表示只需为 DC1(A) 此处@USN= 200。 也就是说，通过 USN 200，DC2 可感知来自 InvocationId A 上下文中的 DC1 的所有更新。  
  
3.  在时间 T3，在时间 T1 拍摄的快照将应用到 DC1。 DC1 已回滚，因此其 USN 回滚到 100，这指示它可从 101 起使用 USN 以与后续更新相关联。 但是，在这种情况下，支持 VM 生成 ID 的虚拟机监控程序上的 VMGID 值将会不同。  
  
4.  随后，当 DC1 执行任何更新时，它会检查其数据库中 VM 生成 ID 的值 （savedVMGID） 与虚拟机驱动程序中的值 (VMGID) 是否相同。 在该案例中，它不相同，因此 DC1 将此推断为回滚的指示，而且它将触发虚拟化安全措施；换言之，它将重置其 InvocationId (ID = B) 并弃用 RID 池（未显示在之前的图中）。 然后，它将 VMGID 的新值保存在其数据库中并在新 InvocationId b。 上下文中提交这些更新 (USN 101 – 250)在下一个复制周期，DC2 知道执行任何操作从 DC1 无法感知 InvocationId B 上下文中以便它请求从 DC1 以 InvocationID B 相关联的所有内容因此，在 DC1 上应用快照之后执行的更新将安全地聚合。 此外，T2 时在 DC1 上执行的更新集（它们已在快照还原后在 DC1 上丢失）将在下一次计划复制时复制回 DC1 中，因为它们已复制到 DC2（如返回 DC1 的虚线表示）。  
  
来宾使用虚拟化安全措施后，NTDS 将从伙伴域控制器非权威地复制 Active Directory 对象差异入站。 将相应地更新目标目录服务的最新程度矢量。 然后，来宾将同步 SYSVOL：  
  
-   如果使用 FRS，则来宾将停止 NTFRS 服务并设置 D2 BURFLAGS 注册表值。 然后，它将启动 NTFRS 服务（该服务非权威地复制入站），并尽可能重新使用现有未改变的 SYSVOL 数据。  
  
-   如果使用 DFSR，来宾将停止 DFSR 服务并删除 DFSR 数据库文件 (默认位置： %systemroot%\system 卷 information\dfsr\\*<database GUID>*)。 然后，它将启动 DFSR 服务（该服务非权威地复制入站），并尽可能重新使用现有未改变的 SYSVOL 数据。  
  
> [!NOTE]  
> -   如果虚拟机监控程序未提供用于比较的 VM 生成 ID，则虚拟机监控程序将无法支持虚拟化安全措施，而且来宾将如同运行 Windows Server 2008 R2 或更早版本的虚拟化域控制器一样运行。 如果存在使用 USN 开始复制的尝试（这些 USN 未超过伙伴 DC 见到的最后一个最高 USN），则来宾将实现 USN 回滚隔离保护。 有关 USN 回滚隔离保护的详细信息，请参阅 [USN 和 USN 回滚](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)  
  



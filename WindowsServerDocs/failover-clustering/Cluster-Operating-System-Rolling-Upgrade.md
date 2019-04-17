---
title: "滚动升级群集操作系统"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2017
ms.openlocfilehash: 8463c163294a4d2223a74b7cfeaea6ac5ae4fcfe
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-operating-system-rolling-upgrade"></a>滚动升级群集操作系统

> 适用于：Windows Server（半年通道）、Windows Server 2016

群集操作系统推出升级使管理员升级操作系统中的群集节点不停止 Hyper-V 或规模文件服务器工作负载。 使用此功能，可以避免停机处罚防御服务级别协议 (SLA)。

群集操作系统推出升级提供以下优势：

- 可以从 Windows Server 2012 R2 升级故障转移群集运行的 Hyper-V 虚拟机和规模文件服务器 (SOFS) 工作负载（上所有节点运行群集）Windows Server 2016（群集的所有群集节点上运行）而无需停机。 其他群集运行工作负载中的，如 SQL Server，将不提供在故障转移到 Windows Server 2016 花费的时间（通常少于五分钟）。  
- 它不需要任何附加的硬件。 不过，你可以添加其他群集节点暂时于小型群集群集操作系统推出升级期间改进群集的可用性处理。  
- 群集不需要将停止或重新启动。  
- 新的群集不是必需的。 现有的群集升级。 此外，便会使用 Active Directory 中存储的现有群集对象。  
- 在升级过程是可逆的直到客户 choses"点的-无-返回"，所有群集节点时运行的 Windows Server 2016，并运行 Update-ClusterFunctionalLevel PowerShell cmdlet 时。  
- 群集可混合操作系统方式运行时支持修补和维护操作。  
- 通过 PowerShell 和 WMI 自动化，它支持。  
- 群集公共属性**ClusterFunctionalLevel**属性指示 Windows Server 2016 群集节点上的群集的状态。 可以使用来自 Windows Server 2016 群集节点属于故障转移群集 PowerShell cmdlet 查询此属性：  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    值**8**指示群集运行 Windows Server 2012 R2 功能级别。 值**9**指示群集运行 Windows Server 2016 功能级别。  

本指南介绍了群集操作系统推出升级过程、安装步骤、功能限制和常见问题（常见问题解答）、的各个阶段和适用于 Windows Server 2016 中的以下操作系统推出升级群集方案：  
- Hyper-V 群集  
- 规模文件服务器群集  

Windows Server 2016 中不支持以下情形：  
-  使用虚拟硬盘群集操作系统推出升级的访客群集 (.vhdx file) 作为共享存储  

群集操作系统推出升级完全受支持的系统中心虚拟机管理器 (SCVMM) 2016 年。 如果你使用的 SCVMM 2016，请参阅[Windows Server 2012 R2 的升级到 Windows Server 2016 中 VMM 群集](https://technet.microsoft.com/library/mt445417.aspx)有关升级群集和自动化本文中所述的步骤的指南。  

## <a name="requirements"></a>要求  
开始群集操作系统推出升级的过程之前完成以下要求：

- 开始与运行 Windows Server（半年通道）、Windows Server 2016 或 Windows Server 2012 2012 R2。
- 升级到 Windows Server 的存储空间直通 群集，版本 1709 年不受支持。
- 如果群集运行工作负载 Hyper-V 虚拟机的功能或规模文件服务器，你可以在未来零宕机升级。
- 验证 Hyper-V 节点具有 Cpu 支持二级地址表 (SLAT) 使用以下方法; 之一  
        -查看[你有多大 SLAT 兼容？于 WP8 SDK 提示 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx)介绍两种方法以查看是否 CPU 支持 SLATs 的文章  
        -下载[Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722)工具来确定是否 CPU 支持 SLAT。

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>群集操作系统推出升级期间群集转换状态

此部分中介绍了在升级到 Windows Server 2016 使用群集操作系统推出升级 Windows Server 2012 R2 群集的各种转换状态。  

为了防止群集好像两个节点 Windows Server 2012 R2 的操作系统运行的工作原理在群集操作系统推出升级过程中，转向 Windows Server 2016 节点群集运行工作负载从 Windows Server 2012 R2 节点运行工作负载。 Windows Server 2016 节点添加到群集后，它们将在 Windows Server 2012 R2 兼容性模式下工作。 称为"混合操作系统模式，"的新概念群集模式允许节点以在相同中存在的不同版本的群集（参见第 1 图）。  

![图显示的三个阶段群集操作系统滚动升级：Windows Server 2012 R2 的所有节点、混合操作系统模式和所有节点 Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**图 1：群集操作系统状态转换**  

Windows Server 2016 节点添加到群集时，Windows Server 2012 R2 群集进入混合操作系统模式。 该进程是完全可逆-Windows Server 2016 节点可从群集和 Windows Server 2012 R2 节点可以添加到在此模式中群集。 Update-ClusterFunctionalLevel PowerShell cmdlet 群集上运行时，会发生"点的否退货"。 为了让此 cmdlet 成功，所有节点都必须都是 Windows Server 2016，，并且所有节点必须都都处于联机状态。  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>转换状态的四个节点群集执行推出操作系统升级时

此部分中所示，并介绍了与从 Windows Server 2012 R2 升级到 Windows Server 2016 其节点共享存储群集不同的四个阶段。  

"阶段 1"是初始状态-我们与 Windows Server 2012 R2 群集开始。  

![图显示初始状态：Windows Server 2012 R2 的所有节点](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**图 2：初始状态：Windows Server 2012 R2 故障转移群集（舞台 1）**  

在"步骤 2"，两个节点已暂停、耗尽、退出，重新格式化，并与 Windows Server 2016 安装。  

![图混合操作系统模式中显示群集：两个节点利用示例 4 节点群集，运行 Windows Server 2016，并且两个节点运行的 Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**图 3：中间状态：混合操作系统模式：Windows Server 2012 R2 和 Windows Server 2016 故障转移群集 (Stage 2)**  

在"步骤 3"，所有中的群集节点已升级到 Windows Server 2016，并且群集已准备好升级的 Update-ClusterFunctionalLevel PowerShell cmdlet。  

> [!NOTE]  
> 在这个阶段，可以完全撤消该过程，，和 Windows Server 2012 R2 节点可以添加到此群集。  

![显示群集已完全升级到 Windows Server 2016，并且已准备好进行 Update-ClusterFunctionalLevel cmdlet 将最多的 Windows Server 2016 的群集功能级别的图示](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**图 4：中间状态：所有节点升级到 Windows Server 2016，可供 Update-ClusterFunctionalLevel (Stage 3)**  

Update-ClusterFunctionalLevelcmdlet 运行后，群集进入"阶段 4"，可以使用新的 Windows Server 2016 群集功能的位置。  

![图显示的群集滚动操作系统升级已成功完成;已升级到 Windows Server 2016 中，所有节点以及群集运行 Windows Server 2016 群集功能级别](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**图 5：最终状态：Windows Server 2016 故障转移群集（舞台 4）**  

## <a name="cluster-os-rolling-upgrade-process"></a>推出升级过程的群集操作系统

本部分介绍执行群集操作系统推出升级的工作流程。  

![显示有关升级的群集的工作流中的图示](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**图 6：群集操作系统推出升级过程的工作流**  

群集操作系统推出升级包含以下步骤：  

1. 准备群集操作系统升级如下所示：  
    1. 群集操作系统推出升级需要从群集一次删除一台节点。 检查是否具有足够的容量群集时从群集操作系统升级删除了某个群集节点保持 HA Sla 上。 换言之，您需要向故障转移到另一个节点的工作负载的功能时个节点从群集群集操作系统推出升级的过程中删除？ 群集是否有容量时要运行所需的工作负载一个节点取下从群集群集操作系统推出升级？  
    2. 针对 Hyper-V 工作负载，查看所有 Windows Server 2016 Hyper-V 主机都有二级地址表 (SLAT) 的支持的 CPU。 仅支持 SLAT 的计算机可以使用 Windows Server 2016 中的 Hyper-V 角色。  
    3. 检查任何工作负载备份完成后，并且考虑备份群集。 添加到群集节点时停止备份操作。  
    4. 查看所有群集节点都处于联机状态/运行/上使用[`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx) cmdlet（参见第 7 图）。  

        ![Screencap 显示运行 Get-ClusterNode cmdlet 结果](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **图 7：如何确定使用 Get-ClusterNode cmdlet 节点状态**  

    5. 如果你运行的群集注意到更新 (CAU)，则验证如果 CAU 当前运行的通过使用**群集意识到更新**UI，或[`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx) cmdlet（请参阅图 8）。 停止使用 CAU [`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx)cmdlet（请参阅图 9）若要防止任何暂停和群集操作系统推出升级过程中耗尽通过 CAU 节点。  

        ![显示的输出 Get-CauRun cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **图 8 部分：使用[`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx) cmdlet 以确定是否群集上运行的群集注意到更新**  

        ![显示的输出 Disable-CauClusterRole cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **图 9：禁用群集注意到更新角色使用[`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx) cmdlet**  

2. 对于每个节点群集中，执行以下操作：  
    1. 使用的 UI 群集管理器中，选择一个节点，然后使用**暂停 |消耗**节点消耗电量的菜单选项（如图 10），或者使用[`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx) cmdlet（参见 11 图）。  

        ![演示如何消耗群集经理 ui 角色 Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **图 10：耗费节点使用故障转移群集管理器中的角色**  

        ![显示的输出 Suspend-ClusterNode cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **图 11：耗费节点使用中的角色[`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx) cmdlet**  

    2.  使用群集经理 UI**退出**从群集或使用已暂停的节点[`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx) cmdlet。  

        ![显示的输出 Remove-ClusterNode cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **图 12 部分：从群集使用删除节点[`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx) cmdlet**  

    3.  重新系统驱动器进行格式化并执行 Windows Server 2016 的"干净操作系统安装"节点使用**自定义：安装 Windows（高级）**安装 (请参阅图 13) setup.exe 中的选项。 避免选择**升级：安装 Windows 并保留文件、设置和应用程序**由于群集操作系统推出升级不建议就地升级选项。  

        ![显示自定义 Windows Server 2016 安装向导 Screencap 安装所选的选项](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Windows Server 2016 图 13：安装可用选项**  

    4.  添加到适当的域的 Active Directory 节点。  
    5.  添加到管理员组适当的用户。  
    6.  使用的 UI 服务器管理器或 Install-WindowsFeature PowerShell cmdlet，安装需要例如 Hyper-V 任何服务器角色。  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  使用服务器管理器 UI 或 Install-WindowsFeature PowerShell cmdlet、安装故障转移群集的功能。  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  安装任何所需的群集的工作负载的其他功能。  
    9. 检查网络和存储使用故障转移群集 Manager UI 的连接设置。  
    10. 如果使用 Windows 防火墙，则检查防火墙设置群集无误。 例如，群集注意到更新 (CAU) 启用群集可能需要防火墙配置。  
    11. 对于 Hyper-V 工作负载中，使用 Hyper-V 管理器 UI 启动虚拟交换机用来管理器对话框（请参阅 14 图）。  

        检查相同的所有 Hyper-V 主机群集节点虚拟交换机用的名称。  

        ![Screencap 显示 Hyper-V 虚拟交换机用来管理器对话框中的位置](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **图 14：虚拟交换机用来管理器**  

    12. 在 Windows Server 2016 节点（不使用 Windows Server 2012 R2 节点），使用故障转移群集管理器（请参阅图 15）连接到群集。  

        ![Screencap 显示选择群集对话框](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **图 15：添加到使用故障转移群集管理器的群集节点**  

    13. 使用故障转移群集管理器 UI 或[`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx) cmdlet（请参阅图 16）添加到群集节点。  

        ![显示的输出 Add-ClusterNode cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **图 16：添加到群集使用节点[`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx) cmdlet**  

        > [!NOTE]  
        > 第一个 Windows Server 2016 节点加入群集时，当群集将进入"混合操作系统"模式和资源移至 Windows Server 2016 节点群集核心。 "混合操作系统"模式下群集是充分发挥功能群集新节点以与旧节点兼容性模式运行的位置。 "混合操作系统"模式下是群集暂时模式。 不应为永久，客户需要更新四个星期内的所有其群集节点。  

    14. Windows Server 2016 后节点成功添加到群集，你可以（可选）移某些群集运行工作负载到新添加的节点以重新工作负载平衡整个群集，如下所示：

        ![显示的输出 Move-ClusterVirtualMachineRole cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **图 17：移动群集运行工作负载（群集 VM 角色）使用[`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx) cmdlet**  

        1. 使用**实时迁移**从故障转移群集管理器虚拟机或[`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx) cmdlet（请参阅图 17）来执行虚拟机的实时迁移。  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. 使用**移动**故障转移群集管理器或[`Move-ClusterGroup`](https://technet.microsoft.com/library/ee461002.aspx) cmdlet 针对其他群集运行工作负载。  

3. 当升级到 Windows Server 2016 的每个节点并将其重新添加到群集，或退出剩余的任何 Windows Server 2012 R2 节点后，请执行以下操作：  

    > [!IMPORTANT]  
    > -   更新群集功能级别后，你不能回退到 Windows Server 2012 R2 功能级别和 Windows Server 2012 R2 节点无法加入群集。
    > -   直到[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)运行 cmdlet、该过程不可逆完全和 Windows Server 2012 R2 节点可以添加到此群集和 Windows Server 2016 节点都可删除。  
    > -   后[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)运行 cmdlet 时，新的功能将不可用。  

    1.  使用故障转移群集管理器 UI 或[`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx) cmdlet，查看所有群集角色按预期都运行群集。 不用于可用存储在以下示例中，改为使用 CSV，因此，提供的存储显示**离线**状态（请参阅图 18）。  

        ![显示的输出 Get-ClusterGroup cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **图 18：验证所有群集组（群集角色）运行的使用[`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx) cmdlet**  

    2.  查看所有群集节点都处于联机状态并运行使用[`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx) cmdlet。  
    3.  运行[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet-没有错误应返回（请参阅图 19）。  

        ![显示的输出 Update-ClusterFunctionalLevel cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **图 19：更新群集使用 PowerShell 功能级别**  

    4.  后[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)运行 cmdlet 时，可以使用新的功能。  

4. Windows Server 2016-恢复正常群集更新和备份：  

    1. 如果你之前已运行 CAU，重新启动它使用 CAU UI 或使用[`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx) cmdlet（请参阅 20 图）。  

        ![显示的输出 Enable-CauClusterRole Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **图 20：启用群集注意到更新角色使用[`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx) cmdlet**  

    2. 恢复备份操作。  

5. 启用并使用 Hyper-V 虚拟机上的 Windows Server 2016 功能。  

    1. 群集已升级到 Windows Server 2016 功能级别后，例如 Hyper-V 虚拟机的功能的很多工作负载将有新功能。 对于新的 Hyper-V 功能的列表。 请参阅[迁移和升级虚拟机](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. 在每个 Hyper-V 主机中群集节点，使用[`Get-VMHostSupportedVersion`](https://technet.microsoft.com/library/mt653838.aspx) cmdlet 以查看受主机 Hyper-V VM 配置版本。  

        ![显示的输出 Get-VMHostSupportedVersion cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **图 21：查看受主机 Hyper-V VM 配置版本**  

   3.  在每个 Hyper-V 主机中群集节点，可以被与用户调度维护简要窗口、备份、关闭虚拟机和运行升级 Hyper-V VM 配置版本[`Update-VMVersion`](https://technet.microsoft.com/library/mt484146.aspx) cmdlet（请参阅图 22）。 这将更新虚拟机的版本，并启用新的 Hyper-V 功能，不再需要的未来的 Hyper-V 集成组件（集成电路）更新。 可从 Hyper-V 节点承载 VM 中，运行此 cmdlet 或`-ComputerName`使用参数远程更新 VM 版本。 在此示例中，以下我们配置版升级 VM1 5.0 从到 7.0 充分利用此 VM 配置版本，如生产检查点（应用程序一致的备份），并二进制 VM 配置文件与相关的许多新 Hyper-V 功能。  

        ![Screencap 操作中显示 Update-VMVersion cmdlet](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
        **图 22：升级使用 Update-VMVersion PowerShell cmdlet VM 版本**  

4.  可以使用升级存储池中[更新 StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/update-storagepool) PowerShell cmdlet-这是一个联机操作。  

尽管我们的目标专用云方案，特别是 Hyper-V 和规模文件服务器群集，而无需停机可以升级，可以任何群集角色中用于群集操作系统推出升级的过程。  

## <a name="restrictions--limitations"></a>限制 / 限制  
- 此功能仅适用于 Windows Server 2012 R2 对 Windows Server 2016 的版本。 此功能无法升级到 Windows Server 2016 的较早版本的 Windows Server，如 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012。  
- Windows Server 2016 的每个节点应仅重新格式化/全新安装。 "位置"升级"不要安装类型。  
- Windows Server 2016 节点必须用于添加到群集节点 Windows Server 2016。  
- 管理混合操作系统模式群集时, 始终运行的 Windows Server 2016 上一级节点执行管理任务。 下层 Windows Server 2012 R2 节点无法使用对 Windows Server 2016 的 UI 或管理工具。  
- 我们鼓励客户遍历群集升级过程中快速由于未针对混合操作系统模式优化某些群集功能。  
- 创建或调整 Windows Server 2016 节点上的存储时群集运行混合操作系统型由于可能不兼容的故障转移从 Windows Server 2016 节点向低级别 Windows Server 2012 R2 节点避免。  

## <a name="frequently-asked-questions"></a>常见问题  
**多长时间可以故障转移群集混合操作系统模式下运行？**  
    我们鼓励客户才能完成升级四个星期内。 有多种优化，在 Windows Server 2016。 我们已成功升级四个小时内总 Hyper-V 和规模文件服务器群集零宕机。  

**你将端口返回到 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 该功能？**  
    我们没有任何套餐以端口回退到以前版本的此功能。 群集操作系统推出升级是我们对 Windows Server 2016 到内外升级 Windows Server 2012 R2 群集的愿景。  

**Windows Server 2012 R2 群集是否需要具有开始群集操作系统推出升级过程之前安装的所有软件更新？**  
    是，在开始之前群集操作系统推出升级的过程，验证，所有群集节点进行都更新的最新的软件更新。  

**我是否可以运行[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet 时节点处于关闭状态，或暂停？**  
    不。 所有群集节点都必须在和活动的成员[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet 工作。  

**将任何群集运行工作负载的群集操作系统推出升级起作用？ 它是否适用于 SQL Server？**  
    是，群集升级操作系统推出适用于任何群集运行工作负载。 但是，它将仅零宕机 Hyper-V 和规模文件服务器群集。 大多数其他工作负载时产生某些宕机（通常几分钟）他们故障转移，并故障转移群集操作系统推出升级过程中至少运行一次，才能。  

**是否可以自动完成此过程中使用 PowerShell？**  
    是，我们已设计群集操作系统推出升级，以便自动使用 PowerShell。  

**对于具有额外的工作负荷和故障转移容量大群集，是否可以升级多个节点同时？**  
    是的。 从群集升级操作系统删除一台节点后，群集故障转移的一个较少节点，因此将拥有减少故障转移的容量。 用于大型有足够的工作负荷和容量故障转移群集中，可以同时升级的多个节点。 暂时，你可以添加群集节点向群集群集操作系统推出升级过程中提供工作负载的改进和故障转移容量。  

**如果我在我的群集后发现了以下问题时[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)已成功运行？**  
    如果你有备份群集数据库与系统状态运行之前备份[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)，你应该能够执行权威还原 Windows Server 2012 R2 群集节点和还原原始群集数据库和配置。  

**可以为每个节点而不是通过重新系统驱动器进行格式化使用清洁操作系统安装使用就地升级？**  
    我们不会建议使用就地升级的 Windows Server，但我们所知的工作原理在某些情况下，使用默认驱动程序的位置。 所有警告消息，请仔细阅读都显示群集节点就地升级的过程。  

**如果我在使用 Hyper-V 复制 Hyper-V VM 我 Hyper-V 群集上，将复制保持不变期间和群集操作系统推出升级的过程后？**  
    是，Hyper-V 副本将保持不变，期间和后群集操作系统推出升级的过程。  

**可以使用系统中心 2016 年虚拟机管理器 (SCVMM) 赛事群集操作系统推出升级？**  
    是，你可以自动使用 VMM，系统中心 2016 年中的群集操作系统推出升级过程。  

## <a name="see-also"></a>请参阅  
-   [在 Windows Server 2016 发行说明：重要问题](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [在 Windows Server 2016 的新增功能](../get-started/What-s-New-in-windows-server-2016.md)  
-   [什么是 Windows 服务器在群集故障转移中的新增功能](whats-new-in-failover-clustering.md)  
---
title: 群集操作系统滚动升级
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 03/27/2018
ms.openlocfilehash: 60dacf63f1a355b961f84169060dbd7122a6fd32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842728"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>群集操作系统滚动升级

> 适用于：Windows 服务器 （半年频道），Windows Server 2016

群集操作系统滚动升级使管理员能够在群集节点的操作系统升级而无需停止 HYPER-V 或横向扩展文件服务器工作负荷。 使用此功能可以避免服务级别协议 (SLA) 的停机时间损失。

群集操作系统滚动升级提供以下优势：

- 运行的 HYPER-V 虚拟机和横向扩展文件服务器 (SOFS) 工作负荷的故障转移群集可以升级从 Windows Server 2012 R2 （在群集中的所有节点上运行） 到 Windows Server 2016 （在群集的所有群集节点上运行） 而无需停机。 其他群集工作负荷，如 SQL Server 故障转移到 Windows Server 2016 所花费的时间 （通常小于 5 分钟） 内将不可用。  
- 它不需要任何额外的硬件。 尽管您可以添加其他群集节点暂时到小型群集在群集操作系统滚动升级过程中提高可用性的群集处理。  
- 群集不需要停止或重新启动。  
- 新的群集不需要。 现有群集升级。 此外，使用现有的群集对象存储在 Active Directory。  
- 客户选择"点-的-无返回"，在所有群集节点时正在运行 Windows Server 2016 之前，并且运行 Update-clusterfunctionallevel PowerShell cmdlet 时，升级过程是可逆的。  
- 群集可以在混合 OS 模式下运行时支持修补和维护操作。  
- 它支持通过 PowerShell 和 WMI 的自动化。  
- 群集公共属性**ClusterFunctionalLevel**属性指示在 Windows Server 2016 群集节点上群集的状态。 使用 PowerShell cmdlet 从属于故障转移群集的 Windows Server 2016 群集节点可以查询此属性：  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    值为**8**指示群集正在运行 Windows Server 2012 R2 功能级别。 值为**9**指示群集正在运行 Windows Server 2016 功能级别。  

本指南介绍了群集操作系统滚动升级过程、 安装步骤、 功能限制和常见问题 (Faq) 的各个阶段，并适用于 Windows Server 2016 中的以下群集操作系统滚动升级方案：  
- HYPER-V 群集  
- 横向扩展文件服务器群集  

在 Windows Server 2016 中不支持以下方案：  
-  群集操作系统滚动升级的来宾群集使用为共享存储的虚拟硬盘 （.vhdx 文件）  

群集操作系统滚动升级是完全支持通过 System Center Virtual Machine Manager (SCVMM) 2016年。 如果使用的 SCVMM 2016，请参阅[在 VMM 中执行滚动升级到 Windows Server 2016 的 HYPER-V 主机群集的](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807)有关升级群集和自动执行本文档中所述的步骤的指导。  

## <a name="requirements"></a>要求  
在开始群集操作系统滚动升级过程之前，请完成以下要求：

- 开始运行 Windows Server （半年频道）、 Windows Server 2016 或 Windows Server 2012 R2 的故障转移群集。
- 不支持升级到 Windows Server 存储空间直通群集，版本 1709年。
- 如果群集工作负荷的 HYPER-V Vm 或横向扩展文件服务器，您可以预计零停机时间升级。
- 验证 HYPER-V 节点具有支持二级地址表 (SLAT) 使用以下某种方法; 的 Cpu  
        -查看[是否 SLAT 兼容？WP8 SDK 提示 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx)篇文章，介绍两种方法来检查 CPU 是否支持 SLATs  
        -下载[Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722)工具来确定 CPU 是否支持 SLAT。

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>群集在群集操作系统滚动升级期间转换状态

本部分介绍 Windows Server 2012 R2 群集正在升级到 Windows Server 2016 使用群集操作系统滚动升级各种转换的状态。  

为了使运行在群集操作系统滚动升级过程中，从 Windows Server 2012 R2 节点的群集工作负荷移动到 Windows Server 2016 的群集工作负荷节点工作起来就好像这两个节点运行 Windows Server 2012 R2 操作系统。 Windows Server 2016 节点添加到群集，它们在 Windows Server 2012 R2 兼容性模式下操作。 新的概念群集模式下，名为"混合 OS 模式"，允许的不同版本可供位于同一节点群集 （请参阅图 1）。  

![显示群集操作系统滚动升级的三个阶段： Windows Server 2012 R2 的所有节点、 混合 OS 模式和 Windows Server 2016 的所有节点](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**图 1：群集操作系统状态转换**  

Windows Server 2016 节点添加到群集时，Windows Server 2012 R2 群集进入混合操作系统模式。 过程可完全撤消-可以从群集中删除 Windows Server 2016 节点，并且可以将 Windows Server 2012 R2 节点添加到在此模式下群集。 在群集上运行 Update-clusterfunctionallevel PowerShell cmdlet 时发生"点的不返回"。 为了使此 cmdlet 成功，所有节点都必须都是 Windows Server 2016 和所有节点必须都都处于联机状态。  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>四节点群集执行滚动升级操作系统时的转换状态

本部分说明了，并描述具有共享存储的节点将从 Windows Server 2012 R2 升级到 Windows Server 2016 的群集的四个不同阶段。  

"阶段 1"的初始状态-我们开始将 Windows Server 2012 R2 群集。  

![图中显示的初始状态： Windows Server 2012 R2 的所有节点](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**图 2:初始状态：Windows Server 2012 R2 故障转移群集 （第 1 阶段）**  

在"阶段 2"，两个节点已暂停，排出、 逐出、 重新格式化，并随 Windows Server 2016 一起安装。  

![在混合 OS 模式下显示群集的示意图： 移出的示例 4 节点群集，两个节点都在运行 Windows Server 2016 和两个节点都在运行 Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**图 3:中间状态：混合操作系统模式：Windows Server 2012 R2 和 Windows Server 2016 故障转移群集 (阶段 2)**  

在"阶段 3"、 在群集中节点的所有已升级到 Windows Server 2016 和群集已准备好升级使用 Update-clusterfunctionallevel PowerShell cmdlet。  

> [!NOTE]  
> 在此阶段，可以完全反向执行此流程，并可以将 Windows Server 2012 R2 节点添加到此群集。  

![显示群集已完全升级到 Windows Server 2016 中，并已准备好 Update-clusterfunctionallevel cmdlet 以便群集功能级别由 Windows Server 2016 的示意图](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**图 4:中间状态：升级到 Windows Server 2016，准备好进行 Update-clusterfunctionallevel (阶段 3) 的所有节点**  

运行更新 ClusterFunctionalLevelcmdlet 后，群集将进入"阶段 4"，可以使用新的 Windows Server 2016 群集功能的位置。  

![显示群集滚动操作系统升级已成功完成;所有节点均已都升级到 Windows Server 2016 和群集运行在 Windows Server 2016 群集功能级别](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**图 5:最终状态：Windows Server 2016 故障转移群集 （阶段 4）**  

## <a name="cluster-os-rolling-upgrade-process"></a>群集操作系统滚动升级过程

本部分介绍用于执行群集操作系统滚动升级的工作流。  

![显示升级群集的工作流示意图](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**图 6:群集操作系统滚动升级过程工作流**  

群集操作系统滚动升级包括以下步骤：  

1. 按如下所示准备用于操作系统升级的群集：  
    1. 群集操作系统滚动升级，则需要从群集中一次删除一个节点。 检查您是否具有足够的容量群集来维护 HA Sla 时从操作系统升级群集中删除其中一个群集节点上。 换而言之，您需要将工作负荷故障转移到另一个节点的功能时的群集操作系统滚动升级过程中从群集删除一个节点？ 群集是否足够容量来时为群集操作系统滚动升级从群集删除一个节点运行所需的工作负荷？  
    2. 对于 HYPER-V 工作负荷，检查所有 Windows Server 2016 HYPER-V 主机都必须支持二级地址表 (SLAT) 的 CPU。 仅支持 SLAT 的计算机可以使用 Windows Server 2016 中的 HYPER-V 角色。  
    3. 检查任何工作负荷备份已完成，并考虑备份群集。 向群集添加节点时停止备份操作。  
    4. 检查所有群集节点都处于联机状态/运行/上使用[ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet （请参阅图 7）。  

        ![方屏幕截图显示正在运行的 Get-clusternode cmdlet 的结果](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **图 7:确定使用 Get-clusternode cmdlet 的节点状态**  

    5. 如果运行的群集感知更新 (CAU)，验证是否当前正在 CAU 运行通过使用**群集意识到更新**UI 中，或[ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet （请参阅图 8）。 停止使用 CAU [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet （见图 9） 以防止被暂停或由 CAU 在群集操作系统滚动升级过程中排除任何节点。  

        ![方屏幕截图显示 Get-caurun cmdlet 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **图 8:使用[ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet 来确定在群集上运行的群集感知更新**  

        ![方屏幕截图显示 Disable-cauclusterrole cmdlet 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **图 9:群集感知更新角色使用禁用[ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet**  

2. 对于群集中每个节点，完成以下任务：  
    1. 使用群集管理器 UI，选择一个节点，然后使用**暂停 |清空**菜单选项来排出该节点 （请参阅图 10） 或使用[ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet （请参阅图 11）。  

        ![方屏幕截图显示了如何清空角色使用群集管理器 UI](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **图 10:排出节点使用故障转移群集管理器中的角色**  

        ![方屏幕截图显示 Suspend-clusternode cmdlet 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **图 11:排出节点使用的角色[ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet**  

    2.  使用群集管理器 UI**逐出**暂停的节点从群集中或使用[ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet。  

        ![方屏幕截图显示删除节点 cmdlet 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **图 12:从群集中使用删除节点[ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet**  

    3.  系统驱动器重新格式化和执行"干净操作系统安装"Windows Server 2016 的节点使用**自定义：仅安装 Windows （高级）** 在 setup.exe 的安装 (请参阅图 13) 选项。 避免选择**升级：安装 Windows 和保留文件、 设置和应用程序**选项由于群集操作系统滚动升级不鼓励在就地升级。  

        ![方的 Windows Server 2016 安装向导显示所选的自定义安装选项的屏幕截图](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **图 13:Windows Server 2016 的可用安装选项**  

    4.  将节点添加到相应的 Active Directory 域。  
    5.  将相应的用户添加到管理员组。  
    6.  使用服务器管理器 UI 或 Install-windowsfeature PowerShell cmdlet 安装任何需要例如 HYPER-V 的服务器角色。  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  使用服务器管理器 UI 或 Install-windowsfeature PowerShell cmdlet 安装故障转移群集功能。  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  安装所需的群集工作负荷的任何其他功能。  
    9. 检查网络和存储的连接设置，请使用故障转移群集管理器 UI。  
    10. 如果使用 Windows 防火墙，检查防火墙设置是否适合群集。 例如，群集感知更新 (CAU) 启用了群集可能需要防火墙配置。  
    11. 对于 HYPER-V 工作负荷，使用 HYPER-V 管理器 UI 中启动的虚拟交换机管理器对话框 （请参阅图 14）。  

        检查虚拟 Switch(s) 使用的名称是相同的 HYPER-V 主机群集中的所有节点。  

        ![方屏幕截图显示 HYPER-V 虚拟交换机管理器对话框中的位置](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **图 14:虚拟交换机管理器**  

    12. Windows Server 2016 节点上 （不使用 Windows Server 2012 R2 节点），使用故障转移群集管理器 （请参阅图 15） 以连接到群集。  

        ![方屏幕截图显示选择群集对话框](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **图 15:将节点添加到群集使用故障转移群集管理器**  

    13. 使用故障转移群集管理器 UI 或[ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet （参阅图 16） 将节点添加到群集。  

        ![方屏幕截图显示 Add-clusternode cmdlet 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **图 16:将节点添加到群集使用[ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet**  

        > [!NOTE]  
        > 当第一个 Windows Server 2016 节点加入群集时，群集进入"Mixed OS"模式和群集核心资源移到 Windows Server 2016 节点。 "混合 OS"模式下群集是新的节点在兼容性模式下使用旧的节点的运行位置的完全正常运行群集。 "混合 OS"模式是为群集的暂时性模式。 它不应为永久重定向，客户需四个星期内更新其群集的所有节点。  

    14. 在 Windows Server 2016 后节点已成功添加到群集，你可以 （可选） 将一些移动的群集工作负荷到新添加的节点才能间重新平衡工作负荷的群集，如下所示：

        ![方屏幕截图显示 Move-clustervirtualmachinerole cmdlet 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **图 17:群集工作负载 （群集 VM 角色） 使用移动[ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet**  

        1. 使用**实时迁移**从故障转移群集管理器的虚拟机或[ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet （参阅图 17） 来执行虚拟机的实时迁移。  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. 使用**移动**从故障转移群集管理器或[ `Move-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) cmdlet 的其他群集工作负荷。  

3. 当升级到 Windows Server 2016 的每个节点并将其添加回群集，或已逐出任何剩余的 Windows Server 2012 R2 节点时，请执行以下操作：  

    > [!IMPORTANT]  
    > -   更新群集功能级别后，不能返回到 Windows Server 2012 R2 的功能级别并不能将 Windows Server 2012 R2 节点添加到群集。
    > -   直到[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)运行 cmdlet，该过程是完全可还原和可以将 Windows Server 2012 R2 节点添加到此群集可以删除 Windows Server 2016 节点。  
    > -   之后[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)运行 cmdlet 时，新功能将不可用。  

    1.  使用故障转移群集管理器 UI 或[ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet，检查所有群集角色都正在群集按预期方式。 在以下示例中，未使用可用的存储，改为使用 CSV，因此，显示可用的存储**脱机**状态 （请参阅图 18）。  

        ![方屏幕截图显示 Get-clustergroup cmdlet 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **图 18:验证所有群集组 （群集角色） 使用运行[ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet**  

    2.  检查所有群集节点都处于联机状态和运行使用[ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet。  
    3.  运行[ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet-不应返回错误 （请参阅图 19）。  

        ![方屏幕截图显示 Update-clusterfunctionallevel cmdlet 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **图 19:更新使用 PowerShell 的群集的功能级别**  

    4.  之后[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)运行 cmdlet 时，提供了新功能。  

4. Windows Server 2016-恢复正常的群集更新和备份：  

    1. 如果以前运行 CAU，请重新启动它使用 CAU UI 或使用[ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet （请参阅图 20）。  

        ![方屏幕截图显示 Enable-cauclusterrole 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **图 20:启用群集感知更新角色使用[ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet**  

    2. 恢复备份操作。  

5. 启用和使用的 HYPER-V 虚拟机上的 Windows Server 2016 功能。  

    1. 群集升级到 Windows Server 2016 功能级别后，许多工作负载，如 HYPER-V Vm 将具有新功能。 有关新的 HYPER-V 功能的列表。 请参阅[迁移和升级虚拟机](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. 在群集中每个 HYPER-V 主机节点上使用[ `Get-VMHostSupportedVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) cmdlet 查看支持的主机的 HYPER-V VM 配置版本。  

        ![方屏幕截图显示 Get VMHostSupportedVersion cmdlet 的输出](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **图 21:查看支持的主机的 HYPER-V VM 配置版本**  

   3.  在群集中的每个 HYPER-V 主机节点，可以通过与用户计划一个短的维护时段、 备份、 关闭虚拟机和运行升级的 HYPER-V VM 配置版本[ `Update-VMVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) cmdlet （请参阅图 22）。 这将更新虚拟机版本，并启用新的 HYPER-V 功能，从而无需将来的 HYPER-V 集成组件 (IC) 更新。 可以从托管 VM 的 HYPER-V 节点运行此 cmdlet 或`-ComputerName`参数可用于远程更新 VM 版本。 在此示例中，此处我们 VM1 的配置版本从升级 5.0 到 7.0 才能利用此虚拟机配置版本为生产检查点 （应用程序一致性备份） 和二进制 VM 等相关联的许多新的 HYPER-V 功能配置文件。  

        ![在操作中显示 Update-vmversion cmdlet 方屏幕截图](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
        **图 22:使用 Update-vmversion PowerShell cmdlet 将 VM 版本升级**  

4.  可以使用升级存储池[Update-storagepool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) PowerShell cmdlet-这是一个联机操作。  

尽管我们面向私有云方案，特别是 HYPER-V 和横向扩展文件服务器群集，可以升级而无需停机，群集操作系统滚动升级过程可以用于任何群集角色。  

## <a name="restrictions--limitations"></a>限制 / 限制  
- 此功能仅适用于 Windows Server 2012 R2 到 Windows Server 2016 版本。 此功能不能如 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 的 Windows Server 的早期版本升级到 Windows Server 2016。  
- Windows Server 2016 中的每个节点应重新格式化/新的安装。 "就地"升级"安装类型，建议不要使用。  
- Windows Server 2016 节点必须用于将 Windows Server 2016 节点添加到群集。  
- 在管理混合 OS 模式群集时，始终从正在运行 Windows Server 2016 的上一级节点中执行管理任务。 下层 Windows Server 2012 R2 节点不能使用针对 Windows Server 2016 的 UI 或管理工具。  
- 我们鼓励客户迁移完成群集升级过程快速由于某些群集功能并没有对混合 OS 模式优化。  
- 避免创建或同时群集在混合 OS 模式下运行由于可能的不兼容性故障转移从 Windows Server 2016 的节点的下级 Windows Server 2012 R2 节点到调整 Windows Server 2016 节点上的存储的大小。  

## <a name="frequently-asked-questions"></a>常见问题解答  
**故障转移群集的运行时间在混合 OS 模式下？**  
    我们鼓励客户完成四个星期内的升级。 有很多优化 Windows Server 2016 中。 我们已成功升级的 HYPER-V 和横向扩展文件服务器在不超过 4 小时内总群集零停机时间。  

**将端口回 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 的此功能？**  
    我们并没有任何端口返回到以前版本此功能的计划。 群集操作系统滚动升级是我们的愿景的 Windows Server 2012 R2 群集升级到 Windows Server 2016 和更高版本。  

**Windows Server 2012 R2 群集是否需要在开始群集操作系统滚动升级过程之前安装的所有软件更新？**  
    是的开始群集操作系统滚动升级过程之前，验证所有群集节点都将都更新与最新的软件更新。  

**可以运行[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet 时节点处于关闭状态或暂停？**  
    否。 所有群集节点都必须都是在和中的有效的成员资格[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet 来工作。  

**对于任何群集工作负荷的群集操作系统滚动升级工作原理？它是否适用于 SQL Server？**  
    是的群集操作系统滚动升级适用于任何群集工作负荷。 但是，它是仅零停机时间的 HYPER-V 和横向扩展文件服务器群集。 大多数其他工作负荷会产生一定的停机时间 （通常需要花费几分钟），当它们至少一次在群集操作系统滚动升级过程中的故障转移和故障转移是必需。  

**可以自动执行此过程使用 PowerShell？**  
    是的我们已经设计群集操作系统滚动升级以使用 PowerShell 自动完成。  

**针对大型群集具有额外的工作负荷和故障转移容量，可以升级多个节点同时？**  
    是。 当从以升级操作系统的群集中删除一个节点时，群集将有一个较少节点进行故障转移，因此将具有降低的故障转移容量。 对于具有足够的工作负荷和故障转移容量的大型群集，可以同时升级多个节点。 您可以临时将群集节点添加到群集以在群集操作系统滚动升级过程中提供改进的工作负荷和故障转移容量。  

**在我的群集后发现了问题怎么办[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)已成功运行？**  
    如果您已备份的群集数据库与之前运行的系统状态备份[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)，您应能够执行授权还原 Windows Server 2012 R2 群集节点上并还原原始的群集数据库和配置。  

**可以为每个节点而不是通过重新格式化系统驱动器中使用的清理操作系统安装使用就地升级？**  
    我们不建议使用的 Windows Server 的就地升级，但我们已经注意到它在某些情况下，使用默认驱动程序的位置工作。 请仔细阅读在群集节点的就地升级过程中显示所有警告消息。  

**如果我正在使用 HYPER-V 复制的 HYPER-V vm 上的 HYPER-V 群集，将复制保持不变期间和之后群集操作系统滚动升级过程？**  
    是的 HYPER-V 副本保持不变，期间和之后群集操作系统滚动升级过程。  

**可以使用 System Center 2016 Virtual Machine Manager (SCVMM) 自动执行群集操作系统滚动升级过程？**  
    是的您可以在 System Center 2016 中使用 VMM 群集操作系统滚动升级过程进行自动化。  

## <a name="see-also"></a>请参阅  
-   [发行说明：Windows Server 2016 中的重要问题](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [什么是 Windows Server 2016 中的新增功能](../get-started/What-s-New-in-windows-server-2016.md)  
-   [什么是 Windows Server 中故障转移群集中的新增功能](whats-new-in-failover-clustering.md)  

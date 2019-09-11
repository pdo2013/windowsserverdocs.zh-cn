---
title: 群集集
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: 本文介绍群集集方案
ms.localizationpriority: medium
ms.openlocfilehash: 973725d56fcd3a276a2aad3c61820b454d613684
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865023"
---
# <a name="cluster-sets"></a>群集集

> 适用于：Windows Server 2019

群集集是 Windows Server 2019 发行版中新的云扩展技术，它按数量级增加单个软件定义数据中心（SDDC）云中群集节点计数。 群集集是多个故障转移群集的松耦合分组：计算、存储或超聚合。 群集集技术允许跨群集集内的成员群集和统一存储命名空间中的虚拟机流畅性，以支持虚拟机流畅性。

在保留成员群集上的现有故障转移群集管理体验的同时，群集集实例还在聚合时提供有关生命周期管理的主要用例。 此 Windows Server 2019 应用场景评估指南提供了必需的背景信息以及使用 PowerShell 评估群集集技术的分步说明。

## <a name="technology-introduction"></a>技术简介

开发群集集技术是为了满足特定客户大规模的操作软件定义数据中心（SDDC）云的要求。 群集集价值陈述可能会概括如下：  

- 通过将多个较小的群集合并为单个大型构造，使支持的 SDDC 云规模大大提高，甚至可将软件故障边界合并为单个群集
- 通过流畅地将虚拟机迁移到跨此大型构造，管理整个故障转移群集生命周期，包括加入和淘汰群集，而不影响租户虚拟机的可用性
- 在超聚合 I 中轻松更改计算与存储的比率
- 在初始虚拟机放置和后续的虚拟机迁移中，在群集上跨群集[实现类似于 Azure 的容错域和可用性集](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)
- 将 CPU 硬件的不同代混合并匹配到同一个群集集构造，即使在将各个容错域保持为最高效率时也是如此。  请注意，每个单独的群集和整个群集集内仍存在相同硬件的建议。

从较高的层次来看，这就是群集集的外观。

![群集集解决方案视图](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

以下内容提供了上图中每个元素的快速摘要：

**管理群集**

群集集中的管理群集是一个故障转移群集，它承载整个群集集和统一存储命名空间（群集集命名空间）引用横向扩展文件服务器（SOFS）的高度可用管理平面。 管理群集从逻辑上与运行虚拟机工作负荷的成员群集分离。 这使得群集集管理平面可以灵活应对任何已本地化的群集范围内的故障，例如，成员群集的功能丢失。   

**成员群集**

群集集中的成员群集通常是运行虚拟机和存储空间直通工作负荷的传统超聚合群集。 多个成员群集参与单个群集集部署，形成更大的 SDDC 云结构。 成员群集在两个主要方面与管理群集不同：成员群集加入容错域和可用性集构造，成员群集的大小也调整为托管虚拟机和存储空间直通工作负荷。 由于此原因，在群集集中移动到群集边界的群集集虚拟机不能在管理群集上托管。

**群集集命名空间引用 SOFS**

群集集命名空间引用（群集集命名空间） SOFS 是一个横向扩展文件服务器，其中群集集命名空间 SOFS 上的每个 SMB 共享都是一个 "SimpleReferral 2019" 类型的引用共享。 此引用允许服务器消息块（SMB）客户端访问成员群集上托管的目标 SMB 共享 SOFS。 群集集命名空间引用 SOFS 是一种轻型引用机制，因此不参与 i/o 路径。 SMB 引用将缓存在每个客户端节点上的永久，群集集命名空间会根据需要动态地自动更新这些引用。

**群集集主机**

在群集集中，成员群集之间的通信松耦合，并通过名为 "群集集主机" （CS-Master）的新群集资源进行协调。 与任何其他群集资源一样，CS-主服务器具有高可用性，并且能够灵活应对各个成员群集故障和/或管理群集节点故障。 通过新群集集 WMI 提供程序，CS-主服务器为所有群集集可管理性交互提供管理终结点。

**群集集辅助角色**

在群集集部署中，CS 主服务器与名为 "群集集辅助角色" （CS 辅助角色）的成员群集上的新群集资源交互。 CS-辅助角色充当群集上的唯一联系，以根据 CS 主管的请求协调本地群集交互。 此类交互的示例包括虚拟机放置和群集本地资源清点。 群集集中的每个成员群集都只有一个 CS 辅助角色实例。 

**容错域**

容错域是指当发生故障时，管理员确定的软件和硬件项目的分组可能会故障转移到一起。  虽然管理员可以将一个或多个群集作为容错域一起指定，但每个节点都可以参与可用性集中的容错域。 群集集按设计，将容错域边界确定决定给精通数据中心拓扑的管理员（例如，PDU、网络）。 

**可用性集**

可用性集可帮助管理员跨容错域配置群集工作负载所需的冗余，方法是将它们组织到可用性集中并将工作负荷部署到该可用性集中。 假设你正在部署双层应用程序，我们建议你在可用性集中为每个层配置至少两个虚拟机，这将确保当该可用性集中的一个容错域出现故障时，你的应用程序将至少具有每一层中的一个虚拟机托管在同一可用性集的不同容错域中。

## <a name="why-use-cluster-sets"></a>为什么使用群集集

群集集可提供大规模权益，而无需牺牲复原能力。  

群集集允许将多个群集聚集在一起，从而创建一个大型构造，而每个群集在恢复时保持独立。  例如，有多个运行虚拟机的4节点 HCI 群集。  每个群集都提供自身所需的复原能力。  如果存储或内存开始填满，则升级是下一步。  进行扩展时，有一些选项和注意事项。

1. 向当前群集添加更多存储。  使用存储空间直通时，这可能很难，因为完全相同的型号/固件驱动器可能不可用。  还需要考虑重新生成时间。
2. 添加内存。  如果极限计算机可以处理的内存，会怎么样？  如果所有可用的内存槽全部可用，会怎么样？
3. 向当前群集添加包含驱动器的其他计算节点。  这会让我们返回到需要考虑的选项1。
4. 购买全新的群集

这就是群集集提供缩放优点的地方。  如果将群集添加到群集集，则可以利用其他群集上可用的存储或内存，而无需进行任何额外的购买。  从复原角度来看，将其他节点添加到存储空间直通不会为仲裁提供额外的投票。  如[此处](drive-symmetry-considerations.md)所述，在停止之前，存储空间直通群集可能会丢失2个节点。  如果有4节点 HCI 群集，3个节点会关闭，从而使整个群集停止运行。  如果有一个8节点群集，3个节点会关闭，从而使整个群集停止运行。  对于集中有两个4节点 HCI 群集的群集集，一个 HCI 中的2个节点会关闭，另一个 HCI 中有1个节点出现故障。  最好创建一个大型16节点存储空间直通群集，或将其分解为四个4节点群集并使用群集集吗？  具有群集集的四个4节点群集提供了相同的规模，但更好的复原能力是，多个计算节点可能会关闭（意外或维护），生产仍保持不变。

## <a name="considerations-for-deploying-cluster-sets"></a>部署群集集的注意事项

考虑是否需要使用群集集时，请考虑以下问题：

- 是否需要超出当前的 HCI 计算和存储规模限制？
- 所有计算和存储都不完全相同吗？
- 是否在群集之间实时迁移虚拟机？
- 是否需要跨多个群集的 Azure 计算机可用性集和容错域？
- 是否需要花时间查看所有群集来确定需要将任何新虚拟机放置在何处？

如果答案为 "是"，则需要群集集。

还有其他几项需要考虑的是，较大的 SDDC 可能会改变总体数据中心策略。  SQL Server 是一个很好的示例。  在群集之间移动 SQL Server 虚拟机是否需要授权 SQL 才能在其他节点上运行？  

## <a name="scale-out-file-server-and-cluster-sets"></a>横向扩展文件服务器和群集集

在 Windows Server 2019 中，有一个名为 "基础结构横向扩展文件服务器" 的新的横向扩展文件服务器角色（SOFS）。 

以下注意事项适用于基础结构 SOFS 角色：

1.  在一个故障转移群集上，最多只能有一个 SOFS 群集角色。 基础结构 SOFS 角色是通过将 " **-基础结构**" 交换机参数指定给**ClusterScaleOutFileServerRole** cmdlet 创建的。  例如：

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  在故障转移过程中创建的每个 CSV 卷都将使用基于 CSV 卷名的自动生成的名称来触发创建 SMB 共享。 管理员不能直接创建或修改 SOFS 角色下的 SMB 共享，而不是通过 CSV 卷创建/修改操作。

3.  在超聚合配置中，一种基础结构 SOFS 允许 SMB 客户端（Hyper-v 主机）与基础结构 SOFS SMB 服务器进行通信，以确保持续可用性（CA）。 此超聚合 SMB 环回 CA 是通过访问其虚拟磁盘（VHDx）文件的虚拟机实现的，其中，在客户端和服务器之间转发所属的虚拟机标识。 与之前一样，此标识转发使 ACL 的 VHDx 文件与标准的超聚合群集配置类似。

创建群集集后，群集集命名空间将依赖于每个成员群集上的基础结构 SOFS，此外还会在管理群集中 SOFS 基础结构。

将成员群集添加到群集集时，管理员在该群集上指定基础结构 SOFS 的名称（如果已存在）。 如果基础结构 SOFS 不存在，则此操作将创建新成员群集上的新基础结构 SOFS 角色。 如果成员群集上已存在基础结构 SOFS 角色，则 Add 操作会根据需要隐式地将其重命名为指定的名称。 群集集对成员群集上的任何现有的单一实例 SMB 服务器或非基础结构 SOFS 角色进行未利用。 

创建群集集时，管理员可以选择使用已有的 AD 计算机对象作为管理群集上的命名空间根目录。 群集集创建操作在管理群集上创建基础结构 SOFS 群集角色，或重命名现有的基础结构 SOFS 角色，就像前面为成员群集所述。 管理群集上的基础结构 SOFS 用作群集集命名空间引用（群集集命名空间） SOFS。 这只意味着群集集命名空间 SOFS 上的每个 SMB 共享都是 "SimpleReferral" 类型的引用共享-在 Windows Server 2019 中是新引入的。  此引用允许 SMB 客户端访问成员群集 SOFS 上托管的目标 SMB 共享。 群集集命名空间引用 SOFS 是一种轻型引用机制，因此不参与 i/o 路径。 SMB 引用将缓存在每个客户端节点上的永久，群集集命名空间会根据需要动态地自动更新这些引用

## <a name="creating-a-cluster-set"></a>创建群集集

### <a name="prerequisites"></a>先决条件

创建群集集时，建议遵循以下先决条件：

1. 配置运行 Windows Server 2019 的管理客户端。
2. 在此管理服务器上安装故障转移群集工具。
3. 创建群集成员（至少有两个群集，每个群集上至少有两个群集共享卷）
4. 创建跨越成员群集的管理群集（物理或来宾）。  这种方法可确保群集集管理平面仍可用，但可能会导致成员群集失败。

### <a name="steps"></a>步骤

1. 根据必备组件中的定义，从三个群集创建新群集集。  下图提供了创建群集的示例。  在此示例中，群集集的名称将为**CSMASTER**。

   | 群集名称               | 稍后要使用的基础结构 SOFS 名称 | 
   |----------------------------|-------------------------------------------|
   | 设置-群集                | SOFS-CLUSTERSET                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. 创建所有群集后，使用以下命令创建群集集主机。

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. 若要将群集服务器添加到群集集，可以使用以下方法。

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > 如果使用的是静态 IP 地址方案，则必须在**ClusterSet**命令中包含 *-StaticAddress x* . x. x. x. x. x. x. x. x。

4. 一旦你创建了群集集的群集成员，你就可以列出节点集及其属性。  枚举群集集中的所有成员群集：

        Get-ClusterSetMember -CimSession CSMASTER

5. 枚举群集集中的所有成员群集（包括管理群集节点）：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. 列出成员群集中的所有节点：

        Get-ClusterSetNode -CimSession CSMASTER

7. 列出整个群集集中的所有资源组：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. 若要验证群集集创建过程创建了一个 SMB 共享（标识为 Volume1，还是在每个群集成员的基础结构 SOFS 上将 ScopeName 标记为 "基础结构文件服务器的名称" 和 "两者" 的路径）CSV 卷：

        Get-SmbShare -CimSession CSMASTER

8. 群集集具有可收集以供查看的调试日志。  可以为所有成员和管理群集收集群集集和群集调试日志。

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. 在所有群集集成员之间配置 Kerberos[约束委派](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/)。

10. 在群集集中的每个节点上，将跨群集虚拟机的实时迁移身份验证类型配置为 Kerberos。

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. 将管理群集添加到群集集中每个节点上的本地管理员组中。

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>创建新虚拟机并将其添加到群集集

创建群集集后，下一步是创建新的虚拟机。  通常情况下，在创建虚拟机并将其添加到群集时，需要在群集上执行一些检查，以确定最好在哪个群集上运行。  这些检查包括：

- 群集节点上有多少内存可用？
- 群集节点上有多少可用磁盘空间？
- 虚拟机是否需要特定的存储要求（也就是说，我想让 SQL Server 虚拟机中转到运行更快驱动器的群集; 或者，我的基础结构虚拟机并不重要，可以在速度较慢的驱动器上运行）。

解决此问题后，你需要在群集上创建需要该虚拟机的虚拟机。  群集集的优点之一是，群集集为你执行这些检查，并将虚拟机放置在最佳节点上。

以下命令将标识最佳群集并将虚拟机部署到该群集。  在下面的示例中，创建了一个新的虚拟机，该虚拟机指定至少有 4 gb 的内存可用于虚拟机，并且需要使用1个虚拟处理器。

- 确保为虚拟机提供4gb
- 设置用于1的虚拟处理器
- 检查以确保至少有 10% 的 CPU 可用于虚拟机

   ```PowerShell
   # Identify the optimal node to create a new virtual machine
   $memoryinMB=4096
   $vpcount = 1
   $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10
   $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
   $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

   # Deploy the virtual machine on the optimal node
   Invoke-Command -ComputerName $targetnode.name -scriptblock { param([String]$storagepath); New-VM CSVM1 -MemoryStartupBytes 3072MB -path $storagepath -NewVHDPath CSVM.vhdx -NewVHDSizeBytes 4194304 } -ArgumentList @("\\SOFS-CLUSTER1\VOLUME1") -Credential $cred | Out-Null
   
   Start-VM CSVM1 -ComputerName $targetnode.name | Out-Null
   Get-VM CSVM1 -ComputerName $targetnode.name | fl State, ComputerName
   ```

完成后，系统会向你提供有关虚拟机及其放置位置的信息。  在上面的示例中，它将显示为：

        State         : Running
        ComputerName  : 1-S2D2

如果没有足够的内存、cpu 或磁盘空间来添加虚拟机，您将收到以下错误：

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

创建虚拟机后，该虚拟机将显示在指定的特定节点上的 Hyper-v 管理器中。  若要将它作为群集集的虚拟机和群集添加到群集中，请执行以下命令。  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

完成后，输出将为：

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

如果已使用现有虚拟机添加了群集，则还需要向群集集注册虚拟机，以便一次注册所有虚拟机，使用以下命令：

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

但是，此过程并未完成，因为需要将虚拟机的路径添加到群集集命名空间中。

例如，添加了现有群集，并且它具有驻留在本地群集共享卷（CSV）的预配置虚拟机，VHDX 的路径将类似于 "C:\ClusterStorage\Volume1\MYVM\Virtual Hard Disks\MYVM.vhdx。  需要完成存储迁移，因为 CSV 路径是由本地设计到单个成员群集的。 因此，一旦虚拟机在各个成员群集中进行实时迁移，就不能访问它们。 

在此示例中，使用 ClusterSetMember 将 CLUSTER3 添加到群集集，并将基础结构横向扩展文件服务器为 SOFS-CLUSTER3。  若要移动虚拟机配置和存储，命令为：

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

完成后，你会收到一条警告：

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

此警告可以忽略，因为警告为 "未检测到虚拟机角色存储配置中的任何更改"。  警告的原因是实际的物理位置不更改;仅配置路径。 

有关 VMStorage 的详细信息，请查看此[链接](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps)。 

在不同群集集群集之间实时迁移虚拟机与过去不同。 在非群集集方案中，步骤如下：

1. 从群集中删除虚拟机角色。
2. 将虚拟机实时迁移到另一个群集的成员节点。
3. 将虚拟机作为新虚拟机角色添加到群集中。

对于群集集，无需执行这些步骤，只需执行一个命令。  首先，应使用以下命令将所有网络设置为可用于迁移：

    Set-VMHost -UseAnyNetworkMigration $true

例如，我想要将群集集的虚拟机从 CLUSTER1 移动到 CLUSTER3 上的 CL3。  单个命令为：

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

请注意，这不会移动虚拟机存储或配置文件。  这并不是必需的，因为虚拟机的路径保留\\为 SOFS-CLUSTER1\VOLUME1。  向群集集注册虚拟机后，该虚拟机将具有基础结构文件服务器共享路径，并且不需要将驱动器和虚拟机与虚拟机位于同一台计算机上。

## <a name="creating-availability-sets-fault-domains"></a>创建可用性集容错域

如简介中所述，可以在群集集中配置类似 Azure 的容错域和可用性集。  这对于群集之间的初始虚拟机放置和迁移非常有用。  

在下面的示例中，有四个群集参与群集集。  在集内，将使用两个分类创建一个逻辑容错域，并使用其他两个分类创建一个容错域。  这两个容错域将包含可用性集。 

在下面的示例中，CLUSTER1 和 CLUSTER2 将位于名为**FD1**的容错域中，而 CLUSTER3 和 CLUSTER4 将位于名为**FD2**的容错域中。  可用性集将称为**CSMASTER** ，并由两个容错域组成。

若要创建容错域，请执行以下命令：

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

若要确保已成功创建它们，可以运行 ClusterSetFaultDomain 并显示其输出。

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

现在，已创建了容错域，需要创建可用性集。

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

若要验证是否已创建，请使用：

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

创建新虚拟机时，需要在确定最佳节点时使用-AvailabilitySet 参数。  这样就会如下所示：

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

由于各种生命周期，从群集集中删除群集。 有时需要从群集集中删除群集。 作为最佳做法，所有群集集的虚拟机都应移出群集。 这可以通过使用**ClusterSetVM**和**VMStorage**命令来完成。

但是，如果还不移动虚拟机，则群集集将运行一系列操作以向管理员提供直观的结果。  从集中删除群集后，在要删除的群集上托管的所有剩余群集集虚拟机将只会成为与该群集绑定的高可用虚拟机，前提是这些虚拟机有权访问其存储。  群集集还将通过以下方式自动更新其清单：

- 不再跟踪正在删除的群集及其上运行的虚拟机的运行状况
- 从群集集命名空间中删除以及对在现在删除的群集上托管的共享的所有引用

例如，从群集集中删除 CLUSTER1 群集的命令将是：

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>常见问题 (FAQ)

**问**在我的群集集中，我是否局限于只使用超聚合群集？ <br>
**答案**否。  可以将存储空间直通与传统分类混合使用。

**问**可以通过 System Center Virtual Machine Manager 来管理群集集吗？ <br>
**答案**System Center Virtual Machine Manager 当前不支持群集集 <br><br> **问**Windows Server 2012 R2 或2016群集是否可以在同一群集集中共存？ <br>
**问**只需让这些群集加入同一群集集，就可以在 Windows Server 2012 R2 或2016群集之外迁移工作负荷了吗？ <br>
**答案**群集集是 Windows Server 2019 中引入的一项新技术，因此在以前的版本中不存在。 基于 OS 的下层群集无法加入群集集。 但是，群集操作系统滚动升级技术应通过将这些群集升级到 Windows Server 2019，提供要查找的迁移功能。

**问**群集集是否允许我缩放存储或计算（单独）？ <br>
**答案**是的，只需添加存储空间直通或传统的 Hyper-v 群集。 对于群集集，即使在超聚合群集集中，它也会直接更改计算到存储的比率。

**问**群集集的管理工具是什么 <br>
**答案**此版本中的 PowerShell 或 WMI。

**问**跨群集实时迁移如何与不同代的处理器配合工作？  <br>
**答案**群集集不能解决处理器差异，取代 Hyper-v 当前支持的功能。  因此，处理器兼容模式必须与快速迁移一起使用。  对于群集集，建议使用每个单独分类中的相同处理器硬件，并使用整个群集集在群集之间实时迁移。

**问**群集是否可以在群集发生故障时自动进行故障转移？  <br>
**答案**在此版本中，只能在群集之间手动实时迁移群集集的虚拟机;但无法自动故障转移。 

**问**如何确保存储对群集故障有复原能力？ <br>
**答案**跨成员群集使用跨群集存储副本（SR）解决方案，以实现群集故障的存储弹性。

**问**我使用存储副本（SR）跨成员群集进行复制。 是否在将 SR 故障转移到副本目标存储空间直通群集时，群集集命名空间存储 UNC 路径是否发生更改？ <br>
**答案**在此版本中，此类群集集命名空间引用更改不会与 SR 故障转移一起发生。 如果此方案对你和你计划使用该方案非常重要，请让 Microsoft 知道这种情况。

**问**在灾难恢复情况下，是否可以跨容错域对虚拟机进行故障转移（假设整个容错域已关闭）？ <br>
**答案**不能，请注意，目前尚不支持逻辑容错域中的跨群集故障转移。 

**问**群集集中的群集是否可以跨多个站点（或 DNS 域）？ <br> 
**答案**这是未经测试的方案，不会立即计划生产支持。 如果此方案对你和你计划使用该方案非常重要，请让 Microsoft 知道这种情况。

**问**群集集是否可与 IPv6 一起使用？ <br>
**答案**群集集与故障转移群集一样，都支持 IPv4 和 IPv6。

**问**群集集的 Active Directory 林要求是什么 <br>
**答案**所有成员群集都必须在同一个 AD 林中。

**问**有多少群集或节点可以作为单个群集集的一部分？ <br>
**答案**在 Windows Server 2019 中，群集集经过测试并支持最多64个群集节点。 但是，群集集体系结构可扩展到更大的限制，并且不是对限制的硬编码。 如果你和你计划使用它的规模是否更大，请让 Microsoft 知道。

**问**群集集中的所有存储空间直通群集是否构成单个存储池？ <br>
**答案**否。 存储空间直通技术仍在单个群集内运行，而不是在群集集中的成员群集上运行。

**问**群集集命名空间是否具有高可用性？ <br>
**答案**是的，群集集命名空间是通过在管理群集上运行的连续可用（CA）引用 SOFS 命名空间服务器提供的。 Microsoft 建议从成员群集中有足够数量的虚拟机，使其能够应对本地化的群集范围内的故障。 但是，若要考虑不可预见的灾难性故障（例如，管理群集中的所有虚拟机同时关闭），则在每个群集集节点上，甚至在重新启动时也会永久缓存引用信息。
 
**问**群集集基于命名空间的存储访问是否会减缓群集集中的存储性能？ <br>
**答案**否。 群集集命名空间在群集集中提供覆盖引用命名空间-在概念上类似于分布式文件系统命名空间（DFSN）。 与 DFSN 不同，所有群集集命名空间引用元数据都是自动填充的，在所有节点上自动更新，而无需管理员干预，因此在存储访问路径中几乎不会产生性能开销。 

**问**如何备份群集集元数据？ <br>
**答案**本指南与故障转移群集的指南相同。 系统状态备份也会备份群集状态。  通过 Windows Server 备份，您可以只还原节点的群集数据库（由于有大量的自修复逻辑，此操作永远不需要）或执行权威还原，以便在所有节点上回滚整个群集数据库。 对于群集集，Microsoft 建议在成员群集上首先执行此类权威还原，然后在需要时执行管理群集。

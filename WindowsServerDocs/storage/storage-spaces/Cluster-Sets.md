---
title: 群集集
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: 本文介绍了群集集方案
ms.localizationpriority: medium
ms.openlocfilehash: 2deeb6968f910e80bacb2354ad2e575060a7797a
ms.sourcegitcommit: 07aefbdbb0eedb42aaed3d195c2429310c761da0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "9041003"
---
# 群集集

> 适用于： Windows Server Insider Preview 版本 17650 及更高版本

群集集是通过极大地增加了单一的软件定义数据中心 (SDDC) 云中的群集节点计数此预览版本中的新云横向扩展技术。 群集集是多个故障转移群集的松散耦合分组： 计算、 存储或超聚合。 群集设置技术使虚拟机流畅性跨群集集和统一的存储命名空间内的成员群集以虚拟机流畅性支持一组。 

虽然保留现有的故障转移群集管理体验成员群集上，群集集实例另外还提供在聚合的生命周期管理周围的密钥使用情况。 此 Windows Server 预览方案评估指南提供必要的背景信息以及评估群集集技术使用 PowerShell 的分步说明。 

## 技术简介

群集集技术开发以满足特定客户请求操作系统大规模的软件定义数据中心 (SDDC) 云中。 群集集价值主张此预览版本中的可能会被汇总为以下：  

- 显著增加受支持的 SDDC 云比例通过组合多个较小群集到单个较大的结构，即使在单个群集保持软件故障边界运行高度可用的虚拟机
- 管理故障转移群集生命周期包括载入和停用群集，而不会影响租户虚拟机的可用性，通过流畅迁移的整个跨此较大的结构的虚拟机
- 轻松更改在你超聚合的计算存储比我
- 跨群集初始虚拟机的位置和后续的虚拟机迁移中受益于[类似于 Azure 的故障域和可用性设置](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)
- 混合和匹配的 CPU 硬件到同一个群集的不同代设置结构，即使同时可使单个容错域保持相同的最大效率。  请注意，在每个单独的群集以及整个群集集仍存在相同的硬件的建议。

从较高级别视图，这是什么群集集可以如下所示。

![群集设置解决方案视图](media\Cluster-sets-Overview\Cluster-sets-solution-View.png)

下面提供了每个元素在上图中的核心摘要：

**管理群集**

在群集集中管理群集是故障转移群集承载整个群集集和统一的存储命名空间 (群集设置 Namespace) 引用横向扩展文件服务器 (SOFS) 的高度可用的管理平面。 管理群集是从运行虚拟机工作负荷的成员群集逻辑分离。 这使群集设置管理平面复原为任何本地化的群集范围内失败，例如丢失成员群集能力。   

**成员群集**

群集集中的成员的群集通常是运行虚拟机和存储空间直通的工作负载的传统超融合群集。 多个成员群集参与形成较大的 SDDC 云构造的单个群集集部署。 成员群集不同于管理群集中两个主要方面： 成员群集参与故障域和可用性设置构造，以及成员群集还大小对主机虚拟机和存储空间直通的工作负载。 不，将在群集集中跨群集的群集设置虚拟机必须位于管理群集出于此原因。

**群集集命名空间引用 SOFS**

群集集命名空间引用 (群集设置 Namespace) SOFS 是，其中每个群集设置 Namespace SOFS 上的 SMB 共享是一个引用共享 – 类型 'SimpleReferral 新此预览版本中引入的横向扩展文件服务器。  此引用允许服务器消息块 (SMB) 客户端访问成员群集 SOFS 上托管 SMB 共享目标。 在群集中的 I/O 路径设置命名空间引用 SOFS 是轻量指示机制，因此，不参与。 SMB 引荐上每个客户端节点永远缓存和群集集命名空间动态自动更新这些引用根据需要。

**群集集大纲**

在群集集中，成员群集之间的通信松散耦合，并通过名为"群集设置主设备"（CS 大纲） 的新群集资源进行协调。 其他任何群集资源，如 CS 主机是高可用性和复原为单个成员群集故障和/或管理群集节点失败。 通过新的群集设置 WMI 提供程序，CS 大纲将为所有群集集可管理性交互提供管理终结点。

**群集集工作人员**

在群集集部署中，CS 大纲与成员群集称为"群集设置工作人员"（CS 工作人员） 上的新群集资源进行交互。 CS 工作人员充当协调的本地群集交互 CS 主机的要求在群集上唯一的联络人。 此类交互的示例包括虚拟机的位置和群集本地资源清查等。 没有只有一个 CS 辅助实例成员的每个群集中群集集。 

**容错域**

容错域是软件的分组，在发生故障时，管理员确定硬件项目可能会在一起失败。  管理员可以指定一个或多个群集一起作为容错域，而每个节点可以参与可用性组中的容错域。 群集将设置由设计树叶的容错域边界确定决定为管理员是与数据中心拓扑注意事项 – 例如 PDU，习惯于网络 – 成员群集共享。 

**可用性设置**

可用性设置有助于通过组织这些入可用性的集合和部署到该可用性设置的工作负荷跨容错域配置的群集的工作负载所需的冗余管理员。 假设是否你正在部署的两层应用程序，我们建议为每个层，这将确保在该可用性设置中的一个容错域发生故障后，你的应用程序将至少具有设置可用性配置至少两个虚拟机托管该相同的可用性设置的不同的容错域上每个层中的一个虚拟机。

## 为什么使用群集集

群集集而不会影响复原好处的比例。  

群集集允许群集的多个群集一起创建较大的结构，每个群集保持独立以让复原达到时。  例如，你有多个的 4 节点 HCI 群集运行虚拟机。  每个群集提供所需的自身复原。  如果存储或内存开始填满，向上扩展是下一步。  与纵向扩展，有一些选项和注意事项。

1. 将多个存储添加到当前群集。  对存储空间直通而言，这可能是棘手为完全相同的模型/固件驱动器可能不可用。  重建时间考虑还需要考虑在内。
2. 添加更多内存。  如果你是达到最大值出计算机可以处理的内存上？  如果所有可用内存插槽已满？
3. 添加到当前群集驱动器其他计算节点。  这需要我们重新选项 1 无需考虑。
4. 购买整个新群集

这是其中群集集提供缩放的优势。  如果添加我的群集到群集集，我可以充分利用存储或可能而无需任何其他购买的另一个群集上的可用内存。  从复原角度看，将其他节点添加到存储空间直通不会提供其他投票，则仲裁。  作为提到[下面](drive-symmetry-considerations.md)，存储空间直通群集可以再向下经受 2 个节点的丢失。  如果你有 4 节点 HCI 群集，3 个节点奔溃将降低整个群集。  如果你有 8 节点的群集，3 个节点奔溃将降低整个群集。  在设置中有两个 4 节点 HCI 群集的群集集，2 个节点中一个 HCI 奔溃和 1 个节点中其他 HCI 奔溃，这两个群集保留设置。  是更好地创建一个较大的 16 节点存储空间直通群集或将其分成四个 4 节点群集和使用群集集？  具有与群集集提供的四个 4 节点群集相同的比例，但更好的复原，因为多个计算节点可以奔溃 （意外或以进行维护） 和生产保持。

## 部署群集集注意事项

在考虑群集集是否需要使用时，请考虑以下问题：

- 你是否需要不止当前的 HCI 计算和存储缩放限制？
- 所有计算和存储不都是具有相同相同？
- 实时迁移群集之间的虚拟机？
- 你想要类似于 Azure 的计算机可用性集和故障域跨多个群集？
- 你是否需要花些时间查看所有群集以确定任何新的虚拟机需要放置？

如果你答案是肯定，群集集是你需要的内容。

有几个其他要考虑的事项较大的 SDDC 可能会对你的整体数据中心策略发生更改。  SQL Server 是一个很好示例。  群集之间移动 SQL Server 虚拟机确实需要许可 SQL 在其他节点上运行？  

## 横向扩展文件服务器和群集集

在 Windows Server 2019，没有调用基础结构横向扩展文件服务器 (SOFS) 的新横向扩展文件服务器角色。 

以下注意事项适用于基础结构 SOFS 角色：

1.  故障转移群集上可以有最只有一个基础结构 SOFS 群集角色。 通过指定创建基础结构 SOFS 角色"**的基础结构**"切换到**添加 ClusterScaleOutFileServerRole** cmdlet 的参数。  例如：

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  在故障转移中自动创建的每个 CSV 卷触发自动生成的名称，具体取决于 CSV 卷名 SMB 共享的创建。 管理员不能直接创建或修改下 SOFS 角色，其他比通过 CSV 卷创建/修改操作的 SMB 共享。

3.  在超聚合配置中，基础结构 SOFS 允许 SMB 客户端 （HYPER-V 主机） 进行通信的保证连续可用性 (CA) 为基础结构 SOFS SMB 服务器。 此超聚合 SMB 环回 CA 通过访问自己的所属的虚拟机标识转发客户端和服务器之间的虚拟磁盘 (VHDx) 文件的虚拟机。 此标识转发允许 ACL 运算 VHDx 文件之前的标准超融合群集配置中一样。

创建群集集后，群集集命名空间依赖于每个成员群集上的基础结构 SOFS 和此外管理群集中的基础结构 SOFS。

在时间成员群集添加到群集集中，管理员在该群集上指定的基础结构 SOFS 名称，如果已存在。 如果基础结构 SOFS 不存在，此操作创建新的成员群集上的新基础结构 SOFS 角色。 如果成员群集上已存在的基础结构 SOFS 角色，添加操作隐式将重命名它为指定的名称根据需要。 任何现有单一实例 SMB 服务器或成员的群集处于未通过群集集利用的基础结构 SOFS 角色。 

在创建群集集时，管理员可以选择要用作命名空间根目录管理群集上的现有 AD 计算机对象。 群集集创建操作管理群集上创建基础结构 SOFS 群集角色或重命名为成员群集只需如上文所述的现有基础结构 SOFS 角色。 管理群集上的基础结构 SOFS 用作群集设置命名空间引用 (群集设置 Namespace) SOFS。 这就意味着，每个群集上的 SMB 共享设置 SOFS 是新此预览版本中引入的 – 的类型 'SimpleReferral-一个引用共享的命名空间。  此引用允许 SMB 客户端访问 SMB 共享目标成员群集 SOFS 上托管。 在群集中的 I/O 路径设置命名空间引用 SOFS 是轻量指示机制，因此，不参与。 SMB 引荐上每个客户端节点永远缓存和群集集命名空间动态自动更新这些引用根据需要

## 创建群集集

### 先决条件

当创建群集设置时，都建议您以下先决条件：

1. 配置运行最新的 Windows Server 预览体验成员版本的管理客户端。
2. 在此管理服务器上安装故障转移群集工具。
3. 创建群集成员 （至少两个群集具有至少两个群集共享卷上每个群集）
4. 创建弊端成员群集管理群集 （物理或来宾）。  此方法可确保管理平面继续可用尽管可能成员群集故障的群集集。

### 步骤

1. 创建新的群集中必备条件定义从 3 个群集设置。  以下图表提供群集创建的示例。  在此示例设置的群集名称将**CSMASTER**。

   | 群集名称               | 基础结构 SOFS 名称以供以后使用 | 
   |----------------------------|-------------------------------------------|
   | 设置群集                | SOFS CLUSTERSET                           |
   | CLUSTER1                   | SOFS CLUSTER1                             |
   | CLUSTER2                   | SOFS CLUSTER2                             |

2. 群集中的所有已创建后，使用以下命令创建群集设置主机。

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. 将群集服务器添加到群集集，下面将使用。

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > 如果你使用静态 IP 地址方案，你必须包括 *-StaticAddress x.x.x.x* **新建 ClusterSet**命令。

4. 一旦你已创建群集集退出群集成员，可以列出该节点集，其属性。  若要枚举群集组中的所有成员群集：

        Get-ClusterSetMember -CimSession CSMASTER

5. 若要枚举设置包括管理群集节点在群集中的所有成员群集：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. 若要列出与成员群集的所有节点：

        Get-ClusterSetNode -CimSession CSMASTER

7. 若要列出所有跨群集的资源组设置：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. 若要验证群集创建过程创建一个 （标识为 Volume1 或任何 CSV 文件夹标有正在的基础结构文件服务器和为这两者的路径名称 ScopeName） 的 SMB 共享上设置基础结构 SOFS 为每个群集成员CSV 卷：

        Get-SmbShare -CimSession CSMASTER

8. 群集集具有可以为查看收集的调试日志。  群集集和群集调试日志可以收集的所有成员和管理群集。

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. 配置 Kerberos[约束委派](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/)所有群集集成员之间。

10. 在群集设置每个节点上配置 Kerberos 跨群集虚拟机的实时迁移身份验证类型。

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. 添加到群集组中每个节点上的本地管理员组管理群集。

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## 创建新的虚拟机并添加到群集集

创建群集设置后下, 一步是创建新的虚拟机。  正常情况下，当创建虚拟机并将它们添加到群集时，你需要执行一些检查，以查看哪些可能的最佳做法上运行群集上。  这些检查可能包括：

- 内存量是群集节点上可用？
- 磁盘空间量是群集节点上可用？
- 虚拟机确实需要特定的存储要求 （即需我 SQL Server 的虚拟机以转到运行更快的驱动器; 的群集或，我基础结构的虚拟机不为关键，可以在较慢的驱动器上运行）。

一旦此回答，你需要它能够在群集上创建虚拟机。  群集集的优势之一是群集集为你执行这些检查和将虚拟机置于最佳的节点上。

下面的命令将同时确定最佳群集并向其部署虚拟机。  在以下示例中，指定至少 4 个千兆字节的内存是可用于虚拟机和，它将需要利用 1 个虚拟处理器创建新的虚拟机。

- 确保 4 gb 可用的虚拟机
- 设置用于在 1 的虚拟处理器
- 检查以确保至少 10 %cpu 可用于虚拟机

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

完成后，将为您提供有关的虚拟机和放置的位置的信息。  在上述示例中，它将显示为：

        State         : Running
        ComputerName  : 1-S2D2

如果你没有足够的内存、 cpu 或磁盘空间，以添加虚拟机，你将收到错误：

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

一旦创建虚拟机，它将显示在 HYPER-V 管理器中指定的特定节点上。  若要将其添加为群集设置虚拟机和到群集，命令是如下。  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

完成后，输出将是：

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

如果你已添加与现有的虚拟机群集，虚拟机还需要使用群集集因此注册所有虚拟机，要使用的命令是进行注册：

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

但是，因为虚拟机的路径需要添加到群集集命名空间过程不完整。

因此，例如，添加现有群集，它具有预配置的虚拟机 reside 上本地群集共享卷 (CSV)，VHDX 的路径将类似于"C:\ClusterStorage\Volume1\MYVM\Virtual 硬 Disks\MYVM.vhdx。  存储迁移将需要为 CSV 路径是设计使然本地到单个成员群集完成。 因此，将无法访问对虚拟机后实时迁移跨成员群集。 

在此示例中，CLUSTER3 已添加到群集集添加 ClusterSetMember 使用基础结构横向扩展文件服务器作为 SOFS CLUSTER3。  若要移动的虚拟机配置和存储，该命令为：

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

完成后，你将收到一条警告：

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

可以忽略此警告，如警告是"检测到虚拟机角色存储配置中的进行任何更改"。  作为的实际物理位置的警告的原因不会更改;仅配置路径。 

有关移动 VMStorage 的详细信息，请查看此[链接](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps)。 

实时迁移虚拟机不同的群集集群集之间是与过去不相同。 在非群集集情况下，是步骤：

1. 从群集中删除虚拟机角色。
2. 实时迁移到不同的群集的成员节点的虚拟机。
3. 将虚拟机作为新的虚拟机角色添加到群集中。

群集集与这些步骤不是必需和需要只有一个命令。  例如，想要在 CLUSTER3 群集设置虚拟机从 CLUSTER1 移动到节点 2 CL3。  为单个命令：

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

请注意，这不会移动虚拟机存储或配置文件。  这不是由于虚拟机的路径作为 \\SOFS-CLUSTER1\VOLUME1 仍保持必要的。  注册在虚拟机后群集集具有基础结构文件服务器共享路径、 驱动器和虚拟机确实需要在虚拟机的同一台计算机上。

## 创建可用性设置故障域

引入中所述，类似于 Azure 的容错域和可用性集可以配置群集组中。  这非常有利于初始虚拟机展示位置和群集之间的迁移。  

在下面的示例中，有四个群集参与群集集。  在集中，将使用两个群集和使用其他两个群集创建容错域的创建逻辑容错域。  以下两个容错域将构成 Availabiilty 设置。 

在下面的示例中，CLUSTER1 以及 CLUSTER2 都将在调用**FD1**时 CLUSTER3 和 CLUSTER4 会在调用**FD2**故障域中的容错域中。  将调用可用性集**CSMASTER-AS**和组成的两个容错域。

若要创建的容错域，这些命令是：

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

若要确保已成功创建它们，可以通过其输出显示运行获取 ClusterSetFaultDomain。

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

既然已经创建容错域，可用性设置需要创建。

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

若要验证它已创建，然后使用：

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

当创建新的虚拟机，则需要确定最佳节点的一部分使用-AvailabilitySet 参数。  因此然后，它将如下所示：

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

由于各种生命周期而从群集中删除群集设置。 是的时候群集需要从一群集中删除。 作为最佳做法，所有群集设置虚拟机应都移出群集。 这可以实现使用**移动 ClusterSetVM**和**移动 VMStorage**命令。

但是，如果也不会移动虚拟机，群集集将运行一系列操作向管理员提供直观的结果。  从组中删除群集时，所有剩余群集集托管的虚拟机正在删除的群集上只需将绑定到该群集，假定它们有权访问他们的存储的高度可用虚拟机。  群集集还会自动将更新由其清单：

- 不再跟踪现在删除群集并在其上运行的虚拟机的运行状况
- 从群集集命名空间和共享托管现在删除群集上的所有引用中移除

例如，若要从群集集删除 CLUSTER1 群集的命令将：

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## 常见问题解答 (FAQ)

**问题：** 在我的群集集中，我能否限制为仅使用超融合群集？ <br>
**答案：** 不。  你可以混合存储空间直通使用传统的群集。

**问题：** 可以管理我群集集通过系统中心虚拟机管理器？ <br>
**答案：** 系统中心虚拟机管理器当前不支持群集集 <br><br> **问题：** 可以 Windows Server 2012 R2 或 2016年群集共存在同一群集集中？ <br>
**问题：** 可以将关闭 Windows Server 2012 R2 的工作负荷迁移，或只需让这些群集 2016年群集加入同一个群集设置？ <br>
**答案：** 群集集是一种新技术引入在 Windows Server 预览版本中，因此，使得中不存在以前的版本。 低级别操作系统基于群集不能加入群集集。 但是，群集操作系统滚动升级技术应提供你正在查找通过将这些群集升级到 Windows Server 2019 的迁移功能。

**问题：** 群集集可以允许我缩放存储或计算 （单独）？ <br>
**答案：** 是，只需添加存储空间直通或传统的 HYPER-V 群集。 使用群集集，则即使在超聚合群集集的计算存储比简单更改。

**问题：** 什么是群集集的管理工具 <br>
**答案：** PowerShell 或 WMI 在此版本中。

**问题：** 如何使用不同的处理器跨群集的实时迁移？  <br>
**答案：** 群集集不会解决处理器差异和取代 HYPER-V 的当前支持。  因此，必须与快速迁移使用处理器兼容性模式。  群集集的推荐方法是使用内每个单独的群集以及整个群集设置相同的处理器硬件之间发生的群集的实时迁移。

**问题：** 可以我的群集集虚拟计算机自动故障转移群集失败？  <br>
**答案：** 在此版本中，虚拟机群集集只能手动的实时迁移跨群集;但不能自动故障转移。 

**问题：** 我们如何确保存储是复原为群集失败？ <br>
**答案：** 使用跨成员群集跨群集存储副本 (SR) 解决方案意识到群集故障存储复原。

**问题：** 使用存储副本 (SR) 能够跨成员群集复制。 群集集命名空间存储 UNC 路径更改上 SR 故障转移到副本目标存储空间直通群集？ <br>
**答案：** 在此版本中，此类群集集命名空间引用更改不会与 SR 故障转移。 请让 Microsoft 了解这种情况下是否对你和你打算使用它的方式至关重要。

**问题：** 在灾难恢复的容错域中都可能故障转移的虚拟机 （假设关闭了整个故障域）？ <br>
**答案：** 否，请注意该跨群集故障转移内逻辑容错域尚不支持。 

**问题：** 我的群集可以在多个站点 （或 DNS 域） 中设置 span 群集？ <br> 
**答案：** 这是一个未经测试的方案，并且不立即计划生产支持。 请让 Microsoft 了解这种情况下是否对你和你打算使用它的方式至关重要。

**问题：** 群集是否设置使用 IPv6？ <br>
**答案：** IPv4 和 IPv6 支持使用群集集与使用故障转移群集。

**问题：** 群集的 Active Directory 林要求有哪些设置 <br>
**答案：** 在相同的 AD 林中必须是所有成员群集。

**问题：** 多少群集节点可以单个群集的一部分设置或？ <br>
**答案：** 在预览中，群集集经过测试，支持最多 64 总群集节点。 但是，群集将体系结构缩放设置为更大的限制，并不是一个是硬编码的限制。 请让 Microsoft 了解更大规模是否对你和你打算使用它的方式至关重要。

**问题：** 将群集组中的所有存储空间直通群集都形成单个的存储池？ <br>
**答案：** 不。 存储空间直通技术仍然运行中的单个群集并不是跨群集组中的成员群集。

**问题：** 群集设置命名空间高度可用？ <br>
**答案：** 是的通过管理群集上运行的持续可用 (CA) 引用 SOFS 命名空间服务器提供群集集命名空间。 Microsoft 建议有足够多的虚拟机从成员群集以使其复原本地化群集范围内失败。 但是，以考虑未预料到灾难性故障 – 例如同时向下转管理群集中所有虚拟机 – 引荐信息此外永久缓存在每个群集集节点，甚至是在重新启动。
 
**问题：** 群集不会设置群集组中的存储性能降低的基于命名空间的存储访问？ <br>
**答案：** 不。 群集集命名空间提供了一群集组 – 在概念上类似分布式文件系统命名空间 (DFSN) 内的覆盖层引用命名空间。 并且与 DFSN 不同引用群集集命名空间的元数据已自动填充，并在无需任何管理员干预，所有节点上自动更新以便中存储的访问权限路径几乎不会影响性能。 

**问题：** 如何备份群集集元数据？ <br>
**答案：** 本指南是相同的故障转移群集。 系统状态备份将备份的群集状态。  通过 Windows Server 备份，你可以执行只需节点的群集数据库 （需要用到它应永远不会由于我们已自我修复逻辑的一组） 的还原或执行授权还原整个群集数据库回滚在所有节点。 对于群集集，Microsoft 建议首先上成员群集，然后管理群集中此类授权还原，如果需要。 

---
title: 群集集
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: 本指南介绍了群集集方案
ms.localizationpriority: medium
ms.openlocfilehash: 349b69835ae68c626e886cad30f4d5a89d358372
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719704"
---
# <a name="cluster-sets"></a>群集集

> 适用于：Windows Server 2019

群集设置的新云向外缩放技术在 Windows Server 2019 版本可极大地提高单个软件定义数据中心 (SDDC) 云中的群集节点计数。 群集组是松散耦合的多个故障转移群集分组： 计算、 存储或超聚合。 群集设置技术启用虚拟机流动性跨群集集合和统一的存储命名空间中的成员群集支持虚拟机流动性组。

保留现有的故障转移群集管理体验成员群集上，而群集组实例此外还提供关于在聚合的生命周期管理的关键用例。 此 Windows Server 2019 方案评估指南提供了必要的背景信息以及评估群集集技术使用 PowerShell 的分步说明。

## <a name="technology-introduction"></a>技术简介

开发群集集技术以满足特定客户请求操作在规模较大的软件定义数据中心 (SDDC) 云。 群集的设置可能会被价值主张汇总如下所示：  

- 显著提高通过组合多个较小的群集，为单个较大的结构，甚至在单个群集中保持软件故障边界运行高度可用的虚拟机支持的 SDDC 云规模
- 管理的整个群集故障转移群集生命周期包括载入和停用，而不会影响租户虚拟机的可用性，通过流畅地迁移虚拟机跨此较大的结构
- 轻松地更改在超聚合中的计算存储比我
- 受益[类似于 Azure 的容错域和可用性集](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)跨群集中初始虚拟机放置和后续虚拟机迁移
- 混合和匹配的 CPU 硬件到同一个群集的不同代设置 fabric，甚至同时保持各个容错域同构的最大效率。  请注意，每个单独的群集，以及在整个群集集合中仍存在的相同的硬件建议。

从较高的层次来看，这是集看起来像是哪些群集。

![群集设置解决方案视图](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

下面提供了在上图中元素的快速摘要：

**管理群集**

群集组中的管理群集是托管的高度可用的管理平面的在整个群集集合和统一的存储命名空间 (群集设置 Namespace) 引用横向扩展文件服务器 (SOFS) 的故障转移群集。 从运行的虚拟机工作负荷的成员分类逻辑上分离的管理群集。 这样，群集将管理平面可复原设置为任何本地化的群集范围内发生故障，例如成员群集的电源中断。   

**成员群集**

在群集组中的成员群集通常是运行虚拟机和存储空间直通的工作负荷的传统超聚合群集。 在单个群集组部署中，从而形成更大的 SDDC 云结构加入多个成员群集。 成员群集不同于在两个关键方面中的管理群集： 成员群集参与容错域和可用性集构造，并且成员群集还调整到主机虚拟机和存储空间直通的工作负荷。 将群集组中跨群集的群集设置虚拟机不必须管理群集上为此托管。

**群集设置命名空间引用 SOFS**

群集组命名空间引用 (群集设置 Namespace) SOFS 是横向扩展文件服务器将每个群集设置 Namespace SOFS 上的 SMB 共享作为引用共享 – 类型 SimpleReferral Windows Server 2019 中新引入。 此引用允许服务器消息块 (SMB) 客户端访问成员群集 SOFS 上托管的 SMB 共享的目标。 在群集中的 I/O 路径设置命名空间引用 SOFS 是一个轻型引用机制，在这种情况下，不参与。 SMB 缓存引用的永久上每个客户端节点和群集设置命名空间动态自动更新这些引用根据需要。

**群集设置主文件夹**

在群集组中，成员群集之间的通信松散耦合，并由名为"群集设置主控形状"（CS 主控形状） 的新群集资源来协调。 像任何其他群集资源，CS 主机是高可用性和从单独的成员发生群集故障和/或管理群集节点故障中复原。 通过新的群集设置 WMI 提供程序，CS Master 提供的管理终结点进行的所有群集设置可管理性交互。

**群集设置辅助角色**

在设置群集部署中，CS Master 与成员名为"群集设置辅助角色"（CS 辅助角色） 的群集上的新群集资源进行交互。 CS-辅助角色是充当群集来安排本地群集交互，根据请求 CS Master 上唯一的联系人。 此类交互的示例包括虚拟机放置和群集本地资源清单。 都只有一个群集集合中群集成员的每个 CS-辅助角色实例。 

**容错域**

容错域是软件的分组，并在发生故障时，管理员决定的硬件项目可能会一起失败。  虽然管理员可以指定一个或多个群集一起作为一个容错域，每个节点可以参与可用性组中的容错域。 群集设置通过设计使做出的决定的容错域边界确定管理员是熟知数据中心拓扑注意事项 – 例如 PDU 到网络 – 成员群集共享。 

**可用性集**

可用性集可帮助管理员通过组织这些数据的可用性集和部署工作负载分配到该可用性组跨容错域配置的群集工作负载所需的冗余性。 让我们假设要部署的两个层应用程序，建议在可用性集中，这将确保，在该可用性集中的一个容错域发生故障后，你的应用程序将至少具有每个层配置至少两个虚拟机托管在不同的容错域中的同一个可用性集中的每个层中的一个虚拟机。

## <a name="why-use-cluster-sets"></a>为什么要使用群集设置

群集设置而无需牺牲复原能力提供了缩放的好处。  

群集设置允许聚类分析多个群集一起创建较大的结构，同时将每个群集保留独立的复原能力。  例如，有几个 4 节点 HCI 群集正在运行的虚拟机。  每个群集提供所需的自身的复原能力。  如果存储或内存开始填满，纵向扩展是下一步。  使用向上扩展，有一些选项和注意事项。

1. 为当前的群集添加更多存储。  使用存储空间直通，这可能是棘手的确切的相同模型/固件驱动器可能不可用。  重新生成时间的考虑也需要考虑在内。
2. 添加内存。  如果您被极限机可以处理的内存上？  如果所有可用内存插槽是否完整？
3. 将添加到当前群集驱动器具有额外的计算节点。  这把我们带回选项 1 无需考虑。
4. 购买一个新群集

这是其中群集集提供了缩放的好处。  如果我将添加到群集设置我的群集，我可以利用存储空间或内存可能会在另一个群集，而无需任何附加购买条件。  从复原能力的角度看，将其他节点添加到存储空间直通不会提供其他用于仲裁投票。  如所述[此处](drive-symmetry-considerations.md)，向下继续之前，存储空间直通群集可以经受的 2 个节点丢失。  如果您有一个 4 节点 HCI 群集，3 个节点停机将记下在整个群集。  如果必须将 8 节点群集，3 个节点停机将记下在整个群集。  在集中具有两个 4 节点 HCI 群集的群集设置，在一个 HCI 的 2 个节点出现故障和其他 HCI 中的 1 个节点出现故障，这两个群集保持正常。  是更好地创建一个大型 16 节点存储空间直通群集或将其分解为四个 4 节点群集并使用群集集？  具有四个 4 节点群集，群集设置为使用同一刻度，但更好的复原能力，多个计算节点 （意外或进行维护） 会下降并且生产保持。

## <a name="considerations-for-deploying-cluster-sets"></a>部署群集集的注意事项

在考虑是否群集设置的某些内容需要使用时，应考虑以下问题：

- 是否需要应对当前 HCI 计算和存储规模限制？
- 所有计算和存储不都是完全相同？
- 实时迁移的群集之间的虚拟机？
- 你希望类似于 Azure 的计算机可用性集和容错域在多个群集？
- 是否需要花点时间看看所有的群集，确定需要放置任何新的虚拟机？

如果您的答案是肯定的群集设置的所需内容。

有几个其他需要考虑的事项，可查看大 SDDC 可能要更改您的整个数据中心策略。  SQL Server 是一个很好示例。  群集之间移动 SQL Server 虚拟机需要许可的其他节点上运行的 SQL？  

## <a name="scale-out-file-server-and-cluster-sets"></a>横向扩展文件服务器和群集设置

在 Windows Server 2019，没有名为基础结构横向扩展文件服务器 (SOFS) 是新横向扩展文件服务器角色。 

下列注意事项适用于基础结构 SOFS 角色：

1.  在故障转移群集可以有最多只能有一个基础结构 SOFS 群集角色。 通过指定创建基础结构 SOFS 角色"**的基础结构**"切换到的参数**添加 ClusterScaleOutFileServerRole** cmdlet。  例如：

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  在故障转移中自动创建每个 CSV 卷触发 SMB 共享的创建具有基于 CSV 卷名称自动生成名称。 管理员不能直接创建或修改 SOFS 角色，而通过 CSV 卷创建/修改操作的 SMB 共享。

3.  在超聚合配置中，基础结构 SOFS 允许 SMB 客户端 （HYPER-V 主机） 进行通信和有保证连续可用性 (CA) 基础结构 SOFS SMB 服务器。 此超聚合 SMB 环回 CA 可以通过虚拟机访问的客户端和服务器之间转发所属的虚拟机标识其虚拟硬盘 (VHDx) 文件。 此身份转发允许 ACL ing VHDx 文件与之前的标准超聚合群集配置中一样。

群集组创建后，群集组命名空间依赖于基础结构 SOFS 上每个成员群集，还管理群集中基础结构 SOFS。

在时间成员群集添加到一群集管理员在该群集上指定的基础结构 SOFS 名称，如果已存在。 如果基础结构 SOFS 不存在，此操作被创建新成员群集上的新基础结构 SOFS 角色。 如果成员群集上已存在基础结构 SOFS 角色，则添加操作隐式地将其重命名为指定的名称根据需要。 任何现有的单一实例 SMB 服务器或保留群集未利用由群集组的成员上的基础结构 SOFS 角色。 

在创建群集集合时，管理员可以选择要用作管理群集上命名空间根路径的现有 AD 计算机对象。 群集组创建操作在管理群集上创建基础结构 SOFS 群集角色，或重命名现有的基础结构 SOFS 角色的成员群集只需按前面所述。 群集设置命名空间引用 (群集设置 Namespace) SOFS 用作基础结构 SOFS 管理群集上。 它只是意味着每个群集上的 SMB 共享设置 SOFS 是引用共享 – 类型 SimpleReferral-在 Windows Server 2019 中新引入的命名空间。  此引用允许成员群集 SOFS 上托管的 SMB 客户端访问 SMB 共享上的目标。 在群集中的 I/O 路径设置命名空间引用 SOFS 是一个轻型引用机制，在这种情况下，不参与。 SMB 缓存引用的永久上每个客户端节点和群集设置命名空间动态自动更新这些引用根据需要

## <a name="creating-a-cluster-set"></a>创建群集集合

### <a name="prerequisites"></a>先决条件

当创建群集设置时，都建议您以下先决条件：

1. 配置管理客户端运行 Windows Server 2019。
2. 此管理服务器上安装故障转移群集工具。
3. 创建群集成员 （至少两个群集与至少两个群集共享卷上的每个群集）
4. 创建跨越成员群集的管理群集 （物理或来宾）。  这种方法可确保管理平面仍然可用，尽管可能的成员发生群集故障的群集组。

### <a name="steps"></a>步骤

1. 创建新的群集设置从三个群集，如先决条件中定义。  图表下方提供了要创建的分类示例。  在本示例中的群集的名称将是**CSMASTER**。

   | 群集名称               | 要用更高版本的基础结构 SOFS 名称 | 
   |----------------------------|-------------------------------------------|
   | 设置群集                | SOFS CLUSTERSET                           |
   | CLUSTER1                   | SOFS CLUSTER1                             |
   | CLUSTER2                   | SOFS CLUSTER2                             |

2. 群集中的所有已创建后，使用以下命令来创建集母版的群集。

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. 若要将群集服务器添加到群集组中，下面将使用。

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > 如果使用静态 IP 地址方案，则必须包括 *-StaticAddress x.x.x.x*上**新建 ClusterSet**命令。

4. 创建带群集成员集的群集后，可以列出的节点集并将其属性。  枚举群集组中的所有成员群集：

        Get-ClusterSetMember -CimSession CSMASTER

5. 若要枚举集包括管理群集节点的群集中的所有成员分类：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. 若要列出成员群集中的所有节点：

        Get-ClusterSetNode -CimSession CSMASTER

7. 若要列出所有在群集范围内的资源组设置：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. 若要验证群集创建过程创建一个 （标识为 Volume1 或使用的基础结构文件服务器和为这两者的路径名称 ScopeName 标记任何 CSV 文件夹） 的 SMB 共享上设置基础结构 SOFS 为每个群集成员CSV 卷：

        Get-SmbShare -CimSession CSMASTER

8. 群集组具有可供查看收集的调试日志。  群集设置，既可以为所有成员和管理群集收集群集调试日志。

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. 配置 Kerberos[约束委派](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/)之间群集中的所有设置成员。

10. 群集设置的每个节点上配置 Kerberos 的跨群集虚拟机实时迁移身份验证类型。

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. 将在管理群集添加到群集组中每个节点上的本地管理员组。

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>创建新虚拟机并将添加到群集设置

创建群集设置后下, 一步是创建新的虚拟机。  通常情况下，在需要创建虚拟机并将其添加到群集时，必须执行一些检查，以查看这可能是最佳上运行的群集上。  这些检查包括：

- 是群集节点上可用的内存量？
- 磁盘空间量是群集节点上可用？
- 虚拟机需要特定的存储要求 （即我希望我的 SQL Server 虚拟机以转到运行更快的驱动器; 的群集或我的基础结构虚拟机不是如此重要，并且可以较慢的驱动器上运行）。

一旦回答此问题，您需要它来为在群集上创建虚拟机。  群集设置的优势之一是群集设置为你执行这些检查的操作和最大程度优化节点上放置虚拟机。

下面的命令将同时标识最佳群集并向其部署虚拟机。  在以下示例中，指定至少 4 千兆字节的内存的虚拟机可用，并且它将需要使用 1 个虚拟处理器创建新的虚拟机。

- 确保 4 gb 就可用于虚拟机
- 设置使用 1 上的虚拟处理器
- 检查，确保在至少 10 %cpu 可用于虚拟机

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

操作完成时，将为您提供有关虚拟机和放置位置的信息。  在上述示例中，它将显示为：

        State         : Running
        ComputerName  : 1-S2D2

如果你没有足够的内存、 cpu 或磁盘空间，添加虚拟机，你将收到错误：

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

一旦创建虚拟机后，它将显示在指定的特定节点上的 HYPER-V 管理器中。  若要将其添加为群集设置虚拟机并登录到群集，下面是该命令。  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

操作完成时，输出将是：

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

如果已添加了现有的虚拟机的群集，虚拟机还需要注册到群集集来注册所有虚拟机，要使用的命令是：

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

但是，随着需求的虚拟机的路径添加到群集组命名空间进程不完整。

因此，例如，添加现有的群集和它已预配置的虚拟机所在上本地群集共享卷 (CSV)，VHDX 的路径将类似于"C:\ClusterStorage\Volume1\MYVM\Virtual 硬 Disks\MYVM.vhdx。  存储迁移需要如 CSV 路径在设计上本地的单个成员群集来完成。 因此，将无法访问虚拟机后实时成员群集之间迁移。 

在此示例中，CLUSTER3 已添加到集添加 ClusterSetMember 与基础结构横向扩展文件服务器用作 SOFS CLUSTER3 的群集。  若要移动的虚拟机配置和存储，该命令是：

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

完成后，您将收到一条警告：

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

可以忽略此警告，因为该警告是"检测到的虚拟机角色存储配置中无更改"。  作为实际的物理位置警告的原因不会更改;仅配置路径。 

有关 Move-vmstorage 的详细信息，请查看此[链接](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps)。 

实时迁移虚拟机之间不同的群集设置群集是不与过去相同。 在非群集集合的情况下，应为步骤：

1. 从群集中删除虚拟机角色。
2. 实时迁移到不同的群集的成员节点的虚拟机。
3. 将虚拟机添加到群集中，为新虚拟机角色。

使用群集集不需要这些步骤，需只能有一个命令。  首先，应设置为可用于迁移的过程中该命令的所有网络：

    Set-VMHost -UseAnyNetworkMigration $true

例如，我想要将群集设置虚拟机从 CLUSTER1 移到 NODE2 CL3，CLUSTER3 上。  将单个命令：

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

请注意这不会移动虚拟机存储或配置文件。  这是不必要的因为虚拟机的路径将一直为\\SOFS CLUSTER1\VOLUME1。  注册虚拟机后使用群集集具有的基础结构文件服务器共享路径、 驱动器和虚拟机不需要在虚拟机所在的同一计算机上。

## <a name="creating-availability-sets-fault-domains"></a>创建可用性集容错域

如简介中所述，可以在群集组中配置类似于 Azure 的容错域和可用性集。  这非常有利于初始虚拟机放置和群集之间迁移。  

在以下示例中，有四个群集集合中包含的群集。  在集内逻辑容错域将创建具有两个群集和容错域与其他两个群集创建。  这些两个容错域将包含可用性组。 

在以下示例中，CLUSTER1 CLUSTER2 将和容错域中名为**FD1** CLUSTER3 和 CLUSTER4 将在一个名为的容错域中时**FD2**。  将名为可用性集**CSMASTER-AS** ，并且由组成的两个容错域。

若要创建的容错域的命令是：

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

若要确保它们已成功创建，可以使用其输出所示运行 Get ClusterSetFaultDomain。

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

现在，已创建的容错域，可用性集需要创建。

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

若要验证它已创建，然后使用：

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

在创建新的虚拟机，然后需要在确定最佳节点使用的可用性集参数。  因此它将然后如下所示：

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

由于各种生命周期从群集中删除群集设置。 有些的时候群集需要从群集组中删除。 最佳做法是，所有群集设置的虚拟机应都移出群集。 可以使用完成这**移动 ClusterSetVM**并**Move-vmstorage**命令。

但是，如果不会同时移动虚拟机，群集集运行一的系列操作以向管理员提供直观的结果。  从集中删除群集后，所有剩余群集组的虚拟机正在删除群集上托管只需将成为绑定到该群集，假定他们具有访问其存储的高可用虚拟机。  群集设置还会自动将更新按其库存量：

- 无法再跟踪立即删除群集和在其上运行的虚拟机的运行状况
- 从群集组命名空间和对共享立即删除群集上托管的所有引用中删除

例如，若要从群集组中删除 CLUSTER1 群集命令应为：

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>常见问题 (FAQ)

**问：** 在我的群集组，我只能安装到仅使用超聚合群集？ <br>
**答案：** 否。  与传统的群集，您可以混合存储空间直通。

**问：** 管理我群集设置通过 System Center Virtual Machine Manager？ <br>
**答案：** System Center Virtual Machine Manager 目前不支持群集设置 <br><br> **问：** 可以 Windows Server 2012 R2 或 2016年群集共存于同一群集集合？ <br>
**问：** 可以迁移工作负荷关闭 Windows Server 2012 R2 或 2016年通过只需让这些群集的群集加入同一群集设置？ <br>
**答案：** 群集集是一项新技术引入了在 Windows Server 2019，因此这种情况下，以前的版本中不存在。 低级别基于操作系统的群集无法加入群集集合。 但是，群集操作系统滚动升级技术应提供你要查找这些群集升级到 Windows Server 2019 的迁移功能。

**问：** 群集设置可以允许我缩放存储或计算 （独立）？ <br>
**答案：** 是的只需添加的存储空间直通或传统的 HYPER-V 群集。 使用群集集时，它是计算存储比率，即使在超聚合群集组中的简单更改。

**问：** 群集设置的管理工具是什么 <br>
**答案：** PowerShell 或 WMI 在此版本中。

**问：** 如何使用不同代的处理器跨群集实时迁移？  <br>
**答案：** 群集集合不会不解决处理器差异和取代的 HYPER-V 目前支持。  因此，必须与快速迁移使用处理器兼容性模式。  有关群集设置的建议是要用于群集发生之间的实时迁移的每个单独的群集，以及整个群集设置中的相同处理器硬件。

**问：** 可以我群集的一组虚拟机自动故障转移群集失败？  <br>
**答案：** 在此版本中，群集设置的虚拟机只能是手动的实时迁移跨群集;但不能自动故障转移。 

**问：** 我们如何确保存储是从群集故障中复原？ <br>
**答案：** 使用跨成员群集跨群集存储副本 (SR) 解决方案实现群集故障存储复原能力。

**问：** 我使用存储副本 (SR) 在成员群集之间复制。 群集组命名空间存储 UNC 路径更改 SR 故障转移到副本的目标存储空间直通群集？ <br>
**答案：** 在此版本中，这样的群集组命名空间引用更改不会发生与 SR 故障转移。 请让 Microsoft 知道这种情况下是否重要的您和您计划如何使用它。

**问：** 在灾难恢复情形中的容错域中是否可以将故障转移虚拟机 （例如整个容错域发生故障）？ <br>
**答案：** 不可以，请注意，跨群集故障转移在逻辑容错域尚不支持。 

**问：** 我的群集可以设置多个站点 （或 DNS 域） 中的 s p a n 群集？ <br> 
**答案：** 这是未经测试的方案，不会立即计划生产支持。 请让 Microsoft 知道这种情况下是否重要的您和您计划如何使用它。

**问：** 群集是否设置了 IPv6 的工作？ <br>
**答案：** IPv4 和 IPv6 支持使用群集集与故障转移群集。

**问：** 什么是群集的 Active Directory 林要求集 <br>
**答案：** 所有成员群集都必须位于相同的 AD 林。

**问：** 群集或节点的数量可以作为单个群集的一部分设置？ <br>
**答案：** 在 Windows Server 2019 群集集合已经过测试并支持最多 64 个总群集节点。 但是，群集设置体系结构扩展到更大的限制，而不是硬编码的限制。 请告知 Microsoft 较大的规模是重要的您和您计划如何使用它。

**问：** 将群集组中的所有存储空间直通群集都形成单个存储池？ <br>
**答案：** 否。 存储空间直通技术仍执行单个群集中，不能跨群集组中的成员群集执行。

**问：** 将群集设置高度可用的命名空间？ <br>
**答案：** 是的通过在管理群集上运行的连续可用 (CA) 引用 SOFS 命名空间服务器提供的群集组命名空间。 Microsoft 建议使用具有足够数量的虚拟机从成员群集，使其对本地化群集范围内发生故障的复原。 但是，来应对无法预见的灾难性故障-例如同时停止运行的管理群集中所有虚拟机 – 引用信息是永久此外缓存在每个群集组节点中，甚至在重新启动后。
 
**问：** 群集是否在群集组中设置基于命名空间的存储访问降低存储性能？ <br>
**答案：** 否。 群集组命名空间提供了群集集 – 从概念上讲如分布式文件系统命名空间 (DFSN) 内的覆盖引用命名空间。 而且和不同 DFSN，所有群集组命名空间引用元数据都是自动填充，而无需任何管理员干预，所有节点上自动更新因此中存储的访问路径几乎不会影响性能。 

**问：** 如何备份群集集元数据？ <br>
**答案：** 本指南是故障转移群集相同。 将备份系统状态备份的群集状态。  可以通过 Windows Server Backup 执行还原的不仅仅是节点的群集数据库 （其中永远不应由于大量自我修复逻辑，我们需要） 或执行授权还原整个群集数据库回滚在所有节点。 在群集设置的情况下 Microsoft 建议首先成员群集，然后在管理群集中此类的权威还原，如果需要。

---
title: 存储服务质量
ms.prod: windows-server
manager: dongill
ms.author: JGerend
ms.technology: storage-qos
ms.topic: get-started-article
ms.assetid: 8dcb8cf9-0e08-4fdd-9d7e-ec577ce8d8a0
author: kumudd
ms.date: 10/10/2016
ms.openlocfilehash: 0e848260dd4ba3b37d1351fba7c24dd3cd283e69
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393942"
---
# <a name="storage-quality-of-service"></a>存储服务质量

> 适用于：Windows Server（半年频道）、Windows Server 2016

通过 Windows Server 2016 中的存储服务质量 (QoS)，可以使用 Hyper-V 和横向扩展文件服务器角色集中监视和管理虚拟机的存储性能。 此功能使用相同的文件服务器群集自动改进多个虚拟机间的存储资源公平度，并允许在标准的 IOPs 单元中配置基于策略的最小和最大性能目标。  

可使用 Windows Server 2016 中的存储 QoS 完成以下操作：  

-   **缓解干扰邻居问题。** 默认情况下，存储 QoS 确保单个虚拟机不会使用所有存储资源，因此不会影响其他虚拟机的存储带宽。  

-   **监视端到端存储性能。** 一旦启动存储在横向扩展文件服务器上的虚拟机后，其性能将被监视。 可从单个位置查看所有运行的虚拟机的性能详细信息和横向扩展文件服务器群集的配置  

-   **根据工作负荷业务需求管理存储 I/O** 存储 QoS 策略定义了虚拟机的性能最小值和最大值，并确保可以达到。 这提供了虚拟机的一致性能，即使处于密集和过度预配的环境中。 如果不符合策略，则当虚拟机不在策略范围内或分配了无效的策略时提供可跟踪的警报。  

本文档概述了你的业务如何从新的存储 QoS 功能中获益。 它假定你已具备 Windows Server、Windows Server 故障转移群集、横向扩展文件服务器、Hyper-V 和 Windows PowerShell 的工作知识。

## <a name="BKMK_Overview"></a>叙述  
本部分介绍使用存储 QoS 的要求、使用存储 QoS 的软件定义的解决方案的概述，以及与存储 QoS 相关的术语列表。  

### <a name="BKMK_Requirements"></a>存储 QoS 要求  
存储 QoS 支持两种部署方案：  

-   **使用横向扩展文件服务器的 Hyper-V** 此方案需要以下两项：  

    -   作为横向扩展文件服务器群集的存储群集  

    -   至少具有一台启用了 Hyper-V 角色的服务器的计算群集  

    对于存储 QoS，在存储服务器上需要故障转移群集，但是计算群集无需处于故障转移群集中。 所有服务器（用于存储和计算的服务器）都必须运行 Windows Server 2016。  

    如果没有为评估目的部署横向扩展文件服务器群集，有关使用现有服务器或虚拟机生成一个群集的分步说明，请参阅 @no__t 0Windows Server 2012 R2 Storage：存储空间、SMB 横向扩展和共享 VHDX （物理） ](http://blogs.technet.com/b/josebda/archive/2013/07/31/windows-server-2012-r2-storage-step-by-step-with-storage-spaces-smb-scale-out-and-shared-vhdx-physical.aspx) 的分步指导。  

-   **使用群集共享卷的 hyper-v。** 此方案需要以下两项：  

    -   启用了 Hyper-V 角色的计算群集  

    -   使用群集共享卷 (CSV) 用于存储的 Hyper-V  

需要故障转移群集。 所有服务器必须运行同一版本的 Windows Server 2016。  

### <a name="BKMK_SolutionOverview"></a>在软件定义的存储解决方案中使用存储 QoS  
存储服务质量被构建在由横向扩展文件服务器和 Hyper-V 提供的 Microsoft 软件定义的存储解决方案中。 横向扩展文件服务器使用 SMB3协议向 Hyper-V 服务器公开文件共享。 新的策略管理器已被添加到文件服务器群集，它提供中央存储性能监视。  

![横向扩展文件服务器和存储 QoS](media/overview-Clustering_SOFSStorageQoS.png)  

**图 1：在软件定义的存储解决方案中使用存储 QoS 横向扩展文件服务器 @ no__t-0  

当 Hyper-V 服务器启动虚拟机时，它们由策略管理器监视。 策略管理器会传达存储 QoS 策略和 Hyper-V 服务器的任何限制或保留，以对虚拟机的性能进行适当的控制。  

当存在由虚拟机对存储 QoS 策略或性能需求进行的更改时，策略管理器将通知 Hyper-V 服务器调整其行为。 此反馈循环确保所有虚拟机 VHD 根据定义的存储 QoS 策略一致地执行。  

### <a name="BKMK_Glossary"></a>词汇表  

|术语|描述|  
|--------|---------------|  
|规范化 IOPs|所有的存储使用情况以“规范化 IOPs”测量。  这是每秒的存储输入/输出操作数。  任何 8 KB 或更小的 IO 被视为一个规范化 IO。  任何大于 8 KB 的 IO 被视为多个规范化 IO。 例如，一个 256 KB 的请求被视为 32 个规范化 IOPs。<br /><br />Windows Server 2016 包括指定用于规范化 IO 的大小的功能。  在存储群集上，可以指定规范化大小，并使其在规范化计算群集范围内生效。  默认值仍为 8 KB。|  
|流|由 Hyper-V 服务器打开到 VHD 或 VHDX 文件的每个文件句柄均可视为一个“流”。 如果一个虚拟机连接了两个虚拟硬盘，则该虚拟机的每个文件的文件服务器群集均有一个流。 如果 VHDX 与多个虚拟机共享，则它的每个虚拟机均有一个流。|  
|InitiatorName|针对每个流的横向扩展文件服务器报告的虚拟机的名称。|  
|InitiatorID|与虚拟机 ID 匹配的标识符。  这可始终用于对单个流虚拟机进行唯一标识，即使该虚拟机具有相同的 InitiatorName。|  
|策略|存储 QoS 策略存储在群集数据库中，具有以下属性：PolicyId、MinimumIOPS、MaximumIOPS、ParentPolicy 和 PolicyType。|  
|PolicyId|策略的唯一标识符。  默认生成，但可根据需要指定。|  
|MinimumIOPS|将由策略提供的最小规范化 IOPS。  也称为“保留”。|  
|MaximumIOPS|将由策略限制的最大规范化 IOPS。  也称为“限制”。|  
|聚合 |一种策略类型，其中指定的MinimumIOPS 和 MaximumIOPS 以及带宽在由策略分配的所有流之间共享。 所有 VHD 分配的该存储系统上的策略均具有单个 I/O 带宽的分配，以供其全部共享。|  
|Dedicated|一种策略类型，其中对指定的最小和最大 IOPS 以及带宽进行管理，以供单个 VHD/VHDx 使用。|  

## <a name="BKMK_SetUpQoS"></a>如何设置存储 QoS 和监视基本性能  
本部分介绍如何启用新的存储 QoS 功能以及如何在未应用自定义策略的情况下监视存储性能。  

### <a name="BKMK_SetupStorageQoSonStorageCluster"></a>在存储群集上设置存储 QoS  
本部分介绍如何在新的或现有的故障转移群集以及运行 Windows Server 2016 的横向扩展文件服务器上启用存储 QoS。  

#### <a name="set-up-storage-qos-on-a-new-installation"></a>在新的安装上设置存储 QoS  
如果你配置了新的故障转移群集并在 Windows Server 2016 上配置了群集共享卷 (CSV)，则存储 QoS 功能将会自动设置。  

#### <a name="verify-storage-qos-installation"></a>验证存储 QoS 安装  
创建故障转移群集并配置 CSV 磁盘后，**存储 QoS 资源**作为群集核心资源显示，并在故障转移群集管理器和 Windows PowerShell 中均可见。 其目的是，故障转移群集系统将管理此资源，而你无需对此资源执行任何操作。  我们将它显示在故障转移群集管理器和 PowerShell 中，使其与其他故障转移群集系统资源（如新的运行状况服务）一致。  

![存储 QoS 资源将在群集核心资源中显示](media/overview-Clustering_StorageQoSFCM.png)  

**图 2:存储 QoS 资源显示为故障转移群集管理器 @ no__t 中的群集核心资源-0  

使用以下 PowerShell cmdlet 查看存储 QoS 资源的状态。  

```PowerShell  
PS C:\> Get-ClusterResource -Name "Storage Qos Resource"  

Name                   State      OwnerGroup        ResourceType                 
----                   -----      ----------        ------------                 
Storage Qos Resource   Online     Cluster Group     Storage QoS Policy Manager  
```  

### <a name="BKMK_SetupStorageQoSonComputeCluster"></a>在计算群集上设置存储 QoS  
Windows Server 2016 中的 Hyper-V 角色具有对存储 QoS 的内置支持并默认启用。  

#### <a name="install-remote-administration-tools-to-manage-storage-qos-policies-from-remote-computers"></a>安装远程管理工具以管理来自远程计算机的存储 QoS 策略  
你可以通过使用远程服务器管理工具从计算主机管理存储 QoS 策略并监视流。  以上各项可作为可选功能在所有 Windows Server 2016 安装中提供，并且可在 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=45520)网站为 Windows 10 单独下载。

**RSAT-Clustering** 可选功能包括适用于故障转移群集的远程管理的 Windows PowerShell 模块，包括存储 QoS。  

-   Windows PowerShell：Add-windowsfeature RSAT 群集  

**RSAT-Hyper-V-Tools** 可选功能包括适用于 Hyper-V 的远程管理的 Windows PowerShell 模块。  

-   Windows PowerShell：Add-windowsfeature RSAT-Hyper-v-工具  

#### <a name="deploy-virtual-machines-to-run-workloads-for-testing"></a>部署运行工作负荷的虚拟机以用于测试  
你将需要存储在具有相关工作负荷的横向扩展文件服务器上的一些虚拟机。  有关如何模拟负载和执行一些压力测试的一些技巧，请参阅以下页面获取推荐的工具（DiskSpd）和示例用法：[DiskSpd、PowerShell 和存储性能：测量本地磁盘和 SMB 文件共享的 IOPs、吞吐量和延迟。](http://blogs.technet.com/b/josebda/archive/2014/10/13/diskspd-powershell-and-storage-performance-measuring-iops-throughput-and-latency-for-both-local-disks-and-smb-file-shares.aspx)  

本指南中所示的示例方案包括五个虚拟机。 BuildVM1、BuildVM2、BuildVM3 和 BuildVM4 运行从低到中等存储需求的桌面工作负荷。 TestVm1 运行具有高存储需求的联机事务处理基准。  

### <a name="view-current-storage-performance-metrics"></a>查看当前存储性能指标  
本部分包括：  

-   如何使用 `Get-StorageQosFlow` cmdlet 查询流。  

-   如何使用 `Get-StorageQosVolume` cmdlet 查看卷的性能。  

#### <a name="query-flows-using-the-get-storageqosflow-cmdlet"></a>使用 Get-StorageQosFlow cmdlet 查询流  

Get-StorageQosFlow cmdlet 显示由 Hyper-V 服务器启动的所有当前流。 所有数据均由横向扩展文件服务器群集收集，因此 cmdlet 可用于横向扩展文件服务器群集中的任何节点上，或用于使用 -CimSession 参数的远程服务器中。  

**以下示例命令演示如何使用 Get-StorageQoSFlow 在服务器上查看 Hyper-V 打开的所有文件**。  

```PowerShell
PS C:\> Get-StorageQosFlow  

InitiatorName    InitiatorNodeNam StorageNodeName  FilePath        Status  
                 e  
-------------    ---------------- ---------------  --------        ------  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM1         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM2         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
```  

对以下示例命令进行格式化以显示按 IOPs 排序的虚拟机名称、Hyper-V 主机名称、IOPs 和 VHD 文件名称。  

```PowerShell  
PS C:\> Get-StorageQosFlow | Sort-Object StorageNodeIOPs -Descending | ft InitiatorName, @{Expression={$_.InitiatorNodeName.Substring(0,$_.InitiatorNodeName.IndexOf('.'))};Label="InitiatorNodeName"}, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName InitiatorNodeName StorageNodeIOPs Status File  
------------- ----------------- --------------- ------ ----  
WinOltp1      plang-c1                     3482     Ok IOMETER.VHDX  
BuildVM2      plang-c2                      544     Ok BUILDVM2.VHDX  
BuildVM1      plang-c2                      497     Ok BUILDVM1.VHDX  
BuildVM4      plang-c2                      316     Ok BUILDVM4.VHDX  
BuildVM3      plang-c2                      296     Ok BUILDVM3.VHDX  
BuildVM4      plang-c2                      195     Ok WIN8RTM_ENTERPRISE_VL_BU...  
TR20-VMM      plang-z400                    156     Ok DATA1.VHDX  
BuildVM3      plang-c2                       81     Ok WIN8RTM_ENTERPRISE_VL_BU...  
WinOltp1      plang-c1                       65     Ok BOOT.VHDX  
                                             18     Ok DefaultFlow  
                                             12     Ok DefaultFlow  
WinOltp1      plang-c1                        4     Ok 9914.0.AMD64FRE.WINMAIN....  
TR20-VMM      plang-z400                      4     Ok DATA2.VHDX  
TR20-VMM      plang-z400                      3     Ok BOOT.VHDX  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
```  

以下示例命令演示如何对基于 InitiatorName 的流进行筛选，以轻松查找特定虚拟机的存储性能和设置。  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName BuildVm1 | Format-List

FilePath           : C:\ClusterStorage\Volume2\SHARES\TWO\BUILDWORKLOAD\BUILDVM1.V  
                     HDX  
FlowId             : ebfecb54-e47a-5a2d-8ec0-0940994ff21c  
InitiatorId        : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
InitiatorIOPS      : 464  
InitiatorLatency   : 26.2684  
InitiatorName      : BuildVM1  
InitiatorNodeName  : plang-c2.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 500  
PolicyId           : b145e63a-3c9e-48a8-87f8-1dfc2abfe5f4  
Reservation        : 500  
Status             : Ok  
StorageNodeIOPS    : 475  
StorageNodeLatency : 7.9725  
StorageNodeName    : plang-fs1.plang.nttest.microsoft.com  
TimeStamp          : 2/12/2015 2:58:49 PM  
VolumeId           : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName     :  
MaximumIops        : 500  
MinimumIops        : 500  
```  

`Get-StorageQosFlow` cmdlet 返回的数据包括：  

-   Hyper-V 主机名 (InitiatorNodeName)。  

-   虚拟机的名称及其 ID（InitiatorName 和 InitiatorId）  

-   Hyper-V 主机观察到的虚拟磁盘的最近平均性能（InitiatorIOPS、InitiatorLatency）  

-   存储群集观察到的虚拟磁盘的最近平均性能（StorageNodeIOPS、StorageNodeLatency）  

-   应用于文件的当前策略（如果有）及其结果配置（PolicyId、Reservation、Limit）  

-   策略的状态  

    -   **正常** - 不存在问题  

    -   InsufficientThroughput - 应用了策略，但无法传输最小 IOPs。  如果 VM 的最小值或全部 VM 超出了存储卷可传输的范围，则会出现此问题。  

    -   **UnknownPolicyId** - 策略被分配到 Hyper-V 主机上的虚拟机，但在文件服务器中丢失。  此策略应从虚拟机配置中删除，或应在文件服务器群集上创建匹配的策略。  

#### <a name="view-performance-for-a-volume-using-get-storageqosvolume"></a>使用 Get-StorageQosVolume 查看卷的性能  
除了每个流的性能指标，还将收集每个存储卷级别上的存储性能指标。  这样将易于查看以规范化 IOPs、延迟以及应用到卷的聚合限制和保留表示的平均总利用率。  

```PowerShell
PS C:\> Get-StorageQosVolume | Format-List  

Interval       : 300000  
IOPS           : 0  
Latency        : 0  
Limit          : 0  
Reservation    : 0  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 434f561f-88ae-46c0-a152-8c6641561504  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 0  

Interval       : 300000  
IOPS           : 1097  
Latency        : 3.1524  
Limit          : 0  
Reservation    : 1568  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 1568  

Interval       : 300000  
IOPS           : 5354  
Latency        : 6.5084  
Limit          : 0  
Reservation    : 781  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 781  
```  

## <a name="BKMK_CreateQoSPolicies"></a>如何创建和监视存储 QoS 策略  
本部分介绍如何创建存储 QoS 策略、如何向虚拟机应用这些策略以及在应用策略后如何监视存储群集。  

### <a name="create-storage-qos-policies"></a>创建存储 QoS 策略  
在横向扩展文件服务器群集中定义和管理存储 QoS 策略。  你可以根据需要创建很多策略以用于灵活的部署（最多为每个存储群集 10000 个）。  

分配到虚拟机的每个 VHD/VHDX 文件可能会配置一个策略。 不同的文件和虚拟机可使用同一策略，也可以为每个文件和虚拟机配置单独的策略。  如果为多个 VHD/VHDX 文件或多个虚拟机配置同一策略，则它们会被聚合在一起并公平地共享 MinimumIOPS 和MaximumIOPS。 如果你对多个 VHD/VHDX 文件或虚拟机使用单独的策略，则最小 IOPs 和最大 IOPs 将被单独跟踪。  

如果你为不同的虚拟机创建多个类似的策略，且虚拟机具有相同的存储需求，则它们将收到类似的 IOPs 共享。  如果其中一个 VM 需要的多，而另一个 VM 需要的少，则 IOPs 将按照此需求。  

### <a name="types-of-storage-qos-policies"></a>存储 QoS 策略的类型  
有两种类型的策略：聚合（以前称为 SingleInstance）和专用（以前称为多实例）。 聚合策略为 VHD/VHDX 文件及其应用的虚拟机的组合集应用最小和最大值。 实际上，它们共享一组特定的 IOPS 和带宽。 专用策略为每个 VHD/VHDx 单独应用最小和最大值。 这样将易于创建向多个 VHD/VHDx 文件应用类似限制的单个策略。  

例如，如果你创建最小值为 300 IOPs、最大值为 500 IOPs 的聚合策略。 如果你将此策略应用到 5 个不同的 VHD/VHDx 文件，即表示你确定保证组合的 5 个 VHD/VHDx 文件至少为 300 IOPs（如果有此需求，且存储系统可以提供此性能）但不超出 500 IOPs。 如果 VHD/VHDx 对 IOPs 具有类似的高需求且存储系统能够满足，则每个 VHD/VHDx 将获得大约 100 IOPs。  

但是，如果你创建具有类似限制的专用策略并将其应用到 5 个不同的虚拟机上的 VHD/VHDx 文件，则每个虚拟机将获得至少 300 IOPs 但不超过 500 IOPs。 如果虚拟机对 IOPs 具有类似的高需求且存储系统能够满足，则每个虚拟机将获得大约 500 IOPs。 .  如果其中一个虚拟机具有配置了同一 MulitInstance 策略的多个 VHD/VHDx 文件，则它们将共享限制以便具有该策略的文件的 VM 的总 IO 将不会超过限制。  

因此，如果你有一组你不想展现同一性能特性且不想创建多个类似策略的 VHD/VHDx 文件，则可以使用单个专用策略并应用于每个虚拟机的文件。

将分配给单一聚合策略的 VHD/VHDx 文件的数量保持为20或更少。  此策略类型旨在在群集上使用几个 Vm 进行聚合。

### <a name="create-and-apply-a-dedicated-policy"></a>创建和应用专用策略  
首先，使用 `New-StorageQosPolicy` cmdlet 在横向扩展文件服务器上创建策略，如以下示例中所示：  

```PowerShell
$desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  
```  

接下来，将其应用于 Hyper-V 服务器上相应的虚拟机的硬盘驱动器。  记下前一步骤的 PolicyId 或将其存储在你的脚本中的变量中。  

在横向扩展文件服务器上，使用 PowerShell 创建存储 QoS 策略并获取其策略 ID，如以下示例中所示：  

```PowerShell
PS C:\> $desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  

C:\> $desktopVmPolicy.PolicyId  

Guid  
----  
cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

在 Hyper-V 服务器上，通过 PowerShell 使用策略 ID 设置存储 QoS 策略，如以下示例中所示：  

```PowerShell
Get-VM -Name Build* | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

### <a name="confirm-that-the-policies-are-applied"></a>确定应用了策略  
使用 `Get-StorageQosFlow`PowerShell cmdlet 确认 MinimumIOPs 和 MaximumIOPs 已被应用到合适的流，如以下示例中所示。  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName |  
 ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
BuildVM1          Ok         100         200             250     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             251     Ok BUILDVM2.VHDX  
BuildVM3          Ok         100         200             252     Ok BUILDVM3.VHDX  
BuildVM4          Ok         100         200             233     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               1     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               5     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666               4     Ok BOOT.VHDX  
WinOltp1          Ok           0           0               0     Ok 9914.0.AMD6...  
WinOltp1          Ok           0           0            5166     Ok IOMETER.VHDX  
WinOltp1          Ok           0           0               0     Ok BOOT.VHDX  
```  

在 Hyper-V 服务器上，你同样可以使用提供的脚本 **Get-VMHardDiskDrivePolicy.ps1** 查看应用到虚拟机硬盘驱动器上的策略是什么。  

```PowerShell
PS C:\> Get-VM -Name BuildVM1 | Get-VMHardDiskDrive | Format-List  

Path                          : \\plang-fs.plang.nttest.microsoft.com\two\BuildWorkload  
                                \BuildVM1.vhdx  
DiskNumber                    :  
MaximumIOPS                   : 0  
MinimumIOPS                   : 0  
QoSPolicyID                   : cd6e6b87-fb13-492b-9103-41c6f631f8e0  
SupportPersistentReservations : False  
ControllerLocation            : 0  
ControllerNumber              : 0  
ControllerType                : IDE  
PoolName                      : Primordial  
Name                          : Hard Drive  
Id                            : Microsoft:AE4E3DD0-3BDE-42EF-B035-9064309E6FEC\83F8638B  
                                -8DCA-4152-9EDA-2CA8B33039B4\0\0\D  
VMId                          : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
VMName                        : BuildVM1  
VMSnapshotId                  : 00000000-0000-0000-0000-000000000000  
VMSnapshotName                :  
ComputerName                  : PLANG-C2  
IsDeleted                     : False  
```  

### <a name="query-for-storage-qos-policies"></a>查询存储 QoS 策略  
@no__t 在横向扩展文件服务器上列出所有已配置的策略及其状态。  

```PowerShell
PS C:\> Get-StorageQosPolicy  

Name                 MinimumIops          MaximumIops          Status  
----                 -----------          -----------          ------  
Default              0                    0                    Ok  
Limit500             0                    500                  Ok  
SilverVm             500                  500                  Ok  
Desktop              100                  200                  Ok  
Limit500             0                    0                    Ok  
VMM                  100                  2000                 Ok  
Vdi                  1                    100                  Ok  
```  

基于系统执行的方式，状态可随时间更改。  

-   **正常** - 使用此策略的所有流将接收其请求的 MinimumIOPs。  

-   **InsufficientThroughput** - 使用此策略的一个或多个流未接收最小 IOPs  

你可以通过将策略发送到 `Get-StorageQosPolicy` 来获取配置的所有流的状态，以使用该策略，如下所示：  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name Desktop | Get-StorageQosFlow | ft InitiatorName, *IOPS, Status, FilePath -AutoSize  

InitiatorName MaximumIops MinimumIops InitiatorIOPS StorageNodeIOPS Status FilePat  
                                                                           h  
------------- ----------- ----------- ------------- --------------- ------ -------  
BuildVM4              100          50           187              17     Ok C:\C...  
BuildVM3              100          50           194              25     Ok C:\C...  
BuildVM1              200         100           195             196     Ok C:\C...  
BuildVM2              200         100           193             192     Ok C:\C...  
BuildVM4              200         100           187             169     Ok C:\C...  
BuildVM3              200         100           194             169     Ok C:\C...  
```  

### <a name="create-an-aggregated-policy"></a>创建聚合策略  
如果你希望多个虚拟硬盘共享单个 IOPs 池和带宽，则可以使用聚合策略。  例如，如果你将同一聚合策略应用到两个虚拟机的硬盘，则最小值将根据需要被拆分。  两个磁盘必定是组合的最小值，且其总和不会超过指定的最大 IOPs 或带宽。  

也可使用同样的方法为虚拟机（该虚拟机组成服务或属于多主机环境中的租户）的所有 VHD/VHDx 文件提供单个分配。  

除了指定的 PolicyType 以外，创建专用和聚合策略的过程没有区别。  

以下示例演示如何创建聚合存储 QoS 策略并在横向扩展文件服务器上获取其 policyID：  

```PowerShell
PS C:\> $highPerf = New-StorageQosPolicy -Name SqlWorkload -MinimumIops 1000 -MaximumIops 5000 -PolicyType Aggregated  
[plang-fs]: PS C:\Users\plang\Documents> $highPerf.PolicyId  

Guid  
----  
7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

以下示例演示如何使用前面示例中获取的 policyID 在 Hyper-V 服务器上应用存储 QoS 策略：  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID 7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

以下示例演示如何从文件服务器查看存储 QoS 策略的效果：  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | format-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | for  
mat-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
```  

每个虚拟硬盘都将具有基于其负荷调整的 MinimumIOPs、MaximumIOPs 和 MaximumIobandwidth 值。  这将确保用于磁盘组的总带宽处于策略定义的范围内。  在上面的示例中，前两个磁盘处于空闲状态，而第三个磁盘允许使用至多最大 IOPs。  如果前两个磁盘再次开始发送 IO，则第三个磁盘的最大 IOPs 将自动减少。  

### <a name="modify-an-existing-policy"></a>修改现有策略  
策略创建后，属性 Name、MinimumIOPs、MaximumIOPs 和 MaximumIoBandwidth 可能被更改。  但是，一旦策略创建后，策略类型（聚合/专用）将不能更改。  

以下 Windows PowerShell cmdlet 演示如何为现有策略更改 MaximumIOPs 属性：  

```PowerShell
[DBG]: PS C:\demoscripts>> Get-StorageQosPolicy -Name SqlWorkload | Set-StorageQosPolicy -MaximumIops 6000  
```  

以下 cmdlet 可验证更改：  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
SqlWorkload             1000                   6000                   Ok    

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageQosPolicy -Name SqlWorkload | Get-Storag  
eQosFlow | Format-Table InitiatorName, PolicyId, MaximumIops, MinimumIops, StorageNodeIops -A  
utoSize  

InitiatorName PolicyId                             MaximumIops MinimumIops StorageNodeIops  
------------- --------                             ----------- ----------- ---------------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        6000        1000            4507  
```  

## <a name="BKMK_KnownIssues"></a>如何识别和解决常见问题  
本部分介绍如何查找具有无效存储 QoS 策略的虚拟机、如何重新创建匹配的策略、如何从虚拟机删除策略以及如何标识不符合存储 QoS 策略要求的虚拟机。  

### <a name="BKMK_FindingVMsWithInvalidPolicies"></a>标识具有无效策略的虚拟机  

将策略从虚拟机删除前，如果策略已从文件服务器删除，则如果未应用任何策略，虚拟机将继续运行。  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload | Remove-StorageQosPolicy  

Confirm  
Are you sure you want to perform this action?  
Performing the operation "DeletePolicy" on target "MSFT_StorageQoSPolicy (PolicyId =  
"7e2f3e73-1ae4-4710-8219-0769a4aba072")".  
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"):  
```  

流状态现在将显示“UnknownPolicyId”  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName          Status MinimumIops MaximumIops StorageNodeIOPs          Status File  
-------------          ------ ----------- ----------- ---------------          ------ ----  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0              10              Ok Def...  
                           Ok           0           0              13              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
BuildVM1                   Ok         100         200             193              Ok BUI...  
BuildVM2                   Ok         100         200             196              Ok BUI...  
BuildVM3                   Ok          50          64              17              Ok WIN...  
BuildVM3                   Ok          50         136             179              Ok BUI...  
BuildVM4                   Ok          50         100              23              Ok WIN...  
BuildVM4                   Ok         100         200             173              Ok BUI...  
TR20-VMM                   Ok          33         666               2              Ok DAT...  
TR20-VMM                   Ok          25         975               3              Ok DAT...  
TR20-VMM                   Ok          75        1025              12              Ok BOO...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId 991...  
WinOltp1      UnknownPolicyId           0           0            4926 UnknownPolicyId IOM...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId BOO...  
```  

#### <a name="BKMK_RecreateMatchingPolicy"></a>重新创建匹配的存储 QoS 策略  
如果无意中删除了策略，则可以使用旧的 PolicyId 创建一个新策略。  首先，获取所需的 PolicyId  

```PowerShell
PS C:\> Get-StorageQosFlow -Status UnknownPolicyId | ft InitiatorName, PolicyId -AutoSize  

InitiatorName PolicyId  
------------- --------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

接下来，使用此 PolicyId 创建一个新策略  

```PowerShell
PS C:\> New-StorageQosPolicy -PolicyId 7e2f3e73-1ae4-4710-8219-0769a4aba072 -PolicyType Aggregated -Name RestoredPolicy -MinimumIops 100 -MaximumIops 2000  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
RestoredPolicy          100                    2000                   Ok  
```  

最后，验证此策略已应用。  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               8     Ok DefaultFlow  
                  Ok           0           0               9     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
BuildVM1          Ok         100         200             192     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             193     Ok BUILDVM2.VHDX  
BuildVM3          Ok          50         100              24     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM3          Ok         100         200             166     Ok BUILDVM3.VHDX  
BuildVM4          Ok          50         100              12     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM4          Ok         100         200             178     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666              10     Ok BOOT.VHDX  
WinOltp1          Ok          25         500               0     Ok 9914.0.AMD64FRE.WINMA...  
```  

#### <a name="BKMK_RemovePolicyFromVM"></a>删除存储 QoS 策略  

如果策略是有意删除的，或如果随 VM 导入了你并不需要的策略，则它可能会被删除。  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID $null  
```  

一旦将 PolicyId 从虚拟硬盘设置中删除，状态将为“正常”，且不会应用最小或最大值。  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ----------- ----------- --------------- ------ ----  
                        0           0               0     Ok DefaultFlow  
                        0           0              16     Ok DefaultFlow  
                        0           0              12     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
BuildVM1              100         200             197     Ok BUILDVM1.VHDX  
BuildVM2              100         200             192     Ok BUILDVM2.VHDX  
BuildVM3                9           9              23     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM3               91         191             171     Ok BUILDVM3.VHDX  
BuildVM4                8           8              18     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM4               92         192             163     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               2     Ok DATA2.VHDX  
TR20-VMM               33         666               1     Ok DATA1.VHDX  
TR20-VMM               33         666               5     Ok BOOT.VHDX  
WinOltp1                0           0               0     Ok 9914.0.AMD64FRE.WINMAIN.1412...  
WinOltp1                0           0            1811     Ok IOMETER.VHDX  
WinOltp1                0           0               0     Ok BOOT.VHDX  
```  

### <a name="BKMK_VMsThatDoNotMeetStorageQoSPoilicies"></a>查找不符合存储 QoS 策略的虚拟机  
**InsufficientThroughput** 状态将分配到以下任何流：  

-   具有由策略设置的指定的最小 IOPs；且  

-   以符合或超出最小值的速率启动 IO；且  

-   未实现最小 IOP 速率  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs                 Status File  
------------- ----------- ----------- ---------------                 ------ ----  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0              15                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
BuildVM3               50         100              20                     Ok WIN8RTM_ENTE...  
BuildVM3              100         200             174                     Ok BUILDVM3.VHDX  
BuildVM4               50         100              11                     Ok WIN8RTM_ENTE...  
BuildVM4              100         200             188                     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               3                     Ok DATA1.VHDX  
TR20-VMM               78        1032             180                     Ok BOOT.VHDX  
TR20-VMM               22         968               4                     Ok DATA2.VHDX  
WinOltp1             3750        5000               0                     Ok 9914.0.AMD64...  
WinOltp1            15000       20000           11679 InsufficientThroughput IOMETER.VHDX  
WinOltp1             3750        5000               0                     Ok BOOT.VHDX  
```  

你可以为任何状态确定流，包括 **InsufficientThroughput**，如以下示例中所示：  

```PowerShell
PS C:\> Get-StorageQosFlow -Status InsufficientThroughput | fl  

FilePath           : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
FlowId             : 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
InitiatorId        : 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
InitiatorIOPS      : 12168  
InitiatorLatency   : 22.983  
InitiatorName      : WinOltp1  
InitiatorNodeName  : plang-c1.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 20000  
PolicyId           : 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
Reservation        : 15000  
Status             : InsufficientThroughput  
StorageNodeIOPS    : 12181  
StorageNodeLatency : 22.0514  
StorageNodeName    : plang-fs2.plang.nttest.microsoft.com  
TimeStamp          : 2/13/2015 12:07:30 PM  
VolumeId           : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName     :  
MaximumIops        : 20000  
MinimumIops        : 15000  
```  

## <a name="BKMK_Health"></a>使用存储 QoS 监视运行状况  
新的运行状况服务简化了存储群集的监视，你在一个位置即可检查任意节点中是否存在可操作事件。 本部分介绍如何使用 `debug-storagesubsystem` cmdlet 监视你的存储群集的运行状况。  

### <a name="view-storage-status-with-debug-storagesubsystem"></a>使用 Debug-StorageSubSystem 查看存储状态  
群集存储空间也在一个位置提供存储群集的运行状况信息。 此功能可以帮助管理员在存储部署中快速识别当前问题并在问题出现或解除时进行监视。  

#### <a name="vm-with-invalid-policy"></a>具有无效策略的 VM  
也将通过存储子系统运行状况监视来报告具有无效策略的 VM。  以下是来自同一状态的示例，如本文档的[查找具有无效策略的 VM](#BKMK_FindingVMsWithInvalidPolicies) 部分所述。  

```PowerShell
C:\> Get-StorageSubSystem -FriendlyName Clustered* | Debug-StorageSubSystem  

EventTime                 :  
FaultId                   : 0d16d034-9f15-4920-a305-f9852abf47c3  
FaultingObject            :  
FaultingObjectDescription : Storage QoS Policy 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
FaultingObjectLocation    :  
FaultType                 : Storage QoS policy used by consumer does not exist.  
PerceivedSeverity         : Minor  
Reason                    : One or more storage consumers (usually Virtual Machines) are  
                            using a non-existent policy with id  
                            5d1bf221-c8f0-4368-abcf-aa139e8a7c72. Consumer details:  

                            Flow ID: 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
                            Initiator ID: 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
                            Initiator Name: WinOltp1  
                            Initiator Node: plang-c1.plang.nttest.microsoft.com  
                            File Path:  
                            C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
RecommendedActions        : {Reconfigure the storage consumers (usually Virtual Machines)  
                            to use a valid policy., Recreate any missing Storage QoS  
                            policies.}  
PSComputerName            :  
```  

#### <a name="lost-redundancy-for-a-storage-spaces-virtual-disk"></a>存储空间虚拟磁盘的冗余丢失  

在此示例中，群集存储空间具有被创建为三向镜像的虚拟磁盘。  出现故障的磁盘已从系统中删除，但是替代的磁盘还未添加。  存储子系统报告 HealthStatus **警告**冗余丢失，但是 OperationalStatus 为**正常**，因为卷仍处于联机状态。  

```PowerShell
PS C:\> Get-StorageSubSystem -FriendlyName Clustered*  

FriendlyName                   HealthStatus                   OperationalStatus  
------------                   ------------                   -----------------  
Clustered Windows Storage o... Warning                        OK  

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageSubSystem -FriendlyName Clustered* | Deb  
ug-StorageSubSystem  

EventTime                 :  
FaultId                   : dfb4b672-22a6-4229-b2ed-c29d7485bede  
FaultingObject            :  
FaultingObjectDescription : Virtual disk 'Two'  
FaultingObjectLocation    :  
FaultType                 : VirtualDiskDegradedFaultType  
PerceivedSeverity         : Minor  
Reason                    : Virtual disk 'Two' does not have enough redundancy remaining to  
                            successfully repair or regenerate its data.  
RecommendedActions        : {Rebalance the pool, replace failed physical disks, or add new  
                            physical disks to the storage pool, then repair the virtual  
                            disk.}  
PSComputerName            :  
```  

### <a name="sample-script-for-continuous-monitoring-of-storage-qos"></a>持续监视存储 QoS 的示例脚本  

本部分包括使用 WMI 脚本显示如何监视常见故障的示例脚本。  它旨在为开发人员提供实时检索运行状况事件的起点。  

**示例脚本：**  

```PowerShell
param($cimSession)  
# Register and display events  
Register-CimIndicationEvent -Namespace root\microsoft\windows\storage -ClassName msft_storagefaultevent -CimSession $cimSession  

while ($true)  
{  
     $e = (Wait-Event)  
     $e.SourceEventArgs.NewEvent  
     Remove-Event $e.SourceIdentifier  
}  
```  

## <a name="frequently-asked-questions"></a>常见问题  

### <a name="how-do-i-retain-a-storage-qos-policy-being-enforced-for-my-virtual-machine-if-i-move-its-vhdvhdx-files-to-another-storage-cluster"></a>将存储 QoS 策略的 VHD/VHDx 文件移动到另一存储群集后，该如何保留强制执行到虚拟机的存储 QoS 策略  

指定策略的 VHD/VHDx 文件上的设置是策略 ID 的 GUID。  创建策略时，可通过使用 **PolicyID** 参数指定 GUID。  如果未指定该参数，将创建随机的 GUID。  因此，你可以在 VM 目前存储其 VHD/VHDx 文件的存储群集上获取 PolicyID，并在目标存储群集上创建相同的策略，然后指定使用同一 GUID 对其创建。  当 VM 文件移动到新的存储群集时，具有同一 GUID 的策略将生效。  

系统中心虚拟机管理器可用于跨多个存储群集应用策略，这使得此方案更易操作。  
### <a name="if-i-change-the-storage-qos-policy-why-dont-i-see-it-take-effect-immediately-when-i-run-get-storageqosflow"></a>更改存储 QoS 策略后，运行 Get-StorageQoSFlow 时为什么没有看到它立即生效  

如果你具有达到策略最大值的流，且将该策略的值更改为更高或更低，然后使用 PowerShell cmdlets 立即确定流的延迟/IOPS/带宽，则需要最多 5 分钟才能看到流上的策略更改所产生的全部效果。  新的限制将在几秒内生效，但是 **Get-StorgeQoSFlow PowerShell cmdlet** 通过使用 5 分钟的滑动窗口来使用每个计数器的平均值。  否则，如果它显示当前值，且你在行中多次运行 PowerShell cmdlet，则你将会看到完全不同的值，因为 IOPS 和延迟的值从一秒到另一秒波动很大。

### <a name="BKMK_Updates"></a>Windows Server 2016 中新增了哪些功能

Windows Server 2016 中已重命名存储 QoS 策略类型名称。  **多实例**策略类型被重命名为**专用**，而**单实例**被重命名为**聚合**。 专用策略的管理行为也被修改 - 具有应用于同一**专用**策略的同一虚拟机内的 VHD/VHDX 文件将不会共享 I/O 分配。  

Windows Server 2016 中新增了两个存储 QoS 功能：  

-   **最大带宽**  

    Windows Server 2016 中的存储 QoS 引入了指定向策略分配的流可使用的最大带宽的功能。  在 **StorageQosPolicy** cmdlet 中指定的参数为 **MaximumIOBandwidth**，且输出表示为字节/秒。  
    如果 **MaximimIops** 和 **MaximumIOBandwidth** 均在策略中设置，则它们都将生效，且流达到的第一个值将限制流的 I/O。  

-   **IOPS 规范化可配置**  

    存储 QoSin 使用 IOPS 规范化。  默认值是使用 8 K 的规范化大小。  Windows Server 2016 中的存储 QoS 引入了为存储群集指定不同规范化大小的功能。  此规范化大小对存储群集上的所有流均生效，且在其更改后立即生效（几秒钟内）。  最小值为 1 KB，最大值为 4 GB（建议不要设置超出 4 MB，因为很少需要超出 4 MB IO）。  

    需要考虑的一点是，由于规范化计算中的更改，当你更改 IOPS 规范化时，同一 IO 模式/吞吐量在存储 QoS 输出中将显示不同的 IOPS 数。  如果你在存储群集中比较 IOPS，你可能也想要验证每个 IOPS 正在使用什么样的规范化值，因为这将影响报告的规范化 IOPS。    

#### <a name="example-1-creating-a-new-policy-and-viewing-the-maximum-bandwidth-on-the-storage-cluster"></a>示例 1：创建新策略并查看存储群集上的最大带宽  
在 PowerShell 中，你可以指定数字表示的单位。  在以下示例中，10 MB 用作最大带宽值。  存储 QoS 将对其转换并另存为字节/秒，因此，10 MB 被转换为 10485760 字节/秒。  

```PowerShell
PS C:\Windows\system32> New-StorageQosPolicy -Name HR_VMs -MaximumIops 1000 -MinimumIops 20 -MaximumIOBandwidth 10MB  

Name   MinimumIops MaximumIops MaximumIOBandwidth Status  
----   ----------- ----------- ------------------ ------  
HR_VMs 20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQosPolicy  

Name    MinimumIops MaximumIops MaximumIOBandwidth Status  
----    ----------- ----------- ------------------ ------  
Default 0           0           0                  Ok  
HR_VMs  20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQoSFlow | fL InitiatorName,FilePath,InitiatorIOPS,InitiatorLatency,InitiatorBandwidth  

InitiatorName      : testsQoS  
FilePath           : C:\ClusterStorage\Volume2\TESTSQOS\VIRTUAL HARD DISKS\TESTSQOS.VHDX  
InitiatorIOPS      : 5  
InitiatorLatency   : 1.5455  
InitiatorBandwidth : 37888  
```  

#### <a name="example-2-get-iops-normalization-settings-and-specify--a-new-value"></a>示例 2：获取 IOPS 规范化设置并指定新值  

以下示例演示如何获取存储群集 IOPS 规范化设置（默认值为 8 KB），然后将其设置为 32 KB，然后再次将其显示。  请注意，在此示例中，指定“32 KB”，因为 PowerShell 允许指定单位，而无需转换为字节。   输出的确以字节/秒为单位显示值。  

```PowerShell
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
8192  

PS C:\Windows\system32> Set-StorageQosPolicyStore -IOPSNormalizationSize 32KB  
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
32768  
```    

## <a name="see-also"></a>请参阅  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Windows Server 2016 中的存储副本](../storage-replica/storage-replica-overview.md)  
- [Windows Server 2016 中的存储空间直通](../storage-spaces/storage-spaces-direct-overview.md)  

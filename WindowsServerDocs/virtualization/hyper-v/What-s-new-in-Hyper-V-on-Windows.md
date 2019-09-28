---
title: Windows Server 2016 上的 Hyper-v 中的新增功能
description: 概述 Hyper-v 中的新功能
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: KBDAzure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: 195d78ff8de75ca9e3a88d4300bb2f52cd45632f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365384"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>Windows Server 上的 Hyper-v 中的新增功能

>适用于：Windows Server 2019，Microsoft Hyper-V Server 2016，Windows Server 2016
  
本文介绍 Windows Server 2019、Windows Server 2016 和 Microsoft Hyper-V Server 2016 上的 Hyper-v 的新功能和更改的功能。 若要在使用 Windows Server 2012 R2 创建的虚拟机上使用新功能，并将其移动或导入到在 Windows Server 2019 或 Windows Server 2016 上运行 Hyper-v 的服务器，你将需要手动升级虚拟机配置版本。 有关说明，请参阅[升级虚拟机版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。  
  
本文包含此是，以及该功能是新增功能还是更新功能。  

## <a name="windows-server-version-1903"></a>Windows Server 版本 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>将 Hyper-v 管理器添加到服务器核心安装（已更新）

在生产环境中使用 Windows Server 半年频道时，我们建议使用 Server Core 安装选项。 但是，Server Core 在默认情况下会忽略许多有用的管理工具。 可以通过安装应用兼容性功能来添加许多最常用的工具，但这仍会缺少一些工具。

因此，根据客户的反馈，我们在此版本中添加了一个用于应用兼容性功能的工具：Hyper-v 管理器（virtmgmt）。

有关详细信息，请参阅 [Server Core 应用兼容性功能](../../get-started-19/install-fod-19.md)。

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>安全性：防护虚拟机改进（新）

- **分支机构改进**

    现在可以利用新的[回退 HGS](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) 和[脱机模式](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode)功能在计算机上运行受防护的虚拟机，以间歇性方式连接到主机保护者服务。 可以通过回退 HGS 配置第二组用于 Hyper-V 的 URL，试试是否无法访问主 HGS 服务器。

    使用脱机模式时，即使不能访问 HGS，也可继续启动受防护的虚拟机，只要 VM 已成功启动一次并且主机的安全配置并未更改即可。

- **故障排除改进**

    我们还通过启用对 VMConnect 增强会话模式和 PowerShell Direct 的支持，简化了[受防护虚拟机的故障排除](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms)。 这些工具尤其适用于到 VM 的网络连接已断开，需要更新其配置才能恢复访问的情况。

    这些功能不需要进行配置。将受防护的 VM 置于运行 Windows Server 1803 或更高版本的 Hyper-V 主机上时，这些功能会自动变为可用状态。

- **Linux 支持**

    如果你运行混合操作系统环境，Windows Server 2019 现在支持在受防护的虚拟机内运行 Ubuntu、Red Hat Enterprise Linux 和 SUSE Linux Enterprise Server。

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>与连接备用\(新设备兼容\)

当 Hyper-v 角色安装在使用始终可用/始终连接（AOAC）电源模型的计算机上时，**连接的待机**电源状态现在可用。  
  
### <a name="discrete-device-assignment-new"></a>离散设备分配\(新\)

此功能允许你授予虚拟机对某些 PCIe 硬件设备的直接和独占访问权限。 以这种方式使用设备将绕过 Hyper-V 虚拟化堆栈，使访问更迅速。 有关支持的硬件的详细信息，请参阅[Windows Server 2016 上的 Hyper-v 系统要求](System-requirements-for-Hyper-V-on-Windows.md)中的 "离散设备分配"。 有关详细信息，包括如何使用此功能和注意事项，请参阅虚拟化博客中的发布 "[离散设备分配—说明和背景](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)"。

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>第1代虚拟机\(中的操作系统磁盘的加密支持（新）

你现在可以使用第1代虚拟机中的 BitLocker 驱动器加密来保护操作系统磁盘。 新功能密钥存储创建一个小型专用驱动器来存储系统驱动器的 BitLocker 密钥。 这是为了实现此目的，而不是使用仅在第2代虚拟机中提供的虚拟受信任的平台模块（TPM）。 若要解密磁盘并启动虚拟机，Hyper-v 主机必须是已获授权的受保护构造的一部分，或者是虚拟机的一个保护者的私钥。 密钥存储需要版本8虚拟机。 有关虚拟机版本的信息，请参阅在[windows 10 或 Windows Server 2016 上的 hyper-v 中升级虚拟机版本](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。  
  
### <a name="host-resource-protection-new"></a>主机资源保护\(新建\)

此功能通过查找过多级别的活动来防止虚拟机使用其系统资源的份额。 这可以帮助防止虚拟机的过多活动降低主机或其他虚拟机的性能。 当监视检测到具有过多活动的虚拟机时，将为虚拟机提供更少的资源。 默认情况下，此监视和强制功能处于关闭状态。 使用 Windows PowerShell 打开或关闭。 若要启用它，请运行以下命令：  
  
```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

有关此 cmdlet 的详细信息，请参阅[VMProcessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor)。

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>网络适配器和内存\(的热添加和删除\)

现在，运行虚拟机期间，可以添加或删除网络适配器而不出现停机。 这适用于运行 Windows 或 Linux 操作系统的第 2 代虚拟机。  
  
你还可以调整在虚拟机运行时分配给该虚拟机的内存量，即使尚未启用动态内存。 这适用于运行 Windows Server 2016 或 Windows 10 的第1代和第2代虚拟机。  

### <a name="hyper-v-manager-improvements-updated"></a>Hyper-v 管理器改进\(已更新\) 
  
-   **备用凭据支持**-当你连接到另一台 Windows Server 2016 或 windows 10 远程主机时，你现在可以在 Hyper-v 管理器中使用一组不同的凭据。 你还可以保存这些凭据，以便更轻松地登录。  
  
-   **管理早期版本**-在 windows server 2019、windows server 2016 和 windows 10 中，可以在 windows server 2012、windows 8、windows Server 2012 R2 和 Windows 8.1 上管理运行 hyper-v 的计算机。  
  
-   **更新的管理协议**-Hyper-v 管理器现在使用 ws-management 协议与远程 hyper-v 主机进行通信，该协议允许 CredSSP、KERBEROS 或 NTLM 身份验证。 使用 CredSSP 连接到远程 Hyper-v 主机时，可以执行实时迁移，而无需在 Active Directory 中启用约束委派。 基于 WS-I 的基础结构还可以更轻松地启用用于远程管理的主机。 WS-MAN 通过端口 80（该端口默认处于打开状态）进行连接。  
  
### <a name="integration-services-delivered-through-windows-update-updated"></a>通过 Windows 更新\(提供的集成服务已更新\) 

针对 Windows 来宾的集成服务更新通过 Windows 更新分配。 对于服务提供商和私有云托管商，这会将应用更新的控制权置于拥有虚拟机的租户手中。 租户现在可以通过单个方法为其 Windows 虚拟机更新所有更新（包括集成服务）。 有关适用于 Linux 来宾的 integration services 的详细信息，请参阅[hyper-v 上的 linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。  
  
> [!IMPORTANT]  
> 不再需要 vmguest.iso 映像文件，因此它不会包含在 Windows Server 2016 上的 Hyper-v 中。  
  
### <a name="linux-secure-boot-new"></a>Linux 安全启动\(新建\) 

在第 2 代虚拟机上运行的 Linux 操作系统现可以使用支持的“安全启动”选项进行启动。 Ubuntu 14.04 及更高版本、SUSE Linux Enterprise Server 12 及更高版本、Red Hat Enterprise Linux 7.0 及更高版本以及 CentOS 7.0 及更高版本可在运行 Windows Server 2016 的主机上启用安全启动。 首次启动虚拟机之前，必须将虚拟机配置为使用 Microsoft UEFI 证书颁发机构。 可以从 Hyper-V 管理器、Virtual Machine Manager 或提升的 Windows Powershell 会话执行此操作。 对于 Windows PowerShell，运行此命令：  
  
```  
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority  
```  
  
有关 Hyper-v 上的 Linux 虚拟机的详细信息，请参阅[hyper-v 上的 linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。 有关 cmdlet 的详细信息，请参阅[set-vmfirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware)。

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>第2代虚拟机和 hyper-v 主机\(的内存和处理器更新\)

从版本8开始，第2代虚拟机可以使用更多的内存和虚拟处理器。 与以前支持的内存和虚拟处理器相比，还可以配置主机。 这些更改支持新方案，例如，为联机事务处理（OLTP）和数据仓库（DW）运行电子商务大型内存中数据库。 Windows Server 博客最近发布了一个虚拟机的性能结果，其中包含 5.5 tb 的内存和128个运行 4 TB 内存中数据库的虚拟处理器。 性能大于物理服务器的 95%。 有关详细信息，请参阅[Windows Server 2016 hyper-v 大规模 VM 性能，用于内存中事务处理](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 有关虚拟机版本的详细信息，请参阅在[windows 10 或 Windows Server 2016 上的 hyper-v 中升级虚拟机版本](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 有关受支持的最大配置的完整列表，请参阅[规划 Windows Server 2016 中的 hyper-v 可伸缩性](./plan/plan-hyper-v-scalability-in-windows-server.md)。 

### <a name="nested-virtualization-new"></a>嵌套虚拟\(化新\)

此功能允许你将虚拟机用作 Hyper-V 主机并在该虚拟化的主机中创建虚拟机。 这对于开发和测试环境尤其有用。 若要使用嵌套虚拟化，你需要：  
  
-   若要在物理 Hyper-v 主机和虚拟化主机上至少运行 Windows Server 2019、Windows Server 2016 或 Windows 10。  
  
-   具有 Intel VT-x 的处理器（嵌套虚拟化目前仅适用于 Intel 处理器）。  
  
有关详细信息和说明，请参阅[使用嵌套虚拟化在虚拟机中运行 hyper-v](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)。  
  
### <a name="networking-features-new"></a>新增网络\(功能\)

新网络功能包括：  
  
-   **远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)** 。 无论是否同时使用 SET，都可以在绑定到 Hyper-V 虚拟交换机的网络适配器上设置 RDMA。 SET 提供与 NIC 组合具有某些相同功能的虚拟交换机。 有关详细信息，请参阅[远程直接内存访问（RDMA）和交换机嵌入式组合（SET）](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。  
  
-   **虚拟机多队列 (VMMQ)** 。 通过为每台虚拟机分配多个硬件队列提升 VMQ 吞吐量。  对于一台虚拟机，默认的一个队列变为一组队列，而流量在队列之间传播。  
  
-   **软件定义网络的服务质量 (QoS)** 。 通过默认类带宽内的虚拟交换机管理流量的默认类。  
  
有关新网络功能的详细信息，请参阅[网络中的新增](../../networking/What-s-New-in-Networking.md)功能。  
  
### <a name="production-checkpoints-new"></a>生产检查\(点新\)

生产检查点是虚拟机的 "时间点" 映像。 当虚拟机运行生产工作负荷时，这些应用程序会提供一种方法来应用符合支持策略的检查点。 生产检查点基于来宾内的备份技术，而不是已保存状态。 对于 Windows 虚拟机，使用卷快照服务 (VSS)。 对于 Linux 虚拟机，会刷新文件系统缓冲区，以创建与文件系统一致的检查点。 如果要使用基于已保存状态的检查点，请改为选择标准检查点。 有关详细信息，请参阅[在 hyper-v 中选择标准或生产检查点](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)。  
  
> [!IMPORTANT]  
> 新的虚拟机将默认使用生产检查点。  
  
### <a name="rolling-hyper-v-cluster-upgrade-new"></a>正在滚动 hyper-v 群集升级\(新\)

现在可以将运行 Windows Server 2019 或 Windows Server 2016 的节点添加到运行 Windows Server 2012 R2 节点的 Hyper-v 群集。 这样就可以在不停机的情况下升级群集。 群集在 Windows Server 2012 R2 功能级别运行，直到升级群集中的所有节点并使用 Windows PowerShell cmdlet [update-clusterfunctionallevel](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel)更新群集功能级别。  
  
> [!IMPORTANT]  
> 更新群集功能级别后，无法将其返回到 Windows Server 2012 R2。  
  
对于运行 Windows Server 2012 R2、Windows Server 2019 和 Windows Server 2016 的节点功能级别为 Windows Server 2012 R2 的 Hyper-v 群集，请注意以下事项：  
  
-   从运行 Windows Server 2016 或 Windows 10 的节点管理群集、Hyper-v 和虚拟机。  
  
-   你可以在 Hyper-v 群集中的所有节点之间移动虚拟机。  
  
-   若要使用新的 Hyper-v 功能，所有节点都必须运行 Windows Server 2016 或并且必须更新群集功能级别。  
  
-   尚未升级现有虚拟机的虚拟机配置版本。 只能在升级群集功能级别后升级配置版本。  
  
-   你创建的虚拟机与 Windows Server 2012 R2 （虚拟机配置级别5）兼容。  
  
更新群集功能级别后：  
  
-   可以启用新的 Hyper-v 功能。  
  
-   若要使新的虚拟机功能可用，请使用 VmConfigurationVersion cmdlet 手动更新虚拟机配置级别。 有关说明，请参阅[升级虚拟机版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。    
-   不能将节点添加到运行 Windows Server 2012 R2 的 Hyper-V 群集。  
  
> [!NOTE]  
> Windows 10 上的 hyper-v 不支持故障转移群集。  
  
有关详细信息和说明，请参阅[群集操作系统滚动升级](https://technet.microsoft.com/library/dn850430.aspx)。  

### <a name="shared-virtual-hard-disks-updated"></a>\(已更新共享的虚拟硬盘\)
现在，可以调整用于来宾群集的共享虚拟硬盘（.vhdx 文件）的大小，而无需停机。 当虚拟机处于联机状态时，可以扩大或收缩共享的虚拟硬盘。 来宾群集现在还可以通过使用 Hyper-v 副本进行灾难恢复来保护共享的虚拟硬盘。

启用集合上的复制。 对集合启用复制**仅通过 WMI 接口公开**。 有关更多详细信息，请参阅[Msvm_CollectionReplicationService 类](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx)的文档。 **不能通过 PowerShell cmdlet 或 UI 管理集合的复制。** Vm 应位于作为 Hyper-v 群集一部分的主机上，以访问特定于集合的功能。 这包括在独立主机上共享 VHD 共享 vhd 不受 Hyper-v 副本支持。

遵循[虚拟硬盘共享](https://technet.microsoft.com/library/dn281956.aspx)中共享的 Vhd 指南概述，并确保共享 vhd 是来宾群集的一部分。 

具有共享 VHD 但没有关联来宾群集的集合不能创建集合的引用点（无论是否在创建引用点时都包含共享的 VHD）。 

### <a name="virtual-machine-backupnew"></a>新虚拟机\(备份\)

如果要备份单个虚拟机（无论主机是否群集），则不应使用 VM 组。  也不应使用快照集合。 VM 组和快照集合仅用于备份使用共享 vhdx 的来宾群集。 相反，你应该使用[HYPER-V WMI v2 提供程序](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx)拍摄快照。 同样，不要使用[故障转移群集 WMI 提供程序](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx)。

### <a name="shielded-virtual-machines-new"></a>新的防护\(虚拟机\)

受防护的虚拟机使用多种功能，使主机上的 Hyper-v 管理员和恶意软件难以检查、篡改或从受防护的虚拟机的状态中盗取数据。 数据和状态是加密的，Hyper-v 管理员看不到视频输出和磁盘，只能在主机保护者服务器确定的已知的正常主机上运行虚拟机限制。 有关详细信息，请参阅[受保护的构造和受防护的 vm](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)。
  
> [!NOTE]  
> 受防护的虚拟机与 Hyper-v 副本兼容。 若要复制受防护的虚拟机，您要复制到的主机必须有权运行受防护的虚拟机。  

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>群集虚拟机\(的启动顺序优先级新建\)

此功能使你可以更好地控制首先启动或重新启动哪些群集虚拟机。 这样就可以更轻松地启动虚拟机，这些虚拟机在使用这些服务的虚拟机之前提供服务。 定义集，将虚拟机放置在集中，并指定依赖项。 使用 Windows PowerShell cmdlet 管理这些集，如[ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset)、 [ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset)和[ClusterGroupSetDependency](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency)。
.  
### <a name="storage-quality-of-service-qos-updated"></a>已更新存储服务质量（QoS \(）\)

你现在可以在横向扩展文件服务器上创建存储 QoS 策略，并将它们分配给 Hyper-V 虚拟机上的一个或多个虚拟磁盘。 存储性能将随存储负载波动自动重新调整以符合策略。 有关详细信息，请参阅[存储服务质量](../../storage/storage-qos/storage-qos-overview.md)。  
  
### <a name="virtual-machine-configuration-file-format-updated"></a>已更新虚拟机配置\(文件格式\)

虚拟机配置文件使用新的格式，使读取和写入配置数据的效率更高。 如果出现存储故障，格式也会使数据损坏的可能性变小。 虚拟机配置数据文件使用 .vmcx 文件扩展名和运行时状态数据文件，并使用 .vmrs 文件扩展名。  
  
> [!IMPORTANT]  
> .Vmcx 文件扩展名指示二进制文件。 不支持编辑. .vmcx 或 .vmrs 文件。  
  
### <a name="virtual-machine-configuration-version-updated"></a>已更新虚拟机\(配置版本\)

版本表示虚拟机的配置、已保存状态和快照文件与 Hyper-v 版本的兼容性。 版本为5的虚拟机与 Windows Server 2012 R2 兼容，并且可以在 Windows Server 2012 R2 和 Windows Server 2016 上运行。 Windows server 2016 和 Windows Server 2019 中引入的版本的虚拟机不会在 Windows Server 2012 R2 上的 Hyper-v 中运行。   
  
如果将虚拟机移动或导入到在 Windows server 2016 上运行 Hyper-v 的服务器或 windows server 2012 R2 上的 Windows Server 2019，则不会自动更新虚拟机的配置。 这意味着可以将虚拟机移动回运行 Windows Server 2012 R2 的服务器。 但这也意味着在你手动更新虚拟机配置的版本前，不能使用新的虚拟机功能。  
  
有关检查和升级版本的说明，请参阅[升级虚拟机版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 本文还列出了其中引入了一些功能的版本。   
  
> [!IMPORTANT]  
> -   更新版本后，不能将虚拟机移动到运行 Windows Server 2012 R2 的服务器。  
> -   不能将配置降级到以前的版本。  
> -   当群集功能级别为 Windows Server 2012 R2 时，Hyper-v 群集上的[VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet 将被阻止。  

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>第2代虚拟机\(的基于虚拟化的安全性新增）

基于虚拟化的安全功能（如 Device Guard 和 Credential Guard）提供对操作系统的增强保护，防止恶意软件攻击。 基于虚拟化的安全在第2代来宾虚拟机中提供，从版本8开始。 有关虚拟机版本的信息，请参阅在[windows 10 或 Windows Server 2016 上的 hyper-v 中升级虚拟机版本](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。

### <a name="windows-containers-new"></a>Windows 容器\(新增\)

Windows 容器允许许多独立的应用程序在一台计算机系统上运行。 它们速度非常快，并且具有高度可伸缩性和可移植性。 提供两种类型的容器运行时，每个都有不同程度的应用程序隔离。 Windows Server 容器使用命名空间和进程隔离。 Hyper-v 容器将轻型虚拟机用于每个容器。  
  
关键功能包括：  
  
-   支持使用 HTTPS 的网站和应用程序  
  
-   Nano server 可托管 Windows Server 和 Hyper-v 容器  
  
-   通过容器共享文件夹管理数据的能力  
  
-   能够限制容器资源  
  
有关详细信息，包括快速入门指南，请参阅[Windows 容器文档](https://docs.microsoft.com/virtualization/windowscontainers/index)。  
  
### <a name="windows-powershell-direct-new"></a>Windows PowerShell 直接\(新建\)

这为你提供了一种从主机在虚拟机中运行 Windows PowerShell 命令的方法。 Windows PowerShell Direct 在主机和虚拟机之间运行。 这意味着不需要网络或防火墙要求，无论远程管理配置如何，它都可以正常工作。  
  
Windows PowerShell Direct 是使用 Hyper-v 管理员连接到 Hyper-v 主机上的虚拟机所用的现有工具的一种替代方法：  
  
-   远程管理工具，例如 PowerShell 或远程桌面  
  
-   Hyper-V 虚拟机连接 (VMConnect)  
  
这些工具工作良好，但需要权衡：VMConnect 可靠，但可能难以自动执行。 远程 PowerShell 功能强大，但可能难以设置和维护。 随着 Hyper-v 部署的增长，这些折衷可能会变得更加重要。 Windows PowerShell Direct 通过提供功能强大的脚本和自动化体验来解决这种情况，这与使用 VMConnect 一样简单。
  
有关要求和说明，请参阅[通过 PowerShell Direct 管理 Windows 虚拟机](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)。  

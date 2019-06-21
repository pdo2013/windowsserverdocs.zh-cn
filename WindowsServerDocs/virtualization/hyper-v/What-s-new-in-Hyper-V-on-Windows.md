---
title: 什么是 Windows Server 2016 上的 HYPER-V 中的新增功能
description: 在 HYPER-V 中提供的新功能的摘要
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: KBDAzure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: 6ec5db82ecae2fb74731f3c52b9113325837a2fb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280009"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>什么是 Windows Server 上的 HYPER-V 中的新增功能

>适用于：Windows Server 2019，Microsoft 的 HYPER-V Server 2016 中，Windows Server 2016
  
本文介绍 Windows Server 2019、 Windows Server 2016 和 Microsoft HYPER-V Server 2016 上的 HYPER-V 的新功能和更改功能。 若要使用 Windows Server 2012 R2 创建和移动或导入到运行 Windows Server 2019 的 HYPER-V 的服务器或 Windows Server 2016 的虚拟机上使用新功能，您将需要手动升级虚拟机配置版本。 有关说明，请参阅[升级虚拟机版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。  
  
下面是本文中包含的内容和功能是新的或更新。  

## <a name="windows-server-version-1903"></a>Windows Server 版本 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>将 Hyper-v 管理器添加到服务器核心安装 （更新）

您可能知道，我们建议使用 Windows Server 中，在生产环境中的半年频道时使用的服务器核心安装选项。 但是，默认情况下的服务器核心中省略了许多有用的管理工具。 您可以添加许多最常用工具的安装应用程序兼容性功能，但仍已一些缺少的工具。

因此，根据客户反馈，我们添加一个更多工具到此版本中的应用程序兼容性功能：HYPER-V 管理器 (virtmgmt.msc)。

有关详细信息，请参阅[服务器核心应用程序兼容性功能](../../get-started-19/install-fod-19.md)。

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>安全性：受防护的虚拟机改进 （新）

- **分支机构改进**

    您现在可以通过利用新的[回退 HGS](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) 和[脱机模式](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode)功能，利用主机保护者服务的间歇性连接在计算机上运行受防护的虚拟机。 回退 HGS 允许你配置第二组用于 Hyper-V 的 URL，以尝试是否无法访问你的主 HGS 服务器。

    即使不能访问 HGS，脱机模式也允许你继续启动受防护的虚拟机，只要虚拟机成功启动一次并且主机的安全配置并未更改即可。

- **疑难解答改进功能**

    我们还通过启用对 VMConnect 增强会话模式和 PowerShell Direct 的支持简化了[受防护虚拟机的故障排除](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms)。 这些工具尤其适用于到 VM 的网络连接已断开，需要更新其配置才能恢复访问的情况。

    这些功能不需要进行配置，当受防护的 VM 置于运行 Windows Server 版本 1803 或更高版本的 Hyper-V 主机上时，这些功能会自动变为可用状态。

- **Linux 支持**

    如果你运行混合操作系统环境，Windows Server 2019 现在支持在受防护的虚拟机内运行 Ubuntu、Red Hat Enterprise Linux 和 SUSE Linux Enterprise Server。

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>与连接待机兼容\(新\)

在使用始终可用/始终连接 (AOAC) 电源模型的计算机上安装 HYPER-V 角色时**连接待机**电源状态现已推出。  
  
### <a name="discrete-device-assignment-new"></a>离散设备分配\(新\)

此功能允许你授予虚拟机对某些 PCIe 硬件设备的直接和独占访问权限。 以这种方式使用设备将绕过 Hyper-V 虚拟化堆栈，使访问更迅速。 支持的硬件的详细信息，请参阅"离散设备分配"中[System requirements for Windows Server 2016 上的 HYPER-V 要求](System-requirements-for-Hyper-V-on-Windows.md)。 有关详细信息，包括如何使用此功能和注意事项，请参阅文章"[离散设备分配-说明和背景](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)"虚拟化博客中。

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>在第 1 代虚拟机中的操作系统磁盘的加密支持\(新)

你现在可以保护在第 1 代虚拟机中使用 BitLocker 驱动器加密的操作系统磁盘。 一项新功能，密钥存储中，创建一个小型的专用的驱动器来存储系统驱动器的 BitLocker 密钥。 这是而不是使用虚拟受信任的平台模块 (TPM)，这仅在第 2 代虚拟机中可用。 若要解密该磁盘并启动虚拟机，HYPER-V 主机必须是授权受保护的构造的一部分或从一个虚拟机的 guardians 具有私钥。 密钥存储需要版本 8 虚拟机。 有关虚拟机版本的信息，请参阅[在 Windows 10 或 Windows Server 2016 上的 HYPER-V 中的升级虚拟机版本](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。  
  
### <a name="host-resource-protection-new"></a>主机资源保护\(新\)

此功能可帮助阻止虚拟机使用多个其共享系统资源寻找过多级别的活动的。 这可以帮助防止虚拟机的活动过多会降低主机或其他虚拟机的性能。 当监视检测与活动过多的虚拟机时，虚拟机提供较少的资源。 这种监视和强制请默认情况下处于关闭状态。 使用 Windows PowerShell，若要将其打开或关闭。 若要将其打开，请运行以下命令：  
  
```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

有关此 cmdlet 的详细信息，请参阅[Set-vmprocessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor)。

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>热添加和删除网络适配器和内存的\(新\)

现在，运行虚拟机期间，可以添加或删除网络适配器而不出现停机。 这适用于运行 Windows 或 Linux 操作系统的第 2 代虚拟机。  
  
此外可以调整分配给虚拟机在运行时，即使尚未启用动态内存的内存量。 这适用于第 1 代和第 2 代虚拟机，运行 Windows Server 2016 或 Windows 10。  

### <a name="hyper-v-manager-improvements-updated"></a>Hyper-v 管理器改进\(更新\) 
  
-   **备用凭据支持**-你现在可以使用一组不同的凭据中的 HYPER-V 管理器连接到另一台 Windows Server 2016 或 Windows 10 的远程主机时。 此外可以保存这些凭据以使其更轻松地重新登录。  
  
-   **管理早期版本**-使用 HYPER-V 管理器在 Windows Server 2019、 Windows Server 2016 和 Windows 10 中，你可以管理 Windows Server 2012 中，Windows 8、 Windows Server 2012 R2 和 Windows 8.1 上运行 HYPER-V 的计算机。  
  
-   **更新的管理协议**-Hyper-v 管理器现在与使用 WS-MAN 协议允许 CredSSP、 Kerberos 或 NTLM 身份验证的远程 HYPER-V 主机进行通信。 当使用 CredSSP 连接到远程 HYPER-V 主机时，不会实时迁移启用 Active Directory 中的约束的委派。 WS MAN 基于基础结构还可以更轻松地启用远程管理的主机。 WS-MAN 通过端口 80（该端口默认处于打开状态）进行连接。  
  
### <a name="integration-services-delivered-through-windows-update-updated"></a>通过 Windows 更新传递的集成服务\(更新\) 

针对 Windows 来宾的集成服务更新通过 Windows 更新分配。 对于服务提供商和私有云托管商，这会将应用更新的控制权置于拥有虚拟机的租户手中。 租户现在可以通过单个方法为其 Windows 虚拟机更新所有更新（包括集成服务）。 有关适用于 Linux 来宾集成服务的详细信息，请参阅[Linux 和 FreeBSD 虚拟机上的 HYPER-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。  
  
> [!IMPORTANT]  
> 不再需要 vmguest.iso 图像文件，因此它不包含在 Windows Server 2016 上的 HYPER-V。  
  
### <a name="linux-secure-boot-new"></a>Linux 安全启动\(新\) 

在第 2 代虚拟机上运行的 Linux 操作系统现可以使用支持的“安全启动”选项进行启动。 Ubuntu 14.04 及更高版本、 SUSE Linux Enterprise Server 12 和更高版本、 Red Hat Enterprise Linux 7.0 及更高版本和 CentOS 7.0 及更高版本的运行 Windows Server 2016 主机上启用安全启动的。 首次启动虚拟机之前，必须将虚拟机配置为使用 Microsoft UEFI 证书颁发机构。 可以从 Hyper-V 管理器、Virtual Machine Manager 或提升的 Windows Powershell 会话执行此操作。 对于 Windows PowerShell，运行此命令：  
  
```  
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority  
```  
  
有关 HYPER-V 上的 Linux 虚拟机的详细信息，请参阅[Linux 和 FreeBSD 虚拟机上的 HYPER-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。 有关该 cmdlet 的详细信息，请参阅[Set-vmfirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware)。

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>更多内存和处理器，用于第 2 代虚拟机和 HYPER-V 主机\(更新\)

从版本 8 开始，第 2 代虚拟机可以使用更多内存和虚拟处理器。 主机还可以配置具有显著更多内存和虚拟处理器不是以前支持。 这些更改支持新方案，例如运行用于联机事务处理 (OLTP) 和数据仓库 (DW) 的大型内存中数据库的电子商务。 Windows Server 博客最近发布的虚拟机的 5.5 万亿字节的内存和 128 个虚拟处理器运行 4 TB 内存中数据库的性能结果。 95%的物理服务器的性能。 有关详细信息，请参阅[内存中事务处理的 Windows Server 2016 HYPER-V 大规模 VM 性能](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 有关虚拟机版本的详细信息，请参阅[在 Windows 10 或 Windows Server 2016 上的 HYPER-V 中的升级虚拟机版本](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 有关支持的最大配置的完整列表，请参阅[规划 Windows Server 2016 中的 HYPER-V 可伸缩性](./plan/plan-hyper-v-scalability-in-windows-server.md)。 

### <a name="nested-virtualization-new"></a>嵌套虚拟化\(新\)

此功能允许你将虚拟机用作 Hyper-V 主机并在该虚拟化的主机中创建虚拟机。 这对于开发和测试环境尤其有用。 若要使用嵌套虚拟化，你需要：  
  
-   若要在至少运行 Windows Server 2019、 Windows Server 2016 或 Windows 10 在物理 HYPER-V 主机和虚拟化的主机上。  
  
-   具有 Intel VT-x 的处理器（嵌套虚拟化目前仅适用于 Intel 处理器）。  
  
有关详细信息和说明，请参阅[运行的 HYPER-V 中具有嵌套虚拟化的虚拟机](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)。  
  
### <a name="networking-features-new"></a>网络功能\(新\)

新网络功能包括：  
  
-   **远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)** 。 无论是否同时使用 SET，都可以在绑定到 Hyper-V 虚拟交换机的网络适配器上设置 RDMA。 SET 提供与 NIC 组合具有某些相同功能的虚拟交换机。 有关详细信息，请参阅[远程直接内存访问 (RDMA) 和交换机嵌入式组合 (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。  
  
-   **虚拟机多队列 (VMMQ)** 。 通过为每台虚拟机分配多个硬件队列提升 VMQ 吞吐量。  对于一台虚拟机，默认的一个队列变为一组队列，而流量在队列之间传播。  
  
-   **软件定义网络的服务质量 (QoS)** 。 通过默认类带宽内的虚拟交换机管理流量的默认类。  
  
有关新网络功能的详细信息，请参阅[什么是网络中的新增功能](../../networking/What-s-New-in-Networking.md)。  
  
### <a name="production-checkpoints-new"></a>生产检查点\(新\)

生产检查点是虚拟机的"时间点"映像。 这些报告提供应用检查点，当虚拟机运行生产工作负荷时，遵循支持策略的方法。 生产检查点基于内而不是已保存状态来宾的备份技术。 对于 Windows 虚拟机，使用卷快照服务 (VSS)。 对于 Linux 虚拟机，文件系统缓冲区刷新到创建检查点与文件系统一致。 如果您更愿意使用基于已保存状态的检查点，请改为选择标准检查点。 有关详细信息，请参阅[中的 HYPER-V 的标准或生产检查点之间进行选择](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)。  
  
> [!IMPORTANT]  
> 新的虚拟机将默认使用生产检查点。  
  
### <a name="rolling-hyper-v-cluster-upgrade-new"></a>HYPER-V 群集滚动升级\(新\)

现在可以添加运行 Windows Server 2019 或 Windows Server 2016 的 HYPER-V 群集包含运行 Windows Server 2012 R2 节点的节点。 这使您无需停机即可群集升级。 群集运行在 Windows Server 2012 R2 功能级别之前升级群集中的所有节点并使用 Windows PowerShell cmdlet 更新群集功能级别, [Update-clusterfunctionallevel](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel)。  
  
> [!IMPORTANT]  
> 更新群集功能级别后，您不能返回到 Windows Server 2012 R2。  
  
对于具有与运行 Windows Server 2012 R2、 Windows Server 2019 和 Windows Server 2016 的节点的功能级别的 Windows Server 2012 R2 的 HYPER-V 群集，请注意以下问题：  
  
-   管理群集、 HYPER-V 和虚拟机从运行 Windows Server 2016 或 Windows 10 的节点。  
  
-   所有的 HYPER-V 群集中的节点之间，可以移动虚拟机。  
  
-   若要使用新的 HYPER-V 功能，所有节点必须都运行 Windows Server 2016 或，必须更新群集功能级别。  
  
-   不升级现有的虚拟机的虚拟机配置版本。 仅在升级群集功能级别后，可以升级配置版本。  
  
-   您创建的虚拟机都与 Windows Server 2012 R2 虚拟机配置级别 5 兼容。  
  
后更新群集功能级别：  
  
-   可以启用新的 HYPER-V 功能。  
  
-   若要使新的虚拟机功能，使用 Update-vmconfigurationversion cmdlet 手动更新虚拟机配置级别。 有关说明，请参阅[升级虚拟机版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。    
-   不能将节点添加到运行 Windows Server 2012 R2 的 Hyper-V 群集。  
  
> [!NOTE]  
> Windows 10 上的 HYPER-V 不支持故障转移群集。  
  
有关详细信息和说明，请参阅[群集操作系统滚动升级](https://technet.microsoft.com/library/dn850430.aspx)。  

### <a name="shared-virtual-hard-disks-updated"></a>共享虚拟硬盘\(更新\)
您还可以调整共享虚拟硬盘 （.vhdx 文件） 用于来宾群集，而无需停机。 共享虚拟硬盘可以增长或收缩虚拟机处于联机状态时。 来宾群集现在还可以保护共享虚拟硬盘通过使用 HYPER-V 副本进行灾难恢复。

集合上启用复制。 集合上启用复制是**通过 WMI 接口仅公开**。 请参阅的文档[Msvm_CollectionReplicationService 类](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx)的更多详细信息。 **不能管理复制通过 UI 或 PowerShell cmdlet 的集合。** Vm 应属于的 HYPER-V 群集，以访问特定于集合的功能的主机上。 这包括共享 VHD-在独立主机上的共享的 Vhd 不受 HYPER-V 副本。

对于共享 Vhd 遵循的准则[Virtual Hard Disk Sharing Overview](https://technet.microsoft.com/library/dn281956.aspx)，并确保共享的 Vhd 是来宾群集的一部分。 

具有共享的 VHD，但没有关联的来宾群集的集合不能创建引用点 （而不考虑共享的 VHD 中是否包含引用点创建与否） 的集合。 

### <a name="virtual-machine-backupnew"></a>虚拟机备份\(新\)

如果要备份单个虚拟机 （无论是否已群集主机），则不应使用 VM 组。  也不应使用快照集合。 VM 组和快照集合是为了仅用于备份正在使用共享的 vhdx 来宾群集。 相反，应执行快照使用[HYPER-V WMI v2 提供程序](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx)。 同样，不要使用[故障转移群集 WMI 提供程序](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx)。

### <a name="shielded-virtual-machines-new"></a>受防护的虚拟机\(新\)

受防护的虚拟机使用的多种功能，以使其更难 HYPER-V 管理员和在主机上的恶意软件检查、 篡改或窃取数据从受防护的虚拟机的状态。 加密数据和状态后，HYPER-V 管理员无法看到视频输出和磁盘，并且虚拟机可以进行限制，从而仅在由主机保护者服务器已知的正常运行的主机上运行。 有关详细信息，请参阅[受保护的构造和受防护的 Vm](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)。
  
> [!NOTE]  
> 受防护的虚拟机都与 HYPER-V 副本兼容。 若要复制的受防护的虚拟机，你想要复制到的主机必须有权运行该受防护的虚拟机。  

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>启动订单为群集虚拟机的优先级\(新\)

此功能可以更好地控制对其启动或首次重新启动群集的虚拟机。 这使得更轻松地开始提供服务之前使用这些服务的虚拟机的虚拟机。 定义集、 将虚拟机放置在集中，并指定依赖项。 使用 Windows PowerShell cmdlet 来管理设置，例如[新建 ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset)， [Get ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset)，并[添加 ClusterGroupSetDependency](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency)。
.  
### <a name="storage-quality-of-service-qos-updated"></a>存储服务质量 (QoS)\(更新\)

你现在可以在横向扩展文件服务器上创建存储 QoS 策略，并将它们分配给 Hyper-V 虚拟机上的一个或多个虚拟磁盘。 存储性能将随存储负载波动自动重新调整以符合策略。 有关详细信息，请参阅[存储服务质量](../../storage/storage-qos/storage-qos-overview.md)。  
  
### <a name="virtual-machine-configuration-file-format-updated"></a>虚拟机配置文件格式\(更新\)

虚拟机配置文件使用新的格式，使读取和写入配置数据更高效。 格式还使数据损坏不太可能在存储失败时。 虚拟机配置数据文件使用.vmcx 文件扩展名和运行时状态数据文件使用.vmrs 文件扩展名。  
  
> [!IMPORTANT]  
> .Vmcx 文件扩展名表示二进制文件。 不支持编辑.vmcx 或.vmrs 文件。  
  
### <a name="virtual-machine-configuration-version-updated"></a>虚拟机配置版本\(更新\)

配置版本表示虚拟机的配置，使用的版本的 HYPER-V 保存状态和快照文件的兼容性。 版本 5 的虚拟机与 Windows Server 2012 R2 兼容，并可以在 Windows Server 2012 R2 和 Windows Server 2016 上运行。 与版本在 Windows Server 2016 和 Windows Server 2019 中引入的虚拟机不会在 HYPER-V 中运行 Windows Server 2012 R2 上。   
  
如果移动或导入虚拟机运行 Windows Server 2016 上的 HYPER-V 的服务器或 Windows Server 2019 从 Windows Server 2012 R2，不会自动更新虚拟机的配置。 这意味着可以将虚拟机移动回运行 Windows Server 2012 R2 的服务器。 但这也意味着在你手动更新虚拟机配置的版本前，不能使用新的虚拟机功能。  
  
检查和升级版本的说明，请参阅[升级虚拟机版本](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 这篇文章还列出了一些功能引入的版本。   
  
> [!IMPORTANT]  
> -   更新版本后，您不能将虚拟机移动到运行 Windows Server 2012 R2 的服务器。  
> -   不能降级到以前的版本的配置。  
> -   [Update-vmversion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet 时，阻止在 HYPER-V 群集上群集功能级别为 Windows Server 2012 R2。  

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>第 2 代虚拟机 Virtualization-based 安全\(新)

基于虚拟化的安全提供支持功能，例如 Device Guard 和 Credential Guard，产品/服务的恶意软件防御攻击的操作系统的增强的保护。 从版本 8 开始生成 2 个来宾虚拟机中提供了基于虚拟化的安全性。 有关虚拟机版本的信息，请参阅[在 Windows 10 或 Windows Server 2016 上的 HYPER-V 中的升级虚拟机版本](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)。

### <a name="windows-containers-new"></a>Windows 容器\(新\)

Windows 容器允许多个独立的应用程序在一个计算机系统上运行。 它们将随时快速构建和高度可缩放且可移植。 有两种类型的容器运行时，每个都具有不同程度的应用程序隔离。 Windows Server 容器使用命名空间和进程隔离。 HYPER-V 容器使用为每个容器的轻型虚拟机。  
  
关键功能包括：  
  
-   对网站和应用程序使用 HTTPS 的支持  
  
-   Nano 服务器可以承载 Windows Server 和 HYPER-V 容器  
  
-   能够通过容器共享文件夹管理数据  
  
-   限制容器资源的功能  
  
有关详细信息，包括快速入门指南，请参阅[Windows 容器文档](https://docs.microsoft.com/virtualization/windowscontainers/index)。  
  
### <a name="windows-powershell-direct-new"></a>Windows PowerShell 直接\(新\)

这样，您从主机的虚拟机中运行 Windows PowerShell 命令的方式。 Windows PowerShell 直接运行在主机和虚拟机之间。 这意味着它不需要网络或防火墙要求以及工作而不考虑远程管理配置。  
  
Windows PowerShell 直接是 HYPER-V 管理员使用连接到虚拟机的 HYPER-V 主机上的现有工具的替代方法：  
  
-   远程管理工具，例如 PowerShell 或远程桌面  
  
-   Hyper-V 虚拟机连接 (VMConnect)  
  
这些工具很好，但需要进行权衡：VMConnect 可靠，但可能难以自动执行。 远程 PowerShell 功能强大，但可能难以设置和维护。 这些取舍之中可能变得更加重要，随着 HYPER-V 部署。 Windows PowerShell 直接解决此问题通过提供功能强大的脚本编写和自动化体验，就像使用 VMConnect 一样简单。
  
有关要求和说明，请参阅[使用 PowerShell Direct 管理 Windows 虚拟机](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)。  

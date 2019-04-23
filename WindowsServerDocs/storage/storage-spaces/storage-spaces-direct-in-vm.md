---
title: 在虚拟机中使用存储空间直通
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: 如何在虚拟机来宾群集-例如，在 Microsoft Azure 中部署存储空间直通。
ms.localizationpriority: medium
ms.openlocfilehash: a741475d09ab7f7ab29f158ef55378ca9a37beac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841018"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>在来宾虚拟机群集中使用存储空间直通

> 适用于：Windows Server 2019、Windows Server 2016

在群集上的物理服务器或虚拟机来宾群集，如本主题中所述，你可以部署存储空间直通。 这种部署跨一组 Vm 上的私有或公有云，以便应用程序高可用性解决方案可用于提高应用程序的可用性，提供了虚拟共享的存储。

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>在 Azure Iaas VM 来宾群集中部署

[Azure 模板](https://github.com/robotechredmond/301-storage-spaces-direct-md)已发布的降低复杂性，配置最佳实践以及你在 Azure Iaas VM 中的存储空间直通部署的速度。 这是用于在 Azure 中部署的推荐的解决方案。

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>要求

在虚拟化环境中部署存储空间直通时，应考虑下列注意事项。

> [!TIP]
> Azure 模板将自动配置下面你和是建议的解决方案在 Azure IaaS Vm 中部署时的注意事项。

-   2 个节点的最小和最大的 3 个节点

-   2 节点部署必须配置见证服务器 （云见证或文件共享见证）

-   3 个节点的部署可以容忍 1 个节点向下的和另一个节点上的一个或多个磁盘丢失。  如果关闭了 2 个节点然后虚拟磁盘我们处于脱机状态，直到其中一个节点返回。  

-   配置要跨容错域部署的虚拟机

    -   Azure-配置可用性集

    -   Hyper-v – 若要在节点上分隔的 Vm 的 Vm 上配置 AntiAffinityClassNames

    -   VMware — 通过创建 DRS 规则类型的配置 VM 虚拟机反相关性规则单独的虚拟机"以在 ESX 主机上分隔的 Vm。 提供用于使用存储空间直通的磁盘应使用 Paravirtual SCSI (PVSCSI) 适配器。 有关 Windows Server PVSCSI 支持，请查阅 https://kb.vmware.com/s/article/1010398。

-   利用低延迟 / 托管的高性能存储-Azure 高级存储磁盘所需

-   使用配置的任何缓存设备部署平面存储设计

-   最小值为 2 个虚拟数据磁盘提供给每个 VM (VHD / VHDX / VMDK)

    此数字是不同的裸机部署，因为虚拟磁盘可作为不容易遭受物理故障的文件。

-   通过运行以下 PowerShell cmdlet 来禁用运行状况服务中的自动驱动器替换功能：

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   不支持：主机级别的虚拟磁盘快照/还原

    而是使用传统的来宾级别备份解决方案来备份和还原存储空间直通的卷上的数据。

-   为复原能力赋予可能 VHD / VHDX / VMDK 来宾群集中的存储延迟增加存储空间 I/O 超时值：

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    十六进制 7530 十进制等效值为 30000，为 30 秒。 请注意，默认值为 1770年十六进制或 6000 （十进制），这是 6 秒。

## <a name="see-also"></a>请参阅

[部署存储空间直通、 视频和分步指南的其他 Azure Iaas VM 模板](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/)。

[其他存储空间直通概述](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)

---
title: 在虚拟机中使用存储空间直通
ms.prod: windows-server
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: 如何在虚拟机来宾群集中部署存储空间直通-例如，在 Microsoft Azure 中。
ms.localizationpriority: medium
ms.openlocfilehash: ab0ce792c5a948e763a48493a78ccdac7a6fe74c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366050"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>在来宾虚拟机群集中使用存储空间直通

> 适用于：Windows Server 2019、Windows Server 2016

可以在物理服务器或虚拟机来宾群集的群集上部署存储空间直通，如本主题中所述。 这种类型的部署在私有或公有云之上跨一组 Vm 提供虚拟共享存储，以便应用程序高可用性解决方案可用于提高应用程序的可用性。

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>在 Azure Iaas VM 来宾群集中部署

已发布[azure 模板](https://github.com/robotechredmond/301-storage-spaces-direct-md)会降低复杂性、配置最佳实践，以及在 AZURE Iaas VM 中部署存储空间直通部署的速度。 这是在 Azure 中部署的推荐解决方案。

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>要求

在虚拟化环境中部署存储空间直通时，请注意以下事项。

> [!TIP]
> Azure 模板会自动为你配置以下注意事项，在 Azure IaaS Vm 中部署时，建议使用此解决方案。

-   最小2个节点，最多3个节点

-   2-节点部署必须配置见证服务器（云见证服务器或文件共享见证服务器）

-   3个节点部署可以容忍1个节点关闭，并在另一个节点上丢失1个或多个磁盘。  如果2个节点关闭，则虚拟磁盘会处于脱机状态，直到其中一个节点返回为止。  

-   配置要在容错域之间部署的虚拟机

    -   Azure –配置可用性集

    -   Hyper-v –在虚拟机上配置 AntiAffinityClassNames 以跨节点分离 Vm

    -   VMware –通过创建类型为 "单独的虚拟机" 的 DRS 规则来配置 VM-VM 抗相关性规则，以便跨 ESX 主机分隔 Vm。 为与存储空间直通一起使用而提供的磁盘应使用半虚拟 SCSI （PVSCSI）适配器。 有关 Windows Server 的 PVSCSI 支持，请参阅 https://kb.vmware.com/s/article/1010398 。

-   利用低延迟/高性能存储-需要 Azure 高级存储托管磁盘

-   在未配置缓存设备的情况之下部署平面存储设计

-   每个 VM 提供的最小2个虚拟数据磁盘（VHD/VHDX/VMDK）

    此数字不同于裸机部署，因为虚拟磁盘可以作为不容易出现物理故障的文件来实现。

-   通过运行以下 PowerShell cmdlet，在运行状况服务中禁用自动驱动器更换功能：

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   不支持：主机级虚拟磁盘快照/还原

    改为使用传统的来宾级别备份解决方案来备份和还原存储空间直通卷上的数据。

-   为了更好地复原来宾群集中可能的 VHD/VHDX/VMDK 存储延迟，请增加存储空间 i/o 超时值：

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    十六进制7530的十进制等效项是30000，即30秒。 请注意，默认值为1770十六进制或 6000 Decimal，即6秒。

## <a name="see-also"></a>请参阅

[用于部署存储空间直通、视频和循序渐进指南的其他 Azure IAAS VM 模板](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126)。

[其他存储空间直通概述](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)

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
ms.sourcegitcommit: 52883945fd6e6bb5c24bd81944ca4bc0c5e6a216
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2018
ms.locfileid: "8964609"
---
# 在来宾虚拟机群集中使用存储空间直通

> 适用于： Windows Server 2019、 Windows Server 2016

你可以部署存储空间直通在群集上的物理服务器或虚拟机来宾群集，如本主题中所述。 这种部署跨顶部的私有或公共云一组的虚拟机，以便应用程序实现高可用性解决方案可用于提高应用程序的可用性，提供了虚拟共享的存储。

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## 在 Azure Iaas 虚拟机来宾群集中部署

[Azure 模板](https://github.com/robotechredmond/301-storage-spaces-direct-md)已发布的降低复杂性、 配置最佳做法和你在 Azure Iaas VM 中的存储空间直通部署的速度。 这是为在 Azure 中部署的推荐的解决方案。

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## 要求

以下注意事项适用的虚拟化环境中部署存储空间直通时。

> [!TIP]
> Azure 模板将自动配置下面你和是推荐的解决方案中 Azure IaaS Vm 部署时的注意事项。

-   2 个节点的最小和最大的 3 个节点

-   2 节点部署必须配置见证 （云见证或文件共享见证）

-   向下的 1 节点和丢失的另一个节点上的一个或多个磁盘可容忍 3 节点部署。  如果 2 个节点都关闭然后虚拟磁盘我们处于脱机状态直到返回其中一个节点。  

-   配置虚拟机将部署到整个容错域

    -   Azure-配置可用性组

    -   Hyper-v – 虚拟机来单独跨节点的 Vm 上配置 AntiAffinityClassNames

    -   VMware – 通过创建类型的 DRS 规则配置 VM 防相关性规则不同的虚拟机"来单独跨 ESX 主机的 Vm。 显示使用存储空间直通的磁盘应使用半虚拟化 SCSI (PVSCSI) 适配器。 对于 Windows server PVSCSI 支持，请参阅https://kb.vmware.com/s/article/1010398。

-   利用低延迟 / 高性能存储-Azure 高级版存储管理磁盘是必需

-   与配置没有缓存设备部署平面的存储设计

-   最小的呈现给每个 VM 的 2 个虚拟数据磁盘 (VHD / VHDX / VMDK)

    此数字是不同于裸机部署，因为虚拟磁盘可以实现为不会遭受物理故障的文件。

-   通过运行以下 PowerShell cmdlet 禁用运行状况服务中的自动驱动器替换功能：

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   不受支持： 主机级别的虚拟磁盘快照/还原

    改为使用传统的来宾级别的备份解决方案来备份和还原存储空间直通卷上的数据。

-   以更好地复原向可能 VHD / VHDX / VMDK 来宾群集中的存储延迟增加存储空间 I/O 超时值：

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    十六进制 7530 的十进制等效项是 30000，这是 30 秒。 请注意，默认值为 1770年十六进制或 6000 （十进制），这是 6 秒。

## 另请参阅

[部署存储空间直通、 视频和分步指南的其他 Azure Iaas 虚拟机模板](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/)。

[其他存储空间直通概述](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)

---
title: 升级到 Windows Server 2016 远程桌面虚拟化主机
description: 本文介绍如何升级现有的远程桌面服务部署到 Windows Server 2016。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: 17bf3a49155d29960684acebea870c0b51f664a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812028"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>升级到 Windows Server 2016 远程桌面虚拟化主机

>适用于：Windows 服务器 （半年频道），Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>已安装 RDS 角色支持 OS 升级
仅从 Windows Server 2012 R2 和 Windows Server 2016 TP5 支持升级到 Windows Server 2016。

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>部署 Vm 本地存储位置中的 RD 虚拟化主机服务器
这些服务器应一次性全部升级。 请执行以下步骤来升级：

1. 注销所有用户。
1. 打开每个主机关闭或保存所有虚拟机。 
1. 升级到 Windows Server 2016 的服务器。 
1. 升级完成后，所有集合都应可用且正常工作。      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>部署虚拟机存储在群集共享卷 (CSV) 的位置中的 RD 虚拟化主机服务器 

1. 确定升级策略将升级某些 RDVH 服务器，其中一些将继续在 Windows Server 2012 R2 上的主机 Vm。  
1. 隔离一个或多针对初始一轮的升级，通过将所有 Vm 都迁移到另一个不用于尚未升级 RDVH 服务器将保留原始的 2012 R2 群集的一部分的 RDVH 服务器。
    1. 打开故障转移群集管理器。 
    1. 单击“角色” 。 
    1. 选择一个或多个 Vm。 右键单击以打开上下文菜单。 
    1. 单击**移动**，然后执行以下**Live**或**快速迁移**将 Vm 移到一个或多个 RD 虚拟化主机服务器不是初始升级的一部分。 使用**Live**或**快速**根据硬件兼容性或联机要求等因素的迁移。 
1. 逐出 RDVH 服务器，准备好进行升级，从原始群集。 
1. 升级独立的 RDVH 服务器。 
1. 已成功升级目标的 RDVH 服务器后，创建新的群集和 CSV，必须是完全不同的 SAN 卷上。
1. 所有联接 RDVH 服务器都升级到新群集。 
1. 在模拟中的现有 CSV 的现有文件夹结构的新 CSV 中创建文件夹结构。 这将包括的集合文件夹以及每个 VM 的顶级子文件夹。 
1. 从原始 CSV 上的各种 VM 集合文件夹，将复制 /IMGS 文件夹及其内容到新的 CSV 上的同一位置中的新集合文件夹。 
1. 源 RDVH 在计算机上，使用群集管理器删除虚拟机的配置以实现高可用性：
    1. 启动群集管理器。 
    1. 单击“角色” 。 
    1. 右键单击 VM 对象，然后依次**删除**。 
1. 一个非升级 RDVH 服务器上，使用 Hyper-v 管理器将所有 Vm 都移动到已升级的 RDVH 服务器和新群集 CSV 之一：
    1. 打开 Hyper-V 管理器。 
    1. 选择一种非升级 RDVH 服务器。 
    1. 右键单击其中一个 Vm 以移动，然后单击**移动**。 
    1. 选择**移动虚拟机**，然后单击**下一步**。 
    1. 提供有关目标已升级的 RDVH 服务器的名称**指定目标计算机**页上，然后依次**下一步**。 
    1. 选择**将虚拟机的数据移到单个位置**，然后单击**下一步**。 
    1. 浏览到目标位置。 
    > [!IMPORTANT]
    > 请确保此路径是到特定虚拟机的空文件夹。 

    > [!NOTE]
    > 如前文所述，需要已创建在此步骤之前新的目标子文件夹。 选择文件夹对话框将允许您在此步骤中创建的子文件夹。 
    
    单击“下一步” ，然后单击“已完成” 。 
1. 一旦 Vm 将重新定位，则将其添加为群集**高可用性**对象：
    1. 打开已升级的 RD 虚拟化主机服务器上的故障转移群集管理器。 
    1. 右键单击**角色**节点，并单击**配置角色**。 单击**下一步**上**启动**高可用性向导的页。 
    1. 选择**虚拟机**从列表中的可用角色，然后单击**下一步**。 将显示一组未配置的 Vm。 
    1. 选择所有的 Vm。 单击**下一步**，然后单击**下一步**再次在确认页后，可以开始配置任务。  
1. 已重新定位的所有 Vm，一旦升级剩余 RDVH 服务器。 用于平衡 VM 位置根据需要使用上面的步骤。

> [!NOTE]  
> 不支持异类群集中的 HYPER-V 服务器。 

---
title: 将远程桌面虚拟化主机升级到 Windows Server 2016
description: 本文介绍如何将现有的远程桌面服务部署升级到 Windows Server 2016。
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
ms.openlocfilehash: a58231d908ff1ac32eca7d4ba3f1d5a6a18dd7fe
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870566"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>将远程桌面虚拟化主机升级到 Windows Server 2016

>适用于：Windows Server（半年频道）、Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>支持的装有 RDS 角色的 OS 升级
仅支持从 Windows Server 2012 R2 和 Windows Server 2016 TP5 升级到 Windows Server 2016。

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>在本地存储 VM 的部署中的 RD 虚拟化主机服务器
应该一次性升级所有这些服务器。 执行以下步骤进行升级：

1. 将所有用户注销。
1. 关闭或保存每台主机上的所有虚拟机。 
1. 将服务器升级到 Windows Server 2016。 
1. 升级完成后，所有集合应该可用且正常运行。      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>在群集共享卷 (CSV) 中存储 VM 的部署中的 RD 虚拟化主机服务器 

1. 确定升级策略，其中的有些 RDVH 服务器将会升级，而有些服务器将继续在 Windows Server 2012 R2 上托管 VM。  
2. 通过将所有 VM 迁移到其他“暂时不会升级”的、保留在原始 2012 R2 群集中的 RDVH 服务器，隔离出要完成首轮升级的一个或多个 RDVH 服务器。
    1. 打开故障转移群集管理器。 
    1. 单击“角色”  。 
    1. 选择一个或多个 VM。 单击右键打开上下文菜单。 
    1. 单击“移动”，然后选择“实时”或“快速迁移”，以将 VM 移到一个或多个不需要完成初始升级的 RD 虚拟化主机服务器。    根据硬件兼容性或联机要求等因素使用“实时”或“快速迁移”。   
3. 从原始群集中逐出已准备好升级的 RDVH 服务器。 
4. 升级隔离的 RDVH 服务器。 
5. 成功升级目标 RDVH 服务器后，创建新的群集和 CSV（需要在完全不同的 SAN 卷上）。
6. 将所有已升级的 RDVH 服务器加入新群集。 
7. 在新的 CSV 中创建一个文件夹结构，用于模拟现有 CSV 中的现有文件夹结构。 这包括集合文件夹以及每个 VM 的顶级子文件夹。 
8. 在原始 CSV 上的各个 VM 集合文件夹中，将 /IMGS 文件夹及其内容复制到新的 CSV 上位于同一位置的新集合文件夹中。 
9. 在源 RDVH 计算机上，使用群集管理器删除 VM 的高可用性配置：
    1. 启动群集管理器。 
    1. 单击“角色”  。 
    1. 右键单击 VM 对象，然后单击“删除”。  
10. 在某个未升级的 RDVH 服务器上，使用 Hyper-v 管理器将所有 VM 移到某个已升级的 RDVH 服务器和新的群集 CSV：
    1. 打开 Hyper-V 管理器。 
    2. 选择某个未升级的 RDVH 服务器。 
    3. 右键单击某个要移动的 VM，然后单击“移动”。  
    4. 选择“移动虚拟机”，然后单击“下一步”。   
    5. 在“指定目标计算机”页上提供已升级的目标 RDVH 服务器的名称，然后单击“下一步”。   
    6. 选择“将虚拟机的数据移到单个位置”，然后单击“下一步”。   
    7. 浏览到目标位置。 
       > [!IMPORTANT]
       > 确保此路径指向特定 VM 的某个空文件夹。 

       > [!NOTE]
       > 如前所述，在执行此步骤之前，需事先创建一个新的目标子文件夹。 执行此步骤时，无法在“选择文件夹”对话框中创建子文件夹。 
    
       单击“下一步”  ，然后单击“已完成”  。 
11. 重新定位 VM 后，请将其添加为群集的“高可用性”对象： 
     1. 在已升级的 RD 虚拟化主机服务器上打开故障转移群集管理器。 
     1. 右键单击“角色”节点，然后单击“配置角色”。   在高可用性向导的“开始”页上，单击“下一步”。   
     1. 从可用角色列表中选择“虚拟机”，然后单击“下一步”。   此时会显示未配置的 VM 列表。 
     1. 选择所有 VM。 单击“下一步”，然后在确认页上再次单击“下一步”，以开始执行配置任务。    
12. 重新定位所有 VM 后，升级剩余的 RDVH 服务器。 在适当的情况下，请使用上述步骤平衡 VM 位置。

> [!NOTE]  
> 不支持在群集中使用异构的 Hyper-V 服务器。 

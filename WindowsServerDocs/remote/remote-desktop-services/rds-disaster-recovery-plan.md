---
title: 创建灾难恢复计划
description: 了解如何为 RDS 部署创建灾难恢复计划。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: e4e9a9ab05e672c72925e3699900218abdf1c682
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404026"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>为 RDS 创建灾难恢复计划

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

你可以在 Azure Site Recovery 中创建一个灾难恢复计划，以自动执行故障转移过程。 将所有 RDS 组件 VM 添加到恢复计划。

在 Azure 中使用以下步骤来创建恢复计划：

1. 在 Azure 门户中，打开 Azure Site Recovery 保管库，然后单击“恢复计划”  。
2. 单击“创建”  并输入计划名称。
3. 选择“源”  和“目标”  。 目标是辅助 RDS 站点或 Azure。
4. 选择托管 RDS 组件的 VM，然后单击“确定”  。

以下部分提供了有关为不同类型的 RDS 部署创建恢复计划的其他信息。

## <a name="sessions-based-rds-deployment"></a>基于会话的 RDS 部署

对于基于 RDS 会话的部署，请对 VM 进行分组，使它们按顺序出现：

1. 故障转移组 1 - 会话主机 VM
2. 故障转移组 2 - 连接代理 VM
3. 故障转移组 3 - Web 访问 VM

你的计划将如下所示： 

![基于会话的 RDS 部署的灾难恢复计划](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>共用桌面 RDS 部署

对于具有共用桌面的 RDS 部署，请对 VM 进行分组，使它们按顺序出现，同时添加手动步骤和脚本。

1. 故障转移组 1 - RDS 连接代理 VM
2. 组 1 手动操作 - 更新 DNS

   在连接代理 VM 上在提升模式下运行 PowerShell。 运行以下命令并等待几分钟，以确保使用新值更新 DNS：

   ```
   ipconfig /registerdns
   ```
3. 组 1 脚本 - 添加虚拟化主机

   修改下面的脚本，以供云中的每个虚拟化主机运行。 通常在将虚拟化主机添加到连接代理之后，需要重启主机。 确保在脚本运行之前，主机没有等待的重启，否则它将失败。

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. 故障转移组 2 - 模板 VM
5. 组 2 脚本 1 - 关闭模板 VM
   
   模板 VM 恢复到辅助站点时将启动，但它是一个系统准备的 VM，无法完全启动。 此外 RDS 要求关闭 VM 以从中创建共用 VM 配置。 因此，我们需要将其关闭。 如果有一台 VMM 服务器，则主站点和辅助站点上的模板 VM 名称相同。 正因为如此，我们使用下面脚本中的 Context  变量指定的 VM ID。 如果你有多个模板，请将其全部关闭。

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. 组 2 脚本 2 - 删除现有的共用 VM

   你需要从连接代理中删除主站点上的共用 VM，以便可以在辅助站点上创建新的 VM。 在这种情况下，你需要指定创建共用 VM 的确切主机。 请注意，这将仅从集合中删除 VM。

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. 组 2 手动操作 - 分配新模板

   你需要将新的模板分配给集合的连接代理，以便可以在恢复站点上创建新的共用 VM。 转到 RDS 连接代理并标识集合。 编辑属性，并指定新的 VM 映像作为其模板。
8. 组 2 脚本 3 - 重新创建所有共用 VM

   通过连接代理在恢复站点上重新创建共用 VM。 在这种情况下，你需要指定创建共用 VM 的确切主机。

   共用 VM 名称需要使用前缀和后缀，并且是唯一的。 如果 VM 名称已经存在，脚本将失败。 此外，如果主端 VM 从 1-5 进行编号，则恢复站点编号将从 6 开始继续。

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. 故障转移组 3 - Web 访问和网关服务器 VM

恢复计划将如下所示：

![具有共用桌面的 RDS 部署的灾难恢复计划](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>个人桌面 RDS 部署

对于具有个人桌面的 RDS 部署，请对 VM 进行分组，使它们按顺序出现，同时添加手动步骤和脚本。

1. 故障转移组 1 - RDS 连接代理 VM
2. 组 1 手动操作 - 更新 DNS

   在连接代理 VM 上在提升模式下运行 PowerShell。 运行以下命令并等待几分钟，以确保使用新值更新 DNS：

   ```
   ipconfig /registerdns
   ```
3. 组 1 脚本 - 添加虚拟化主机
      
   修改下面的脚本，以供云中的每个虚拟化主机运行。 通常在将虚拟化主机添加到连接代理之后，需要重启主机。 确保在脚本运行之前，主机没有等待的重启，否则它将失败。

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. 故障转移组 2 - 模板 VM
5. 组 2 脚本 1 - 关闭模板 VM
   
   模板 VM 恢复到辅助站点时将启动，但它是一个系统准备的 VM，无法完全启动。 此外 RDS 要求关闭 VM 以从中创建共用 VM 配置。 因此，我们需要将其关闭。 如果有一台 VMM 服务器，则主站点和辅助站点上的模板 VM 名称相同。 正因为如此，我们使用下面脚本中的 Context  变量指定的 VM ID。 如果你有多个模板，请将其全部关闭。

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. 故障转移组 3 - 个人 VM
7. 组 3 脚本 1 - 删除现有个人 VM 并进行添加

   从连接代理中删除主站点上的个人 VM，以便可以在辅助站点上创建新的 VM。 你需要提取 VM 的分配，并使用分配的哈希将虚拟机重新添加到连接代理。 这只会从集合中删除个人 VM 并重新添加。 个人桌面分配将导出并导入到集合中。

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. 故障转移组 3 - Web 访问和网关服务器 VM

你的计划将如下所示： 

![个人桌面 RDS 部署的灾难恢复计划](media/rds-asr-personal-desktops-drplan.png)

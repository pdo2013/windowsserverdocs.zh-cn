---
title: 创建灾难恢复计划
description: 了解如何创建针对 RDS 部署的灾难恢复计划。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 8ad759a73e4a0ce1dc28f2b8e8d80f4365895430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879498"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>创建 RDS 在灾难恢复计划

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以在 Azure Site Recovery 自动执行故障转移过程中创建一个灾难恢复计划。 将 RDS 组件的所有 Vm 都添加到恢复计划。

在 Azure 中使用以下步骤来创建恢复计划：

1. 在 Azure 门户中，打开 Azure Site Recovery 保管库，然后单击**恢复计划**。
2. 单击**创建**并输入计划的名称。
3. 选择你**源**并**目标**。 目标是辅助 RDS 站点或 Azure。
4. 选择的 Vm，承载您的 RDS 组件，然后单击**确定**。

以下部分提供有关创建恢复计划进行的不同类型的 RDS 部署的其他信息。

## <a name="sessions-based-rds-deployment"></a>基于会话的 RDS 部署

有关 RDS 基于会话的部署的 Vm 分组，以便在序列中显示：

1. 故障转移组 1-会话主机 VM
2. 故障转移组 2-连接 Broker VM
3. 故障转移组 3-Web 访问 VM

你的计划将如下所示： 

![灾难恢复计划进行的基于会话的 RDS 部署](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>共用的桌面 RDS 部署

有关使用共用桌面的 RDS 部署的 Vm 分组，因此它们都将在序列中，添加手动步骤和脚本。

1. 故障转移组 1 的 RDS 连接 Broker VM
2. 组 1 的手动操作-更新 DNS

   连接 Broker VM 上在提升模式下运行 PowerShell。 运行以下命令并等待几分钟时间才能确保使用新值更新 DNS:

   ```
   ipconfig /registerdns
   ```
3. 组 1 脚本-添加虚拟化主机

   修改以下脚本以运行适用于在云中每个虚拟化主机。 通常将添加到连接代理的虚拟化主机后，您需要重新启动主机。 请确保该主机不具有挂起的重新启动脚本运行前，否则它将会失败。

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. 故障转移组 2 的模板 VM
5. 组 2 脚本 1-启用模板 VM 关闭
   
   模板 VM 恢复到辅助站点时将启动，但它是经过系统准备 VM，不能完全启动。 此外 RDS 要求将关闭共用的 VM 配置通过它来创建 VM。 因此，我们需要将其关闭。 如果有一台 VMM 服务器时，模板 VM 名称是相同的主和辅助数据库。 正因为如此，我们将该 VM ID 用作指定的*上下文*中下面的脚本变量。 如果你有多个模板，将其全部关闭。

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. 组 2 脚本 2-删除现有的共用虚拟机

   您需要从连接代理中删除主站点上的共用的 Vm，因此可以在辅助站点上创建新的 Vm。 在这种情况下需要指定要在其中创建共用的 VM 的确切主机。 请注意，这将从集合中删除 Vm。

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. 组 2 手动操作-分配新的模板

   您需要将新的模板分配给集合连接代理，因此你可以在恢复站点上创建新池的 Vm。 转到 RDS 连接代理，并标识集合。 编辑属性，并为其模板指定新的 VM 映像。
8. 组 2 脚本 3-重新创建所有共用虚拟机

   重新创建连接代理通过在恢复站点上的共用的 Vm。 在这种情况下，需要指定要在其中创建共用的 VM 的确切主机。

   共用的 VM 名称必须是唯一的使用的前缀和后缀。 如果 VM 名称已存在，脚本将失败。 此外，如果主端虚拟机进行编号从 1-5，恢复站点的编号 6 中将继续。

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. 故障转移组 3-Web 访问和网关服务器 VM

恢复计划将如下所示：

![共用桌面的 RDS 部署灾难恢复计划](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>个人桌面 RDS 部署

有关使用个人桌面的 RDS 部署的 Vm 分组，因此它们都将在序列中，添加手动步骤和脚本。

1. 故障转移组 1 的 RDS 连接 Broker VM
2. 组 1 的手动操作-更新 DNS

   连接 Broker VM 上在提升模式下运行 PowerShell。 运行以下命令并等待几分钟时间才能确保使用新值更新 DNS:

   ```
   ipconfig /registerdns
   ```
3. 组 1 脚本-添加虚拟化主机
      
   修改以下脚本以运行适用于在云中每个虚拟化主机。 通常将添加到连接代理的虚拟化主机后，您需要重新启动主机。 请确保该主机不具有挂起的重新启动脚本运行前，否则它将会失败。

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. 故障转移组 2 的模板 VM
5. 组 2 脚本 1-启用关闭 VM 模板
   
   模板 VM 恢复到辅助站点时将启动，但它是经过系统准备 VM，不能完全启动。 此外 RDS 要求将关闭共用的 VM 配置通过它来创建 VM。 因此，我们需要将其关闭。 如果有一台 VMM 服务器时，模板 VM 名称是相同的主和辅助数据库。 正因为如此，我们将该 VM ID 用作指定的*上下文*中下面的脚本变量。 如果你有多个模板，将其全部关闭。

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. 故障转移组 3-个人虚拟机
7. 组 3 脚本 1-删除现有个人虚拟机，并将其添加

   删除主站点上的个人虚拟机从连接代理，以便可以在辅助站点上创建新的 Vm。 您需要提取的 Vm 的分配，并将虚拟机重新添加到具有分配的哈希连接代理。 这仅将从集合中移除个人虚拟机并重新添加它们。 将导出和导入回集合个人桌面配置。

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. 故障转移组 3-Web 访问和网关服务器 VM

你的计划将如下所示： 

![个人桌面 RDS 部署灾难恢复计划](media/rds-asr-personal-desktops-drplan.png)

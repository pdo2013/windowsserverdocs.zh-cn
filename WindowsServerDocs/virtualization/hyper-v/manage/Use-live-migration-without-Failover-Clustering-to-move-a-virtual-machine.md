---
title: 使用没有故障转移群集的实时迁移来移动虚拟机
description: 提供在独立环境中进行实时迁移的先决条件和说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
author: KBDAzure
ms.author: kathydav
ms.date: 01/17/2017
ms.openlocfilehash: 55c96ff4696871e4013c3abd6247209d0d4517c0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392553"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>使用没有故障转移群集的实时迁移来移动虚拟机

>适用于：Windows Server 2016

本文说明如何在不使用故障转移群集的情况下执行实时迁移来移动虚拟机。 实时迁移在 Hyper-v 主机之间移动正在运行的虚拟机，而无需任何明显的停机时间。   
  
若要实现此目的，需要：   

- 在源计算机和目标计算机上均为本地 Hyper-v 管理员组或管理员组成员的用户帐户。 
  
- 在源服务器和目标服务器上安装的 Windows Server 2016 或 Windows Server 2012 R2 中的 Hyper-v 角色，并为实时迁移设置。 如果虚拟机至少为版本5，则可以在运行 Windows Server 2016 和 Windows Server 2012 R2 的主机之间执行实时迁移。

    有关版本升级说明，请参阅在[windows 10 或 Windows Server 2016 上的 hyper-v 中升级虚拟机版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 有关安装说明，请参阅[设置用于实时迁移的主机](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)。

- Hyper-v 管理工具安装在运行 Windows Server 2016 或 Windows 10 的计算机上，除非在源服务器或目标服务器上安装了这些工具，并且你将在其中运行这些工具。  
   
## <a name="use-hyper-v-manager-to-move-a-running-virtual-machine"></a>使用 Hyper-v 管理器移动正在运行的虚拟机  
  
1.  打开 Hyper-V 管理器。 （从服务器管理器中，单击 "**工具**"  >> "**hyper-v 管理器**"。）  
  
2.  在导航窗格中，选择其中一个服务器。 （如果未列出此项，请右键单击 " **Hyper-v 管理器**"，单击 "**连接到服务器**"，键入服务器名称，然后单击 **"确定"** 。 重复此步骤以添加更多服务器。）  
  
3.  在 "**虚拟机**" 窗格中，右键单击虚拟机，然后单击 "**移动**"。 这将打开 "移动向导"。 
  
4.  使用向导页可以选择移动的类型、目标服务器和选项。
  
5.  在 **“摘要”** 页面，检查你的选项，然后单击 **“完成”** 。  

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>使用 Windows PowerShell 移动正在运行的虚拟机
  
下面的示例使用 LMTest cmdlet 将名为 " " 的虚拟机移到名为 " *TestServer02* " 的目标服务器，并将虚拟硬盘和其他文件（如检查点和智能页面文件）移到*D:\LMTest*目标服务器上的目录。  
  
```  
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest  
```  
  
## <a name="troubleshooting"></a>疑难解答

### <a name="failed-to-establish-a-connection"></a>未能建立连接 

如果尚未设置约束委派，则必须先登录到源服务器，然后才能移动虚拟机。 如果不这样做，身份验证尝试将失败，出现错误，并显示以下消息：  
  
"迁移源的虚拟机迁移操作失败。  
未能建立与主机*计算机名称*的连接：Security package 0x8009030E 中没有可用的凭据。
  
 若要解决此问题，请登录到源服务器，然后重试移动。 若要避免在执行实时迁移之前登录到源服务器，请设置约束委派。 你将需要域管理员凭据来设置约束委派。 有关说明，请参阅[设置用于实时迁移的主机](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)。 
 
 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>失败，因为主机硬件不兼容
 
 如果虚拟机未打开处理器兼容性，并且有一个或多个快照，则当主机具有不同的处理器版本时，移动会失败。 出现错误并显示此消息：
 
无法将0The 虚拟机移动到目标计算机。 @no__t目标计算机上的硬件与此虚拟机的硬件要求不兼容。 **
 
 若要解决此问题，请关闭虚拟机并打开 "处理器兼容性" 设置。
 
1. 从 Hyper-v 管理器的 "**虚拟机**" 窗格中，右键单击虚拟机，然后单击 "设置"。
2. 在导航窗格中，展开 "**处理器**" 并单击 "**兼容性**"。
3. 选中 "**迁移到具有不同处理器版本的计算机**"。
4. 单击 **“确定”** 。
 
   若要使用 Windows PowerShell，请使用[VMProcessor](https://technet.microsoft.com/library/hh848533.aspx) cmdlet：
 
   ```
   PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
   ```
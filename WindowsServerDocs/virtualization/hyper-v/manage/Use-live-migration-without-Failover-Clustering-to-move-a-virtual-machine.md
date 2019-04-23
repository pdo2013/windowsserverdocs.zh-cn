---
title: 使用没有故障转移群集的实时迁移移动虚拟机
description: 提供了系统必备组件和操作在独立环境中的实时迁移的说明。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
author: KBDAzure
ms.author: kathydav
ms.date: 01/17/2017
ms.openlocfilehash: a33912e09d664296f6eda964c40177353718d49c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851538"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>使用没有故障转移群集的实时迁移移动虚拟机

>适用于：Windows Server 2016

本文介绍如何通过执行实时迁移操作而无需使用故障转移群集迁移的虚拟机。 实时迁移而无需任何明显的停机时间的 HYPER-V 主机之间移动正在运行的虚拟机。   
  
若要能够执行此操作，您将需要：   

- 用户帐户，本地 HYPER-V 管理员组或源和目标计算机上的 Administrators 组的成员。 
  
- 在 Windows Server 2016 或 Windows Server 2012 R2 中的 HYPER-V 角色安装在源和目标服务器上，并为实时迁移设置。 您可以执行如果虚拟机是在至少运行 Windows Server 2016 和 Windows Server 2012 R2 的主机之间的实时迁移版本 5。 <br>版本升级的说明，请参阅[在 Windows 10 或 Windows Server 2016 上的 HYPER-V 中的升级虚拟机版本](..\deploy\Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)。 有关安装说明，请参阅[设置用于实时迁移的主机](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)。 
  
- 除非源或目标服务器上安装这些工具安装在运行 Windows Server 2016 或 Windows 10 的计算机上的 HYPER-V 管理工具将从那里运行它们。  
   
## <a name="BKMK_Step3"></a>使用 Hyper-v 管理器移动正在运行的虚拟机  
  
1.  打开 Hyper-V 管理器。 (从服务器管理器中，单击**工具** >>**Hyper-v 管理器**。)  
  
2.  在导航窗格中，选择一台服务器。 (如果未列出，请右键单击**Hyper-v 管理器**，单击**连接到服务器**，键入服务器名称，然后单击**确定**。 重复此步骤添加更多的服务器。）  
  
3.  从**虚拟机**窗格中，右键单击虚拟机，然后单击**移动**。 这将打开移动向导。 
  
4.  使用向导页来选择移动、 目标服务器和选项的类型。
  
5.  在 **“摘要”** 页面，检查你的选项，然后单击 **“完成”**。  

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>使用 Windows PowerShell 将移动正在运行的虚拟机
  
下面的示例使用 MOVE-VM cmdlet 将名为虚拟机移动*LMTest*到名为的目标服务器*TestServer02*并将虚拟硬盘和其他文件，此类检查点和为智能分页文件*D:\LMTest*目录在目标服务器上。  
  
```  
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest  
```  
  
## <a name="troubleshooting"></a>疑难解答

### <a name="failed-to-establish-a-connection"></a>无法建立连接 

如果你尚未设置约束委派，您必须登录到源服务器之前可以移动虚拟机。 如果不这样做，身份验证尝试失败，出现错误，并显示此消息：  
  
"在迁移源虚拟机迁移操作失败。  
未能与主机建立连接*计算机名*:无凭据中提供了安全包 0x8009030E。"
  
 若要解决此问题，请登录到源服务器并尝试移动试。 若要避免必须在登录到源服务器执行实时迁移之前，请设置约束委派。 你将需要域管理员凭据来设置约束委派。 有关说明，请参阅[设置用于实时迁移的主机](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)。 
 
 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>失败，因为主机硬件不兼容
 
 如果虚拟机不具有打开的处理器兼容性，并且具有一个或多个快照，移动失败，如果主机具有不同处理器版本。 发生错误并显示此消息：
 
**将虚拟机不能移动到目标计算机。目标计算机上的硬件不与此虚拟机的硬件要求兼容。**
 
 若要解决此问题，请关闭虚拟机，并启用处理器兼容性设置。
 
1. 从 HYPER-V 管理器中**虚拟机**窗格中，右键单击虚拟机，然后单击设置。
2. 在导航窗格中，展开**处理器**然后单击**兼容性**。
3. 检查**迁移到具有不同处理器版本的计算机**。
4. 单击 **“确定”**。
 
 若要使用 Windows PowerShell，请使用[Set-vmprocessor](https://technet.microsoft.com/library/hh848533.aspx) cmdlet:
 
  ```
  PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
  ```
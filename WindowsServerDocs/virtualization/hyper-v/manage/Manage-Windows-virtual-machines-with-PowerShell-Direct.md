---
title: 通过 PowerShell Direct 管理 Windows 虚拟机
description: 说明如何使用 PowerShell Direct 来管理虚拟机，而无需依赖于网络或与之建立远程连接。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc09093ba2d
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f42e221b073cf199117f85ea96beef4997bbbbc5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392742"
---
# <a name="manage-windows-virtual-machines-with-powershell-direct"></a>通过 PowerShell Direct 管理 Windows 虚拟机

>适用于：Windows 10、Windows Server 2016、Windows Server 2019
  
可以使用 PowerShell Direct 从 Windows 10、Windows server 2016 或 Windows Server 2019 Hyper-v 主机远程管理 Windows 10、Windows Server 2016 或 Windows Server 2019 虚拟机。 当 Hyper-v 主机或虚拟机上的网络配置或远程管理设置不同时，PowerShell Direct 允许在虚拟机中管理 Windows PowerShell。 这使得 Hyper-V 管理员能够更简单地自动化虚拟机管理和配置，并为其编写脚本。  
  
可通过两种方法运行 PowerShell Direct：  
  
- 使用 PSSession cmdlet 创建和退出 PowerShell Direct 会话
  
- 通过调用-Command cmdlet 运行脚本或命令
  
如果要管理较旧的虚拟机，请使用虚拟机连接 (VMConnect) 或[为虚拟机配置虚拟网络](https://technet.microsoft.com/library/cc816585.aspx)。  
  
## <a name="create-and-exit-a-powershell-direct-session-using-pssession-cmdlets"></a>使用 PSSession cmdlet 创建和退出 PowerShell Direct 会话  
  
1. 在 Hyper-V 主机上，以管理员身份打开 Windows PowerShell。  
  
2. 使用[Enter-PSSession](https://technet.microsoft.com/library/hh849707.aspx) cmdlet 连接到虚拟机。 通过使用虚拟机名称或 GUID 运行以下命令之一来创建会话：  
  
    ```  
    Enter-PSSession -VMName <VMName>  
    ```  
  
    ```  
    Enter-PSSession -VMGUID <VMGUID>  
    ```  
  
3. 键入虚拟机的凭据。   
4. 运行所需的任意命令。 这些命令在与其创建了会话的虚拟机上运行。  
  
5.  完成后，使用[Exit-PSSession](https://technet.microsoft.com/library/hh849743.aspx)关闭会话。   
  
    ```  
    Exit-PSSession  
    ```  
  
## <a name="run-script-or-command-with-invoke-command-cmdlet"></a>通过调用-Command cmdlet 运行脚本或命令  
你可以使用 [Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command) cmdlet 在虚拟机上运行一组预先确定的命令。 下面是如何使用 Invoke-Command cmdlet 的示例，其中 PSTest 是虚拟机名称，而要运行的脚本 (foo.ps1) 位于 C:/ 驱动器上的脚本文件夹中：  
  
```  
Invoke-Command -VMName PSTest  -FilePath C:\script\foo.ps1  
```  
  
若要运行单个命令，请使用 **-ScriptBlock** 参数：  
  
```  
Invoke-Command -VMName PSTest  -ScriptBlock { cmdlet }  
```  
  
## <a name="whats-required-to-use-powershell-direct"></a>使用 PowerShell Direct 的要求是什么？  
若要在虚拟机上创建 PowerShell Direct 会话，  
  
-   虚拟机必须在主机上本地运行并已启动。  
  
-   必须以 Hyper-V 管理员身份登录主机计算机。  
  
-   必须为虚拟机提供有效用户凭据。  
  
-   主机操作系统必须至少运行 Windows 10 或 Windows Server 2016。
  
-   虚拟机必须至少运行 Windows 10 或 Windows Server 2016。  
  
你可以使用[GET VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet 来检查你使用的凭据是否具有 hyper-v 管理员角色，以及如何获取在主机上本地运行并已启动的虚拟机的列表。  
  
## <a name="see-also"></a>请参阅  
[Enter-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enter-PSSession)  
[Exit-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Exit-PSSession)  
[调用-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command)  
  



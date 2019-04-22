---
title: 使用 PowerShell Direct 管理 Windows 虚拟机
description: 提供有关使用 PowerShell Direct 管理虚拟机而不依赖于网络或远程连接到它们的说明。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc09093ba2d
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 4081a9737825d2f50f0d3b19b3bada3b9bbc76f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814698"
---
# <a name="manage-windows-virtual-machines-with-powershell-direct"></a>使用 PowerShell Direct 管理 Windows 虚拟机

>适用于：Windows 10，Windows Server 2016 中，Windows Server 2019
  
可以使用 PowerShell Direct 远程管理 Windows 10、 Windows Server 2016 或 Windows Server 2019 虚拟机，从 Windows 10、 Windows Server 2016 或 Windows Server 2019 HYPER-V 主机。 PowerShell Direct 都允许 Windows PowerShell 管理网络配置或远程管理的 HYPER-V 主机上的设置如何或虚拟机。 这使得 Hyper-V 管理员能够更简单地自动化虚拟机管理和配置，并为其编写脚本。  
  
可通过两种方法运行 PowerShell Direct：  
  
- 创建和退出 PowerShell Direct 会话使用 PSSession cmdlet
  
- 使用 Invoke-command cmdlet 运行脚本或命令
  
如果要管理较旧的虚拟机，请使用虚拟机连接 (VMConnect) 或[为虚拟机配置虚拟网络](https://technet.microsoft.com/library/cc816585.aspx)。  
  
## <a name="create-and-exit-a-powershell-direct-session-using-pssession-cmdlets"></a>创建和退出 PowerShell Direct 会话使用 PSSession cmdlet  
  
1. 在 Hyper-V 主机上，以管理员身份打开 Windows PowerShell。  
  
2. 使用[Enter-pssession](https://technet.microsoft.com/library/hh849707.aspx) cmdlet 连接到虚拟机。 运行以下命令以创建会话使用的虚拟机名称或 GUID 之一：  
  
    ```  
    Enter-PSSession -VMName <VMName>  
    ```  
  
    ```  
    Enter-PSSession -VMGUID <VMGUID>  
    ```  
  
3. 键入虚拟机的凭据。   
4. 运行所需的任意命令。 这些命令在与其创建了会话的虚拟机上运行。  
  
5.  完成后，使用[Exit-pssession](https://technet.microsoft.com/library/hh849743.aspx)关闭会话。   
  
    ```  
    Exit-PSSession  
    ```  
  
## <a name="run-script-or-command-with-invoke-command-cmdlet"></a>使用 Invoke-command cmdlet 运行脚本或命令  
你可以使用 [Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command) cmdlet 在虚拟机上运行一组预先确定的命令。 下面是如何使用 Invoke-Command cmdlet 的示例，其中 PSTest 是虚拟机名称，而要运行的脚本 (foo.ps1) 位于 C:/ 驱动器上的脚本文件夹中：  
  
```  
Invoke-Command -VMName PSTest  -FilePath C:\script\foo.ps1  
```  
  
若要运行单个命令，请使用 **-ScriptBlock** 参数：  
  
```  
Invoke-Command -VMName PSTest  -ScriptBlock { cmdlet }  
```  
  
## <a name="whats-required-to-use-powershell-direct"></a>所需使用 PowerShell Direct？  
若要在虚拟机上创建 PowerShell Direct 会话，  
  
-   虚拟机必须在主机上本地运行并已启动。  
  
-   必须以 Hyper-V 管理员身份登录主机计算机。  
  
-   必须为虚拟机提供有效用户凭据。  
  
-   主机操作系统必须至少运行 Windows 10 或 Windows Server 2016。
  
-   虚拟机必须至少运行 Windows 10 或 Windows Server 2016。  
  
可以使用[GET-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet 检查是否正在使用的凭据具有 HYPER-V 管理员角色并获取一系列虚拟机在主机上本地运行并已启动。  
  
## <a name="see-also"></a>请参阅  
[Enter-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enter-PSSession)  
[Exit-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Exit-PSSession)  
[Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command)  
  



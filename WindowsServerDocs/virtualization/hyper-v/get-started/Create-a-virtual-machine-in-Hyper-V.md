---
title: 在 HYPER-V 中创建的虚拟机
description: 提供有关如何创建虚拟机使用 Hyper-v 管理器或 Windows PowerShell 的说明
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 3b850d19663115b72d809af1ae92a444f5491496
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810423"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>在 HYPER-V 中创建的虚拟机

>适用于：Windows 10、 Windows Server 2016、 Microsoft HYPER-V Server 2016，Windows Server 2019，Microsoft HYPER-V Server 2019

了解如何使用 Hyper-v 管理器创建虚拟机和 Windows PowerShell 和选项后，在 Hyper-v 管理器中创建虚拟机。  

## <a name="create-a-virtual-machine-by-using-hyper-v-manager"></a>使用 Hyper-v 管理器创建虚拟机  

1.  打开**HYPER-V 管理器**。  

2.  从**操作**窗格中，单击**新建**，然后单击**虚拟机**。  

3.  从**新的虚拟机向导**，单击**下一步**。  

4.  在每个页上进行恰当的选择为虚拟机。 有关详细信息，请参阅[新的虚拟机选项和 Hyper-v 管理器中的默认值](#options-in-hyper-v-manager-new-virtual-machine-wizard)本主题中更高版本。  

5.  在验证中的选项后**摘要**页上，单击**完成**。  

6.  在 HYPER-V 管理器中，右键单击虚拟机，并选择**连接**。  

7.  在虚拟机连接窗口中，选择**操作** > **启动**。  

## <a name="create-a-virtual-machine-by-using-windows-powershell"></a>使用 Windows PowerShell 创建虚拟机  

1. 在 Windows 桌面上，单击“开始”按钮并键入名称 **Windows PowerShell** 的任一部分。  

2. 右键单击**Windows PowerShell** ，然后选择**以管理员身份运行**。  

3. 获取你想要通过使用的虚拟机的虚拟交换机的名称[Get-vmswitch](https://technet.microsoft.com/library/hh848499.aspx)。  例如，  

   ```  
   Get-VMSwitch  * | Format-Table Name  
   ```  

4. 使用[NEW-VM](https://technet.microsoft.com/library/hh848537.aspx) cmdlet 来创建虚拟机。  请参阅以下示例。  

   > [!NOTE]  
   > 可以将此虚拟机移动到运行 Windows Server 2012 R2 的 HYPER-V 主机，如果使用-Version 参数与[NEW-VM](https://technet.microsoft.com/library/hh848537.aspx)将虚拟机配置版本设置为 5。 Windows Server 2016 的默认虚拟机配置版本不支持 Windows Server 2012 R2 或更早版本。 创建虚拟机后，您无法更改虚拟机配置版本。 有关详细信息，请参阅[受支持的虚拟机配置版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)。  

   - **现有虚拟硬盘**-若要使用现有虚拟硬盘，创建虚拟机可以使用以下命令，  
     - **-Name**是为要创建虚拟机提供的名称。  
     - **-MemoryStartupBytes**是可供在启动虚拟机的内存量。  
     - **-BootDevice**是虚拟机网络适配器 （网络适配器） 或虚拟硬盘 (VHD) 等启动时启动的设备。  
     - **-VHDPath**是你想要使用的虚拟机磁盘的路径。  
     - **-Path**是用于存储虚拟机配置文件的路径。  
     - **代**是虚拟机代次。 第 1 代用于 VHD 和 vhdx 第 2 代。 请参阅[应在 HYPER-V 中创建第 1 或 2 代虚拟机？。](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
     - **-切换**是你想要用于连接到其他虚拟机或网络的虚拟机的虚拟交换机的名称。 请参阅[创建的虚拟交换机的 HYPER-V 虚拟机。](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。  

       ```  
       New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
       ```  

       例如：  

       ```  
       New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
       ```  

       这将创建名为 Win10VM 具有 4 GB 内存的第 2 代虚拟机。 它从当前目录中的文件夹 VMs\Win10.vhdx 启动并使用名为 ExternalSwitch 虚拟交换机。 虚拟机配置文件都存储在文件夹 VMData。  

   - **新的虚拟硬盘**-若要使用新的虚拟硬盘创建虚拟机，替换 **-VHDPath**从上面的示例中使用参数 **-NewVHDPath** ，并添加 **-NewVHDSizeBytes**参数。 例如，  

     ```  
     New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
     ```  

   - **新建虚拟硬盘的引导到操作系统映像**-若要使用引导到操作系统映像，请参阅中的 PowerShell 示例的新虚拟磁盘创建虚拟机[上为 HYPER-V 创建虚拟机演练Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm)。  

5. 通过使用启动虚拟机[START-VM](https://technet.microsoft.com/library/hh848589.aspx) cmdlet。 运行以下 cmdlet，其中名称是你创建的虚拟机的名称。  

   ```  
   Start-VM -Name <Name>  
   ```  

   例如：  

   ```  
   Start-VM -Name Win10VM  
   ```  

6. 使用虚拟机连接 (VMConnect) 连接到虚拟机。  

   ```  
   VMConnect.exe  
   ```  

## <a name="options-in-hyper-v-manager-new-virtual-machine-wizard"></a>在新虚拟机向导中的 HYPER-V 管理器选项  
下表列出了在创建虚拟机中的 HYPER-V 管理器和默认值为每个时可以选择的选项。  

|Page|默认值为 Windows Server 2016 和 Windows 10|其他选项|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**指定名称和位置**|名称：新的虚拟机。<br /><br />位置:**C:\ProgramData\Microsoft\Windows\Hyper-V\\** .|您还可以输入自己的名称，然后选择为虚拟机的另一个位置。<br /><br />这是将存储虚拟机配置文件的位置。|  
|**指定生成**|第 1 代|您还可以选择创建的第 2 代虚拟机。 有关详细信息，请参阅[应在 HYPER-V 中创建第 1 或 2 代虚拟机？。](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**分配内存**|启动内存：1024 MB<br /><br />动态内存：**未选择**|您可以设置启动内存从 32 MB 到 5902 MB。<br /><br />您还可以选择使用动态内存。 有关详细信息，请参阅[HYPER-V 动态内存概述](https://technet.microsoft.com/library/hh831766.aspx)。|  
|**配置网络**|未连接|可以选择要从现有的虚拟交换机的列表中使用的虚拟机的网络连接。 请参阅[创建的虚拟交换机的 HYPER-V 虚拟机。](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。|  
|**连接虚拟硬盘**|创建虚拟硬盘<br /><br />名称： <*vmname*>.vhdx<br /><br />**位置**：**C:\Users\Public\Documents\Hyper-V\Virtual 硬盘\\**<br /><br />**大小**：127GB|您还可以选择使用现有的虚拟硬盘或等待并稍后附加虚拟硬盘。|  
|**安装选项**|更高版本安装操作系统|这些选项更改虚拟机的启动顺序，以便你可以从.iso 文件，可启动软盘或网络安装服务，如 Windows 部署服务 (WDS) 安装。|  
|**摘要**|显示您已选择的选项，以便可以验证它们正确。<br /><br />-名称<br />-生成<br />-内存<br />-网络<br />-硬盘<br />-操作系统|**更改暂存文件夹路径**可以复制摘要页中，并将其粘贴到电子邮件或其他位置来帮助你跟踪你的虚拟机。|  

## <a name="see-also"></a>请参阅  

- [New-VM](https://technet.microsoft.com/library/hh848537.aspx)  

- [支持的虚拟机配置版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)  

-   [应在 HYPER-V 中创建第 1 或 2 代虚拟机？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [为 Hyper-V 虚拟机创建虚拟交换机](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  

---
title: 在 Hyper-v 中创建虚拟机
description: 提供有关使用 Hyper-v 管理器或 Windows PowerShell 创建虚拟机的说明
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 739691650ce3cda8066e9f7ac77626f53f22affa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364255"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>在 Hyper-v 中创建虚拟机

>适用于：Windows 10、Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

了解如何使用 Hyper-v 管理器和 Windows PowerShell 创建虚拟机，以及在 Hyper-v 管理器中创建虚拟机时使用的选项。  

## <a name="create-a-virtual-machine-by-using-hyper-v-manager"></a>使用 Hyper-v 管理器创建虚拟机  

1.  打开**Hyper-v 管理器**。  

2.  在 "**操作**" 窗格中，单击 "**新建**"，然后单击 "**虚拟机**"。  

3.  在**新建虚拟机向导**中，单击 "**下一步**"。  

4.  在每个页面上为虚拟机进行适当的选择。 有关详细信息，请参阅本主题后面的[Hyper-v 管理器中的新虚拟机选项和默认值](#options-in-hyper-v-manager-new-virtual-machine-wizard)。  

5.  在 "**摘要**" 页中验证你的选择后，单击 "**完成**"。  

6.  在 "Hyper-v 管理器" 中，右键单击虚拟机，然后选择 "**连接**"。  

7.  在 "虚拟机连接" 窗口中，选择 "**操作** > **启动**"。  

## <a name="create-a-virtual-machine-by-using-windows-powershell"></a>使用 Windows PowerShell 创建虚拟机  

1. 在 Windows 桌面上，单击“开始”按钮并键入名称 **Windows PowerShell** 的任一部分。  

2. 右键单击 " **Windows PowerShell** " 并选择 "以**管理员身份运行**"。  

3. 使用[获取-VMSwitch](https://technet.microsoft.com/library/hh848499.aspx)获取虚拟机要使用的虚拟交换机的名称。  例如，  

   ```  
   Get-VMSwitch  * | Format-Table Name  
   ```  

4. 使用[新的-VM](https://technet.microsoft.com/library/hh848537.aspx) cmdlet 来创建虚拟机。  请参阅以下示例。  

   > [!NOTE]  
   > 如果可以将此虚拟机移动到运行 Windows Server 2012 R2 的 Hyper-v 主机，请将-Version 参数与[New-VM](https://technet.microsoft.com/library/hh848537.aspx)一起使用，将虚拟机配置版本设置为5。 Windows server 2012 R2 或更低版本不支持 Windows Server 2016 的默认虚拟机配置版本。 创建虚拟机后，无法更改虚拟机配置版本。 有关详细信息，请参阅[支持的虚拟机配置版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)。  

   - **现有虚拟硬盘**-若要使用现有的虚拟硬盘创建虚拟机，可以使用以下命令，其中，  
     - **-Name**是为你要创建的虚拟机提供的名称。  
     - **-MemoryStartupBytes**是虚拟机在启动时可用的内存量。  
     - **-BootDevice**是虚拟机在启动时启动到的设备，如网络适配器（网络适配器）或虚拟硬盘（VHD）。  
     - **-VHDPath**是要使用的虚拟机磁盘的路径。  
     - **-Path**是用于存储虚拟机配置文件的路径。  
     - **-代**为虚拟机生成。 为 VHD 使用第1代，为 VHDX 使用第2代。 请参阅[应在 hyper-v 中创建第1代还是第2代虚拟机？。](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
     - **-Switch**是你希望虚拟机用于连接到其他虚拟机或网络的虚拟交换机的名称。 请参阅[创建用于 hyper-v 虚拟机的虚拟交换机](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。  

       ```  
       New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
       ```  

       例如：  

       ```  
       New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
       ```  

       这将创建一个名为 Win10VM 的第2代虚拟机，内存为4GB。 它从当前目录中的 VMs\Win10.vhdx 文件夹启动，并使用名为 ExternalSwitch 的虚拟交换机。 虚拟机配置文件存储在 VMData 文件夹中。  

   - **新虚拟硬盘**-若要使用新的虚拟硬盘创建虚拟机，请将上述示例中的 **-VHDPath**参数替换为 **-NewVHDPath** ，并添加 **-NewVHDSizeBytes**参数。 例如，  

     ```  
     New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
     ```  

   - **用于启动到操作系统映像的新虚拟硬盘**-若要使用启动到操作系统映像包的新虚拟磁盘创建虚拟机，请参阅在[Windows 10 上创建虚拟机演练](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm)中的 PowerShell 示例。  

5. 使用[启动 VM](https://technet.microsoft.com/library/hh848589.aspx) cmdlet 启动虚拟机。 运行以下 cmdlet，其中 Name 是你创建的虚拟机的名称。  

   ```  
   Start-VM -Name <Name>  
   ```  

   例如：  

   ```  
   Start-VM -Name Win10VM  
   ```  

6. 使用虚拟机连接（VMConnect）连接到虚拟机。  

   ```  
   VMConnect.exe  
   ```  

## <a name="options-in-hyper-v-manager-new-virtual-machine-wizard"></a>Hyper-v 管理器新建虚拟机向导中的选项  
下表列出了在 Hyper-v 管理器中创建虚拟机时可以选取的选项，以及每个虚拟机的默认值。  

|Page|Windows Server 2016 和 Windows 10 的默认值|其他选项|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**指定名称和位置**|名称：新虚拟机。<br /><br />位置:**C:\ProgramData\Microsoft\Windows\Hyper-V @ no__t-1**。|你还可以输入自己的名称，并为虚拟机选择另一个位置。<br /><br />这是将存储虚拟机配置文件的位置。|  
|**指定生成**|第 1 代|你还可以选择创建第2代虚拟机。 有关详细信息，请参阅是否[应在 hyper-v 中创建第1代或第2代虚拟机？。](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**分配内存**|启动内存：1024 MB<br /><br />动态内存：**未选择**|可以将启动内存从32MB 设置为5902MB。<br /><br />你还可以选择使用动态内存。 有关详细信息，请参阅[hyper-v 动态内存概述](https://technet.microsoft.com/library/hh831766.aspx)。|  
|**配置网络**|未连接|你可以从现有虚拟交换机列表中选择虚拟机要使用的网络连接。 请参阅[创建用于 hyper-v 虚拟机的虚拟交换机](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。|  
|**连接虚拟硬盘**|创建虚拟硬盘<br /><br />名称： <*vmname*> .vhdx<br /><br />**位置**：**C:\Users\Public\Documents\Hyper-V\Virtual 硬盘 @ no__t-1**<br /><br />**大小**：127GB|你还可以选择使用现有的虚拟硬盘，或者等待并在以后附加虚拟硬盘。|  
|**安装选项**|稍后安装操作系统|这些选项将更改虚拟机的启动顺序，以便可以从 .iso 文件、可启动软盘或网络安装服务（如 Windows 部署服务（WDS））安装。|  
|**摘要**|显示您选择的选项，以便您可以验证它们是否正确。<br /><br />-Name<br />-生成<br />-内存<br />-网络<br />-硬盘<br />-操作系统|**更改暂存文件夹路径**可以从页面复制摘要，并将其粘贴到电子邮件或其他地方，以帮助跟踪虚拟机。|  

## <a name="see-also"></a>请参阅  

- [新 VM](https://technet.microsoft.com/library/hh848537.aspx)  

- [支持的虚拟机配置版本](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)  

-   [是否应在 Hyper-v 中创建第1代或第2代虚拟机？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [为 Hyper-V 虚拟机创建虚拟交换机](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  

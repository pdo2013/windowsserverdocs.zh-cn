---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: 描述在 VMConnect 中使用本地资源的要求
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: KBDAzure
ms.author: kathyDav
ms.date: 12/06/2016
ms.openlocfilehash: 70bf72ec2277679820d985c9f78f10a4ea6e04df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392895"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>适用于：Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2

虚拟机连接（VMConnect）允许你在虚拟机中使用计算机的本地资源，如可移动的 USB 闪存驱动器或打印机。 增强会话模式还允许调整 VMConnect 窗口的大小。 本文介绍如何配置主机，并向虚拟机授予对本地资源的访问权限。

增强会话模式和类型剪贴板文本仅适用于运行最近的 Windows 操作系统的虚拟机。 @no__t[使用本地资源的0See 要求](#requirements-for-using-local-resources)，请见下。 \) 

有关运行 Ubuntu 的虚拟机，请参阅[在 HYPER-V VM 中更改 Ubuntu 屏幕分辨率](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/)。 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>在 Hyper-v 主机上打开增强会话模式  
如果 Hyper-v 主机运行的是 Windows 10 或 Windows 8.1，则默认情况下，"增强会话模式" 处于打开状态，因此您可以跳过此操作并转到下一节。 但是，如果主机运行的是 Windows Server 2016 或 Windows Server 2012 R2，请先执行此操作。 
  
启用增强会话模式：

1.  请连接到承载虚拟机的计算机。  
  
2.  在 "Hyper-v 管理器" 中，选择主机的计算机名。  
  
    ![屏幕截图，显示在左窗格的 "Hyper-v 管理器" 下列出的主机名。](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  选择“Hyper-V 设置”。  
  
    ![屏幕截图，显示右窗格中 "操作" 下的 "Hyper-v 设置" 选项。](media/HyperV-ActionsHyperVSettings.png)  
  
4.  在“服务器”下，选择“增强会话模式策略”。  
  
    ![显示 "安全性" 部分下的 "增强会话模式" 策略选项的屏幕截图。](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  选中“允许增强会话模式”复选框。  
  
    ![屏幕截图，显示 "允许增强会话模式" 复选框以获取增强会话模式策略。](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  在“用户”下，选择“增强会话模式”。  
  
    ![屏幕截图，显示 "用户" 部分下的 "增强会话模式" 选项。 ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  选中“允许增强会话模式”复选框。  
  
8.  单击“确定”。  
  
## <a name="choose-a-local-resource"></a>选择本地资源

本地资源包括打印机、剪贴板，以及运行 VMConnect 的计算机上的本地驱动器。 有关更多详细信息，请参阅下面[的使用本地资源的要求](#requirements-for-using-local-resources)。  
  
选择本地资源：
  
1.  打开 VMConnect。  
  
2.  选择要连接到的虚拟机。  
  
3.  单击“显示选项”。  
  
    ![调用显示对话框左下角的选项的屏幕截图。](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  选择“本地资源”。  
  
    ![调用 "本地资源" 选项卡的屏幕截图。](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  单击“更多”。  
  
    ![调用 "更多" 按钮的屏幕截图。](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  选择要在虚拟机上使用的驱动器，然后单击“确定”。  
  
    ![屏幕截图，显示你可以选择的本地资源和驱动器。](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  选择“保存我的设置以便在将来连接到此虚拟机”。  
  
    ![用于调用复选框以选择此选项的屏幕截图。](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  单击“连接”。  
  
## <a name="edit-vmconnect-settings"></a>编辑 VMConnect 设置

可以通过在 Windows PowerShell 或命令提示符处运行以下命令，针对 VMConnect 方便地编辑连接设置：  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>使用本地资源的要求

若要能够在虚拟机上使用计算机的本地资源，请执行以下操作：  
  
-   Hyper-v 主机必须打开 "**增强会话模式策略**" 和 "**增强会话模式**" 设置。  
  
-   使用 VMConnect 的计算机必须运行 Windows 10、Windows 8.1、Windows Server 2016 或 Windows Server 2012 R2。  
  
-   虚拟机必须启用远程桌面服务，并运行 Windows 10、Windows 8.1、Windows Server 2016 或 Windows Server 2012 R2 作为来宾操作系统。  
  
如果运行 VMConnect 的计算机和虚拟机均满足要求，则可以使用以下任何本地资源（如果可用）：  
  
-   显示器配置  
  
-   Audio
  
-   打印机  
  
-   用于复制和粘贴的剪贴板  
  
-   智能卡  
  
-   USB 设备  
  
-   驱动器  
  
-   支持的即插即用设备  
  
## <a name="why-use-a-computers-local-resources"></a>为什么使用计算机的本地资源？
你可能想要使用计算机的本地资源来执行以下操作：  
  
-   在虚拟机没有网络连接的情况下对虚拟机进行疑难解答。  
  
-   按照使用远程桌面连接 (RDP) 进行复制和粘贴的相同方式，将文件复制并粘贴到虚拟机以及从虚拟机复制并粘贴文件。  
  
-   通过使用智能卡登录到虚拟机。  
  
-   从虚拟机打印到本地打印机。  
  
-   在不使用 RDP 的情况下测试需要 USB 和声音重定向的开发人员应用程序并进行疑难解答。  
  
## <a name="see-also"></a>请参阅  
[连接到虚拟机](https://technet.microsoft.com/library/cc742407.aspx)  
[是否应在 Hyper-v 中创建第1代或第2代虚拟机？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)




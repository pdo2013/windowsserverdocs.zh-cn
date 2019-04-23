---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: 介绍有关与 VMConnect 使用本地资源的要求
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: KBDAzure
ms.author: kathyDav
ms.date: 12/06/2016
ms.openlocfilehash: 196a32d57877662ccd73647835e16af9348135c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845288"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>适用于：Windows 10 中，Windows 8.1 和 Windows Server 2016，Windows Server 2012 R2

虚拟机连接 (VMConnect) 允许您在虚拟机中，使用计算机的本地资源，类似于可移动的 USB 闪存驱动器或打印机。 增强的会话模式还允许您调整 VMConnect 窗口的大小。 本文介绍如何配置主机，然后将虚拟机访问提供给本地资源。

增强的会话模式和类型剪贴板文本只能用于运行最新的 Windows 操作系统的虚拟机。 \(请参阅[使用本地资源的要求](#BKMK_NEW)下文。\) 

对于运行 Ubuntu 的虚拟机，请参阅[中的 HYPER-V VM 更改 Ubuntu 屏幕分辨率](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/)。 
  
## <a name="BKMK_OVER"></a>打开增强的会话模式的 HYPER-V 主机上  
如果 HYPER-V 主机运行 Windows 10 或 Windows 8.1，增强的会话模式是在默认情况下，因此您可以跳过此并移动到下一节。 但是，如果主机运行 Windows Server 2016 或 Windows Server 2012 R2，先执行此操作。 
  
开启增强的会话模式：

1.  请连接到承载虚拟机的计算机。  
  
2.  在 HYPER-V 管理器中，选择主机的计算机名。  
  
    ![显示计算机名称的主机的屏幕截图列出的 HYPER-V 管理器下的左窗格中。](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  选择“Hyper-V 设置” 。  
  
    ![在右窗格中显示操作下的 HYPER-V 设置选项的屏幕截图。](media/HyperV-ActionsHyperVSettings.png)  
  
4.  在“服务器”下，选择“增强会话模式策略”。  
  
    ![在安全性部分下显示增强会话模式策略选项的屏幕截图。](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  选中“允许增强会话模式”复选框。  
  
    ![屏幕快照，显示允许增强会话模式的增强会话模式策略的复选框。](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  在“用户”下，选择“增强会话模式”。  
  
    ![在用户部分下显示增强会话模式选项的屏幕截图。 ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  选中“允许增强会话模式”复选框。  
  
8.  单击“确定”。  
  
## <a name="choose-a-local-resource"></a>选择本地资源

本地资源包括打印机、 剪贴板和正在运行 VMConnect 的计算机上的本地驱动器。 有关更多详细信息，请参阅[使用本地资源的要求](#BKMK_NEW)下文。  
  
若要选择的本地资源：
  
1.  打开 VMConnect。  
  
2.  选择要连接到的虚拟机。  
  
3.  单击“显示选项” 。  
  
    ![调用左下角的对话框中的显示选项的屏幕截图。](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  选择“本地资源”。  
  
    ![明确了本地资源选项卡的屏幕截图。](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  单击“更多”。  
  
    ![还指出其他按钮的屏幕截图。](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  选择要在虚拟机上使用的驱动器，然后单击“确定” 。  
  
    ![显示本地资源和你可以选择的驱动器的屏幕截图。](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  选择“保存我的设置以便在将来连接到此虚拟机”。  
  
    ![明确了该复选框以选择此选项的屏幕截图。](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  单击“连接”。  
  
## <a name="edit-vmconnect-settings"></a>编辑 VMConnect 设置

可以通过在 Windows PowerShell 或命令提示符处运行以下命令，针对 VMConnect 方便地编辑连接设置：  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="BKMK_NEW"></a>使用本地资源的要求

若要能够使用虚拟机上的计算机的本地资源：  
  
-   HYPER-V 主机必须具有**增强会话模式策略**并**增强会话模式**在打开设置。  
  
-   在其使用 VMConnect 的计算机必须运行 Windows 10、 Windows 8.1、 Windows Server 2016 或 Windows Server 2012 R2。  
  
-   虚拟机必须具有远程桌面服务启用，并且运行 Windows 10、 Windows 8.1、 Windows Server 2016 或 Windows Server 2012 R2 作为来宾操作系统。  
  
如果运行 VMConnect 和虚拟机的计算机都满足要求，如果它们可用时才可以使用任何以下本地资源：  
  
-   显示器配置  
  
-   Audio
  
-   打印机  
  
-   用于复制和粘贴的剪贴板  
  
-   智能卡  
  
-   USB 设备  
  
-   驱动器  
  
-   支持的即插即用设备  
  
## <a name="BKMK_APP"></a>为什么使用计算机的本地资源？
你可能想使用到的计算机的本地资源：  
  
-   在虚拟机没有网络连接的情况下对虚拟机进行疑难解答。  
  
-   按照使用远程桌面连接 (RDP) 进行复制和粘贴的相同方式，将文件复制并粘贴到虚拟机以及从虚拟机复制并粘贴文件。  
  
-   通过使用智能卡登录到虚拟机。  
  
-   从虚拟机打印到本地打印机。  
  
-   在不使用 RDP 的情况下测试需要 USB 和声音重定向的开发人员应用程序并进行疑难解答。  
  
## <a name="see-also"></a>请参阅  
[连接到虚拟机](https://technet.microsoft.com/library/cc742407.aspx)  
[应在 HYPER-V 中创建第 1 或 2 代虚拟机？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)




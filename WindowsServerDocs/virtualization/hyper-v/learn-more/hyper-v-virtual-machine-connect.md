---
title: HYPER-V 虚拟机连接
description: 描述虚拟机连接，它提供对虚拟机的远程访问。 包括有关如何执行常见任务，例如发送 Ctrl、 Alt、 删除到虚拟机的详细信息。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: deae35b9-7647-42b8-b6bf-45645a44c9c4
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: e1f3260fdbbd82a97c3b0949936afc6a04ec5e5a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887838"
---
# <a name="hyper-v-virtual-machine-connection"></a>HYPER-V 虚拟机连接

>适用于：Windows Server 2016 中，Windows 10、 Windows 8.1、 Windows Server 2012 R2、 Windows Server 2012、 Windows 8

虚拟机连接\(VMConnect\)是一个工具，用于连接到虚拟机，以便可以安装或来宾操作系统的虚拟机中与之交互。 一些可以使用 VMConnect 执行的任务包括：  
  
-   启动和关闭虚拟机  
  
-   连接到的 DVD 映像\(.iso 文件\)或 USB 闪存驱动器  
  
-   创建检查点  
  
-   修改虚拟机的设置  
    
## <a name="tips-for-using-vmconnect"></a>使用 VMConnect 的提示  
有关使用 VMConnect 可能会发现以下信息很有帮助：  
  
|若要执行此操作...|执行此操作...|  
|---------------|------------|  
|发送鼠标单击或虚拟机的键盘输入|在虚拟机窗口中单击任意位置。 当您连接到正在运行的虚拟机，将鼠标指针可能显示为小点。|  
|返回鼠标单击或物理计算机的键盘输入|按 CTRL\+ALT\+左箭头，然后将鼠标指针移动到虚拟机窗口之外。 可以在超中更改此鼠标释放组合键\-Hyper V 设置\-管理器。|  
|发送 CTRL\+ALT\+DELETE 键组合到虚拟机|选择**操作** > **Ctrl\+Alt\+删除**或使用组合键 CTRL\+ALT\+结束。|  
|从窗口模式切换到完整\-全屏模式|选择**视图** > **全屏模式**。 若要切换回窗口模式下运行，请按 CTRL\+ALT\+中断。|  
|创建检查点以捕获的当前状态机进行故障排除|选择**操作** > **检查点**或使用组合键 CTRL\+n。|  
|更改虚拟机的设置|选择**文件** > **设置**。|  
|连接到的 DVD 映像\(.iso 文件\)或虚拟软盘\(.vfd 文件\)|选择**媒体**。<br /><br />第 2 代虚拟机不支持虚拟软盘。 有关详细信息，请参阅[应在 HYPER-V 中创建第 1 或 2 代虚拟机？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)。|  
|使用主机的本地资源上超\-V 虚拟机类似于 USB 闪存驱动器|打开增强的会话模式的 HYPER-V 主机上，使用 VMConnect 连接到虚拟机，并在连接之前，选择你想要使用的本地资源。 有关特定步骤，请参阅[使用本地资源上超\-V 虚拟机通过 VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)。|  
|保存更改的虚拟机的 VMConnect 设置|在 Windows PowerShell 或命令提示符中运行以下命令：<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|防止 VMConnect 用户接管另一个用户的 VMConnect 会话|[打开增强的会话模式的 HYPER-V 主机上](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#BKMK_OVER)。<br /><br />不具有增强会话模式打开可能会带来安全和隐私风险。 如果用户连接并登录到虚拟机通过 VMConnect 和另一个授权的用户连接到同一个虚拟机、 会话将接管第二个用户和第一个用户将丢失该会话。 第二个用户将能够查看第一个用户的桌面、 文档和应用程序。|
|管理 integration services 或允许 VM 与 HYPER-V 主机进行通信的组件| 在运行 Windows 10 或 Windows Server 2016 的 HYPER-V 主机，不能管理通过 VMConnect 的集成服务。 请参阅以下主题： <br />- [打开或关闭的 HYPER-V 主机的集成服务](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [打开或关闭 Windows 虚拟机的集成服务](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [开启/关闭 Linux 虚拟机的集成服务](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [保留为虚拟机更新的集成服务](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />对于运行 Windows Server 2012 或 Windows Server 2012 R2 的主机，请参阅[Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx)。|
|调整 VMConnect 窗口的大小|可以更改运行 Windows 操作系统的第 2 代虚拟机在 VMConnect 窗口的大小。 若要执行此操作，可能需要打开增强的会话模式的 HYPER-V 主机上。 有关详细信息，请参阅[上打开增强的会话模式的 HYPER-V 主机](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#BKMK_OVER)。 对于运行 Ubuntu 的虚拟机，请参阅[中的 HYPER-V VM 更改 Ubuntu 屏幕分辨率](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/)。|


## <a name="keyboard-shortcuts"></a>键盘快捷方式  
默认情况下，键盘输入和鼠标点击发送到虚拟机。 因此可能需要按 CTRL + ALT + 左箭头，然后使用以下键盘快捷方式。 

|组合键|描述|  
|-------------------|---------------|  
|CTRL\+ALT\+向左的箭头|鼠标释放|  
|CTRL\+ALT\+END|等效的 CTRL\+ALT\+虚拟机中删除|  
|CTRL\+ALT\+BREAK|从完整切换\-全屏模式返回到窗口模式|  
|CTRL\+O|打开虚拟机的设置|  
|CTRL\+S|启动虚拟机|  
|CTRL\+N|创建检查点|  
|CTRL\+E|还原到检查点|  
|CTRL\+C|执行屏幕捕获|  

## <a name="see-also"></a>请参阅  
-   [通过 VMConnect 的 HYPER-V 虚拟机上使用本地资源](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Windows Server 2016 上的 hyper V](../Hyper-V-on-Windows-Server.md)  
-   [Windows 10 上的 hyper V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  

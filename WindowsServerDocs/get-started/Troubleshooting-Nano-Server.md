---
title: Nano Server 疑难解答
description: 恢复控制台, 紧急管理服务, 内核调试
ms.prod: windows-server
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e427c66f-9571-4b8c-b65d-e7370d91544d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 48b63e74ab406d2e66996097d33d71f2eeaeaf20
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391550"
---
# <a name="troubleshooting-nano-server"></a>Nano Server 疑难解答

>适用于：Windows Server 2016

> [!IMPORTANT]
> 自 Windows Server 版本 1709 开始，Nano Server 将仅用作[容器基本 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)。 查看[对 Nano Server 进行的更改](nano-in-semi-annual-channel.md)以了解这意味着什么。 

本主题包括有关可用于连接、诊断和修复 Nano Server 安装的工具的信息。  
  
## <a name="using-the-nano-server-recovery-console"></a>使用 Nano Server 恢复控制台 
 
Nano Server 包括一个恢复控制台，可以确保即使网络错误配置影响连接到 Nano Server，也能够访问 Nano Server。 可以使用恢复控制台修复网络，然后使用常用的远程管理工具。  
  
在任一虚拟机或具有监视器和附带键盘的物理计算机上启动 Nano Server 时，将看到全屏幕、文本模式登录提示。 使用管理员帐户登录此提示，以查看 Nano Server 的计算机名和 IP 地址。 可使用这些命令在此控制台中导航：  
  
-   使用箭头键进行滚动  
  
-   使用 TAB 移至以 **>** 开头的任何文本，然后按 ENTER 键进行选择。  
  
-   若要返回到某个屏幕或页面，请按 Esc。 如果位于主页，按 Esc 键将注销。  
  
-   某些屏幕在屏幕的最后一行显示了其他功能。 例如，如果你浏览网络适配器，则 F4 将禁用网络适配器。  
  
使用恢复控制台可查看和配置网络适配器和 TCP/IP 设置以及防火墙规则。
> [!NOTE]
> 恢复控制台仅支持基本的键盘功能。 不支持键盘指示灯、10 键部分和键盘布局切换（例如大写锁定和数字锁定）。 仅支持英语键盘和字符集。

## <a name="accessing-nano-server-over-a-serial-port-with-emergency-management-services"></a>通过串行端口利用紧急管理服务访问 Nano Server  
利用紧急管理服务 (EMS) 可以执行基本疑难解答，获取网络状态和通过串行端口使用终端仿真程序打开控制台会话（包括 CMD/PowerShell）。 这样即无需使用键盘和监视器对服务器进行疑难解答。 有关 EMS 的详细信息，请参阅[紧急管理服务技术参考](https://technet.microsoft.com/library/cc784411(v=ws.10).aspx)。

若要在 Nano Server 映像上启用 EMS，以备以后需要时使用，请运行此 cmdlet：  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\EnablingEMS.vhdx   -EnableEMS   -EMSPort 3   -EMSBaudRate 9600`  
  
此示例 cmdlet 可在串行端口上 3 上启用 EMS，且波特率为 9600 bps。 如果不包含这些参数，默认值为端口 1 和 115200 bps。 若要将此 cmdlet 用于 VHDX 媒体，请务必包括 Hyper-V 功能和相应的 Windows PowerShell 模块。

## <a name="kernel-debugging"></a>内核调试  
可以使用多种方法将 Nano Server 映像配置为支持内核调试。 若要结合使用内核调试与 VHDX 映像，请务必包括 Hyper-V 功能和相应的 Windows PowerShell 模块。 有关远程内核调试的信息，通常请参阅[通过网络电缆手动设置内核模式调试](https://msdn.microsoft.com/library/windows/hardware/hh439346%28v=vs.85%29.aspx) 和[使用 WinDbg 远程调试](https://msdn.microsoft.com/library/windows/hardware/hh451173%28v=vs.85%29.aspx)。  
  
### <a name="debugging-using-a-serial-port"></a>使用串行端口进行调试  
使用此示例 cmdlet 来启用要使用串行端口进行调试的映像：  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingSerial   -DebugMethod Serial   -DebugCOMPort 1   -DebugBaudRate 9600`  
  
此示例通过串行端口 2 启用调试，且波特率为 9600 bps。 如果未指定这些参数，默认值为端口 2 和 115200 bps。 如果打算使用 EMS 和内核调试，必须将其配置为使用两个单独的串行端口。  
  
### <a name="debugging-over-a-tcpip-network"></a>通过 TCP/IP 网络调试  
使用此示例 cmdlet 来启用要通过 TCP/IP 网络调试的映像：  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000`  
  
此 cmdlet 可启用内核调试，以便只允许 IP 地址为 192.168.1.100 的计算机连接，并通过端口 64000 进行所有通信。 -DebugRemoteIP 和 -DebugPort 参数是必需项，且端口号大于 49152。 此 cmdlet 在生成的 VHD 旁边的文件中生成加密密钥，且需要该 VHD 通过端口进行通信。 或者，可以使用 -DebugKey 参数指定自己的密钥，如本示例中所示：  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000   -DebugKey 1.2.3.4`  
  
### <a name="debugging-using-the-ieee1394-protocol-firewire"></a>使用 IEEE1394 协议 (Firewire) 调试  
若要通过 IEEE1394 启用调试，请使用此示例 cmdlet：  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingFireWire   -DebugMethod 1394   -DebugChannel 3`  
  
-DebugChannel 参数是必需项。  
  
### <a name="debugging-using-usb"></a>使用 USB 调试  
可以通过 USB 使用此 cmdlet 启用调试：  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingUSB   -DebugMethod USB   -DebugTargetName KernelDebuggingUSBNano`  
  
将远程调试器连接到生成的 Nano Server 时，通过 -DebugTargetName 参数将目标名称指定作为集。    
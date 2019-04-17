---
title: 网络 Shell (Netsh)
description: 本主题提供 Windows Server 2016 中的网络 Shell (netsh) 命令行实用程序的概述。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a23d60bc3f181fee62ade105e546bbb7161c133
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh"></a>网络 Shell \(Netsh\)

>适用于：Windows Server（半年通道），Windows Server 2016

网络 shell (netsh) 是一个命令行实用工具，以便你可以通过配置并安装在运行 Windows Server 2016 的计算机上后显示各种网络通信 server 角色和组件的状态。

>[!NOTE]
>本主题中，除了以下网络 Shell 内容才可用。
>
> - [Netsh 命令语法、上下文和格式](netsh-contexts.md)
> - [网络 Shell (Netsh) 的示例批量文件](netsh-wins.md)
> - [Netsh 技术参考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc) 

某些客户技术，如动态主机配置协议 \(DHCP\) 客户端和分支缓存，还提供 netsh 命令，您将配置客户端计算机正在运行 Windows 10。

在大多数情况下，netsh 命令提供了相同的功能，当你使用 Microsoft 管理控制台 \(MMC\) snap\ 时才可用-在每个网络 server 角色或网络功能。 例如，你可以通过使用 NPS mmc 贴靠或 netsh 命令的配置网络策略服务器 \(NPS\) **netsh nps**上下文。

此外，还有 netsh 命令网络技术，例如 IPv6、网络大桥和远程过程调用 \(RPC\)，这不是适用于 Windows Server 作为 mmc 贴靠。

>[!IMPORTANT]
>建议你使用的 Windows PowerShell 管理网络中的技术[Windows Server 2016 和 Windows 10](https://technet.microsoft.com/library/mt156917.aspx)而不是网络 Shell。 但是，网络 Shell 是包含在内供您的脚本的兼容性，支持它的使用。

## <a name="network-shell-netsh-technical-reference"></a>网络 Shell (Netsh) 技术参考

Netsh 技术参考提供了全面 netsh 命令参考，包括语法、参数和 netsh 命令的示例。 你可以使用 Netsh 技术参考通过网络上计算机运行的 Windows Server 2016 和 Windows 10 的技术的本地或远程管理 netsh 命令版本的脚本和批量文件。  
  
### <a name="content-availability"></a>内容的可用性  
  
网络 Shell 技术参考是可供下载，帮助 Windows 从 TechNet 库的 \(*.chm\) 格式：[Netsh 技术参考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  


---
title: Network Shell (Netsh)
description: 本主题概述了 Windows Server 2016 中的 Network Shell (netsh) 命令行实用程序。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 038b21783ef1d27619657ec3f9a472cf3caba68e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849358"
---
# <a name="network-shell-netsh"></a>网络 Shell \(Netsh\)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

网络 shell (netsh) 是命令行实用工具，可让你能够配置和运行 Windows Server 2016 的计算机上安装后显示各种网络通信服务器角色和组件的状态。

有些客户端技术，如动态主机配置协议\(DHCP\)客户端和 BranchCache，还提供了可用于配置运行 Windows 10 的客户端计算机的 netsh 命令。

在大多数情况下，netsh 命令提供的相同功能，当你使用 Microsoft 管理控制台时才可用\(MMC\)对齐\-中每个网络的服务器角色或网络功能。 例如，可以配置网络策略服务器\(NPS\)通过使用 NPS MMC 管理单元或中的 netsh 命令**netsh nps**上下文。

此外，还有用于网络技术的 netsh 命令如 IPv6、 网桥，和远程过程调用\(RPC\)，不是作为一个 MMC 管理单元的 Windows Server 中可用。

>[!IMPORTANT]
>建议使用 Windows PowerShell 来管理中的网络技术[Windows Server 2016 和 Windows 10](https://technet.microsoft.com/library/mt156917.aspx)而不是 Network Shell。 但是，网络 Shell 是包含与您的脚本的兼容性和支持其使用。

## <a name="network-shell-netsh-technical-reference"></a>网络 Shell (Netsh) 技术参考

Netsh 技术参考提供了全面的 netsh 命令参考，包括语法、 参数和 netsh 命令的示例。 可以使用 Netsh 技术参考为本地或远程管理运行 Windows Server 2016 和 Windows 10 的计算机上的网络技术的使用 netsh 命令生成脚本和批处理文件。  
  
### <a name="content-availability"></a>内容可用性  
  
网络程序技术参考是可供下载的 Windows 帮助\(*.chm\) TechNet 库中的格式：[Netsh 技术参考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---

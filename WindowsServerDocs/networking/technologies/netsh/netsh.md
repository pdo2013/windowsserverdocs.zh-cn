---
title: Network Shell (Netsh)
description: 本主题概述了 Windows Server 2016 中的 Network Shell （netsh）命令行实用工具。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: ac440c8187424733c0636cf2013342458f08d2f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405550"
---
# <a name="network-shell-netsh"></a>网络 Shell \(Netsh @ no__t-1

>适用于：Windows Server（半年频道）、Windows Server 2016

Network shell （netsh）是一个命令行实用工具，可用于在运行 Windows Server 2016 的计算机上安装和显示各种网络通信服务器角色和组件的状态。

某些客户端技术（如动态主机配置协议 \(DHCP @ no__t）客户端和 BranchCache）还提供允许你配置运行 Windows 10 的客户端计算机的 netsh 命令。

在大多数情况下，netsh 命令提供的功能与使用 Microsoft 管理控制台时可用的相同功能 \(MMC @ no__t; 每个网络服务器角色或网络功能的 no__t-2in。 例如，可以使用 NPS MMC 管理单元或**netsh NPS**上下文中的 netsh 命令来配置网络策略服务器 \(NPS @ no__t-1。

此外，还有一些用于网络技术的 netsh 命令，例如 for IPv6、网桥和远程过程调用 \(RPC @ no__t-1，它们在 Windows Server 中不可用作 MMC 管理单元。

>[!IMPORTANT]
>建议使用 Windows PowerShell 管理[Windows Server 2016 和 windows 10](https://technet.microsoft.com/library/mt156917.aspx)中的网络技术，而不是使用网络 Shell。 提供网络 Shell 是为了与脚本兼容，但支持使用它。

## <a name="network-shell-netsh-technical-reference"></a>Network Shell （Netsh）技术参考

Netsh 技术参考提供了一个全面的 netsh 命令参考，其中包括用于 netsh 命令的语法、参数和示例。 您可以使用 netsh 技术参考来构建脚本和批处理文件，方法是在运行 Windows Server 2016 和 Windows 10 的计算机上使用用于本地或远程管理网络技术的 netsh 命令。  
  
### <a name="content-availability"></a>内容可用性  
  
可从 TechNet 库中的 Windows 帮助 \( * .chm @ no__t-1 格式下载网络 Shell 技术参考：[Netsh 技术参考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---

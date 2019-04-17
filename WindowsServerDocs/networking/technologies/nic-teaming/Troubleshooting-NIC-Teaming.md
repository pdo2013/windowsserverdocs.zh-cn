---
title: 故障排除 NIC 组合
description: 本主题提供 NIC 组合 Windows Server 2016 中的疑难解答信息。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 634ec1a5eee0cd661ca89c2673e5b74abd458cd6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-nic-teaming"></a>故障排除 NIC 组合

>适用于：Windows Server（半年通道），Windows Server 2016

本主题介绍如何解决 NIC 组合，并包含以下部分中，其中介绍了可能导致 NIC 组合的问题的原因。  
  
-   [硬件不符合规范](#bkmk_hardware)  
  
-   [物理开关安全功能](#bkmk_switch)  
  
-   [禁用和与 Windows PowerShell 启用](#bkmk_ps)  
  
## <a name="bkmk_hardware"></a>硬件不符合规范  
当标准协议实现硬件不符合规格时，NIC 组合的性能可能会受到影响。  
  
在正常运行，期间 NIC 组合可能会发送数据包从同一 IP 地址，而又具有多个不同的源代码媒体访问控制 (MAC) 地址。 根据协议标准这些数据包接收器都必须到特定的 MAC 地址，而不会响应接收数据包的 MAC 地址解决主机或 VM 的 IP 地址。  正确实施的地址分辨率协议 IPv4 的地址分辨率协议 (ARP) 或 IPv6 的邻居发现协议 (NDP) 的客户端将发送具有正确的目标 MAC 地址（VM 或主机拥有该 IP 地址的 MAC 地址）数据包。  
  
但是，某些嵌入硬件不正确实现地址分辨率协议，并且还可能不显式解决 IP 地址为使用 ARP 或 NDP MAC 地址。  存储区域 (SAN) 网络控制器是这种方式可能执行设备的一个示例。 设备不符合中接收的包复制源包含的 MAC 地址，并将其用作目标相应传出数据包中的 MAC 地址。  
  
这将导致包被发送到错误目标 MAC 地址。 出于此原因，因为它们不匹配任何已知的目的地数据包 Hyper-V 虚拟交换机用来通过中删除。  
  
如果你遇到时遇到问题连接到 SAN 控制器或其他嵌入硬件，你应获取数据包捕获并确定 ARP 或 NDP，，是否正确实现你的硬件并硬件供应商联系以获取支持。  
  
## <a name="bkmk_switch"></a>物理开关安全功能  
根据配置，NIC 组合可能数据包从同一 IP 地址发送具有多个不同的源的 MAC 地址。  这可以触发了安全功能，如动态 ARP 检查或 IP 源物理交换机保护，尤其是在物理切换不知道的端口是团队的一部分。 如果您将配置 NIC 组合独立切换模式，发生这种。  你应该检查切换日志以确定是否切换安全功能会导致连接问题 NIC 组合。  
  
## <a name="bkmk_ps"></a>禁用和使用 Windows PowerShell 启用网络适配器  
NIC 团队失败的一个常见原因是团队界面已禁用。 在许多情况下，界面已禁用无意中，运行以下命令 Windows PowerShell 序列时：  
  
```  
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  
这一系列命令不会启用它禁用 NetAdapters 所有。  
  
这是因为禁用所有基础物理成员 Nic 导致删除并不会再显示在 Get-NetAdapter NIC 团队界面。 出于此原因，**启用 NetAdapter \ ***命令不会启用 NIC 团队中，由于适配器已删除。  
  
**启用 NetAdapter \ ***命令，但是，启用成员 Nic，从而然后（后短时间）导致的团队界面重新创建。 在此情况下，团队界面是仍处于"禁用"状态因为尚未重新启用。 启用团队界面之后重新创建它, 将使网络通信重新排列开始。  
  
## <a name="see-also"></a>请参阅  
[NIC 组合](NIC-Teaming.md)  
  



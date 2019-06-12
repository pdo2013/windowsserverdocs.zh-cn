---
title: NIC 组合疑难解答
description: 本主题提供有关 Windows Server 2016 中的 NIC 组合的疑难解答信息。
manager: dougkim
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
ms.date: 09/13/2018
ms.openlocfilehash: a6af3cbd038e97d889269b83d72c77c50680e513
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446168"
---
# <a name="troubleshooting-nic-teaming"></a>NIC 组合疑难解答

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，我们将讨论如何解决 NIC 组合，如硬件和物理交换机证券。  标准协议的硬件实现不符合规范，NIC 组合可能会影响性能。 此外，具体取决于配置中，NIC 组合可能会将数据包发送从具有使物理交换机上的安全功能的多个 MAC 地址相同的 IP 地址。

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>不符合规范的硬件  
  
正常操作期间，NIC 组合可能会发送数据包从相同的 IP 地址，但具有多个 MAC 地址。 根据协议标准，这些数据包的接收方必须解析为特定的 MAC 地址，而不是响应来自接收数据包的 MAC 地址的主机或 VM 的 IP 地址。  正确实现地址解析协议 （ARP 和 ndp 下） 的客户端发送具有正确的目标 MAC 地址的数据包，即，VM 或拥有该 IP 地址的主机的 MAC 地址。 
  
一些嵌入式的硬件未正确实现地址解析协议和也可能不显式 IP 地址解析为使用 ARP 或 NDP 的 MAC 地址。  例如，可能会以这种方式执行存储区域网络 (SAN) 控制器。 非符合设备接收的数据包从复制的源 MAC 地址，然后将其用作中相应的传出数据包，从而导致发送到错误的目标 MAC 地址的数据包的目标 MAC 地址。 正因为如此，因为它们不匹配任何已知的目标数据包由 HYPER-V 虚拟交换机中删除。  
  
如果您是无故障连接到 SAN 控制器或其他嵌入硬件，你应采取数据包捕获，以确定 ARP 或 ndp 下，如果您的硬件正确实现并联系硬件供应商联系以获取支持。  

  
## <a name="physical-switch-security-features"></a>物理交换机安全功能  
根据配置，NIC 组合可能会发送数据包从具有使物理交换机上的安全功能的多个源 MAC 地址相同的 IP 地址。 例如，动态 ARP 检查或 IP 源保护，尤其是如果物理切换不知道端口是一个团队，在切换独立模式下配置 NIC 组合时，会发生的一部分。 检查交换机日志，确定是否交换机安全功能导致连接问题。 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>禁用和启用网络适配器使用 Windows PowerShell  

NIC 组失败的常见原因是，禁用组接口，在许多情况下，意外时运行一系列命令。  此特定命令序列不会启用所有禁用，因为禁用所有 Nic 的基础物理成员中都删除 NIC 团队接口 NetAdapters。 

在这种情况下，NIC 组接口不再显示在 Get-netadapter，并正因为如此，**启用 NetAdapter \\** * 不会启用 NIC 组。 **启用 NetAdapter \\** * 命令，但是，启用的成员的 Nic，然后 （后短时间） 重新创建组接口。 团队接口处于"已禁用"状态，直到重新启用，允许网络流量，以便开始流动。 

以下 Windows PowerShell 命令序列可能会无意中禁用组接口：  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>相关主题  
- [NIC 组合](NIC-Teaming.md):在本主题中，我们提供您的网络接口卡 (NIC) 合作概述 Windows Server 2016 中。 NIC 组合允许你将介于 1 和 32 之间分组到一个或多个基于软件的虚拟网络适配器的物理以太网网络适配器。 这些虚拟网络适配器可以提高性能，并在网络适配器发生故障时提供容错能力。   

- [NIC 组合 MAC 地址的使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md):在 NIC 组配置为切换独立模式和地址哈希或动态负载分发时，该团队使用的媒体访问控制 (MAC) 地址的出站流量上的主 NIC 组成员。 主 NIC 团队成员是由操作系统来自团队成员的初始集选择的网络适配器。

- [NIC 组合设置](nic-teaming-settings.md):在本主题中，我们为您提供的 NIC 组属性，如组合概述和负载平衡模式。 我们还会为您提供的备用适配器设置和主要组接口属性的详细信息。 如果 NIC 组中具有至少两个网络适配器，您不需要指定的容错能力的备用适配器。
  



---
title: Windows Server 2019 网关性能
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: a6530b29ce7ffb0d18e0266e70cb2ca45188915c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845628"
---
# <a name="windows-server-2019-gateway-performance"></a>Windows Server 2019 网关性能

>适用于：Windows Server


在 Windows Server 2016 中，客户关心的问题之一是 SDN 网关无法满足吞吐量要求的现代网络。 IPsec 和 GRE 隧道的网络吞吐量有与正在约 300 Mbps 的 IPsec 连接和 GRE 连接正在大约 2.5 Gbps 的单个连接吞吐量的限制。

我们已得到明显改进在 Windows Server 2019 号分别为 1.8 Gbps 和用于 IPsec 和 GRE 连接，为 15 Gbps 迅速加大。 这一切都，采用的 CPU 周期 / 每个字节，从而提供相当性能吞吐量要少得多的 CPU 使用率的显著降低。

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>启用 Windows Server 2019 中的网关的高性能

有关**GRE 连接**，一旦您部署/升级到 Windows Server 2019 生成网关 Vm，应会自动看到改进的性能。 任何手动步骤。

有关**IPsec 连接**，默认情况下，为你的虚拟网络，创建连接时获得的 Windows Server 2016 数据路径和性能的编号。 若要启用 Windows Server 2019 数据路径，请执行以下操作：

   1. 在 VM 的 SDN 网关，请转到**Services**控制台 (services.msc)。
   2. 找到名为**Azure 网关服务**，并将启动类型设置为**自动**。
   3. 重新启动网关 VM。
      在此网关故障转移到冗余网关 VM 的活动连接。
   4. 有关网关 Vm 的其余部分重复前面的步骤。

>[!TIP]
>为获得最佳性能结果，确保 cipherTransformationConstant 和快速模式设置中的 IPsec 连接使用 authenticationTransformConstant **GCMAES256**密码套件。
>
>获得最佳性能，网关主机硬件必须支持 AES-NI 和 PCLMULQDQ CPU 指令集。 这些是任何 Westmere (32nm) 和更高版本除 AES-NI 其中禁用了的模型上的 Intel CPU 上可用。 您可以查看硬件供应商文档，以查看 CPU 是否支持 AES-NI 和 PCLMULQDQ CPU 指令设置。

下面是最佳的安全算法与 IPsec 连接的 REST 示例：

```PowerShell
# NOTE: The virtual gateway must be created before creating the IPsec connection. More details here.
# Create a new object for Tenant Network IPsec Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   

# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 2000000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 2000000  

# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "111_aaa"   

$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 3600   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   

$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 28800
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   

# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   

# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "<<On premise subnet that must be reachable over the VPN tunnel. Ex: 10.0.0.0/24>>"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   

# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "<<Public IP address of the On-Premise VPN gateway. Ex: 192.168.3.4>>"   

# Add the new Network Connection for the tenant. Note that the virtual gateway must be created before creating the IPsec connection. $uri is the REST URI of your deployment and must be in the form of “https://<REST URI>”  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force
```

## <a name="testing-results"></a>测试结果

我们投入了大量性能测试在我们的测试实验室中的 SDN 网关。 在测试中，我们有比较与 Windows Server 2019 SDN 方案和非 SDN 方案中的网关网络性能。 可以查找结果和测试设置的详细信息的博客文章中捕获[此处](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/)。

---
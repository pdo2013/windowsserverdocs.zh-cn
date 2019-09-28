---
title: Windows Server 2019 网关性能
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 58e85c15723126f2976fac3ccc21b3cfc6585750
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355925"
---
# <a name="windows-server-2019-gateway-performance"></a>Windows Server 2019 网关性能

>适用于：Windows Server


在 Windows Server 2016 中，客户关心的一项问题是无法满足 SDN 网关的要求，无法满足新式网络的吞吐量要求。 IPsec 和 GRE 隧道的网络吞吐量有一些限制： IPsec 连接的单个连接吞吐量约为 300 Mbps，而 GRE 连接速度约为 2.5 Gbps。

我们在 Windows Server 2019 中进行了重大改进，它们的数字分别为 soaring 到 1.8 Gbps，15 Gbps 用于 IPsec 和 GRE 连接。 所有这些都能在 CPU 周期/每个字节内大幅降低，从而提供超高性能的吞吐量，但 CPU 利用率要低得多。

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>使用 Windows Server 2019 中的网关实现高性能

对于**GRE 连接**，在网关 vm 上部署/升级到 Windows Server 2019 后，应该会自动查看改进的性能。 不涉及手动步骤。

对于**IPsec 连接**，默认情况下，当你为虚拟网络创建连接时，将获得 Windows Server 2016 数据路径和性能数字。 若要启用 Windows Server 2019 数据路径，请执行以下操作：

   1. 在 SDN 网关 VM 上，请参阅**服务**控制台（services.msc）。
   2. 找到名为 " **Azure 网关服务**" 的服务，并将 "启动类型" 设置为 "**自动**"。
   3. 重新启动网关 VM。
      此网关上的活动连接故障转移到冗余网关 VM。
   4. 对其余的网关 Vm 重复上述步骤。

>[!TIP]
>为获得最佳性能结果，请确保 IPsec 连接的快速模式设置中的 cipherTransformationConstant 和 authenticationTransformConstant 使用**GCMAES256**密码套件。
>
>为了获得最佳性能，网关主机硬件必须支持 AES-NI 和 PCLMULQDQ CPU 指令集。 除了已禁用 AES 的型号，在任何 Westmere （32nm）和更高版本的 Intel CPU 上都提供这些功能。 可以查看硬件供应商文档，查看 CPU 是否支持 AES-NI 和 PCLMULQDQ CPU 指令集。

下面是使用最佳安全算法实现 IPsec 连接的 REST 示例：

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

我们已在测试实验室中为 SDN 网关完成了大量性能测试。 在测试中，我们在 SDN 方案和非 SDN 方案中比较了网关网络性能和 Windows Server 2019。 可在[此处](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/)找到博客文章中捕获的结果和测试设置详细信息。

---
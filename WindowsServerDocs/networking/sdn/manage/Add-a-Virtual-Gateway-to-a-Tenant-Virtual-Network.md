---
title: 添加到租户虚拟网络虚拟网关
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b9552054-4eb9-48db-a6ce-f36ae55addcd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 351cafd1466c4b3c20e907ea221c9f58129b3443
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="add-a-virtual-gateway-to-a-tenant-virtual-network"></a>添加到租户虚拟网络虚拟网关

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何配置租户虚拟网关、使用 Windows PowerShell cmdlet 和脚本复制，租户的虚拟网络提供站点的连接到他们的站点上组织和 Internet。   
  
RAS 网关支持达 100 租户，具体取决于使用的每个租户带宽。 使用网络控制器添加租户实例 RAS 网关成员网关池的虚拟网关。 网络控制器自动确定要使用时你租户用于部署新的虚拟网关的最佳 RAS 网关。  
  
每个虚拟网关对应于特定租户中，它包括一个或多个网络连接（站点的 VPN 隧道）和，还可以边框网关协议 (BGP) 连接。 这使你的客户将其租户虚拟网络连接到外部网络，如租户企业网络、服务提供商网络或 Internet。  
  
部署租户虚拟网关时，你有以下配置选项：  
  
**网络连接选项**  
- IPSec 站点的虚拟专用网络 (VPN)   
- 一般路由封装 (GRE)   
- 第 3 层转移  
  
**BGP 配置选项**  
- BGP 路由器配置  
- BGP 等配置  
- BGP 路由策略配置  
  
示例脚本的 Windows PowerShell 和本主题中的命令演示如何将其租户虚拟网关上的每个选项 RAS 网关部署。  
  
本主题包含以下部分。  
  
- [为租户添加虚拟网关](#bkmk_addgwy)  
- [添加站点的 VPN 网络连接有关租户（IPsec、GRE 或 L3）](#bkmk_s2s1)  
- [配置为 BGP 路由器网关](#bkmk_bgp1)  
- [使用所有三种连接类型 (IPsec，GRE L3) 配置网关和 BGP](#bkmk_all3)  
- [修改或删除虚拟网络网关](#bkmk_modify)  
  
   
>[!IMPORTANT]  
>运行任何命令示例 Windows PowerShell 和提供的脚本本主题中之前，你必须以使这些值正确适用于你的部署更改所有变量的值。  
  
## <a name="bkmk_addgwy"></a>为租户添加虚拟网关  
  
第 1 步：验证网络控制器中存在网关池对象。  
```  
$uri = "https://ncrest.contoso.com"   
  
# Retrieve the Gateway Pool configuration  
$gwPool = Get-NetworkControllerGatewayPool -ConnectionUri $uri  
  
# Display in JSON format  
$gwPool | ConvertTo-Json -Depth 2   
  
  
```  
第 2 步：验证要用于路由租户的虚拟网络退出数据包子网存在在网络控制器;和检索的用于路由虚拟网络租户网关之间的虚拟子网。  
  
```  
$uri = "https://ncrest.contoso.com"   
  
# Retrieve the Tenant Virtual Network configuration  
$Vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri  -ResourceId "Contoso_Vnet1"   
  
# Display in JSON format  
$Vnet | ConvertTo-Json -Depth 4   
  
# Retrieve the Tenant Virtual Subnet configuration  
$RoutingSubnet = Get-NetworkControllerVirtualSubnet -ConnectionUri $uri  -ResourceId "Contoso_WebTier" -VirtualNetworkID $vnet.ResourceId   
  
# Display in JSON format  
$RoutingSubnet | ConvertTo-Json -Depth 4   
  
```  
第 3 步：创建虚拟网关 JSON 对象，然后将其添加到网络控制器。  
  
```  
# Create a new object for Tenant Virtual Gateway  
$VirtualGWProperties = New-Object Microsoft.Windows.NetworkController.VirtualGatewayProperties   
  
# Update Gateway Pool reference  
$VirtualGWProperties.GatewayPools = @()   
$VirtualGWProperties.GatewayPools += $gwPool   
  
# Specify the Virtual Subnet that is to be used for routing between the gateway and Virtual Network   
$VirtualGWProperties.GatewaySubnets = @()   
$VirtualGWProperties.GatewaySubnets += $RoutingSubnet   
  
# Update the rest of the Virtual Gateway object properties  
$VirtualGWProperties.RoutingType = "Dynamic"   
$VirtualGWProperties.NetworkConnections = @()   
$VirtualGWProperties.BgpRouters = @()   
  
# Add the new Virtual Gateway for tenant   
$virtualGW = New-NetworkControllerVirtualGateway -ConnectionUri $uri  -ResourceId "Contoso_VirtualGW" -Properties $VirtualGWProperties -Force   
  
```  
  
## <a name="bkmk_s2s1"></a>添加站点的 VPN 网络连接有关租户（IPsec、GRE 或 L3）  
  
你可以创建的站点的 VPN 连接 IPsec、GRE，或层 3 平板电脑 (L3) 转发通过使用下面的示例，对于每个网关类型。  
  
### <a name="ipsec-vpn-site-to-site-network-connection"></a>IPsec VPN 站点的网络连接  
  
创建网络连接 JSON 对象并将其添加到网络控制器。  
  
```  
# Create a new object for Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "P@ssw0rd"   
  
$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "SHA256128"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "DES3"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 1233   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   
  
$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 1234   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   
  
# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.1.10.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "10.127.134.121"   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force   
  
  
```  
### <a name="gre-vpn-site-to-site-network-connection"></a>GRE VPN 站点的网络连接  
  
创建网络连接 JSON 对象并将其添加到网络控制器。  
  
```  
# Create a new object for the Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "GRE"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration   
$nwConnectionProperties.GreConfiguration.GreKey = 1234   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.2.20.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "10.127.134.122"   
  
# L3 specific configuration (leave blank for GRE)  
$nwConnectionProperties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration   
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_GreGW" -Properties $nwConnectionProperties -Force   
  
```  
### <a name="l3-forwarding-network-connection"></a>L3 转发网络连接  
  
若要配置 L3 转发网络连接，你必须配置相应的逻辑网络。   
  
第 1 步：将配置逻辑网络中的 L3 转发网络连接。  
  
```  
# Create a new object for the Logical Network to be used for L3 Forwarding  
$lnProperties = New-Object Microsoft.Windows.NetworkController.LogicalNetworkProperties  
  
$lnProperties.NetworkVirtualizationEnabled = $false  
$lnProperties.Subnets = @()  
  
# Create a new object for the Logical Subnet to be used for L3 Forwarding and update properties  
$logicalsubnet = New-Object Microsoft.Windows.NetworkController.LogicalSubnet  
$logicalsubnet.ResourceId = "Contoso_L3_Subnet"  
$logicalsubnet.Properties = New-Object Microsoft.Windows.NetworkController.LogicalSubnetProperties  
$logicalsubnet.Properties.VlanID = 1001  
$logicalsubnet.Properties.AddressPrefix = "10.127.134.0/25"  
$logicalsubnet.Properties.DefaultGateways = "10.127.134.1"  
  
$lnProperties.Subnets += $logicalsubnet  
  
# Add the new Logical Network to Network Controller  
$vlanNetwork = New-NetworkControllerLogicalNetwork -ConnectionUri $uri -ResourceId "Contoso_L3_Network" -Properties $lnProperties -Force  
  
```  
第 2 步：创建网络连接 JSON 对象，然后将其添加到网络控制器。  
  
```  
# Create a new object for the Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "L3"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# GRE specific configuration (leave blank for L3)  
$nwConnectionProperties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration   
$nwConnectionProperties.L3Configuration.VlanSubnet = $vlanNetwork.properties.Subnets[0]   
  
$nwConnectionProperties.IPAddresses = @()   
$localIPAddress = New-Object Microsoft.Windows.NetworkController.CidrIPAddress   
$localIPAddress.IPAddress = "10.127.134.55"   
$localIPAddress.PrefixLength = 25   
$nwConnectionProperties.IPAddresses += $localIPAddress   
  
$nwConnectionProperties.PeerIPAddresses = @("10.127.134.65")   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.2.20.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_L3GW" -Properties $nwConnectionProperties -Force   
  
```  
  
## <a name="bkmk_bgp1"></a>配置为 BGP 路由器网关  
  
你可以使用下面的示例脚本将网关配置为边框网关协议 (BGP) 路由器。  
  
### <a name="add-a-bgp-router-for-the-tenant"></a>为租户添加 BGP 路由器  
  
创建 BGP 路由器 JSON 对象并将其添加到网络控制器。  
  
```  
# Create a new object for the Tenant BGP Router  
$bgpRouterproperties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties   
  
# Update the BGP Router properties  
$bgpRouterproperties.ExtAsNumber = "0.64512"   
$bgpRouterproperties.RouterId = "192.168.0.2"   
$bgpRouterproperties.RouterIP = @("192.168.0.2")   
  
# Add the new BGP Router for the tenant  
$bgpRouter = New-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_BgpRouter1" -Properties $bgpRouterProperties -Force   
   
  
```  
### <a name="add-a-bgp-peer-for-this-tenant-corresponding-to-the-site-to-site-vpn-network-connection-added-above"></a>为此租户，分别对应于上方添加站点-VPN 网络连接添加 BGP 等  
  
创建 BGP 等 JSON 对象并将其添加到网络控制器。  
  
```  
# Create a new object for Tenant BGP Peer  
$bgpPeerProperties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties   
  
# Update the BGP Peer properties  
$bgpPeerProperties.PeerIpAddress = "14.1.10.1"   
$bgpPeerProperties.AsNumber = 64521   
$bgpPeerProperties.ExtAsNumber = "0.64521"   
  
# Add the new BGP Peer for tenant  
New-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -BgpRouterName $bgpRouter.ResourceId -ResourceId "Contoso_IPSec_Peer" -Properties $bgpPeerProperties -Force   
  
```  
## <a name="bkmk_all3"></a>使用所有三种连接类型 (IPsec，GRE L3) 配置网关和 BGP  
（可选），可以将所有之前的步骤和配置租户虚拟网关所有连接的三个选项：   
  
```  
# Create a new Virtual Gateway Properties type object  
$VirtualGWProperties = New-Object Microsoft.Windows.NetworkController.VirtualGatewayProperties  
  
# Update GatewayPool reference  
$VirtualGWProperties.GatewayPools = @()  
$VirtualGWProperties.GatewayPools += $gwPool  
  
# Specify the Virtual Subnet that is to be used for routing between GW and VNET  
$VirtualGWProperties.GatewaySubnets = @()  
$VirtualGWProperties.GatewaySubnets += $RoutingSubnet  
  
# Update some basic properties  
$VirtualGWProperties.RoutingType = "Dynamic"  
  
# Update Network Connection object(s)  
$VirtualGWProperties.NetworkConnections = @()  
  
# IPSec Connection configuration  
$ipSecConnection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$ipSecConnection.ResourceId = "Contoso_IPSecGW"  
$ipSecConnection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$ipSecConnection.Properties.ConnectionType = "IPSec"  
$ipSecConnection.Properties.OutboundKiloBitsPerSecond = 10000  
$ipSecConnection.Properties.InboundKiloBitsPerSecond = 10000  
  
$ipSecConnection.Properties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration  
  
$ipSecConnection.Properties.IpSecConfiguration.AuthenticationMethod = "PSK"  
$ipSecConnection.Properties.IpSecConfiguration.SharedSecret = "P@ssw0rd"  
  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode  
  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "SHA256128"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "DES3"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 1233  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000  
  
$ipSecConnection.Properties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode  
  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 1234  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000  
  
$ipSecConnection.Properties.IPAddresses = @()  
$ipSecConnection.Properties.PeerIPAddresses = @()  
  
$ipSecConnection.Properties.Routes = @()  
  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.1.10.1/32"  
$ipv4Route.metric = 10  
$ipSecConnection.Properties.Routes += $ipv4Route  
  
$ipSecConnection.Properties.DestinationIPAddress = "10.127.134.121"  
  
# GRE Connection configuration  
$greConnection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$greConnection.ResourceId = "Contoso_GreGW"  
  
$greConnection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$greConnection.Properties.ConnectionType = "GRE"  
$greConnection.Properties.OutboundKiloBitsPerSecond = 10000  
$greConnection.Properties.InboundKiloBitsPerSecond = 10000  
  
$greConnection.Properties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration  
$greConnection.Properties.GreConfiguration.GreKey = 1234  
  
$greConnection.Properties.IPAddresses = @()  
$greConnection.Properties.PeerIPAddresses = @()  
  
$greConnection.Properties.Routes = @()  
  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.2.20.1/32"  
$ipv4Route.metric = 10  
$greConnection.Properties.Routes += $ipv4Route  
  
$greConnection.Properties.DestinationIPAddress = "10.127.134.122"  
  
$greConnection.Properties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration  
  
# L3 Forwarding connection configuration  
$l3Connection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$l3Connection.ResourceId = "Contoso_L3GW"  
  
$l3Connection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$l3Connection.Properties.ConnectionType = "L3"  
$l3Connection.Properties.OutboundKiloBitsPerSecond = 10000  
$l3Connection.Properties.InboundKiloBitsPerSecond = 10000  
  
$l3Connection.Properties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration  
$l3Connection.Properties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration  
$l3Connection.Properties.L3Configuration.VlanSubnet = $vlanNetwork.properties.Subnets[0]  
  
$l3Connection.Properties.IPAddresses = @()  
$localIPAddress = New-Object Microsoft.Windows.NetworkController.CidrIPAddress  
$localIPAddress.IPAddress = "10.127.134.55"  
$localIPAddress.PrefixLength = 25  
$l3Connection.Properties.IPAddresses += $localIPAddress  
  
$l3Connection.Properties.PeerIPAddresses = @("10.127.134.65")  
  
$l3Connection.Properties.Routes = @()  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.2.20.1/32"  
$ipv4Route.metric = 10  
$l3Connection.Properties.Routes += $ipv4Route  
  
# Update BGP Router Object  
$VirtualGWProperties.BgpRouters = @()  
  
$bgpRouter = New-Object Microsoft.Windows.NetworkController.VGwBgpRouter  
$bgpRouter.ResourceId = "Contoso_BgpRouter1"  
$bgpRouter.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties  
  
$bgpRouter.Properties.ExtAsNumber = "0.64512"  
$bgpRouter.Properties.RouterId = "192.168.0.2"  
$bgpRouter.Properties.RouterIP = @("192.168.0.2")  
  
$bgpRouter.Properties.BgpPeers = @()  
  
# Create BGP Peer Object(s)  
# BGP Peer for IPSec Connection  
$bgpPeer_IPSec = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_IPSec.ResourceId = "Contoso_IPSec_Peer"  
  
$bgpPeer_IPSec.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_IPSec.Properties.PeerIpAddress = "14.1.10.1"  
$bgpPeer_IPSec.Properties.AsNumber = 64521  
$bgpPeer_IPSec.Properties.ExtAsNumber = "0.64521"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_IPSec  
  
# BGP Peer for GRE Connection  
$bgpPeer_Gre = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_Gre.ResourceId = "Contoso_Gre_Peer"  
  
$bgpPeer_Gre.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_Gre.Properties.PeerIpAddress = "14.2.20.1"  
$bgpPeer_Gre.Properties.AsNumber = 64522  
$bgpPeer_Gre.Properties.ExtAsNumber = "0.64522"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_Gre  
  
# BGP Peer for L3 Connection  
$bgpPeer_L3 = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_L3.ResourceId = "Contoso_L3_Peer"  
  
$bgpPeer_L3.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_L3.Properties.PeerIpAddress = "14.3.30.1"  
$bgpPeer_L3.Properties.AsNumber = 64523  
$bgpPeer_L3.Properties.ExtAsNumber = "0.64523"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_L3  
  
$VirtualGWProperties.BgpRouters += $bgpRouter  
  
# Finally Add the new Virtual Gateway for tenant  
New-NetworkControllerVirtualGateway -ConnectionUri $uri  -ResourceId "Contoso_VirtualGW" -Properties $VirtualGWProperties -Force  
  
```  
## <a name="bkmk_modify"></a>修改或删除虚拟网络网关  
  
你可以使用下面的示例脚本修改或删除现有网关。  
  
### <a name="modify-the-configuration-of-an-existing-gateway"></a>修改现有网关配置  
你可以使用以下命令修改现有网关。  
  
第 1 步：检索该组件的配置并将其存储在变量  
  
```  
$nwConnection = Get-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW"  
```  
第 2 步：导航变量结构访问所需的属性，并将其设置为更新的值  
  
```  
$nwConnection.properties.IpSecConfiguration.SharedSecret = "C0mplexP@ssW0rd"  
```  
第 3 步：添加了修改后的配置替换网络控制器上的较旧配置  
  
```  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId $nwConnection.ResourceId -Properties $nwConnection.Properties -Force  
```  
### <a name="remove-a-gateway"></a>删除网关  
可以使用下面的 Windows PowerShell 命令删除个别网关功能或整个网关。  
  
#### <a name="remove-a-network-connection"></a>删除网络连接  
```  
Remove-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW" -Force  
```  
#### <a name="remove-a-bgp-peer"></a>删除 BGP 等  
```  
Remove-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -BgpRouterName "Contoso_BgpRouter1" -ResourceId "Contoso_IPSec_Peer" -Force  
```  
#### <a name="remove-a-bgp-router"></a>删除 BGP 路由器  
```  
Remove-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_BgpRouter1" -Force  
```  
#### <a name="remove-a-gateway"></a>删除网关  
```  
Remove-NetworkControllerVirtualGateway -ConnectionUri $uri -ResourceId "Contoso_VirtualGW" -Force   
```  
  
  



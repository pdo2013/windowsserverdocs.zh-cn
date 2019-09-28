---
title: 将虚拟网关添加到租户虚拟网络
description: 了解如何使用 Windows PowerShell cmdlet 和脚本为租户的虚拟网络提供站点到站点连接。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b9552054-4eb9-48db-a6ce-f36ae55addcd
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: deeb226d81c48e0e389ea4d2619c43aca01e25c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355885"
---
# <a name="add-a-virtual-gateway-to-a-tenant-virtual-network"></a>将虚拟网关添加到租户虚拟网络 

>适用于：Windows Server（半年频道）、Windows Server 2016 

了解如何使用 Windows PowerShell cmdlet 和脚本为租户的虚拟网络提供站点到站点连接。 在本主题中，将使用网络控制器将租户虚拟网关添加到作为网关池成员的 RAS 网关实例。 根据每个租户使用的带宽，RAS 网关最多支持100个租户。 网络控制器在为租户部署新的虚拟网关时，自动确定要使用的最佳 RAS 网关。  

每个虚拟网关都对应于一个特定租户，并包含一个或多个网络连接（站点到站点 VPN 隧道）以及（可选）边界网关协议（BGP）连接。 提供站点到站点连接时，你的客户可以将其租户虚拟网络连接到外部网络，例如租户企业网络、服务提供商网络或 Internet。

**部署租户虚拟网关时，可以使用以下配置选项：**  


|                                                        网络连接选项                                                         |                                              BGP 配置选项                                               |
|-------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| <ul><li>IPSec 站点到站点虚拟专用网络（VPN）</li><li>通用路由封装（GRE）</li><li>第 3 层转发</li></ul> | <ul><li>BGP 路由器配置</li><li>BGP 对等配置</li><li>BGP 路由策略配置</li></ul> |

---

本主题中的 Windows PowerShell 示例脚本和命令演示了如何使用其中的每个选项在 RAS 网关上部署租户虚拟网关。  


>[!IMPORTANT]  
>在运行提供的任何示例 Windows PowerShell 命令和脚本之前，必须更改所有变量值，以便这些值适用于你的部署。  

1.  验证网络控制器中是否存在网关池对象。 

    ```PowerShell
    $uri = "https://ncrest.contoso.com"   

    # Retrieve the Gateway Pool configuration  
    $gwPool = Get-NetworkControllerGatewayPool -ConnectionUri $uri  

    # Display in JSON format  
    $gwPool | ConvertTo-Json -Depth 2   

    ```  

2.  验证网络控制器中是否存在用于将数据包路由到租户的虚拟网络的子网。 你还可以检索用于租户网关和虚拟网络之间的路由的虚拟子网。  

    ```PowerShell 
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

3.  为租户虚拟网关创建新的对象，然后更新网关池引用。  你还可以指定用于网关和虚拟网络之间的路由的虚拟子网。  指定虚拟子网后，请更新虚拟网关对象属性的其余部分，然后为该租户添加新的虚拟网关。

    ```PowerShell  
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

4. 使用 IPsec、GRE 或第3层（L3）转发创建站点到站点 VPN 连接。  

   >[!TIP]
   >或者，你可以将前面的所有步骤组合在一起，并使用所有三个连接选项配置租户虚拟网关。  有关更多详细信息，请参阅[使用所有三种连接类型（IPsec、GRE、L3）和 BGP 配置网关](#optional-step-configure-a-gateway-with-all-three-connection-types-ipsec-gre-l3-and-bgp)。

   **IPsec VPN 站点到站点网络连接**

   ```PowerShell  
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

   **GRE VPN 站点到站点网络连接**

   ```PowerShell  
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

   **L3 转发网络连接**<p>
   要使 L3 转发网络连接正常工作，必须配置相应的逻辑网络。   

   1. 为 L3 转发网络连接配置逻辑网络。  <br>

      ```PowerShell  
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

   2. 创建一个网络连接 JSON 对象并将其添加到网络控制器。  

      ```PowerShell 
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

5. 将网关配置为 BGP 路由器，并将其添加到网络控制器。 

   1. 为租户添加 BGP 路由器。  

      ```PowerShell  
      # Create a new object for the Tenant BGP Router  
      $bgpRouterproperties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties   

      # Update the BGP Router properties  
      $bgpRouterproperties.ExtAsNumber = "0.64512"   
      $bgpRouterproperties.RouterId = "192.168.0.2"   
      $bgpRouterproperties.RouterIP = @("192.168.0.2")   

      # Add the new BGP Router for the tenant  
      $bgpRouter = New-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_BgpRouter1" -Properties $bgpRouterProperties -Force   

      ```  

   2. 为此租户添加一个 BGP 对等方，对应于上面添加的站点到站点 VPN 网络连接。  

      ```PowerShell
      # Create a new object for Tenant BGP Peer  
      $bgpPeerProperties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties   

      # Update the BGP Peer properties  
      $bgpPeerProperties.PeerIpAddress = "14.1.10.1"   
      $bgpPeerProperties.AsNumber = 64521   
      $bgpPeerProperties.ExtAsNumber = "0.64521"   

      # Add the new BGP Peer for tenant  
      New-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -BgpRouterName $bgpRouter.ResourceId -ResourceId "Contoso_IPSec_Peer" -Properties $bgpPeerProperties -Force   

      ```  

## <a name="optional-step-configure-a-gateway-with-all-three-connection-types-ipsec-gre-l3-and-bgp"></a>（可选步骤）使用所有三种连接类型（IPsec、GRE、L3）和 BGP 配置网关  
（可选）可以将前面的所有步骤和使用全部三个连接选项配置租户虚拟网关：   

```PowerShell  
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

## <a name="modify-a-gateway-for-a-virtual-network"></a>修改虚拟网络的网关  


**检索组件的配置，并将其存储在变量中**

```PowerShell  
$nwConnection = Get-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW"  
```  

**导航变量结构以访问所需属性，并将其设置为 updates 值**

```PowerShell  
$nwConnection.properties.IpSecConfiguration.SharedSecret = "C0mplexP@ssW0rd"  
```  

**添加修改后的配置以替换网络控制器上的较旧配置**

```PowerShell  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId $nwConnection.ResourceId -Properties $nwConnection.Properties -Force  
```  


## <a name="remove-a-gateway-from-a-virtual-network"></a>从虚拟网络中删除网关 
你可以使用以下 Windows PowerShell 命令删除单个网关功能或整个网关。  

**删除网络连接**  
```PowerShell  
Remove-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW" -Force  
```  

**删除 BGP 对等** 
```PowerShell  
Remove-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -BgpRouterName "Contoso_BgpRouter1" -ResourceId "Contoso_IPSec_Peer" -Force  
```  

**删除 BGP 路由器**
```PowerShell  
Remove-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_BgpRouter1" -Force  
```

**删除网关**  
```PowerShell  
Remove-NetworkControllerVirtualGateway -ConnectionUri $uri -ResourceId "Contoso_VirtualGW" -Force   
```  

---
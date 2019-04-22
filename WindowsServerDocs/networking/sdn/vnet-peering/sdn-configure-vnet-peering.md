---
title: 配置虚拟网络对等
description: 配置虚拟网络对等互连涉及创建获取对等互连的两个虚拟网络。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 3ef3db879080e3372e7b287dcc55ae052c1fe109
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816388"
---
# <a name="configure-virtual-network-peering"></a>配置虚拟网络对等

>适用于：Windows Server

在此过程中，使用 Windows PowerShell 来创建两个虚拟网络，每个都有一个子网。 然后，配置之间启用二者之间的连接的两个虚拟网络对等互连。

- [第 1 步。创建第一个虚拟网络](#step-1-create-the-first-virtual-network)

- [第 2 步。创建第二个虚拟网络](#step-2-create-the-second-virtual-network)

- [第 3 步。配置从第一个虚拟网络将到第二个虚拟网络的对等互连](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [第 4 步。配置从第二个虚拟网络将到第一个虚拟网络的对等互连](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>请记住更新你的环境的属性。

## <a name="step-1-create-the-first-virtual-network"></a>步骤 1： 创建第一个虚拟网络

在此步骤中，您使用 Windows PowerShell 查找 HNV 提供程序逻辑网络创建第一个虚拟网络包含一个子网。 以下示例脚本创建具有一个子网的 Contoso 虚拟网络。

``` PowerShell
#Find the HNV Provider Logical Network  

$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"
$uri=”https://restserver”  

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-2-create-the-second-virtual-network"></a>步骤 2： 创建第二个虚拟网络

在此步骤中，具有一个子网创建第二个虚拟网络。 以下示例脚本创建具有一个子网 Woodgrove 的虚拟网络。

``` PowerShell

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Woodgrove"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
$uri=”https://restserver”

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.2.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Woodgrove_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network"></a>步骤 3： 配置从第一个虚拟网络将到第二个虚拟网络的对等互连

在此步骤中，可以配置第一个虚拟网络之间在前两个步骤中创建的第二个虚拟网络对等互连。 以下示例脚本建立从虚拟网络对等互连**Contoso_vnet1**到**Woodgrove_vnet1**。

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties
$vnet2 = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Woodgrove_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2

#Indicate whether communication between the two virtual networks
$peeringProperties.allowVirtualnetworkAccess = $true

#Indicates whether forwarded traffic is allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true

#Indicates whether the peer virtual network can access this virtual networks gateway
$peeringProperties.allowGatewayTransit = $false

#Indicates whether this virtual network uses peer virtual networks gateway
$peeringProperties.useRemoteGateways =$false

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Contoso_vnet1” -ResourceId “ContosotoWoodgrove” -Properties $peeringProperties

```

>[!IMPORTANT]
>在创建后此对等互连，vnet 状态显示**启动**。

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>步骤 4： 配置从第二个虚拟网络将到第一个虚拟网络的对等互连

在此步骤中，可以配置第二个虚拟网络之间在以上步骤 1 和 2 中创建的第一个虚拟网络对等互连。 以下示例脚本建立从虚拟网络对等互连**Woodgrove_vnet1**到**Contoso_vnet1**。

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network’s gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network’s gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

创建此对等互连，vnet 对等互连状态之后将显示**已连接**这两个对等节点。 现在，一个虚拟网络中的虚拟机可与其对等互连的虚拟网络中的虚拟机。

---
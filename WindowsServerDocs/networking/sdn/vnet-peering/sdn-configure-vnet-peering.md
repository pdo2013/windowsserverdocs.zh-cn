---
title: 配置虚拟网络对等
description: 配置虚拟网络对等互连涉及创建两个获取对等互连的虚拟网络。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 4d35501b8d876f2a178a4744d495125dea8da6c7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405821"
---
# <a name="configure-virtual-network-peering"></a>配置虚拟网络对等

>适用于：Windows Server

在此过程中，使用 Windows PowerShell 创建两个虚拟网络，每个虚拟网络具有一个子网。 然后，在两个虚拟网络之间配置对等互连，以启用它们之间的连接。

- [步骤 1.创建第一个虚拟网络](#step-1-create-the-first-virtual-network)

- [步骤 2.创建第二个虚拟网络](#step-2-create-the-second-virtual-network)

- [步骤 3.配置从第一个虚拟网络到第二个虚拟网络的对等互连](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [步骤 4.配置从第二个虚拟网络到第一个虚拟网络的对等互连](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>请记得更新环境的属性。

## <a name="step-1-create-the-first-virtual-network"></a>步骤 1： 创建第一个虚拟网络

在此步骤中，你将使用 Windows PowerShell 查找 HNV 提供程序逻辑网络，以创建包含一个子网的第一个虚拟网络。 下面的示例脚本创建 Contoso 的包含一个子网的虚拟网络。

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

在此步骤中，将创建一个具有一个子网的第二个虚拟网络。 下面的示例脚本创建 Woodgrove 的虚拟网络，其中包含一个子网。

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

## <a name="step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network"></a>步骤 3： 配置从第一个虚拟网络到第二个虚拟网络的对等互连

在此步骤中，你将配置在上两个步骤中创建的第一个虚拟网络与第二个虚拟网络之间的对等互连。 以下示例脚本建立从**Contoso_vnet1**到**Woodgrove_vnet1**的虚拟网络对等互连。

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
>创建此对等互连之后，vnet 状态显示为 "已**启动**"。

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>步骤 4： 配置从第二个虚拟网络到第一个虚拟网络的对等互连

在此步骤中，你将配置在上面的步骤1和步骤2中创建的第二个虚拟网络和第一个虚拟网络之间的对等互连。 以下示例脚本建立从**Woodgrove_vnet1**到**Contoso_vnet1**的虚拟网络对等互连。

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network's gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network's gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

创建此对等互连后，vnet 对等互连状态显示对两个对等节点的**连接**。 现在，一个虚拟网络中的虚拟机可以与对等互连虚拟网络中的虚拟机进行通信。

---
---
title: 创建、 删除或更新租户虚拟网络
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ef30dcc31593e15c36f846cf6d64afcd4b85f19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>创建、 删除或更新租户虚拟网络

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何创建、 删除和更新 HYPER-V 网络虚拟化虚拟网络后部署软件定义网络 (SDN)。  
  
使用 HYPER-V 网络虚拟化，你可以将隔离租户网络，以便每个租户网络是无跨连接可能完全单独的实体除非您将配置公众工作负载。  
  
你可以为租户创建新的虚拟网络，可以修改现有虚拟网络，如果租户不再需要特定资源，或者如果租户不再是你的客户，你可以删除租户虚拟网络。  
  
## <a name="create-a-new-virtual-network"></a>创建新的虚拟网络  
  
租户创建虚拟网络时，它被放置在 HYPER-V 主机上的唯一路由地域内。  
  
以下是步骤创建新的虚拟网络。  
  
1. 确定你想要创建虚拟子网 IP 地址前缀。   
2. 识别逻辑提供商的网络，在其租户交通隧道传输。   
3. 创建您在第 1 步中定义每个 IP 前缀至少一个虚拟子网。   
  
>[!NOTE]  
>每个虚拟网络下没有至少一个虚拟子网。 虚拟子网由 IP 前缀和参考之前定义的访问控制列表。  
  
（可选） 完成这些步骤后，你可以添加的以前创建的访问控制列表到虚拟子网，或为租户添加网关连接。    
  
下表包含示例子网 Id 和前缀的两个虚构租户。 租户 Fabrikam 有两个虚拟子网，而 Contoso 租户有三个虚拟个子网。  
  
  
  
租户名称  |虚拟子网 ID  |虚拟子网前缀    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
下面的示例脚本使用 Windows PowerShell 命令导出从**NetworkController**模块创建 Contoso 虚拟网络和一个子网：   
  
```  
import-module networkcontroller  
$URI = "https://ncrest.contoso.local"  
  
#Find the HNV Provider Logical Network  
  
$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   
  
#Find the Access Control List to user per virtual subnet  
  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
#Create the Virtual Subnet  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_WebTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"  
  
#Create the Virtual Network  
  
$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties  
  
  
```  
  
## <a name="modify-an-existing-virtual-network"></a>修改现有虚拟网络  
Windows PowerShell 可用于现有的虚拟子网或网络更新。   
  
运行以下示例脚本，将更新的资源只需放到具有相同的资源 ID 网络控制器 如果你租户 Contoso 想要向其虚拟网络中添加新的虚拟子网 (24.30.2.0/24)，你或 Contoso 管理员可以使用下面的脚本。  
  
```  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
$vnet = Get-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri  
  
$vnet.properties.AddressSpace.AddressPrefixes += "24.30.2.0/24"  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_DBTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
  
$vnet.properties.Subnets += $vsubnet  
  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -properties $vnet.properties  
  
```  
  
## <a name="delete-a-virtual-network"></a>删除虚拟网络  
  
你可以使用 Windows PowerShell 删除虚拟网络。  
  
下面的 Windows PowerShell 示例发出资源 id URI 到 HTTP 删除，从而中删除租户虚拟网络  
  
    Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  



---
title: 创建、删除或更新租户虚拟网络
description: 在本主题中，你将了解如何在部署软件定义的网络（SDN）后创建、删除和更新 Hyper-v 网络虚拟化虚拟网络。 Hyper-v 网络虚拟化可帮助隔离租户网络，使每个租户网络为单独的实体。 除非你配置了公共访问工作负荷，否则每个实体都不能进行交叉连接。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: pashort
author: shortpatti
ms.date: 08/24/2018
ms.openlocfilehash: 779c7bc4f6c4ff1e66fca68ced8b0eeb4d54abc5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406062"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>创建、删除或更新租户虚拟网络

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，你将了解如何在部署软件定义的网络（SDN）后创建、删除和更新 Hyper-v 网络虚拟化虚拟网络。 Hyper-v 网络虚拟化可帮助隔离租户网络，使每个租户网络为单独的实体。 除非你配置了公共访问工作负荷，否则每个实体都不能进行交叉连接。   
  
## <a name="create-a-new-virtual-network"></a>创建新的虚拟网络  
为租户创建虚拟网络会将其放在 Hyper-v 主机上的唯一路由域中。 在每个虚拟网络下，至少有一个虚拟子网。 虚拟子网通过 IP 前缀来定义，并引用以前定义的 ACL。  

创建新虚拟网络的步骤如下：

1. 确定要从中创建虚拟子网的 IP 地址前缀。   
2. 确定用于为租户通信建立隧道的逻辑提供程序网络。   
3. 为你在步骤1中标识的每个 IP 前缀至少创建一个虚拟子网。 
4. 可有可无将以前创建的 Acl 添加到虚拟子网，或添加租户的网关连接。 

下表包括两个虚构租户的示例子网 Id 和前缀。 租户 Fabrikam 有两个虚拟子网，而 Contoso 租户有三个虚拟子网。  
 
  
租户名称  |虚拟子网 ID  |虚拟子网前缀    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
下面的示例脚本使用从**NetworkController**模块导出的 Windows PowerShell 命令创建 Contoso 的虚拟网络和一个子网：   
  
```Powershell  
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
  
## <a name="modify-an-existing-virtual-network"></a>修改现有的虚拟网络  
你可以使用 Windows PowerShell 来更新现有虚拟子网或网络。   
  
运行下面的示例脚本时，只需将更新的资源放入具有相同资源 ID 的网络控制器。 如果你的租户 Contoso 想要将新的虚拟子网（24.30.2.0/24）添加到其虚拟网络，则你或 Contoso 管理员可以使用以下脚本。  
  
```PowerShell  
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
  
可以使用 Windows PowerShell 删除虚拟网络。  
  
以下 Windows PowerShell 示例通过向资源 ID 的 URI 发出 HTTP delete 来删除租户虚拟网络。  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```


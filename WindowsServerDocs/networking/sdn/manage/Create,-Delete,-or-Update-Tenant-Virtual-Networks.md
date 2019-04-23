---
title: 创建、 删除或更新租户虚拟网络
description: 在本主题中，您将学习如何创建、 删除和更新的 HYPER-V 网络虚拟化的虚拟网络，在部署软件定义网络 (SDN) 之后。 HYPER-V 网络虚拟化可帮助你隔离租户网络，以便每个租户网络是一个单独的实体。 每个实体都不跨连接可能，除非您配置了公共访问权限的工作负荷。
manager: dougkim
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
ms.date: 08/24/2018
ms.openlocfilehash: a125ec220b4769a57a6be30f1425283afb7f0fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838348"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>创建、删除或更新租户虚拟网络

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，您将学习如何创建、 删除和更新的 HYPER-V 网络虚拟化的虚拟网络，在部署软件定义网络 (SDN) 之后。 HYPER-V 网络虚拟化可帮助你隔离租户网络，以便每个租户网络是一个单独的实体。 每个实体都不跨连接可能，除非您配置了公共访问权限的工作负荷。   
  
## <a name="create-a-new-virtual-network"></a>创建新的虚拟网络  
为租户创建虚拟网络将将其放在一个唯一路由域中的 HYPER-V 主机上。 每个虚拟网络下没有至少一个虚拟子网。 虚拟子网获取 IP 前缀定义和引用以前定义的 ACL。  

若要创建新的虚拟网络的步骤如下：

1. 确定想要创建的虚拟子网的 IP 地址前缀。   
2. 标识在其租户通信进行隧道传输的逻辑提供程序网络。   
3. 在步骤 1 中创建的每个 IP 前缀标识的至少一个虚拟子网。 
4. （可选）将以前创建的 Acl 添加到的虚拟子网或租户添加网关连接。 

下表包含示例子网 Id 和前缀为两个虚构的租户。 Fabrikam 的租户有两个虚拟子网，而 Contoso 租户有三个虚拟子网。  
 
  
租户名称  |虚拟子网 ID  |虚拟子网前缀    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
下面的示例脚本使用 Windows PowerShell 命令从导出**NetworkController**模块来创建 Contoso 的虚拟网络和一个子网：   
  
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
可以使用 Windows PowerShell 更新现有的虚拟子网或网络。   
  
当运行以下示例脚本时，更新的资源是简单地说到网络控制器相同的资源 id。 如果你的租户 Contoso 想要将新的虚拟子网 (24.30.2.0/24) 添加到其虚拟网络，你或 Contoso 管理员可以使用以下脚本。  
  
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
  
以下 Windows PowerShell 示例将通过向的资源 id。 URI 发出 HTTP delete 删除租户虚拟网络  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```


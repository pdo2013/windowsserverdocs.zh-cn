---
title: 配置数据中心防火墙访问控制列表 (ACL)
description: 可以将特定的 Acl 应用于网络接口。  如果在网络接口连接到的虚拟子网上还设置了 Acl，则这两个 Acl 将应用，但网络接口 Acl 优先于虚拟子网 Acl 之上。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 2e3f365a820de67dec87ea209cbfeb22d9091616
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406103"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>配置数据中心防火墙访问控制列表（Acl）

>适用于：Windows Server（半年频道）、Windows Server 2016

创建 ACL 并将其分配给虚拟子网后，你可能需要使用单个网络接口的特定 ACL 替代虚拟子网上的默认 ACL。  在这种情况下，你可以将特定 Acl 直接应用到连接到 Vlan 的网络接口，而不是虚拟网络。 如果在连接到网络接口的虚拟子网上设置了 Acl，则这两个 Acl 将应用并排定虚拟子网 Acl 以上的网络接口 Acl 的优先级。

>[!IMPORTANT]
>如果尚未创建 ACL 并将其分配给虚拟网络，请参阅[使用访问控制列表（acl）管理数据中心网络流量流](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)，创建 acl 并将其分配给虚拟子网。  

在本主题中，我们将向你演示如何将 ACL 添加到网络接口。 还介绍了如何使用 Windows PowerShell 和网络控制器 REST API 从网络接口中删除 ACL。

- 示例：[将 ACL 添加到网络接口 @ no__t-0
- 示例：[使用 Windows Powershell 和网络控制器从网络接口中删除 ACL REST API @ no__t-0


## <a name="example-add-an-acl-to-a-network-interface"></a>例如：向网络接口添加 ACL
在此示例中，我们将演示如何将 ACL 添加到虚拟网络。 

>[!TIP]
>还可以在创建网络接口的同时添加 ACL。

1. 获取或创建要将 ACL 添加到的网络接口。
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. 获取或创建将添加到网络接口中的 ACL。
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. 将 ACL 分配到网络接口的 AccessControlList 属性
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. 在网络控制器中添加网络接口
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>例如：使用 Windows Powershell 和网络控制器从网络接口中删除 ACL REST API
在此示例中，我们将向你展示如何删除 ACL。 删除 ACL 会将默认的规则集应用于网络接口。 默认规则集允许所有出站流量，但会阻止所有入站流量。

>[!NOTE]
>如果要允许所有入站流量，则必须按照前面的[示例](#example-add-an-acl-to-a-network-interface)添加允许所有入站和所有出站流量的 ACL。


1. 获取将从中删除 ACL 的网络接口。<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. 将 $NULL 分配到 ipConfiguration 的 AccessControlList 属性。<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. 在网络控制器中添加网络接口对象。<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```

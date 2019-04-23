---
title: 配置数据中心防火墙访问控制列表 (ACL)
description: 可以将特定的 Acl 应用到网络接口。  如果 Acl 同时设置上的网络接口连接到的虚拟子网中，两个应用 Acl，但网络接口的 Acl 的优先级别是上面的 Acl 的虚拟子网。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 77a7706e39da265eedd65342a0ccf2174ab050ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853398"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>配置数据中心防火墙访问控制列表 (Acl)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

一旦创建 ACL 并分配给虚拟子网后，你可能想要重写该默认 ACL 上具有单个网络接口的特定 ACL 的虚拟子网。  在这种情况下，直接对附加到 Vlan，而不是虚拟网络的网络接口应用特定的 Acl。 如果必须连接到网络接口的虚拟子网中设置的 Acl，这两个 Acl 应用，并使网络接口的虚拟子网 Acl 上面的 Acl 的优先级。

>[!IMPORTANT]
>如果你尚未创建 ACL 和分配给虚拟网络，请参阅[使用访问控制列表 (Acl) 管理数据中心网络流量流到](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)创建 ACL 并将其分配给虚拟子网。  

在本主题中，我们向您怎样将 ACL 添加到网络接口。 我们还展示如何从网络接口使用 Windows PowerShell 和网络控制器 REST API 删除 ACL。

- [示例：将 ACL 添加到网络接口](#example-add-an-acl-to-a-network-interface)
- [示例：使用 Windows Powershell 和网络控制器 REST API 从网络接口删除 ACL](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>例如：将 ACL 添加到网络接口
在此示例中，我们演示了如何将 ACL 添加到虚拟网络。 

>[!TIP]
>还有可能要将 ACL 添加在同时创建网络接口。

1. 获取或创建将向其添加 ACL 的网络接口。
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. 获取或创建将添加到网络接口的 ACL。
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. 将 ACL 分配给网络接口的 AccessControlList 属性
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. 在网络控制器中添加网络接口
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>例如：使用 Windows Powershell 和网络控制器 REST API 从网络接口删除 ACL
在此示例中，我们如何删除 ACL。 删除 ACL 适用于网络接口的默认规则集。 默认规则集允许所有出站流量，但会阻止所有入站的流量。

>[!NOTE]
>如果你想要允许所有入站的流量，则必须按照前面[示例](#example-add-an-acl-to-a-network-interface)添加允许所有入站和所有出站流量的 ACL。


1. 获取网络接口将删除 ACL。<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. 将 $NULL 分配给 ip 配置的 AccessControlList 属性。<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. 网络控制器中添加网络接口对象。<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```

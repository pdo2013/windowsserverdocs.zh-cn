---
title: 配置数据中心防火墙访问 Acl)
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
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
ms.openlocfilehash: d5f7c4baad24a720e073857cb6c835167e5b419b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>配置数据中心防火墙访问 Acl)

>适用于：Windows Server（半年通道），Windows Server 2016

你可以与网络接口应用特定 Acl。  Acl 也虚拟子网网络接口连接到设置，如果同时 Acl 应用，但网络接口 Acl 优先虚拟子网 Acl 上方。

本主题包含以下部分。

- [示例： 将 ACL 添加到网络接口](#bkmk_addacl)
- [示例： 解除 ACL 网络接口的 Windows Powershell 和网络控制器 REST API 使用](#bkmk_removeacl)

##<a name="bkmk_addacl"></a>示例： 将 ACL 添加到网络接口

主题中[使用访问控制列出 (Acl) 到管理数据中心网络交通流](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)您学习了如何创建 ACL 和为其指定为虚拟子网。  但是，在某些情况下，你可能希望覆盖 virtaul 子网个别网络接口了特定 acl 该默认 ACL。  你将需要应用直接与而不是虚拟网络附加 Vlan 到的网络接口 Acl。

此示例中演示如何将 ACL 添加到虚拟网络。 

>[!NOTE]
>也可能将你创建网络接口次添加 ACL 它是。

###<a name="step-1-get-or-create-the-network-interface-to-which-you-will-add-the-acl"></a>第 1 步： 获取或创建网络接口将向其添加 ACL

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-get-or-create-the-acl-you-will-add-to-the-network-interface"></a>第 2 步： 获取或创建将添加到网络接口 ACL
你可以使用下面的示例命令或创建 ACL 获取。 

    $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"

###<a name="step-3-assign-the-acl-to-the-accesscontrollist-property-of-the-network-interface"></a>第 3 步： 将 ACL 分配给网络接口 AccessControlList 属性
可以使用下面的示例命令将 ACL 给 AccessControlList 属性。

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl

###<a name="step-4-add-the-network-interface-in-network-controller"></a>第 4 步： 添加网络接口在网络控制器
你可以使用下面的示例命令添加到网络控制器的网络接口。

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid


##<a name="bkmk_removeacl"></a>示例： 解除 ACL 网络接口的 Windows Powershell 和网络控制器 REST API 使用
你可以使用此示例中，如果你想要删除 ACL。 当您删除 ACL 时，规则的默认设置应用到网络接口。

规则套默认允许所有站通信，但会阻止所有传入的通信。

>[!NOTE]
>如果你想要允许所有的入站的通信，您必须遵循上例添加允许所有传入和传出所有流量 ACL。

###<a name="step-1-get-the-network-interface-from-which-you-will-remove-the-acl"></a>第 1 步： 获取网络接口从中将删除 ACL
你可以使用下面的示例命令检索网络接口。

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-assign-null-to-the-accesscontrollist-property-of-the-ipconfiguration"></a>第 2 步： 将 $NULL 分配给 ip 配置 AccessControlList 属性
可以使用下面的示例命令将 $NULL 分配给 AccessControlList 属性。

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $null

###<a name="step-3-add-the-network-interface-object-in-network-controller"></a>第 3 步： 添加网络接口对象在网络控制器
你可以使用下面的示例命令添加网络接口对象在网络控制器

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid


---
title: 使用网络虚拟装置虚拟网络
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: db46189931263d230f013431f319eb2497589dee
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>使用网络虚拟装置虚拟网络

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用此主题以了解如何将其租户虚拟网络上的网络虚拟装置部署。

你可以将网络虚拟设备添加到执行用户定义路由和端口镜像功能的网络。

本主题包含以下部分。

- [类型的网络虚拟装置](#bkmk_types)
- [部署虚拟一网络的设备](#bkmk_deploy)
- [示例： 用户定义的路由](#bkmk_routing)
- [示例： 端口镜像](#bkmk_port)

## <a name="bkmk_types"></a>类型的网络虚拟装置

有两种类型的虚拟虚拟网络可以使用的设备：

1. **用户定义的路由**。 用户定义路由替换虚拟装置路由功能虚拟网络上的分布式的路由器。  使用用户定义路由，虚拟装置用作之间虚拟网络上的虚拟子网路由器。
2. **端口镜像**。 使用镜像端口，所有网络通信输入或离开监视的端口是重复并发送到虚拟设备以供分析。 
## <a name="bkmk_deploy"></a>部署虚拟一网络的设备

部署虚拟设备，必须首先创建虚拟机 (VM)，其中包含设备，然后将 VM 连接到相应的虚拟网络个子网。

>[!NOTE]
>有关详细信息，请参阅[创建租户 VM 和连接到租户虚拟网络或 VLAN](Create-a-Tenant-VM.md)

某些设备需要多个虚拟网络适配器。 用于处理交通其他适配器时，通常是一个网络适配器致力于装置管理。 

如果你的设备需要多个网络适配器，您必须在网络控制器创建每个网络接口。 

您还必须分配接口 ID 为每个不同的虚拟子网上的其他适配器每台主机上。

您已完成网络虚拟装置部署后，你可以使用设备用户定义路由、 端口镜像或两者的。

##<a name="bkmk_routing"></a>示例： 用户定义的路由

对于大多数环境你仅将需要已由虚拟网络分布式路由器系统路线。 但是，你可能需要创建路线表添加一个或多个路线在特定的情况下，如：

* 通过你的本地网络 internet 隧道强制。
* 使用您的环境中的虚拟装置。

对于这些方案中，你必须创建路线表，并添加到表的用户定义路线。 你有多个路线表，并相同的路线表可关联到一个或多个个子网。 

每个子网只能关联到单个路线表。 所有 Vm 子网中的都使用关联到该子网路线表。

个子网依赖系统路线，直到路线表关联到子网。 存在关联后，路由完成基于上长前缀匹配 (LPM) 之间用户定义路线并系统路线。 

如果多个使用相同的 LPM 匹配的路线，然后用户定义的路线首先选中-之前的系统路线。 

###<a name="step-1-create-the-route-table-properties"></a>第 1 步： 创建路线表属性

该表路线将包含所有用户定义的路线。  系统路线规则上面定义仍然适用。

可以使用下面的示例命令创建路线表格属性。

    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties

###<a name="step-2-add-a-route-to-the-route-table-properties"></a>第 2 步： 添加路由到路线表属性

此升级方式显示 12.0.0.0/8 子网发送到任何通信应获取发送虚拟在 192.168.1.10 路由设备。  请务必设备已连接到虚拟网络分配给网络接口该 ip 虚拟网络适配器。

可以使用下面的示例命令添加路由到路线表格属性。

    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route

你可以通过为你想要定义每个路线的重复此步骤添加更多条路线。
s
###<a name="step-3-add-the-route-table-to-network-controller"></a>第 3 步： 将路线表添加到网络控制器
可以使用下面的示例命令添加到网络控制器的路线的表。

    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties

###<a name="step-4-apply-the-route-table-to-the-virtual-subnet"></a>第 4 步： 将路线表应用到虚拟子网
 
当你到虚拟子网应用路线表时，Tenant1_Vnet1 网络中的第一个虚拟子网使用路线表。 可以根据需要为尽可能多个子网虚拟网络中的分配路线表。

可以使用下面的示例命令将应用到虚拟子网路线表。

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid

尽快向虚拟网络应用路线表的通信到虚拟装置。 你必须配置路由表虚拟装置转发的通信方式是适合您的环境中。

##<a name="bkmk_port"></a>示例： 端口镜像

此示例中允许你，以便交通镜像到 Appliance_Ethernet1 配置 MyVM_Ethernet1 的交通。

此示例中假定已已经部署了两个虚拟机的功能，其中一个作为设备和其中一个作为 VM 监控镜像。

请务必设备具有管理的第二个网络接口，因为镜像作为上 Appliance_Ethernet1 目标启用后，它将不再接收有配置 IP 接口发送到的交通。

###<a name="step-1-get-the-virtual-network-on-which-your-vms-are-located"></a>第 1 步： 获取虚拟网络你 Vm 位于
你可以使用下面的示例命令以获取虚拟网络。

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"

###<a name="step-2-get-the-network-controller-network-interfaces-for-the-mirroring-source-and-destination"></a>第 2 步： 获取网络控制器网络接口镜像源代码和目的地
可以使用下面的示例命令以获取网络控制器网络接口镜像源代码和目的地。

    $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
    $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-3-create-a-serviceinsertionproperties-object-to-contain-the-port-mirroring-rules-and-the-element-which-represents-the-destination-interface"></a>第 3 步： 创建 serviceinsertionproperties 对象包含镜像规则和这表示目标接口元素端口
下面示例命令用于创建目的地 serviceinsertionproperties 对象。

    $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
    $portMirror.Priority = 1

###<a name="step-4-create-a-serviceinsertionrules-object-to-contain-the-rules-that-must-be-matched-in-order-for-the-traffic-to-be-sent-to-the-appliance"></a>第 4 步： 创建 serviceinsertionrules 对象包含必须匹配会发送到设备的通信的顺序规则

规则定义如下匹配项所有通信，传入和传出，这表示传统镜像。  如果你感兴趣的特定端口或特定的源目的地镜像，你可以调整这些规则。

可以使用下面的示例命令创建 serviceinsertionproperties 对象。

    $portmirror.ServiceInsertionRules = [Microsoft.Windows.NetworkController.ServiceInsertionRule[]]::new(1)

    $portmirror.ServiceInsertionRules[0] = [Microsoft.Windows.NetworkController.ServiceInsertionRule]::new()
    $portmirror.ServiceInsertionRules[0].ResourceId = "Rule1"
    $portmirror.ServiceInsertionRules[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionRuleProperties]::new()

    $portmirror.ServiceInsertionRules[0].Properties.Description = "Port Mirror Rule"
    $portmirror.ServiceInsertionRules[0].Properties.Protocol = "All"
    $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeStart = "0"
    $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeEnd = "65535"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeStart = "0"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeEnd = "65535"
    $portmirror.ServiceInsertionRules[0].Properties.SourceSubnets = "*"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationSubnets = "*"

###<a name="step-5-create-a-serviceinsertionelements-object-to-contain-the-network-interface-of-the-appliance-you-are-mirroring-to"></a>步骤 5： 创建包含你镜像到设备的网络接口 serviceinsertionelements 对象
可以使用下面的示例命令创建网络接口 serviceinsertionelements 对象。

    $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

    $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
    $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
    $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

    $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
    $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
    $portmirror.ServiceInsertionElements[0].Properties.Order = 1

###<a name="step-6-add-the-service-insertion-object-in-network-controller"></a>第 6 步： 添加到网络控制器的服务插入对象
当你发出此命令时，将停止设备网络界面的上一步中指定的所有通信。

可以使用下面的示例命令添加到网络控制器的服务插入对象。

    $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"

###<a name="step-7-update-the-network-interface-of-the-source-to-be-mirrored"></a>第 7 步： 更新网络接口镜像的源
可以使用下面的示例命令以进行更新的网络接口。

    $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
    $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId

当你完成这些步骤后时，从 MyVM_Ethernet1 界面交通 Appliance_Ethernet1 接口镜像。
 

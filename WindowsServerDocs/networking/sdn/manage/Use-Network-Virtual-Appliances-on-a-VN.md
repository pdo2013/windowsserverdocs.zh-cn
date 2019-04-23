---
title: 在虚拟网络上使用网络虚拟设备
description: 在本主题中，您将学习如何部署租户虚拟网络上的网络虚拟设备。 您可以将网络虚拟设备添加到执行用户定义的路由和端口镜像功能的网络。
manager: dougkim
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
ms.date: 08/30/2018
ms.openlocfilehash: e715a782651a5b9867f3b45251fd6ea6e4a9e4f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847368"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>在虚拟网络上使用网络虚拟设备

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，您将学习如何部署租户虚拟网络上的网络虚拟设备。 您可以将网络虚拟设备添加到执行用户定义的路由和端口镜像功能的网络。

## <a name="types-of-network-virtual-appliances"></a>类型的网络虚拟设备

可以使用虚拟设备的两种类型之一：

1. **用户定义的路由**-虚拟网络上的分布式的路由器替换为虚拟设备的路由功能。  使用用户定义路由时，虚拟设备获取用作虚拟网络上的虚拟子网之间的路由器。

2. **端口镜像**-所有网络流量进入或离开受监视的端口是复制并发送到虚拟设备以进行分析。 


## <a name="deploying-a-network-virtual-appliance"></a>部署网络虚拟设备

若要部署网络虚拟设备，你必须先创建包含该设备的 VM，然后将 VM 连接到相应的虚拟网络子网。 有关更多详细信息，请参阅[创建租户 VM 和连接到租户虚拟网络或 VLAN](Create-a-Tenant-VM.md)。

某些设备需要多个虚拟网络适配器。 通常情况下，一个网络适配器专用于设备管理时其他适配器处理通信。  如果你的设备需要多个网络适配器，则必须在网络控制器中创建每个网络接口。 你还必须分配为每个位于不同的虚拟子网的其他适配器的每个主机上的接口 ID。

部署网络虚拟设备后，你可以定义路由，移植镜像和 / 或使用设备。 


## <a name="example-user-defined-routing"></a>例如：用户定义的路由

对于大多数环境中，只需通过虚拟网络的分布式路由器已定义的系统路由。 但是，您可能需要创建一个路由表并在特定情况下，添加一个或多个路由，例如：

- 强制隧道连接到 Internet 的本地网络。
- 使用您的环境中的虚拟设备。

对于这些方案，必须创建一个路由表并向表中添加用户定义的路由。 可以有多个路由表，并可以将关联到一个或多个子网的同一个路由表。 只能将关联到同一个路由表的每个子网。 子网中的所有 Vm 都使用路由表关联到子网。

子网依赖于系统路由，直到路由表获取到的子网相关联。 存在关联后，路由是基于完成上最长前缀匹配 (LPM) 在用户定义路由和系统路由之间。 如果有多个路由的 LPM 匹配情况相同，则用户定义的路由是在之前系统路由的第一次的选择。
 
**过程：**

1. 创建路由表属性，它包含所有用户定义的路由。<p>根据上面定义的规则仍适用系统路由。

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. 将路由添加到路由表属性。<p>12.0.0.0/8 子网发往任何路由到虚拟设备在 192.168.1.10 路由。 设备必须具有与该 IP 分配给网络接口附加到虚拟网络的虚拟网络适配器。

   ```PowerShell
    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route
   ```
   >[!TIP]
   >如果你想要添加更多的路由，重复此步骤为你想要定义每个路由。

3. 将路由表添加到网络控制器。

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. 适用于虚拟子网的路由表。<p>当将路由表应用到的虚拟子网时，Tenant1_Vnet1 网络中的第一个虚拟子网将使用的路由表。 根据需要，可以将路由表分配到尽可能多的虚拟网络中的子网。

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

只要将路由表应用到虚拟网络，流量转发到虚拟设备。 必须在虚拟设备转发的流量，以适合你的环境的方式来配置路由表。

## <a name="example-port-mirroring"></a>例如：端口镜像

在此示例中，您将流量配置为 MyVM_Ethernet1 到镜像 Appliance_Ethernet1。  我们假定您已经部署了两个 Vm，一个为该设备，作为 VM 要使用镜像监视另一个。 

设备必须具有管理的第二个网络接口。 启用镜像作为 Appliciance_Ethernet1 上的目标后，它不会再收到流量去往存在配置的 IP 接口。


**过程：**

1. 获取 Vm 位于虚拟网络。

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. 获取镜像的源和目标网络控制器网络接口。

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. 创建 serviceinsertionproperties 对象，以包含端口镜像的规则和表示目标接口的元素。

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. 创建 serviceinsertionrules 对象，以包含必须按顺序发送到设备的流量匹配的规则。<p>匹配以下所有流量，入站和出站，它表示传统镜像都定义的规则。  如果您感兴趣的特定端口或特定的源/目标镜像，则可以调整这些规则。

   ```PowerShell
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
   ```

5. 创建包含镜像的设备的网络接口的 serviceinsertionelements 对象。

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. 网络控制器中添加服务插入对象。<p>时发出此命令，到设备的所有流量的都网络接口指定在上一步骤会停止。

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. 更新源要镜像的网络接口。

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

完成这些步骤后，Appliance_Ethernet1 接口镜像 MyVM_Ethernet1 接口的流量。
 
---
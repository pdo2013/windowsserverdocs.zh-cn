---
title: 在虚拟网络上使用网络虚拟设备
description: 本主题介绍如何在租户虚拟网络上部署网络虚拟设备。 可以将网络虚拟设备添加到执行用户定义的路由和端口镜像功能的网络中。
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
ms.openlocfilehash: 21f8698236e5358e0909ad0ac43a6a220013fee4
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869927"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>在虚拟网络上使用网络虚拟设备

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何在租户虚拟网络上部署网络虚拟设备。 可以将网络虚拟设备添加到执行用户定义的路由和端口镜像功能的网络中。

## <a name="types-of-network-virtual-appliances"></a>网络虚拟设备的类型

可以使用两种类型的虚拟设备之一：

1. **用户定义的路由**-将虚拟网络上的分布式路由器替换为虚拟设备的路由功能。  使用用户定义的路由时，虚拟设备将用作虚拟网络上虚拟子网之间的路由器。

2. **端口镜像**-会复制进入或离开监视的端口的所有网络流量，并将其发送到虚拟设备进行分析。 


## <a name="deploying-a-network-virtual-appliance"></a>部署网络虚拟设备

若要部署网络虚拟设备，你必须首先创建一个包含该设备的 VM，然后将该 VM 连接到相应的虚拟网络子网。 有关更多详细信息，请参阅[创建租户 VM 并连接到租户虚拟网络或 VLAN](Create-a-Tenant-VM.md)。

某些设备需要多个虚拟网络适配器。 通常，一个网络适配器专用于设备管理，而其他适配器则处理流量。  如果设备需要多个网络适配器，则必须在网络控制器中创建每个网络接口。 还必须在每个主机上为不同虚拟子网上的其他每个适配器分配一个接口 ID。

部署网络虚拟设备后，可以使用该设备进行定义的路由、移植镜像或同时使用这两者。 


## <a name="example-user-defined-routing"></a>例如：用户定义的路由

对于大多数环境，只需要虚拟网络的分布式路由器已经定义的系统路由。 不过，您可能需要创建一个路由表并在特定情况下添加一个或多个路由，例如：

- 通过本地网络强制隧道连接到 Internet。
- 在您的环境中使用虚拟设备。

对于这些情况，您必须创建一个路由表，并将用户定义的路由添加到表中。 你可以有多个路由表，并且可以将相同的路由表关联到一个或多个子网。 只能将每个子网关联到单个路由表。 子网中的所有 Vm 都使用与子网关联的路由表。

在将路由表关联到子网之前，子网依赖于系统路由。 存在关联后，将根据用户定义的路由和系统路由之间的最长前缀匹配（LPM）完成路由。 如果有多个路由的 LPM 匹配情况相同，则首先选择 "用户定义的路由"，然后选择系统路由。
 
**方法**

1. 创建路由表属性，其中包含所有用户定义的路由。<p>根据上面定义的规则，系统路由仍适用。

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. 向路由表属性添加路由。<p>任何发送到 12.0.0.0/8 子网的路由都将路由到192.168.1.10 上的虚拟设备。 设备必须将虚拟网络适配器连接到虚拟网络，并将该 IP 分配到网络接口。

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
   >如果要添加更多路由，请对要定义的每个路由重复此步骤。

3. 将路由表添加到网络控制器。

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. 将路由表应用到虚拟子网。<p>将路由表应用到虚拟子网时，Tenant1_Vnet1 网络中的第一个虚拟子网使用路由表。 你可以根据需要将路由表分配给虚拟网络中的多个子网。

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

将路由表应用到虚拟网络后，会立即将流量转发到虚拟设备。 您必须将虚拟设备中的路由表配置为以适合您的环境的方式转发流量。

## <a name="example-port-mirroring"></a>例如：端口镜像

在此示例中，将 MyVM_Ethernet1 的流量配置为 mirror Appliance_Ethernet1。  假设你已部署了两个 Vm，一个作为设备，另一个用作 VM 来监视镜像。 

设备必须具有另一个用于管理的网络接口。 在 Appliciance_Ethernet1 上启用镜像作为目标后，它将不再接收目标为在其中配置的 IP 接口的流量。


**方法**

1. 获取 Vm 所在的虚拟网络。

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. 获取用于镜像源和目标的网络控制器网络接口。

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. 创建 serviceinsertionproperties 对象以包含端口镜像规则和表示目标接口的元素。

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. 若要将流量发送到设备，请创建一个 serviceinsertionrules 对象，以包含必须匹配的规则。<p>下面定义的规则与所有流量（入站和出站）都匹配，后者表示传统镜像。  如果你有兴趣镜像特定端口或特定源/目标，则可以调整这些规则。

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

5. 创建 serviceinsertionelements 对象以包含镜像设备的网络接口。

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. 在网络控制器中添加服务插入对象。<p>发出此命令时，与上一步骤中指定的设备网络接口的所有流量都会停止。

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. 更新要镜像的源的网络接口。

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

完成这些步骤后，Appliance_Ethernet1 接口将镜像来自 MyVM_Ethernet1 接口的流量。
 
---
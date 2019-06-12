---
title: 为负载平衡和网络地址转换 (NAT) 配置软件负载平衡器
description: 本主题是软件定义的网络指南如何管理租户工作负荷和 Windows Server 2016 中的虚拟网络的一部分。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 4d53c4bcbe1f37f532f2861d5669201959a9f091
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446669"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>为负载平衡和网络地址转换 (NAT) 配置软件负载平衡器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你可以使用本主题以了解如何使用软件定义网络\(SDN\)软件负载均衡器\(SLB\)提供出站网络地址转换\(NAT\)，入站 NAT，或应用程序的多个实例之间的负载平衡。

## <a name="software-load-balancer-overview"></a>软件负载均衡器概述

SDN 软件负载均衡器\(SLB\)到您的应用程序提供高可用性和网络性能。 它是第 4 层\(TCP、 UDP\)负载均衡器的云服务或虚拟机中负载均衡器集定义中的健康服务实例之间分配传入流量。

配置 SLB 来执行以下操作：

- 负载平衡传入流量的虚拟网络到虚拟机的外部\(Vm\)，也称为公共 VIP 负载平衡。
- 传入流量进行负载平衡的虚拟网络中的 vm、 云服务中的 Vm 之间或在本地计算机和跨界虚拟网络中的 Vm 之间。 
- 将从虚拟网络的 VM 网络流量转发到外部目标使用网络地址转换 (NAT)，也称为出站 nat。
- 将外部流量转发到特定的 VM，也称为入站 nat。




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>例如：创建负载均衡的虚拟网络上的两个 Vm 的池的公共 VIP

在此示例中，您创建负载均衡器对象与公共 VIP 和两个 Vm 作为池成员，若要为请求提供服务到 VIP。 此示例代码还将添加 HTTP 运行状况探测来检测是否其中一个池成员变为无响应。

1. 准备负载均衡器对象。

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. 将分配到前端的 IP 地址，通常称为虚拟 IP (VIP)。<p>VIP 必须为一个逻辑网络 IP 池提供给负载均衡器管理器中未用的 IP。 

   ```PowerShell
    $VIPIP = "10.127.134.5"
    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException
    
    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"
      
    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig
   ```

3. 后端地址池，其中包含动态 Ip (Dip) 组成的 Vm 负载平衡集的成员分配。 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. 定义运行状况探测的负载均衡器用于确定后端池成员的运行状况状态。<p>在此示例中，定义 HTTP 探测的查询的 RequestPath"/ health.htm。"  查询运行由 IntervalInSeconds 属性指定每隔 5 秒。<p>运行状况探测必须接收 HTTP 响应代码为 200 的探测以使其处于正常状态的后端 IP，请考虑为 11 次连续查询。 如果不正常的后端 IP，它不从负载均衡器接收流量。

   >[!IMPORTANT]
   >不会阻止与第一个 IP 子网中的流量的任何访问控制列表 (Acl)，因为这是用于探测的资助创始费点适用于后端 IP。

   使用下面的示例来定义运行状况探测。

   ```PowerShell
    $Probe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $Probe.ResourceId = "Probe1"
    $Probe.ResourceRef = "/loadBalancers/$LBResourceId/Probes/$($Probe.ResourceId)"

    $Probe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties
    $Probe.properties.Protocol = "HTTP"
    $Probe.properties.Port = "80"
    $Probe.properties.RequestPath = "/health.htm"
    $Probe.properties.IntervalInSeconds = 5
    $Probe.properties.NumberOfProbes = 11

    $LoadBalancerProperties.Probes += $Probe
   ```

5. 定义负载均衡规则以发送到后端 IP 的前端 IP 在到达的流量。  在此示例中后, 端池接收到端口 80 TCP 流量。<p>使用下面的示例定义负载均衡规则：

   ```PowerShell
   $Rule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
   $Rule.ResourceId = "webserver1"

   $Rule.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
   $Rule.Properties.FrontEndIPConfigurations += $FrontEndIPConfig
   $Rule.Properties.backendaddresspool = $BackEndAddressPool 
   $Rule.Properties.protocol = "TCP"
   $Rule.Properties.FrontEndPort = 80
   $Rule.Properties.BackEndPort = 80
   $Rule.Properties.IdleTimeoutInMinutes = 4
   $Rule.Properties.Probe = $Probe

   $LoadBalancerProperties.loadbalancingRules += $Rule
   ```

6. 将负载均衡器配置添加到网络控制器。<p>使用下面的示例将负载均衡器配置添加到网络控制器：

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. 按照下一步的示例，将网络接口添加到此后端池。


## <a name="example-use-slb-for-outbound-nat"></a>例如：出站 NAT 使用 SLB

在此示例中，与后端池提供出站 NAT 功能的虚拟网络的专用地址空间上的虚拟机，若要连接到 internet 的出站配置 SLB。 

1. 创建负载均衡器属性、 前端 IP 和后端池。

   ```PowerShell
    import-module NetworkController
    $URI = "https://sdn.contoso.com"

    $LBResourceId = "OutboundNATMMembers"
    $VIPIP = "10.127.134.6"

    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"

    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig

    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"
    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

2. 定义出站 NAT 规则。

   ```PowerShell
    $OutboundNAT = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRule
    $OutboundNAT.ResourceId = "onat1"
    
    $OutboundNAT.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRuleProperties
    $OutboundNAT.properties.frontendipconfigurations += $FrontEndIPConfig
    $OutboundNAT.properties.backendaddresspool = $BackEndAddressPool
    $OutboundNAT.properties.protocol = "ALL"

    $LoadBalancerProperties.OutboundNatRules += $OutboundNAT
   ```

3. 网络控制器中添加负载均衡器对象。

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

4. 按照下一步的示例，添加你想要提供的 internet 访问的网络接口。

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>例如：将网络接口添加到后端池
在此示例中，你将网络接口添加到后端池。  必须为可处理对 VIP 请求每个网络接口重复此步骤。 

您还可以重复此过程将其添加到多个负载均衡器对象的单个网络接口上。 例如，如果您有 web server VIP 的负载均衡器对象和单独的负载均衡器对象，以提供出站 nat。
    
1. 获取包含要添加网络接口的后端池的负载均衡器对象。

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
   ```

2. 获取网络接口，并将 backendaddress 池添加到 loadbalancerbackendaddresspools 数组。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. 将网络接口以应用更改。 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>例如：使用软件负载均衡器转发的流量
如果你需要将虚拟 IP 映射到虚拟网络上的单个网络接口，而无需定义单个端口，可以创建 L3 转发规则。  此规则将转发所有流量传入和传出 VM 通过 PublicIPAddress 对象中包含已分配的 VIP。

如果定义了 VIP 和 DIP 作为同一子网，则等效于执行 L3 转发，而无需 nat。

>[!NOTE]
>此过程不需要您创建负载均衡器对象。  将 PublicIPAddress 分配给网络接口是足够的信息来执行其配置的软件负载均衡器。


1. 创建包含 VIP 的公共 IP 对象。

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. 将 PublicIPAddress 分配给网络接口。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>例如：使用软件负载均衡器转发的流量使用的动态分配的 VIP
此示例重复上一示例中，相同的操作，但它会自动分配可用池中的 Vip 中而不是指定特定的 IP 地址的负载均衡器的 VIP。 

1. 创建包含 VIP 的公共 IP 对象。

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. 要确定哪个 IP 地址分配的 PublicIPAddress 资源的查询。

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   IpAddress 属性包含分配的地址。  输出将类似于此：
   ```
    Counters                 : {}
    ConfigurationState       :
    IpAddress                : 10.127.134.2
    PublicIPAddressVersion   : IPv4
    PublicIPAllocationMethod : Dynamic
    IdleTimeoutInMinutes     : 4
    DnsSettings              :
    ProvisioningState        : Succeeded
    IpConfiguration          :
    PreviousIpConfiguration  :
   ```
 
3. 将 PublicIPAddress 分配给网络接口。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
   ## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>例如：删除正用于转发流量的 PublicIP 地址并将其返回到 VIP 池
   此示例删除由前面的示例创建的 PublicIPAddress 资源。  一旦删除 PublicIPAddress，对 PublicIPAddress 的引用将自动从网络接口、 流量将停止正在转发和 IP 地址将返回到以便重复使用的公共 VIP 池。  

4. 删除 PublicIP

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 
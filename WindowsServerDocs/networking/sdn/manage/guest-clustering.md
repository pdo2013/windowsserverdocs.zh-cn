---
title: 在虚拟网络群集的访客
description: 本主题介绍的软件定义网络指南如何管理租户工作负载和 Windows Server 2016 中的虚拟网络的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: shortpatti
ms.openlocfilehash: 5cab7e7c0ca0af848b4b58362388701cc4357860
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="guest-clustering-in-a-virtual-network"></a>在虚拟网络群集的访客

>适用于：Windows Server（半年通道），Windows Server 2016

连接到虚拟网络的虚拟机获准仅用于网络控制器具备以便通信网络上的 IP 地址。  这意味着群集技术，需要浮动 IP 地址，如 Microsoft 故障转移群集需要一些额外的步骤才能正常工作。

对进行浮动 IP 到达方法是使用软件负载平衡 \(SLB\) 虚拟 IP \(VIP\)。  必须配置端口该 IP 上健康探测软件负载平衡以便 SLB 将定向到当前已该 IP 该计算机的交通。

## <a name="example-load-balancer-configuration"></a>示例： 负载平衡配置

此示例中假定已已经创建了虚拟机的功能，这将变得群集节点，连接到虚拟网络它们。  有关指南，请参阅[创建 VM 和连接到租户虚拟网络或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。  

在此示例中，你将创建虚拟 IP 地址 (192.168.2.100) 代表群集的浮动 IP 地址和配置健康探测监控 TCP 端口 59999 以确定哪个节点处于活动状态一。

### <a name="step-1-select-the-vip"></a>第 1 步： 选择 VIP
准备通过分配 VIP IP 地址。  此地址可以相同子网群集节点中的任何未使用或保留地址。  VIP 必须匹配群集浮动地址。

    $VIP = "192.168.2.100"
    $subnet = "Subnet2"
    $VirtualNetwork = "MyNetwork"
    $ResourceId = "MyNetwork_InternalVIP"

### <a name="step-2-create-the-load-balancer-properties-object"></a>第 2 步： 创建负载平衡属性对象

可以使用下面的示例命令来创建负载平衡属性对象。

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

### <a name="step-3-create-a-front-end-ip-address"></a>第 3 步： 创建 front\ 最终 IP 地址

可以使用下面的示例命令创建 front\ 最终 IP 地址。

    $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEnd.resourceId = "Frontend1"
    $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
    $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
    $FrontEnd.properties.privateIPAddress = $VIP
    $FrontEnd.properties.privateIPAllocationMethod = "Static"

### <a name="step-4-create-a-back-end-pool-to-contain-the-cluster-nodes"></a>第 4 步： 创建包含群集节点 back\ 结束池

你可以使用下面的示例命令创建 back\ 结束池

    $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
    $BackEnd.resourceId = "Backend1"
    $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
    $LoadBalancerProperties.backendAddressPools += $BackEnd

### <a name="step-5-add-a-probe"></a>第 5 步： 添加探测
探测有必要检测浮动地址当前处于活动状态的群集节点。

>[!NOTE]
>在下面的定义端口 VM 永久地址探测查询。  端口必须仅活动节点上的响应。 

    $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties.protocol = "TCP"
    $lbprobe.properties.port = "59999"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11

### <a name="step-5-add-the-load-balancing-rules"></a>第 5 步： 添加负载平衡规则
此步骤创建负载平衡 TCP 端口 1433年规则。  你可以修改协议并根据需要端口。  你也可以重复此步骤多次的其他端口，此 VIP 上的 protcols。  很重要，因为这会告知负载平衡发送给就地原始 VIP 节点数据包 $true 到设置 EnableFloatingIP。

    $LoadBalancerProperties.loadbalancingRules += $lbrule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
    $lbrule.properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
    $lbrule.ResourceId = "Rules1"

    $lbrule.properties.frontendipconfigurations += $FrontEnd
    $lbrule.properties.backendaddresspool = $BackEnd 
    $lbrule.properties.protocol = "TCP"
    $lbrule.properties.frontendPort = $lbrule.properties.backendPort = 1433 
    $lbrule.properties.IdleTimeoutInMinutes = 4
    $lbrule.properties.EnableFloatingIP = $true
    $lbrule.properties.Probe = $lbprobe

### <a name="step-5-create-the-load-balancer-in-network-controller"></a>步骤 5： 创建负载平衡网络控制器

你可以使用下面的示例命令创建负载平衡。

    $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force

### <a name="step-6-add-the-cluster-nodes-to-the-backend-pool"></a>第 6 步： 添加到的后端池的群集节点

此示例显示添加两个池成员，但你可以添加多个节点池根据需要为群集。

    # Cluster Node 1

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

你已创建负载平衡并后端池中添加网络接口后，你就可以配置群集。  如果你使用的 Microsoft 故障转移群集你可以继续下一个示例。 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>示例 2 部分： 配置 Microsoft 故障转移群集

你可以使用下面的步骤配置故障转移群集。

### <a name="step-1-install-failover-clustering"></a>第 1 步： 安装故障转移群集

可以使用下面的示例命令来安装和配置为故障转移群集属性。

    add-windowsfeature failover-clustering -IncludeManagementTools
    Import-module failoverclusters

    $ClusterName = "MyCluster"
   
    $ClusterNetworkName = "Cluster Network 1"
    $IPResourceName =  
    $ILBIP = “192.168.2.100” 

    $nodes = @("DB1", "DB2")

### <a name="step-2-create-the-cluster-on-one-node"></a>第 2 步： 创建一个节点上的群集

你可以使用下面的示例命令创建节点上的群集。

    New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]

### <a name="step-3-stop-the-cluster-resource"></a>第 3 步： 停止群集资源

你可以使用下面的示例命令停止群集资源。

    Stop-ClusterResource "Cluster Name" 

### <a name="step-4-set-the-cluster-ip-and-probe-port"></a>第 4 步： 将群集设置 IP 和探测端口
IP 地址必须匹配在之前的示例中，使用前端 ip 地址，而探测端口必须匹配上例中的探测端口。

    Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

### <a name="step-5-start-the-cluster-resources"></a>第 5 步： 启动群集资源

可以使用下面的示例命令以启动群集资源。

    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 

### <a name="step-6-add-the-remaining-nodes"></a>第 6 步： 添加其余节点

你可以使用下面的示例命令添加群集节点。

    Add-ClusterNode $nodes[1]

完成后的最后一步，群集处于活动状态。 将在活动节点引导指定端口上转到 VIP 交通。
---
title: 虚拟网络中的来宾群集
description: 连接到虚拟网络的虚拟机只被允许使用网络控制器分配为在网络上通信的 IP 地址。  聚类分析技术需要浮动 IP 地址，如 Microsoft 故障转移群集，需要一些额外步骤才能正常工作。
manager: dougkim
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
ms.date: 08/26/2018
ms.openlocfilehash: fcd37ebb3739f1d7118ce41dfc61764486c920d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844958"
---
# <a name="guest-clustering-in-a-virtual-network"></a>虚拟网络中的来宾群集

>适用于：Windows 服务器 （半年频道），Windows Server 2016

连接到虚拟网络的虚拟机只被允许使用网络控制器分配为在网络上通信的 IP 地址。  聚类分析技术需要浮动 IP 地址，如 Microsoft 故障转移群集，需要一些额外步骤才能正常工作。

从而浮动 IP 可访问的方法是使用软件负载均衡器\(SLB\)虚拟 IP \(VIP\)。  必须使用运行状况探测端口上的 IP 配置软件负载均衡器，以便 SLB 将流量定向到当前具有该 IP 的计算机。


## <a name="example-load-balancer-configuration"></a>例如：负载均衡器配置

此示例假定，您已创建的将成为群集节点，并附加到虚拟网络的 Vm。  有关指南，请参阅[创建 VM 和连接到租户虚拟网络或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。  

在此示例将创建一个虚拟 IP 地址 (192.168.2.100) 来表示浮动 IP 地址的群集，并配置运行状况探测来监视 TCP 端口 59999，以确定哪一节点是活动。

1. 选择 VIP。<p>准备通过分配一个 VIP IP 地址，这可以是群集节点位于同一子网中的任何未使用或保留地址。  VIP 必须与群集的浮动地址匹配。

   ```PowerShell
   $VIP = "192.168.2.100"
   $subnet = "Subnet2"
   $VirtualNetwork = "MyNetwork"
   $ResourceId = "MyNetwork_InternalVIP"
   ```

2. 创建负载均衡器的属性对象。

   ```PowerShell
   $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

3. 创建 front\-结束 IP 地址。

   ```PowerShell
   $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
   $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
   $FrontEnd.resourceId = "Frontend1"
   $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
   $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
   $FrontEnd.properties.privateIPAddress = $VIP
   $FrontEnd.properties.privateIPAllocationMethod = "Static"
   ```

4. 创建备份\-端包含在群集节点的池。

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. 添加探测来检测浮动地址当前处于活动状态上的群集节点。 

   >[!NOTE]
   >针对 VM 的永久在下面定义的端口地址的探测查询。  端口必须只能响应的活动节点上。 

   ```PowerShell
   $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
   $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

   $lbprobe.ResourceId = "Probe1"
   $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
   $lbprobe.properties.protocol = "TCP"
   $lbprobe.properties.port = "59999"
   $lbprobe.properties.IntervalInSeconds = 5
   $lbprobe.properties.NumberOfProbes = 11
   ```

6. 添加负载均衡规则的 TCP 端口 1433年。<p>您可以修改协议和端口根据需要。  您可以多次重复此步骤，为其他端口和此 VIP 上的 protcols。  务必 EnableFloatingIP 属性设置为 $true 因为这将告知负载均衡器将数据包发送到与原始 VIP 就地节点。

   ```PowerShell
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
   ```

7. 在网络控制器中创建负载均衡器。

   ```PowerShell
   $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force
   ```

8. 将群集节点添加到后端池。<p>您可以添加任意多个节点到池根据需要为群集。

   ```PowerShell
   # Cluster Node 1

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force
   ```

   一旦创建负载均衡器并添加到后端池的网络接口后，你现可配置群集。  

9. （可选）如果使用 Microsoft 故障转移群集，继续进行下一个示例。 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>示例 2：配置 Microsoft 故障转移群集

可以使用以下步骤来配置故障转移群集。

1. 安装和配置故障转移群集的属性。

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"
   
   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =  
   $ILBIP = “192.168.2.100” 

   $nodes = @("DB1", "DB2")
   ```

2. 一个节点上创建群集。

   ```PowerShell
   New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]
   ```

3. 停止群集资源。

   ```PowerShell
   Stop-ClusterResource "Cluster Name" 
   ```

4. 将群集设置 IP 和探测端口。<p>IP 地址必须匹配在上一示例中，使用的前端 ip 地址和探测端口必须与上一示例中的探测端口匹配。

   ```PowerShell
   Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

5. 启动群集资源。

   ```PowerShell
    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 
   ```

6. 添加剩余的节点。

   ```PowerShell
   Add-ClusterNode $nodes[1]
   ```

_**群集处于活动状态。**_ 指定的端口上转到 VIP 的流量定向到活动节点。

---
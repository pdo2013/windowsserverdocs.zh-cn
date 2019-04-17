---
title: 已合并好的 NIC 成的 NIC 配置
description: 本主题提供有关如何配置汇聚 NIC datacenter 配置 Windows Server 2016 中的说明进行操作。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ac6c2915301b1cf64486f24c197ebbf22bb5e2e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-teamed-nic-configuration"></a>已合并好的 NIC 成的 NIC 配置

>适用于：Windows Server（半年通道），Windows Server 2016

以下部分中提供用于部署汇聚 NIC 切换 Embedded 组队 \(SET\) 搭配使用 NIC 配置中的说明进行操作。 本指南中的示例配置展示了两个 Hyper-V 主机、Hyper-V 主机 1，和 Hyper-V 主机 2。

## <a name="test-connectivity-between-source-and-destination"></a>测试之间源代码和目的地的连接

此部分中提供所需测试连接来源和目标 Hyper-V 主机之间的步骤。

下图显示了两个 Hyper-V 主机，**Hyper-V 主机 1**和**Hyper-V 主机 2**。 

这两个 Hyper-V 主机将有两个网络适配器。 每个主机，一个适配器已连接到 192.168.1.x/24 子和一个适配器已连接到 192.168.2.x/24 子网。

![Hyper-V 主机](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>测试 Hyper-V 虚拟交换机 NIC 连接

通过使用此步骤，你可以确保物理 NIC，以后将为其创建一个 Hyper-V 虚拟交换机用来，可以连接到目标主机。 

此测试演示通过使用一层 3 \(L3\)-或 IP 层-以及第二层 \(L2\) 虚拟本地区域网络连接 \(VLAN\)。

可以使用下面的 Windows PowerShell 命令以获取该网络适配器的属性。

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
     
下面是此命令示例结果。

|名称|InterfaceDescription|ifIndex|状态|MacAddress|LinkSpeed|
|-----|--------------------|-------|-----|----------|---------|
|
|测试 40G 1|Mellanox ConnectX 3 专业版的以太网适配器|11|向上|E4-1D-2D-07-43-D0|40 Gbps|

你可以使用以下命令之一以获取更多适配器属性，包括 IP 地址。

    Get-NetIPAddress -InterfaceAlias "Test-40G-1"

    Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
    
以下是示例这些命令的结果。

|参数|值|
|---------|-----|
|
|Ip 地址| 192.168.1.3|
|InterfaceIndex|11|
|InterfaceAlias|测试 40G 1|
|AddressFamily|IPv4|
|键入| 单址广播|
|PrefixLength|24|

### <a name="verify-that-nic-team-member-has-a-valid-ip-address"></a>验证 NIC 团队成员具有有效的 IP 地址

你可以使用此步骤以确认其他 NIC 团队或切换 Embedded 团队 \(SET\) 成员物理 Nic \(pNICs\) 也有一个有效的 IP 地址。

对于此步骤中，你可使用单独的子网 \ (xxx.xxx。**2**带句点 vs xxx.xxx。**1**。xxx\)，以便在将该适配器发送到目的地。 

否则为如果找到这两个 pNICs 相同子网上，Windows TCP/IP 堆栈负载平衡接口之间且简单的验证变得更复杂。

可以使用 Windows PowerShell 以下命令以获取该第二个的网络适配器的属性。

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

下面是此命令示例结果。

|名称 |InterfaceDescription |ifIndex |状态 |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|测试 40G 2 |Mellanox ConnectX 3 专业版的以太网 A....\ #2 |13 |向上 |E4-1D-2D-07-40-70 |40 Gbps|

你可以使用以下命令之一以获取更多适配器属性，包括 IP 地址。

    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$\_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

以下是示例这些命令的结果。

|参数|值|
|---------|-----|
|
|Ip 地址|192.168.2.3|
|InterfaceIndex|13|
|InterfaceAlias|测试 40G 2|
|AddressFamily|IPv4|
|键入|单址广播|
|PrefixLength|24|

### <a name="verify-that-additional-nics-have-valid-ip-addresses"></a>验证其他 Nic 具有有效的 IP 地址

你可以使用此步骤以确保另一个 NIC 具有有效的 IP 地址。

对于此步骤中，你可使用单独的子网 \ (xxx.xxx。**2**带句点 vs xxx.xxx。**1**。xxx\)，以便在将该适配器发送到目的地。 

否则为如果找到这两个 pNICs 相同子网上，Windows TCP/IP 堆栈负载平衡接口之间且简单的验证变得更复杂。

可以使用 Windows PowerShell 以下命令以获取该第二个的网络适配器的属性。

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

下面是此命令示例结果。

|名称 |InterfaceDescription |ifIndex |状态 |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|测试 40G 2 |Mellanox ConnectX 3 专业版的以太网 A....\ #2 |13 |向上 |E4-1D-2D-07-40-70 |40 Gbps|

你可以使用以下命令之一以获取更多适配器属性，包括 IP 地址。
    
    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

以下是示例这些命令的结果。

|参数|值|
|---------|-----|
|
|Ip 地址|192.168.2.3|
|InterfaceIndex|13|
|InterfaceAlias|测试 40G 2|
|AddressFamily|IPv4|
|键入|单址广播|
|PrefixLength|24|

### <a name="ensure-that-source-and-destination-can-communicate"></a>确保可以通信源代码和目的地

你可以使用此步骤验证双向通信 \ (来源到目标，在这两个 systems\ 反之亦然 ping)。  在以下示例中，**测试 NetConnection**使用 Windows PowerShell 命令时，但如果你希望你可以使用**ping**命令。 

    Test-NetConnection 192.168.1.5

下面是此命令示例结果。

|参数|值|
|---------|-----|
|
|计算机名称|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|测试 40G 1|
|SourceAddress|192.168.1.3|
|PingSucceeded|假|
|PingReplyDetails \(RTT\)|0 ms|

在某些情况下，你可能需要禁用高级安全 Windows 防火墙成功执行此测试。 如果你禁用防火墙，请记住安全，并且确保你配置符合你的组织的安全要求。

下面的示例命令允许您禁用所有防火墙配置文件。

    Set-NetFirewallProfile -All -Enabled False
    
禁用防火墙后，你可以使用以下命令以测试连接。

    Test-NetConnection 192.168.1.5

下面是此命令示例结果。

|参数|值|
|---------|-----|
|
|计算机名称|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|测试 40G 1|
|SourceAddress|192.168.1.3|
|PingSucceeded|假|
|PingReplyDetails \(RTT\)|0 ms|

### <a name="verify-connectivity-for-additional-nics"></a>对于其他 Nic 验证连接

使用此步骤中，您可以的包含 NIC 或一组团队中的所有后续 pNICs 重复上述步骤。

你可以使用以下命令以测试连接。
    
    Test-NetConnection 192.168.2.5

下面是此命令示例结果。

|参数|值|
|---------|-----|
|
|计算机名称|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|测试 40G 2|
|SourceAddress|192.168.2.3|
|PingSucceeded|假|
|PingReplyDetails \(RTT\)|0 ms|


## <a name="configure-vlans"></a>配置 Vlan

对于此步骤，Nic 处于**访问**模式。 但是你创建一个虚拟交换机用 Hyper-V \ (vSwitch \) 在本指南，稍后 VLAN 属性应用在 vSwitch 端口级别。 

切换可以主机多个 Vlan，因为是必需的顶部的机架 \(ToR\) 物理切换到已配置主干型其端口。

有关如何配置主干模式切换上的说明，请查阅或切换文档。

下图显示了使用具有 VLAN 101 每个的两个网络适配器的两个 Hyper-V 主机，并且在属性网络适配器的 VLAN 102 配置。

![配置虚拟本地区域网络](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)

### <a name="configure-the-vlan-ids-for-both-nics"></a>这两个 Nic 配置 VLAN Id

可以使用此步骤的安装在你的 Hyper-V 主机 Nic 配置 VLAN Id。

根据电气和网络标准电子工程师 \(IEEE\)，在物理服务质量 \(QoS\) 属性 NIC 采取措施，嵌入在 802.1 q 802.1 p 标题 \(VLAN\) 标题时配置 VLAN id。

#### <a name="configure-nic-test-40g-1"></a>配置 NIC 测试 40G 1

使用以下命令，将配置 VLAN ID 为第一个 NIC、测试 40G 1，然后查看结果配置。

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    
下面是此命令示例结果。

|名称 |显示名称| DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|测试 40G 1|VLAN ID|101|VlanID|{101}|


确保 VLAN ID 生效的网络适配器实现独立通过使用以下命令以重启该网络适配器。

    Restart-NetAdapter -Name "Test-40G-1"

你可以使用以下命令以确保是网络适配器状态**向上**之前。

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

下面是此命令示例结果。

|名称|InterfaceDescription|ifIndex| 状态|MacAddress|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|测试 40G 1|Mellanox ConnectX 3 专业版的以太网艾达…|11|向上|E4-1D-2D-07-43-D0|40 Gbps|

#### <a name="configure-nic-test-40g-2"></a>配置 NIC 测试 40G 2

使用以下命令，将第二个 NIC、测试 40G 2，然后查看结果配置为配置 VLAN ID。

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    

下面是此命令示例结果。

|名称 |显示名称| DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|测试 40G 2|VLAN ID|102|VlanID|{102}|

确保 VLAN ID 生效的网络适配器实现独立通过使用以下命令以重启该网络适配器。

`Restart-NetAdapter -Name "Test-40G-2"  `              

你可以使用以下命令以确保是网络适配器状态**向上**之前。

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

下面是此命令示例结果。

|名称|InterfaceDescription|ifIndex| 状态|MacAddress|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|测试 40G 2 |Mellanox ConnectX 3 专业版的以太网艾达… |11 |向上 |E4-1D-2D-07-43-D1 |40 Gbps|


### <a name="verify-connectivity"></a>验证连接

你可以使用此部分中验证连接后重新启动的网络适配器。 你可以在应用于这两个适配器 VLAN 标记后确认连接。 如果连接将失败，你可以检查切换 VLAN 配置或目标参与相同 VLAN。 

>[!IMPORTANT]
>在上一节中执行这些步骤后，可能需要几秒钟该设备重新启动并在网络上可用。

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>验证连接用于 NIC 测试-40G-1

第一个 nic 验证连接，你可以运行以下命令。

    Test-NetConnection 192.168.1.5
    
下面是此命令示例结果。

|参数|值|
|---------|-----|
|
|计算机名称|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|测试 40G 1|
|SourceAddress|192.168.1.5|
|PingSucceeded|如此|
|PingReplyDetails \(RTT\)|0 ms|

#### <a name="verify-connectivity-for-nic-test-40g-2"></a>验证 NIC 测试-40G-2 的连接性

可以使用此部分中 NIC 测试-40G-2 的测试的连接。

你可以使用以下命令以测试连接的第二个 NIC
    
    Test-NetConnection 192.168.2.5
    
下面是此命令示例结果。

|参数|值|
|---------|-----|
|
|计算机名称|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|测试 40G 2|
|SourceAddress|192.168.2.3|
|PingSucceeded|如此|
|PingReplyDetails \(RTT\)|0 ms|

一个**测试 NetConnection**或执行后立即出现的 ping 发生故障**重启 NetAdapter**并不少见，因此等待完全初始化，该网络适配器，然后再试一次。

如果 VLAN 101 连接的工作，但 VLAN 102 连接不起作用，该问题可能需要配置为所需的 VLAN 上允许端口交通交换机。 你可以通过临时将失败适配器设置为 VLAN 101 并重复连接测试检查此。

## <a name="configure-quality-of-service-qos"></a>配置服务 \(QoS\) 质量

下一步是配置优质的服务 \(QoS\)，需要在你首次安装 Windows Server 2016 功能数据中心桥 \(DCB\)。

下图成功配置上一步中的 Vlan 后演示 Hyper-V 主机。

![配置服务质量](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)

### <a name="install-data-center-bridging-dcb"></a>安装桥 \(DCB\) 数据中心

你可以使用此步骤以安装并启用 DCB。 在大多数情况下，此步骤 iWarp 实现为可选，但必须结构大规模，例如至于跨机架方案。

你可以使用以下命令在每个 Hyper-V 主机上安装 DCB。 

    Install-WindowsFeature Data-Center-Bridging

下面是此命令示例结果。

|成功 |需要重启 |退出代码|功能结果|
|------- |-------------- |--------- |-------------- |
|
|如此 |不 |成功| {数据中心桥}|

### <a name="set-the-qos-policies-for-smb-direct"></a>设置 SMB 直接 QoS 策略 

可以使用以下命令以直接 SMB 配置 QoS 策略。

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

下面是此命令示例结果。

|参数|值|
|---------|-----|
|
|名称 |SMB|
|所有者|组策略 \(Machine\)|
|NetworkProfile|所有|
|优先|127|
|JobObject|&nbsp;| 
|NetDirectPort|445
|PriorityValue|3

### <a name="set-policies-for-other-traffic-on-the-interface"></a>界面上的其他交通组策略 

你可以使用以下命令设置其他 QoS 策略。

    New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
 
下面是此命令示例结果。

|参数|值|
|---------|-----|
|
|名称 | 默认设置|
|所有者|组策略 \(Machine\)|
|NetworkProfile|所有|
|优先|127|
|模板| 默认设置|
|JobObject| &nbsp;|
|PriorityValue|0|

### <a name="turn-on-flow-control-for-smb"></a>针对 SMB 打开流量控制 

你可以使用以下命令以启用 SMB 流控制并查看结果。

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

下面是此命令示例结果。

|优先级|启用|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |假 |全球|&nbsp;|&nbsp;|
|1 |假 |全球|&nbsp;|&nbsp;|
|2 |假 |全球|&nbsp;|&nbsp;|
|3 |如此 |全球|&nbsp;|&nbsp;|
|4 |假 |全球|&nbsp;|&nbsp;|
|5 |假 |全球|&nbsp;|&nbsp;|
|6 |假 |全球|&nbsp;|&nbsp;|
|7 |假 |全球|&nbsp;|&nbsp;|

如果你的结果与因为 3 以外的商品已启用值为 True 这些结果不匹配，你必须执行下一步。 结果匹配这些示例结果，并且仅项目 3 平板电脑启用值为 True，如果你不需要执行下一步单步执行，并可跳过低到**目标和目的地的网络适配器支持服务质量**。

### <a name="ensure-flow-control-is-disabled-for-other-traffic-classes-optional"></a>确保其他交通类 \(Optional\) 禁用流控制

不需要执行此步骤，如果**FlowControl**未启用交通类 0,1,2,4,5,6，并 7。

如果**FlowControl**已经启用对任何交通类以外的 3 \（0,1,2,4,5,6，并 7\），必须先禁用**FlowControl**这些类。 

>[!NOTE]
>下更复杂配置中，其他交通类可能需要流控件，但是在这些方案是范围之外的本指南。

你可以使用以下命令以禁用 SMB 流控制交通类 0,1,2,4,5,6，并 7 中，并查看结果。

    Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
    Get-NetQosFlowControl

下面是此命令示例结果。

|优先级|启用|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |假 |全球|&nbsp;|&nbsp;|
|1 |假 |全球|&nbsp;|&nbsp;|
|2 |假 |全球|&nbsp;|&nbsp;|
|3 |如此 |全球|&nbsp;|&nbsp;|
|4 |假 |全球|&nbsp;|&nbsp;|
|5 |假 |全球|&nbsp;|&nbsp;|
|6 |假 |全球|&nbsp;|&nbsp;|
|7 |假 |全球|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>启用本地和目的地的网络适配器 QoS 

使用此步骤，您可以启用 QoS 本地和目的地的网络适配器。 确保你运行以下命令，在这两个 Hyper-V 主机上。

#### <a name="enable-qos-for-nic-test-40g-1"></a>启用 QoS NIC 测试 40G 1

可以使用以下命令以启用 QoS 并查看您的配置的结果。

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
    Get-NetAdapterQos -Name "Test-40G-1"

下面是此命令示例结果。

**名称**：测试 40G 1**启用**：如此**功能**:   

|参数|硬件|当前|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|无|无|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|带宽|优先级|
|----|-----|--------|-------|
|
|0| 严格|&nbsp;|0-7|

**OperationalFlowControl**：优先级 3 启用**OperationalClassifications**:

|协议|端口/类型|优先级|
|--------|---------|--------|
|
|默认设置|&nbsp;|0|
|NetDirect| 445|3|

#### <a name="enable-qos-for-nic-test-40g-2"></a>启用 QoS NIC 测试 40G 2

可以使用以下命令以启用 QoS 并查看您的配置的结果。

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
    Get-NetAdapterQos -Name "Test-40G-2"

 
下面是此命令示例结果。

**名称**：测试-40G-2**启用**：如此**功能**:   

|参数|硬件|当前|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|无|无|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|带宽|优先级|
|----|-----|--------|-------|
|
|0| 严格|&nbsp;|0-7|

**OperationalFlowControl**：优先级 3 启用**OperationalClassifications**:

|协议|端口/类型|优先级|
|--------|---------|--------|
|
|默认设置|&nbsp;|0|
|NetDirect| 445|3|


### <a name="allocate-50-of-the-bandwidth-reservation-to-smb-direct-rdma"></a>分配到 SMB 直接 \(RDMA\) 带宽保留一半

你可以使用以下命令要一半带宽保留分配给 SMB 直接。

    New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS

下面是此命令示例结果。

|名称|算法 |Bandwidth(%)| 优先级 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 50 |3 |全球 |&nbsp;|&nbsp;|                                      
 
你可以使用以下命令以查看带宽预订信息。

    Get-NetQosTrafficClass | ft -AutoSize

下面是此命令示例结果。
 
|名称|算法 |Bandwidth(%)| 优先级 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[默认]| ETS|50 |0-2,4-7|  全球|&nbsp;|&nbsp;| 
SMB |ETS|50 |3 |全球|&nbsp;|&nbsp;| 

### <a name="create-traffic-classes-for-tenant-ip-traffic-optional"></a>为租户 IP 交通创建通信类 \(optional\)

可以使用此步骤创建两个额外的流量类租户 IP 交通。 

运行以下命令示例，可以忽略"ip1 开始"和"IP2"的值，如果你希望。

    New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS

下面是此命令示例结果。

|名称|算法 |Bandwidth(%)| 优先级 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP1 开始 |ETS |10 |1 |全球|&nbsp;|&nbsp;|


    New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS

下面是此命令示例结果。

|名称|算法 |Bandwidth(%)| 优先级 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP2 |ETS |10 |2 |全球|&nbsp;|&nbsp;|

你可以使用以下命令以查看 QoS 通信类。

    Get-NetQosTrafficClass | ft -AutoSize

下面是此命令示例结果。

|名称|算法 |Bandwidth(%)| 优先级 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[默认] |ETS |30 |0,4-7 |全球|&nbsp;|&nbsp;|
|SMB |ETS |50 |3 |全球|&nbsp;|&nbsp;|
|IP1 开始 |ETS |10 |1 |全球|&nbsp;|&nbsp;|
|IP2 |ETS |10 |2 |全球|&nbsp;|&nbsp;|

## <a name="configure-debugger-optional"></a>配置 \(Optional\) 调试程序

你可以使用此步骤配置调试程序。

默认情况下，连接调试程序阻止 NetQos。 你可以使用以下命令覆盖调试程序和查看结果。

    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger

下面是此命令示例结果。

    AllowFlowControlUnderDebugger
    -----------------------------
    1

## <a name="test-rdma-mode-1"></a>测试 RDMA \(Mode 1\)

可以使用此步骤以确保之前创建 vSwitch 并即将转换为 RDMA \(Mode 2\) 正确配置了结构。

下图显示了 Hyper-V 主机的当前状态。

![测试 RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)

若要验证 RDMA 配置，你可以运行以下命令。

    Get-NetAdapterRdma | ft -AutoSize

下面是此命令示例结果。

|名称 |InterfaceDescription |启用|
|----|--------------------|-------|
|
|测试 40G 1| Mellanox ConnectX 4 VPI 适配器 #2 |如此|
|测试 40G 2| VPI Mellanox ConnectX 4 适配器 |如此|


### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>确定你目标适配器的 ifIndex 值

你可以使用以下命令发现的目标适配器 ifIndex 值。

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

下面是此命令示例结果。

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|测试 40G 1 |14 |{192.168.1.3}|
|测试 40G 2 | 13 |{192.168.2.3}|

### <a name="download-diskspdexe-and-a-powershell-script"></a>下载 DiskSpd.exe 和 PowerShell 脚本

若要继续，必须首先下载以下各项。

- 下载 DiskSpd.exe 实用程序，并提取到 C:\TEST\ 该实用程序[http://tinyurl.com/z68h3rc](http://tinyurl.com/z68h3rc)

- 下载到 C:\TEST\ Test-RDMA powershell 脚本 [https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="run-the-powershell-script"></a>运行 PowerShell 脚本

Test-Rdma.ps1 Windows PowerShell 脚本运行时，你可以将 ifIndex 值传递到脚本，以及在同一 VLAN 远程适配器的 IP 地址。

可以使用下面的示例命令网络适配器 192.168.1.5 上运行的 14 ifIndex 与的脚本。
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter TEST-40G-1 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 662979201 RDMA bytes written per second
    VERBOSE: 37561021 RDMA bytes sent per second
    VERBOSE: 1023098948 RDMA bytes written per second
    VERBOSE: 8901349 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
    

>[!NOTE]
>如果 RDMA 交通无法正常工作，RoCE 用例特别是，请在或切换配置获得应匹配主机设置正确 PFC/ETS 设置。 请参阅本文参考值 QoS 部分。

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>确定你目标适配器的 ifIndex 值

你可以使用以下命令发现的目标适配器 ifIndex 值。

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

下面是此命令示例结果。

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|测试 40G 1 |14 |{192.168.1.3}|
|测试 40G 2 | 13 |{192.168.2.3}|

可以使用下面的示例命令网络适配器 192.168.2.5 上运行的 13 ifIndex 与的脚本。

    
    C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter TEST-40G-2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
    VERBOSE: Remote IP 192.168.2.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 541185606 RDMA bytes written per second
    VERBOSE: 34821478 RDMA bytes sent per second
    VERBOSE: 954717307 RDMA bytes written per second
    VERBOSE: 35040816 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
    

## <a name="create-a-hyper-v-virtual-switch"></a>创建一个虚拟交换机用 Hyper-V

你可以使用此部分中创建一个虚拟交换机用 Hyper-V \ (vSwitch \) Hyper-V 主机上。

下图显示了使用 vSwitch Hyper-V 主机 1。

![创建一个虚拟交换机用 Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

### <a name="create-a-vswitch-in-switch-embedded-teaming-set-mode"></a>在切换 Embedded 组队 \(SET\) 模式中创建 vSwitch

你可以使用下面的示例命令来创建一组切换独立团队名为**VMSTEST** Hyper-V 主机 1 上。 这两个测试 40G 1 和测试 40G 2 这台计算机上的网络适配器添加到使用此命令设置团队。
    
    New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
    
下面是此命令示例结果。

|名称 |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |外部 |搭配使用界面|

你可以使用此命令以查看集中物理适配器团队。

    Get-VMSwitchTeam -Name "VMSTEST" | fl

下面是此命令示例结果。

**名称**: VMSTEST **Id**: ad9bb542-dda2-4450-a00e-f96d44bdfbec **NetAdapterInterfaceDescription**: {Mellanox ConnectX 3 专业版的以太网适配器、Mellanox ConnectX 3 专业版的以太网适配器 #2} **TeamingMode**: SwitchIndependent **LoadBalancingAlgorithm**：动态

#### <a name="display-two-views-of-the-host-vnic"></a>显示两种主机 vNIC 视图

你可以使用以下两个命令进行主机 vNIC 的两种不同的视图。

可以使用此命令以显示的主机 vNIC 属性。

    Get-NetAdapter

下面是此命令示例结果。

|名称 |InterfaceDescription | ifIndex |状态 |MacAddress |LinkSpeed | |------|---|---|---|---| | | vEthernet (VMSTEST) |Hyper-V 虚拟以太网适配器 #2 | 28 |向上 |E4-1D-2D-07-40-71|80 Gbps |

可以使用此命令以显示的主机 vNIC 其他属性。

    Get-VMNetworkAdapter -ManagementOS

下面是此命令示例结果。

|名称 |IsManagementOs |VMName |SwitchName |MacAddress |状态 |以及|
|----|--------------|------|----------|----------|------|-----------|
|
|VMSTEST|如此 |VMSTEST |E41D2D074071| {确定}|&nbsp;|

#### <a name="test-the-network-connection-to-the-remote-vlan-101-adapter"></a>测试的网络连接到远程 VLAN 101 适配器

可以使用此命令以测试连接到远程 VLAN 101 适配器。

`Test-NetConnection 192.168.1.5` 

下面是此命令示例结果。

    WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 10.199.48.170
    PingSucceeded  : False
    PingReplyDetails (RTT) : 0 ms
    
### <a name="reconfigure-vlans"></a>重新 Vlan 配置

若要从物理 NIC 删除访问 VLAN 设置并设置 VLANID 使用该 vSwitch，你可以使用此步骤。

你必须删除为了防止这两个自动的标签不正确 VLAN id 出口交通访问 VLAN 设置，并从筛选入口通信，这不匹配访问 VLAN id。

你可以使用以下命令要删除访问 VLAN 设置。
    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
    

你可以使用以下命令设置 VLANID 使用 vSwitch 特定 Windows PowerShell 命令以及查看此操作的结果。

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

下面是此命令示例结果。

VMName VMNetworkAdapterName 模式 VlanList
------ -------------------- ----   --------
       VMSTEST              Access 101     

你可以使用以下命令网络连接测试和查看结果。

    Test-NetConnection 192.168.1.5
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

如果你结果并非类似于示例结果和 ping 失败并显示消息"警告：到 192.168.1.5 Ping 失败-状态：DestinationHostUnreachable，"确认"vEthernet (VMSTEST)"通过运行以下命令具有正确的 IP 地址。

    
    Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
    

如果未设置的 IP 地址，则你可以使用以下命令以更正该问题，并查看你的动作的结果。

    
    New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
    
    IPAddress : 192.168.1.3
    InterfaceIndex: 37
    InterfaceAlias: vEthernet (VMSTEST)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Tentative
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : ActiveStore
    
#### <a name="rename-the-management-nic"></a>重命名管理 NIC

你可以使用本部分重命名管理 NIC 中，并以后用于 RDMA 单独主机 vNIC 实例。

你可以运行以下命令，若要重命名管理 NIC 并查看一些 NIC 属性。

    Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
    Get-VMNetworkAdapter -ManagementOS

下面是此命令示例结果。

|名称 |IsManagementOs |VMName |SwitchName |MacAddress |状态 |以及
|----|--------------|------|----------|----------|------|-----------|
|
|CORP 外部开关 |如此 |&nbsp;|CORP 外部开关 |001B785768AA |{确定}|&nbsp;|
|管理 |如此 |&nbsp;|VMSTEST |E41D2D074071 |{确定}|&nbsp;|

你可以通过运行以下命令以查看一些其他 NIC 属性。

    Get-NetAdapter

下面是此命令示例结果。

|名称 |InterfaceDescription |ifIndex |状态 |MacAddress |LinkSpeed|
|----|--------------------|------|----------|---------|------|
|
|vEthernet（管理） |Hyper-V 虚拟以太网适配器 #2 |28 |向上 | E4-1D-2D-07-40-71 |80 Gbps|

## <a name="test-hyper-v-virtual-switch-rdma"></a>测试 Hyper-V 虚拟交换机用来 RDMA

下图显示了你 Hyper-V 主机，包括该 vSwitch Hyper-V 主机 1 上的当前状态。

![测试 Hyper-V 虚拟交换机用来](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

### <a name="set-priority-tagging-on-the-host-vnic-to-complement-the-previous-vlan-settings"></a>设置优先级标签上主机 vNIC 来补充以前 VLAN 设置

可以使用下面的示例命令设置优先级主机 vNIC 上标记，并查看你的动作的结果。
    
    Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
    

    Name: MGT
    IeeePriorityTag : On
    
### <a name="create-two-host-vnics-for-rdma"></a>为 RDMA 创建两个主机 vNICs

可以使用下面的示例命令来为 RDMA 创建两个主机 vNICs 并将其连接到该 vSwitch VMSTEST。
    
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
    
你可以使用下面的示例命令以查看管理 NIC 属性。
    
    Get-VMNetworkAdapter -ManagementOS
    
下面是此命令示例结果。

|名称 |IsManagementOs |VMName |SwitchName |MacAddress |状态 |以及|
|----|--------------|------|----------|----------|------|-----------|
|
|CORP 外部开关 |如此 |CORP 外部开关 |001B785768AA|{确定} |&nbsp;| 
|管理 |如此 |VMSTEST |E41D2D074071 |{确定} |&nbsp;|
|SMB1 |如此 |VMSTEST |00155D30AA00 |{确定} |&nbsp;|
|SMB2 |如此 |VMSTEST |00155D30AA01 |{确定} |&nbsp;|

### <a name="assign-an-ip-address-to-the-smb-host-vnics"></a>将 SMB 主机 vNICs 指定 IP 地址

你可以使用本部分若要指定 IP addressrd 到 SMB 主机 vNICs vEthernet \(SMB1\) 和 vEthernet \(SMB2\)。

测试 40G 1 和测试-40G-2 物理适配器仍然可以访问 VLAN 101 以及 102 配置。 出于此原因，适配器标记流量-，并 ping 成功。

以前，配置为零，这两个 pNIC VLAN Id，然后将 VMSTEST vSwitch 设置为 VLAN 101。 在此之后，仍可以使用管理 vNIC，ping 远程 VLAN 101 适配器，但当前没有 VLAN 102 的成员。

现在，你可以使用下面的示例命令删除物理 NIC 阻止这两个自动的标签不正确 VLAN id 出口交通并阻止它过滤入口通信的访问权限 VLAN 设置，不匹配访问 VLAN id。

你可以使用下面的示例命令来实现这些目标，通过添加新的 IP 地址接口 vEthernet (SMB1)，并查看结果。

    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
    
    IPAddress : 192.168.2.111
    InterfaceIndex: 40
    InterfaceAlias: vEthernet (SMB1)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Invalid
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : PersistentStore
    

你可以使用下面的示例命令测试远程 VLAN 102 适配器，并查看结果。
    
    Test-NetConnection 192.168.2.5 
    
    ComputerName   : 192.168.2.5
    RemoteAddress  : 192.168.2.5
    InterfaceAlias : vEthernet (SMB1)
    SourceAddress  : 192.168.2.111
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
你可以使用下面的示例命令添加新的 IP 地址界面 vEthernet \(SMB2\)，并查看结果。
    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
    
    IPAddress : 192.168.2.222
    InterfaceIndex: 44
    InterfaceAlias: vEthernet (SMB2)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Invalid
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : PersistentStore
    
不需要重新连接测试，这是因为建立通信。

### <a name="place-the-rdma-host-vnics-on-vlan-102"></a>放入 VLAN 102 RDMA 主机 vNICs

你可以使用此部分中分配到 VLAN 102 RDMA 主机 vNICs。

"管理"主机 vNIC 位于 VLAN 101，因为可以使用下面的示例命令 RDMA 主机 vNICs 放预现有 VLAN 102 并查看你的动作的结果。

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
    Get-VMNetworkAdapterVlan -ManagementOS
    
    VMName VMNetworkAdapterName Mode VlanList
    ------ -------------------- ---- --------
       SMB1 Access   102 
       Mgt  Access   101 
       SMB2 Access   102 
       CORP-External-Switch Untagged
    
### <a name="inspect-the-mapping-of-smb1-and-smb2-to-the-underlying-physical-nics"></a>检查映射的 SMB1 和基础的物理 Nic 到 SMB2

你可以使用此部分中检查 SMB1 和到该 vSwitch 设置团队下基础物理 Nic SMB2 的地图。  物理 Nic 到主机 vNIC 关联是随机和遵循重新平衡期间创建和销毁。

在此情况下可以使用间接机制查看当前关联。

SMB1 和 SMB2 的 MAC 地址与程序关联 NIC 团队成员测试-40G-2。 这是不适用，因为测试 40G 1 没有关联的 SMB 主机 vNIC，并且不允许利用率百分比 RDMA 通信的链接上的直到 SMB 主机 vNIC 映射到它。

你可以使用下面的示例命令以查看此信息。

    
    Get-NetAdapterVPort (Preferred)
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
    
    Get-VMNetworkAdapter -ManagementOS
    
    Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
    ---- -------------- ------ ----------   ----------   ------ -----------
    CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
    Mgt  True  VMSTEST  E41D2D074071 {Ok}  
    SMB1 True  VMSTEST  00155D30AA00 {Ok}  
    SMB2 True  VMSTEST  00155D30AA01 {Ok}  
    

由于你拥有不执行地图，下面这两个命令应返回任何信息。
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
    
可以使用下面的示例命令映射 SMB1 和 SMB2 单独物理 NIC 团队的成员，并查看你的动作的结果。

>[!IMPORTANT]
>确保您完成此步骤继续之前，或者你实现将失败。
    
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
    
    NetAdapterName : Test-40G-1
    NetAdapterDeviceId : {BAA9A00F-A844-4740-AA93-6BD838F8CFBA}
    ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB1'
    IsTemplate : False
    CimSession : CimSession: .
    ComputerName   : 27-3145G0803
    IsDeleted  : False
    
    NetAdapterName : Test-40G-2
    NetAdapterDeviceId : {B7AB5BB3-8ACB-444B-8B7E-BC882935EBC8}
    ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB2'
    IsTemplate : False
    CimSession : CimSession: .
    ComputerName   : 27-3145G0803
    IsDeleted  : False
    
### <a name="confirm-the-mac-associations"></a>确认 MAC 关联

你可以使用下面的示例命令以确认你之前创建的 MAC 关联。
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    

由于这两个主机 vNICs 位于相同子网内，并且具有相同 VLAN ID \(102\)，你可以验证的远程系统通信，并查看你的动作的结果。

>[!IMPORTANT]
>在远程计算机上运行以下命令。

    
    Test-NetConnection 192.168.2.111
    
    
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
    Test-NetConnection 192.168.2.222
    
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    
        
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    
    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    

    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    
    
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
      

### <a name="validate-rdma-functionality-from-the-remote-system"></a>验证的远程系统 RDMA 功能

此部分中可用于验证 RDMA 功能从远程系统本地系统，已 vSwitch，从而测试这两个适配器 vSwitch 设置团队的成员。

因为这两个主机 vNICs \（SMB1 和 SMB2\）分配给 VLAN 102，则你可以选择 VLAN 102 远程系统中的适配器。  

在此过程中，NIC 测试 40G 2 RDMA 无法到 SMB1 (192.168.2.111) 和 SMB2 (192.168.2.222)。

可选：你可能需要禁用此系统上的防火墙。  有关详细信息，请查阅结构策略。

    
    Set-NetFirewallProfile -All -Enabled False
    
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
    
    Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
    ----  -----------------------   --------------- -------------  
    .
    .
    Test-40G-2VLAN ID102VlanID  {102} 
    
    Get-NetAdapter
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
    
    
    Get-NetAdapterRdma
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter Test-40G-2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.111, is reachable.
    VERBOSE: Remote IP 192.168.2.111 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 34251744 RDMA bytes sent per second
    VERBOSE: 967346308 RDMA bytes written per second
    VERBOSE: 35698177 RDMA bytes sent per second
    VERBOSE: 976601842 RDMA bytes written per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.111
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter Test-40G-2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.222, is reachable.
    VERBOSE: Remote IP 192.168.2.222 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 485137693 RDMA bytes written per second
    VERBOSE: 35200268 RDMA bytes sent per second
    VERBOSE: 939044611 RDMA bytes written per second
    VERBOSE: 34880901 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.222
    
### <a name="test-for-rdma-traffic-from-the-local-to-the-remote-computer"></a>从本地到远程计算机的 RDMA 通信的测试

您可以在此部分中验证 RDMA 交通发送时的本地计算机从到远程计算机。

可以使用下面的示例命令来测试和查看流量。

    
    Get-NetAdapter | ft –AutoSize
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (SMB1) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
    VERBOSE: Remote IP 192.168.2.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 15250197 RDMA bytes sent per second
    VERBOSE: 896320913 RDMA bytes written per second
    VERBOSE: 33947559 RDMA bytes sent per second
    VERBOSE: 912160540 RDMA bytes written per second
    VERBOSE: 34091930 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (SMB2) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
    VERBOSE: Remote IP 192.168.2.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 385169487 RDMA bytes written per second
    VERBOSE: 33902277 RDMA bytes sent per second
    VERBOSE: 907354685 RDMA bytes written per second
    VERBOSE: 33923662 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
    
在输出中，最终行"RDMA 交通测试成功：RDMA 交通而非发送到 192.168.2.5，"已成功配置汇聚 NIC 你的适配器上的说明。

## <a name="all-topics-in-this-guide"></a>本指南中的所有主题

本指南中包括以下主题。

- [已合并好的 NIC 配置了一个网络适配器](cnic-single.md)
- [已合并好的 NIC 成的 NIC 配置](cnic-datacenter.md)
- [已合并好 nic 物理开关配置](cnic-app-switch-config.md)
- [故障排除汇聚 NIC 配置](cnic-app-troubleshoot.md)

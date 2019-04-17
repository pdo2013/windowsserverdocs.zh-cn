---
title: 已合并好的 NIC 配置了一个网络适配器
description: 本主题提供有关如何部署与 Windows Server 2016 的网络适配器汇聚 NIC 说明进行操作。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6663a966026afb301a4bad90a9573d16fc82875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>已合并好的 NIC 配置了一个网络适配器

>适用于：Windows Server（半年通道），Windows Server 2016

以下部分提供了用于在 Hyper-V 主机中的一个 NIC 与配置汇聚 NIC 的说明进行操作。

本指南中的示例配置展示了两个 Hyper-V 主机，**Hyper-V 主机 A**，并**Hyper-V 主机 B**。

## <a name="test-connectivity-between-source-and-destination"></a>测试之间源代码和目的地的连接

此部分中提供所需测试的源和 Hyper-V 主机目标之间的连接的步骤。

下图显示了两个 Hyper-V 主机，**Hyper-V 主机 A**和**Hyper-V 主机 B**。 

这两个服务器已安装的单个物理 NIC (pNIC) 和 Nic 连接到顶部机架 \(ToR\) 物理切换。 此外，这些服务器位于相同子网，即 192.168.1.x/24。

![Hyper-V 主机](../../media/Converged-NIC/1-single-test-conn.jpg)


### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>测试 Hyper\ V 虚拟交换机用来 NIC 连接

通过使用此步骤，你可以确保物理 NIC，以后将为其创建一个 Hyper\ V 虚拟交换机用来，可以连接到目标主机。 

该测试通过使用一层 3 \(L3\)-或 IP 层-以及第二层 \(L2\) 演示连接。

可以使用下面的 Windows PowerShell 命令以获取该网络适配器的属性。

    Get-NetAdapter
     
下面是此命令示例结果。

|名称|InterfaceDescription|ifIndex|状态|MacAddress|LinkSpeed|
|-----|--------------------|-------|-----|----------|---------|
|
|M1|Mellanox ConnectX 3 的专业版…| 4| 向上|7C-FE-90-93-8F-A1|40 Gbps|

你可以使用以下命令之一以获取更多适配器属性，包括 IP 地址。

    Get-NetAdapter M1 | fl *

以下是编辑的示例此命令的结果。
    
    MacAddress   : 7C-FE-90-93-8F-A1
    Status   : Up
    LinkSpeed: 40 Gbps
    MediaType: 802.3
    PhysicalMediaType: 802.3
    AdminStatus  : Up
    MediaConnectionState : Connected
    DriverInformation: Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60
    DriverFileName   : mlx4eth63.sys
    NdisVersion  : 6.60
    ifOperStatus : Up
    ifAlias  : M1
    InterfaceAlias   : M1
    ifIndex  : 4
    ifDesc   : Mellanox ConnectX-3 Pro Ethernet Adapter
    ifName   : ethernet_32773
    DriverVersion: 5.25.12665.0
    LinkLayerAddress : 7C-FE-90-93-8F-A1
    Caption  :
    Description  :
    ElementName  :
    InstanceID   : {39B58B4C-8833-4ED2-A2FD-E105E7146D43}
    CommunicationStatus  :
    DetailedStatus   :
    HealthState  :
    InstallDate  :
    Name : M1
    OperatingStatus  :
    OperationalStatus:
    PrimaryStatus:
    StatusDescriptions   :
    AvailableRequestedStates :
    EnabledDefault   : 2
    EnabledState : 5
    OtherEnabledState:
    RequestedState   : 12
    TimeOfLastStateChange:
    TransitioningToState : 12
    AdditionalAvailability   :
    Availability :
    CreationClassName: MSFT_NetAdapter
    

### <a name="ensure-that-source-and-destination-can-communicate"></a>确保可以通信源代码和目的地

你可以使用此步骤验证双向通信 \ (来源到目标，在这两个 systems\ 反之亦然 ping)。  在以下示例中，**测试 NetConnection**使用 Windows PowerShell 命令时，但如果你希望你可以使用**ping**命令。 

>[!NOTE]
>如果你确定您的主机可以互相，你可以跳过此步骤。

    Test-NetConnection 192.168.1.5

下面是此命令示例结果。

|参数|值|
|---------|-----|
|
|计算机名称|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|M1|
|SourceAddress|192.168.1.3|
|PingSucceeded|如此|
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

## <a name="configure-vlans-optional"></a>配置 Vlan \(Optional\)

许多网络配置使 Vlan 的使用。 如果你打算使用 Vlan 网络中，你必须具有 Vlan 配置重复以前测试。 \ (如果你打算 RoCE 用于 RDMA 服务，则你必须启用 Vlan。\)

对于此步骤，Nic 处于**访问**模式。 但是你创建一个虚拟交换机用 Hyper-V \ (vSwitch \) 在本指南，稍后 VLAN 属性应用在 vSwitch 端口级别。 

切换可以主机多个 Vlan，因为它是必需的顶部的机架 \(ToR\) 物理切换到已主机连接到型主干配置的端口。

>[!NOTE]
>有关如何配置主干模式切换上的说明，请查阅或切换文档。

下图显示了两个 Hyper-V 主机，每个人都带个物理的网络适配器，以及每个配置为在 VLAN 101 通信。

![配置虚拟本地区域网络](../../media/Converged-NIC/2-single-configure-vlans.jpg)

### <a name="configure-the-vlan-id"></a>配置 VLAN ID

可以使用此步骤的安装在你的 Hyper-V 主机 Nic 配置 VLAN Id。

#### <a name="configure-nic-m1"></a>配置 NIC M1

使用以下命令，将 VLAN ID 配置的第一个 nic，M1，然后进行查看结果配置。

>[!IMPORTANT]
>如果不会运行此命令到主机远程连接通过该接口，因为这样做会导致无法访问到主机。
    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
    
下面是此命令示例结果。

|名称 |显示名称| DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|M1|VLAN ID|101|VlanID|{101}|


确保 VLAN ID 生效的网络适配器实现独立通过使用以下命令以重启该网络适配器。

    Restart-NetAdapter -Name "M1"

你可以使用以下命令以确保是网络适配器状态**向上**之前。

    Get-NetAdapter -Name "M1"

下面是此命令示例结果。

|名称|InterfaceDescription|ifIndex| 状态|MacAddress|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|M1|Mellanox ConnectX 3 专业版的以太网艾达…|4|向上|7C-FE-90-93-8F-A1|40 Gbps|

确保你的本地和目的地的服务器上执行此步骤。 如果未使用本地服务器相同 VLAN ID 配置目标服务器，这两个无法进行通信。

### <a name="verify-connectivity"></a>验证连接

你可以使用此部分中验证连接后重新启动的网络适配器。 你可以在应用于这两个适配器 VLAN 标记后确认连接。 如果连接将失败，你可以检查切换 VLAN 配置或目标参与相同 VLAN。 

>[!IMPORTANT]
>在上一节中执行这些步骤后，可能需要几秒钟该设备重新启动并在网络上可用。

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>验证连接用于 NIC 测试-40G-1

第一个 nic 验证连接，你可以运行以下命令。

    Test-NetConnection 192.168.1.5
    
## <a name="configure-data-center-bridging-dcb"></a>配置数据中心桥 \(DCB\)

下一步是配置 DCB 和服务质量 \(QoS\)，需要在你首次安装 Windows Server 2016 功能 DCB。

>[!NOTE]
>你必须执行的所有以下 DCB 和 QoS 配置步骤所有旨在彼此的服务器上。

### <a name="install-data-center-bridging-dcb"></a>安装桥 \(DCB\) 数据中心

你可以使用此步骤以安装并启用 DCB。 

>[!IMPORTANT]
>- 安装和配置 DCB 是**可选**iWarp 用于 RDMA 服务的网络配置为。
>- 安装和配置 DCB 是**需要**RoCE \(any version\) 用于 RDMA 服务的网络配置为。


你可以使用以下命令在每个 Hyper-V 主机上安装 DCB。 

    Install-WindowsFeature Data-Center-Bridging

### <a name="set-the-qos-policies-for-smb-direct"></a>设置 SMB 直接 QoS 策略 

可以使用以下命令以直接 SMB 配置 QoS 策略。

>[!IMPORTANT]
>- 这一步是可选的则使用 iWarp 网络配置。
>- 则需要使用 RoCE 的网络配置为此步骤。
>- 在以下示例命令，"3 平板电脑"的值是任意。 你可以使用 1 和 7 之间的任意值，只要你一直使用相同的值整个 QoS 策略配置。

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

### <a name="for-roce-deployments-turn-on-priority-flow-control-for-smb-traffic"></a>对于 RoCE 部署打开 SMB 交通优先级流控制 

如果你使用 RoCE RDMA 服务，你可以使用以下命令以启用 SMB 流控制并查看结果。 优先级流控制需要 RoCE，但不需要的当你使用的 iWarp。

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

以下是示例结果**获取 NetQosFlowControl**命令。

|优先级|启用|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |假 |全球|&nbsp;|&nbsp;|
|1 |假 |全球|&nbsp;|&nbsp;|
|2 |假 |全球|&nbsp;|&nbsp;|
|3 |如此  |全球|&nbsp;|&nbsp;|
|4 |假 |全球|&nbsp;|&nbsp;|
|5 |假 |全球|&nbsp;|&nbsp;|
|6 |假 |全球|&nbsp;|&nbsp;|
|7 |假 |全球|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>启用本地和目的地的网络适配器 QoS
使用此步骤，您可以启用 DCB 上的特定网络适配器。

>[!IMPORTANT]
>-  使用 iWarp 的网络配置为不需要此步骤。
>-  则需要使用 RoCE 的网络配置为此步骤。




#### <a name="enable-qos-for-nic-m1"></a>为 NIC M1 启用 QoS

可以使用以下命令以启用 QoS 并查看您的配置的结果。

    Enable-NetAdapterQos -InterfaceAlias "M1"
    Get-NetAdapterQos -Name "M1"

下面是此命令示例结果。

**名称**: M1**启用**：如此**功能**:   

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
|0| ETS|70%|0-2,4-7|
|1|ETS|30%|3

**OperationalFlowControl**：优先级 3 启用**OperationalClassifications**:

|协议|端口/类型|优先级|
|--------|---------|--------|
|
|默认设置|&nbsp;|0|
|NetDirect| 445|3|

### <a name="reserve-a-percentage-of-the-bandwidth-for-smb-direct-rdma"></a>对于 SMB 直接 \(RDMA\) 保留的带宽百分比

你可以使用以下命令保留为 SMB 直接带宽的百分比。  

在此示例中，使用 30%带宽保留。 你应该选择表示你希望将要求你存储流量的值。 值**-bandwidthpercentage**参数必须的 10%倍数。

    New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS

下面是此命令示例结果。

|名称|算法 |Bandwidth(%)| 优先级 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 30 |3 |全球 |&nbsp;|&nbsp;|                                      
 
你可以使用以下命令以查看带宽预订信息。

    Get-NetQosTrafficClass

下面是此命令示例结果。
 
|名称|算法 |Bandwidth(%)| 优先级 |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[默认]|ETS|70 |0-2,4-7| 全球|&nbsp;|&nbsp;| 
|SMB      |ETS|30 |3 |全球|&nbsp;|&nbsp;| 

## <a name="remove-debugger-conflict-mellanox-adapter-only"></a>删除调试程序冲突 \(Mellanox adapter only\)

如果你使用的从 Mellanox 适配器，你需要执行此步骤配置调试程序。 默认情况下，当使用 Mellanox 适配器时，连接调试程序阻止 NetQos。 你可以使用以下命令覆盖调试程序。

    
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    

## <a name="test-rdma-native-host"></a>测试 RDMA（本机主机）

可以使用此步骤以确保之前创建 vSwitch 并即将转换为 RDMA \(Converged NIC\) 正确配置了结构。

下图显示了 Hyper-V 主机的当前状态。

![测试 RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

若要验证 RDMA 配置，你可以运行以下命令。

    Get-NetAdapterRdma

下面是此命令示例结果。

|名称 |InterfaceDescription |启用|
|----|--------------------|-------|
|
|M1| Mellanox ConnectX 3 专业版的以太网适配器 |如此|

### <a name="download-diskspdexe-and-a-powershell-script"></a>下载 DiskSpd.exe 和 PowerShell 脚本

若要继续，必须首先下载以下各项。

- 下载 DiskSpd.exe 实用程序，并提取到 C:\TEST\ 实用工具[Diskspd 实用程序：可靠存储测试工具（取代 SQLIO）](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223)

- 下载到 C:\TEST\ Test-RDMA powershell 脚本 [https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>确定你目标适配器的 ifIndex 值

你可以使用以下命令发现的目标适配器 ifIndex 值。 如果您运行的你已下载的脚本，可以在后续步骤使用此值。

    Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

下面是此命令示例结果。

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|
|M2 |14 |{192.168.1.5}|

### <a name="run-the-powershell-script"></a>运行 PowerShell 脚本

Test-Rdma.ps1 Windows PowerShell 脚本运行时，你可以将 ifIndex 值传递到脚本，以及在同一 VLAN 远程适配器的 IP 地址。

可以使用下面的示例命令网络适配器 192.168.1.5 上运行的 14 ifIndex 与的脚本。
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter M2 is a physical adapter
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

## <a name="remove-the-access-vlan-setting"></a>删除访问 VLAN 设置

为了准备提供创建 Hyper-V 切换必须要删除 VLAN 设置上述你安装。  你可以使用以下命令以从物理 NIC 删除访问 VLAN 设置 此操作将自动标签不正确 VLAN id，外出交通阻止 NIC 并还会阻止它过滤入口通信，不匹配访问 VLAN id。

    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
    

你可以使用下面的示例命令以确认 VlanID 设置和查看结果，显示 VLAN ID 值为零。

    
    Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
    


## <a name="create-a-hyper-v-virtual-switch"></a>创建一个虚拟交换机用 Hyper-V

你可以使用此部分中创建一个虚拟交换机用 Hyper-V \ (vSwitch \) Hyper-V 主机上。

下图显示了使用 vSwitch Hyper-V 主机 1。

![创建一个虚拟交换机用 Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


### <a name="create-an-external-hyper-v-virtual-switch"></a>创建外部 Hyper-V 虚拟交换机用来

你可以使用此部分中 Hyper-V Hyper-V 主机 A.上创建外部 vSwitch

可以使用下面的示例命令来创建一个名为 VMSTEST 开关。

>[!NOTE]
>参数**AllowManagementOS**以下命令在创建主机 vNIC 派生的 MAC 地址和物理 NIC IP 地址

    New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true

下面是此命令示例结果。

|名称 |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |外部 |Mellanox ConnectX 3 专业版的以太网适配器|

你可以使用以下命令以查看该网络适配器属性。

    Get-NetAdapter | ft -AutoSize

下面是此命令示例结果。

|名称 |InterfaceDescription | ifIndex |状态 |MacAddress |LinkSpeed|
|---- |-------------------- |-------| ------|----------|---------|
|
|vEthernet \(VMSTEST\) |Hyper-V 虚拟以太网适配器 #2|27 |向上 |E4-1D-2D-07-40-71 |40 Gbps|


你可以管理主机 vNIC 以下两种方式。 一种方法是**NetAdapter**视图中，这表示根据"vEthernet \(VMSTEST\)"名称。

另一个方法是**VMNetworkAdapter**视图，其中丢弃"vEthernet"前缀，并且只需使用 vmswitch 名称。

**VMNetworkAdapter**视图中显示的某些中不显示的网络适配器属性**NetAdapter**命令。

你可以使用以下命令以查看结果**VMNetworkAdapter**方法。

    Get-VMNetworkAdapter –ManagementOS | ft -AutoSize

下面是此命令示例结果。

|名称 |IsManagementOs |VMName |SwitchName |MacAddress |状态 |以及|
|----|-------------- |------ |----------|----------|------ |-----------|
|
|CORP 外部开关 |如此 |CORP 外部开关| 001B785768AA |{确定} |&nbsp;|
|VMSTEST |如此 |VMSTEST | E41D2D074071| {确定} | &nbsp;| 

### <a name="test-the-connection"></a>测试连接

你可以使用下面的示例命令连接测试和查看结果。
    
    Test-NetConnection 192.168.1.5

    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
可以使用下面的示例命令指定和查看网络适配器 VLAN 设置。
    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

|VMName |VMNetworkAdapterName |模式 |VlanList|
|------ |-------------------- |---- |--------|
|
|&nbsp;|VMSTEST |访问 |101     
 
### <a name="test-the-connection"></a>测试连接

重新调用更改可能需要几秒钟完成，然后你可以成功 ping 其他适配器。

你可以使用下面的示例命令连接测试和查看结果。
    
    Test-NetConnection 192.168.1.5
     
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

## <a name="test-hyper-v-virtual-switch-rdma-mode-2"></a>测试 Hyper-V 虚拟交换机用来 RDMA（模式 2）

下图显示了你 Hyper-V 主机，包括该 vSwitch Hyper-V 主机 1 上的当前状态。

![测试 Hyper-V 虚拟交换机用来](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


### <a name="set-priority-tagging-on-the-host-vnic"></a>在主机 vNIC 标签设置优先级

可以使用下面的示例命令设置优先级主机 vNIC 上标记，并查看你的动作的结果。
    
    Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
    
    Name: VMSTEST
    IeeePriorityTag : On
    
你可以使用下面的示例命令以查看 RDMA 信息的网络适配器。 在结果中，当参数**启用**具有值**False**，这意味着 RDMA 不已启用。
    
    Get-NetAdapterRdma
    
|名称 |InterfaceDescription |启用 |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Hyper-V 虚拟以太网适配器 #2|假|

### <a name="enable-rdma-on-the-host-vnic"></a>在主机 vNIC 上启用 RDMA

可以使用下面的示例命令以查看的网络适配器属性，RDMA 启用该适配器，然后查看网络适配器 RDMA 信息。
    
    Get-NetAdapter
    
|名称 |InterfaceDescription |ifIndex |状态 |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Hyper-V 虚拟以太网适配器 #2|27|向上|E4-1D-2D-07-40-71|40 Gbps|

    Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
    Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"

在下面的结果中，当参数**启用**具有值**如此**，这意味着 RDMA 已启用。

|名称 |InterfaceDescription |启用 |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Hyper-V 虚拟以太网适配器 #2|如此|


    
    Get-NetAdapter 
    

|名称 |InterfaceDescription |ifIndex |状态 |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Hyper-V 虚拟以太网适配器 #2|27|向上|E4-1D-2D-07-40-71|40 Gbps|

### <a name="perform-rdma-traffic-test-by-using-the-script"></a>使用脚本执行 RDMA 交通测试

你可以使用以下命令以运行的脚本你下载并查看结果。
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 27 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (VMSTEST) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 9162492 RDMA bytes sent per second
    VERBOSE: 938797258 RDMA bytes written per second
    VERBOSE: 34621865 RDMA bytes sent per second
    VERBOSE: 933572610 RDMA bytes written per second
    VERBOSE: 35035861 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
    
在输出中，最终行"RDMA 交通测试成功：RDMA 交通而非发送到 192.168.1.5，"已成功配置汇聚 NIC 你的适配器上的说明。

## <a name="all-topics-in-this-guide"></a>本指南中的所有主题

本指南中包括以下主题。

- [已合并好的 NIC 配置了一个网络适配器](cnic-single.md)
- [已合并好的 NIC 成的 NIC 配置](cnic-datacenter.md)
- [已合并好 nic 物理开关配置](cnic-app-switch-config.md)
- [故障排除汇聚 NIC 配置](cnic-app-troubleshoot.md)

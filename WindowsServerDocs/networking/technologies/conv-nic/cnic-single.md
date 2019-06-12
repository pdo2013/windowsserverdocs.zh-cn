---
title: 聚合的 NIC 配置具有单个网络适配器
description: 在本主题中，我们向你提供的说明配置具有单个 NIC 的 HYPER-V 主机中聚合的 NIC。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 93d317534af46c87c4b2e874a5a5475687e2efa0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447065"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>聚合的 NIC 配置具有单个网络适配器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，我们向你提供的说明配置具有单个 NIC 的 HYPER-V 主机中聚合的 NIC。

本主题中的示例配置介绍了两个 HYPER-V 主机**的 HYPER-V 主机 A**，并**HYPER-V 主机 B**。这两个主机都具有单个物理 NIC (pNIC) 安装，并为 Nic 连接到机架顶部\(ToR\)物理交换机。 此外，主机位于同一子网，这是 192.168.1.x/24。

![Hyper-V 主机](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>步骤 1： 测试源和目标之间的连接

确保物理 NIC 可以连接到目标主机。 此测试演示通过使用第 3 层连接\(L3\) -或者 IP 层-第 2 层以及\(L2\)。

1. 查看网络适配器属性。

   ```PowerShell
   Get-NetAdapter
   ```

   _**结果：** _  


   | 名称 |    InterfaceDescription     | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |------|-----------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX 3 Pro... |    4    |   向上   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

2. 查看其他适配器的属性，其中包括 IP 地址。

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**结果：** _

   ```PowerShell   
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
   ``` 

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>步骤 2： 确保源和目标可以进行通信

在此步骤中，我们使用**Test-netconnection** Windows PowerShell 命令，但如果可以使用**ping**命令如果您更喜欢。 

>[!TIP]
>如果您确定您的主机可以彼此通信，则可以跳过此步骤。

1. 验证的双向通信。

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**结果：** _


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      |     M1      |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0 毫秒     |

   ---

   在某些情况下，可能需要禁用 Windows 防火墙高级安全，若要成功执行此测试。 如果禁用防火墙，请记住的安全，并确保你的配置符合你组织的安全要求。

2. 禁用所有防火墙配置文件。

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. 禁用防火墙配置文件之后, 再次测试连接。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**结果：** _


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | 测试-40 G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0 毫秒     |

   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>步骤 3： （可选）为安装在 HYPER-V 主机中的 Nic 配置 VLAN Id

许多网络配置进行使用的 Vlan，并且如果想要在网络中使用 Vlan，则必须重复前面的测试 vlan 配置。 此外，如果你打算使用 RoCE 的 RDMA 服务必须启用 Vlan。

对于此步骤的 Nic 位于**访问**模式。 但是，当创建 HYPER-V 虚拟交换机\(vSwitch\)更高版本在本指南中，VLAN 属性在 vSwitch 端口级别应用。 

由于交换机可以托管多个 Vlan，因此有必要为机架顶部\(ToR\)物理交换机以具有与该主机已连接到在 Trunk 模式下配置的端口。

>[!NOTE]
>有关如何在交换机上配置的 Trunk 模式的说明，请参阅 ToR 交换机文档。

下图显示了两个 HYPER-V 主机，每个都有一个物理网络适配器，并且每个配置 VLAN 101 上进行通信。

![配置虚拟局域网](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>在本地和目标服务器上执行此操作。 如果目标服务器未配置与本地服务器相同的 VLAN ID，这两个不能进行通信。


1. 为安装在 HYPER-V 主机中的 Nic 配置 VLAN ID。

   >[!IMPORTANT]
   >不运行此命令到主机远程连接通过此接口，因为这会导致无法访问主机。

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**结果：** _


   | 名称 | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------|-------------|--------------|-----------------|---------------|
   |  M1  |   VLAN ID   |     101      |     VlanID      |     {101}     |

   ---

2. 重新启动的网络适配器应用 VLAN id。

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. 确保状态为**向上**。

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```

   _**结果：** _


   | 名称 |          InterfaceDescription           | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |------|-----------------------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX 3 Pro 以太网 Ada... |    4    |   向上   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >可能需要几秒钟，让设备重新启动并使其在网络上可用。 

4. 验证的连接。<p>如果连接失败，检查交换机 VLAN 配置或目标参与同一 VLAN 中。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

## <a name="step-4-configure-quality-of-service-qos"></a>步骤 4： 配置服务质量\(QoS\)

>[!NOTE]
>必须用于相互通信的所有主机上执行所有以下 DCB 和 QoS 配置步骤。

1. 安装数据中心桥接\(DCB\)每台 HYPER-V 主机上。

   - **可选**iWarp 用于 RDMA 服务的网络配置。
   - **所需**的网络配置，使用 RoCE\(任何版本\)RDMA 服务。

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. 为 SMB Direct 设置 QoS 策略：

   - **可选**用于使用 iWarp 的网络配置。
   - **所需**用于使用 RoCE 的网络配置。

   在以下示例命令中，"3"的值是任意的。 只要您一直使用相同的值在整个配置 QoS 策略，可以使用 1 到 7 之间的任何值。

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**结果：** _


   |   参数    |          值           |
   |----------------|--------------------------|
   |      名称      |           SMB            |
   |     所有者      | 组策略\(机\) |
   | NetworkProfile |           全部            |
   |   优先级   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. 对于 RoCE 部署，开启**优先级流控制**对于 SMB 流量，这不需要 iWarp。

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**结果：** _


   | Priority | Enabled | PolicySet | ifIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  False  |  全局   | &nbsp;  | &nbsp;  |
   |    1     |  False  |  全局   | &nbsp;  | &nbsp;  |
   |    2     |  False  |  全局   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  全局   | &nbsp;  | &nbsp;  |
   |    4     |  False  |  全局   | &nbsp;  | &nbsp;  |
   |    5     |  False  |  全局   | &nbsp;  | &nbsp;  |
   |    6     |  False  |  全局   | &nbsp;  | &nbsp;  |
   |    7     |  False  |  全局   | &nbsp;  | &nbsp;  |

   ---

4. 适用于本地和目标网络适配器启用 QoS。

   - **不需要**用于使用 iWarp 的网络配置。
   - **所需**用于使用 RoCE 的网络配置。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**结果：** _

   **名称**：M1  
   **已启用**：True  

   _**功能：** _   


   |      参数      |   硬件   |   当前    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     无     |     无     |
   | NumTCs(Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses:** _ 


   | TC | TSA | 带宽 | 优先级 |
   |----|-----|-----------|------------|
   | 0  | ETS |    70%    |  0-2,4-7   |
   | 1  | ETS |    30%    |     3      |

   ---

   _**OperationalFlowControl:** _  

   优先级为 3 已启用  

   _**OperationalClassifications:** _  


   | Protocol  | 端口/类型 | Priority |
   |-----------|-----------|----------|
   |  默认  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

5. 用于 SMB 直通保留带宽的百分比\(RDMA\)。

    在此示例中，使用 30%的带宽预留。 应选择一个值，表示您期望存储通信需要该值。 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**结果：** _


   | 名称 | 算法 | Bandwidth(%) | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      30      |    3     |  全局   | &nbsp;  | &nbsp;  |

   ---                                      

6. 查看带宽保留设置。  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**结果：** _


   |   名称    | 算法 | Bandwidth(%) | Priority | PolicySet | ifIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | [默认] |    ETS    |      70      | 0-2,4-7  |  全局   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      30      |    3     |  全局   | &nbsp;  | &nbsp;  |

   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>步骤 5： （可选）解决 Mellanox 适配器调试器冲突 

默认情况下，使用 Mellanox 适配器时，附加调试程序会阻止 NetQos，这是一个已知的问题。 因此，如果使用的从 Mellanox 适配器并想要附加调试器，使用以下命令解决此问题。 如果你不想要附加调试器，或者如果您不使用 Mellanox 适配器，则不需要此步骤。

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>步骤 6： 验证 RDMA 配置 （本地主机）

你想要确保在创建 vSwitch，并转换为 RDMA (聚合 NIC) 之前正确配置了构造。 

下图显示的 HYPER-V 主机的当前状态。

![测试 RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. 验证 RDMA 配置。

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**结果：** _


   | 名称 |           InterfaceDescription           | Enabled |
   |------|------------------------------------------|---------|
   |  M1  | Mellanox ConnectX 3 Pro 以太网适配器 |  True   |

   ---

2. 确定**ifIndex**目标适配器的值。<p>运行下载的脚本时，可以在后续步骤中使用此值。

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**结果：** _ 


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |       M2       |       14       | {192.168.1.5} |

   ---

3. 下载[DiskSpd.exe 实用工具](https://aka.ms/diskspd)并将其解压缩到 C:\TEST\.

4. 下载[测试 RDMA powershell 脚本](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)到本地驱动器中，例如，C:\TEST 上的测试文件夹\.

5. 运行**测试 Rdma.ps1** PowerShell 脚本，以将 ifIndex 值传递给脚本，以及与同一 VLAN 上的远程适配器的 IP 地址。<p>在此示例中，脚本会将**ifIndex**上的远程网络适配器 IP 地址为 192.168.1.5 14 的值。

   ```PowerShell
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
   ```

   >[!NOTE]
   >如果具体而言，RDMA 通信失败，RoCE 用例应匹配的主机设置的正确 PFC/ETS 设置查阅 ToR 交换机配置。 请参阅本文档引用值中的 QoS 部分。

## <a name="step-7-remove-the-access-vlan-setting"></a>步骤 7： 删除访问 VLAN 设置

供将来创建的 HYPER-V 交换机，则必须删除您在上面安装的 VLAN 设置。  

1. 从物理 NIC，以防止自动标记不正确的 VLAN id 的出口流量 NIC 中删除访问 VLAN 设置<p>删除此设置还会阻止它从筛选入口流量不与匹配访问 VLAN id。

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. 确认**VlanID 设置**显示为零的 VLAN ID 值。

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>步骤 8。 上的 HYPER-V 主机创建 HYPER-V vSwitch

下图描绘的 HYPER-V 主机 1 与 vSwitch。

![创建 HYPER-V 虚拟交换机](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. 在 HYPER-V 主机 A.的 HYPER-V 中创建外部 HYPER-V vSwitch <p>在此示例中，此开关为 VMSTEST。 此外，参数**请将 AllowManagementOS**创建继承物理 NIC 的 MAC 和 IP 地址的主机 vNIC

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**结果：** _


   |  名称   | SwitchType |      NetAdapterInterfaceDescription      |
   |---------|------------|------------------------------------------|
   | VMSTEST |  外部  | Mellanox ConnectX 3 Pro 以太网适配器 |

   ---

2. 查看网络适配器属性。

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**结果：** _


   |         名称          |        InterfaceDescription         | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |-----------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet \(VMSTEST\) | Hyper V 虚拟以太网适配器 #2 |   27    |   向上   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---

3. 管理主机 vNIC 中两种方式之一。 

   - **NetAdapter**视图运行基于"vEthernet \(VMSTEST\)"名称。 在此视图中显示不是所有网络适配器属性。
   - **VMNetworkAdapter**视图删除"vEthernet"前缀，并仅使用 vmswitch 名称。 （建议） 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**结果：** _


   |         名称         | IsManagementOs |        VMName        |  SwitchName  | MacAddress | 状态 | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-External-Switch |      True      | CORP-External-Switch | 001B785768AA |    {确定}    | &nbsp; |             |
   |       VMSTEST        |      True      |       VMSTEST        | E41D2D074071 |    {确定}    | &nbsp; |             |

   ---

4. 测试连接。

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**结果：** _ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. 分配并查看网络适配器的 VLAN 设置。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**结果：** _


   | VMName | VMNetworkAdapterName |  模式  | VlanList |
   |--------|----------------------|--------|----------|
   | &nbsp; |       VMSTEST        | 访问 |   101    |

   ---  

6. 测试连接。<p>可能需要几秒钟即可完成之前可以成功地 ping 其他适配器。  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**结果：** _

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>步骤 9： 测试的 HYPER-V 虚拟交换机 RDMA （模式 2）

下图描绘了你的 HYPER-V 主机，包括 HYPER-V 主机 1 上的 vSwitch 的当前状态。

![测试的 HYPER-V 虚拟交换机](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. 设置主机 vNIC 上标记的优先级。

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  

   _**结果：** _

    名称：VMSTEST IeeePriorityTag:开


2. 查看网络适配器 RDMA 信息。 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**结果：** _


   |         名称          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST\) | Hyper V 虚拟以太网适配器 #2 |  False  |

   ---

   >[!NOTE]
   >如果将参数**已启用**具有值**False**，则表示未启用 RDMA。


3. 查看网络适配器信息。

   ```PowerShell
   Get-NetAdapter
   ```

   _**结果：** _   


   |        名称         |        InterfaceDescription         | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Hyper V 虚拟以太网适配器 #2 |   27    |   向上   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---


4. 在主机 vNIC 上启用 RDMA。

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**结果：** _


   |         名称          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST\) | Hyper V 虚拟以太网适配器 #2 |  True   |

   ---

   >[!NOTE]
   >如果将参数**已启用**具有值**True**，这意味着，启用 RDMA。

5. 执行 RDMA 流量测试。

   ```PowerShell    
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
   ```

此输出中的最后一行"RDMA 流量测试成功：RDMA 流量发送到为 192.168.1.5，"显示已成功配置聚合 NIC 上您的适配器。

## <a name="related-topics"></a>相关主题
- [聚合的 NIC 成组的 NIC 配置](cnic-datacenter.md)
- [聚合的 NIC 的物理交换机配置](cnic-app-switch-config.md)
- [故障排除聚合 NIC 配置](cnic-app-troubleshoot.md)

---
title: 使用单个网络适配器汇聚 NIC 配置
description: 在本主题中，我们将向你提供有关使用 Hyper-v 主机中的单个 NIC 配置收敛 NIC 的说明。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 2ad7592fd9faf1e92893e6271daabdad907d3aaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405794"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>使用单个网络适配器汇聚 NIC 配置

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，我们将向你提供有关使用 Hyper-v 主机中的单个 NIC 配置收敛 NIC 的说明。

本主题中的示例配置描述了两个 Hyper-v 主机、 **Hyper-v 主机 A**和**hyper-v 主机 B**。这两个主机都安装了单个物理 NIC （pNIC），Nic 连接到机架 \(ToR @ no__t-1 物理交换机。 此外，这些主机位于同一子网中，即 192.168.1/24。

![Hyper-V 主机](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>步骤 1： 测试源和目标之间的连接

确保物理 NIC 可以连接到目标主机。 此测试通过使用第3层 \(L3 @ no__t 或 IP 层以及第2层 \(L2 @ no__t 来演示连接性。

1. 查看网络适配器属性。

   ```PowerShell
   Get-NetAdapter
   ```

   _**后果**_  


   | 名称 |    InterfaceDescription     | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |------|-----------------------------|---------|--------|-------------------|-----------|
   |  限  | Mellanox ConnectX-3 专业版 。 |    4    |   向上   | 7C-8F-A1 |  40 Gbps  |

   ---

2. 查看其他适配器属性，包括 IP 地址。

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**后果**_

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

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>步骤 2： 确保源和目标可以通信

在此步骤中，我们将使用**Test-netconnection** Windows PowerShell 命令，但如果你愿意，可以使用**ping**命令。 

>[!TIP]
>如果你确信主机可以相互通信，则可以跳过此步骤。

1. 验证双向通信。

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**后果**_


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 为192.168.1.5 |
   |      RemoteAddress       | 为192.168.1.5 |
   |      InterfaceAlias      |     限      |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---

   在某些情况下，可能需要禁用具有高级安全性的 Windows 防火墙才能成功执行此测试。 如果禁用防火墙，请记住安全，并确保配置符合组织的安全要求。

2. 禁用所有防火墙配置文件。

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. 禁用防火墙配置文件后，再次测试连接。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**后果**_


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 为192.168.1.5 |
   |      RemoteAddress       | 为192.168.1.5 |
   |      InterfaceAlias      | 测试-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>步骤 3： 可有可无为安装在 Hyper-v 主机中的 Nic 配置 VLAN Id

许多网络配置利用 Vlan，如果你计划在网络中使用 Vlan，则必须重复前面的测试并配置 Vlan。 此外，如果打算使用 RoCE 进行 RDMA 服务，则必须启用 Vlan。

对于此步骤，Nic 处于**访问**模式。 但是，在本指南的后面部分创建 hyper-v 虚拟\(交换机\) vSwitch 时，VLAN 属性将应用于 vSwitch 端口级别。 

由于交换机可以托管多个 vlan，因此机架\(ToR\)物理交换机的顶部需要具有主机连接到的端口才能在 Trunk 模式下进行配置。

>[!NOTE]
>有关如何在交换机上配置干线模式的说明，请参阅 ToR 交换机文档。

下图显示了两个 Hyper-v 主机，每个主机都有一个物理网络适配器，并且每个主机都配置为在 VLAN 101 上进行通信。

![配置虚拟局域网](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>在本地和目标服务器上执行此项。 如果目标服务器未配置与本地服务器相同的 VLAN ID，则这两个服务器无法通信。


1. 配置 Hyper-v 主机上安装的 Nic 的 VLAN ID。

   >[!IMPORTANT]
   >如果通过此接口远程连接到主机，则不要运行此命令，因为这样会导致失去对主机的访问权限。

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**后果**_


   | 名称 | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------|-------------|--------------|-----------------|---------------|
   |  限  |   VLAN ID   |     101      |     VlanID      |     {101}     |

   ---

2. 重新启动网络适配器以应用 VLAN ID。

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. 确保状态为 "已**启动**"。

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```

   _**后果**_


   | 名称 |          InterfaceDescription           | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |------|-----------------------------------------|---------|--------|-------------------|-----------|
   |  限  | Mellanox ConnectX-3 Pro 以太网 Ada 。 |    4    |   向上   | 7C-8F-A1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >设备可能需要几秒钟才能重新启动并在网络上变为可用。 

4. 验证连接。<p>如果连接失败，请在同一 VLAN 中检查交换机 VLAN 配置或目标参与情况。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

## <a name="step-4-configure-quality-of-service-qos"></a>步骤 4： 配置服务\(QoS 的质量\)

>[!NOTE]
>必须在打算彼此通信的所有主机上执行以下所有 DCB 和 QoS 配置步骤。

1. 在每个 hyper-v \(主机\)上安装数据中心桥接 DCB。

   - 对于使用 iWarp for RDMA 服务的网络配置是**可选**的。
   - 对于使用 RoCE \(的网络配置，需要使用\) RDMA 服务的任何版本。

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. 为 SMB Direct 设置 QoS 策略：

   - 对于使用 iWarp 的网络配置是**可选**的。
   - 对于使用 RoCE 的网络配置是**必需**的。

   在下面的示例命令中，值 "3" 为任意值。 您可以使用介于1到7之间的任何值，只要在 QoS 策略的配置中始终使用相同的值。

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**后果**_


   |   参数    |          ReplTest1           |
   |----------------|--------------------------|
   |      名称      |           SMB            |
   |     所有者      | 组策略\(计算机\) |
   | NetworkProfile |           全部            |
   |   优先级   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. 对于 RoCE 部署，为 SMB 流量启用**优先级流控制**，这对于 iWarp 不是必需的。

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**后果**_


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

4. 为本地和目标网络适配器启用 QoS。

   - 对于使用 iWarp 的网络配置，**不是必需**的。
   - 对于使用 RoCE 的网络配置是**必需**的。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**后果**_

   **名称**：限  
   **已启用**：True  

   _**功能**_   


   |      参数      |   硬件   |   当前    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     无     |     无     |
   | NumTCs （Max/ETS/PFC） |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses:**_ 


   | TC | TSA | 带宽 | 因素 |
   |----|-----|-----------|------------|
   | 0  | ETS |    70%    |  0-2，4-7   |
   | 1  | ETS |    为期    |     3      |

   ---

   _**OperationalFlowControl:**_  

   优先级3已启用  

   _**OperationalClassifications:**_  


   | Protocol  | 端口/类型 | Priority |
   |-----------|-----------|----------|
   |  默认  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

5. 保留 SMB 直通 \(RDMA @ no__t 的带宽百分比。

    在此示例中，使用的带宽为 30%。 应选择一个值来表示你希望存储流量所需的内容。 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**后果**_


   | 名称 | 算法 | 带宽（%） | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      30      |    3     |  全局   | &nbsp;  | &nbsp;  |

   ---                                      

6. 查看带宽预留设置。  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**后果**_


   |   名称    | 算法 | 带宽（%） | Priority | PolicySet | ifIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | 缺省值 |    ETS    |      70      | 0-2，4-7  |  全局   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      30      |    3     |  全局   | &nbsp;  | &nbsp;  |

   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>步骤 5： 可有可无解决 Mellanox 适配器调试器冲突 

默认情况下，使用 Mellanox 适配器时，附加的调试程序会阻止 NetQos，这是一个已知问题。 因此，如果使用的是 Mellanox 中的适配器并且打算附加调试器，请使用以下命令解决此问题。 如果你不想要附加调试器，或者使用的不是 Mellanox 适配器，则不需要执行此步骤。

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>步骤 6： 验证 RDMA 配置（本机主机）

在创建 vSwitch 并过渡到 RDMA （汇聚 NIC）之前，你需要确保正确配置了构造。 

下图显示了 Hyper-v 主机的当前状态。

![测试 RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. 验证 RDMA 配置。

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**后果**_


   | 名称 |           InterfaceDescription           | Enabled |
   |------|------------------------------------------|---------|
   |  限  | Mellanox ConnectX 3 Pro 以太网适配器 |  True   |

   ---

2. 确定目标适配器的**ifIndex**值。<p>运行下载的脚本时，请在后续步骤中使用此值。

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**后果**_ 


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |       M2       |       14       | 为192.168.1.5 |

   ---

3. 下载[DiskSpd 实用工具](https://aka.ms/diskspd)，并将其解压缩到 C:\TEST\.

4. 将[测试-RDMA powershell 脚本](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)下载到本地驱动器上的测试文件夹，例如，C:\TEST @ no__t-1

5. 运行**Test-Rdma** PowerShell 脚本，将 ifIndex 值连同同一 VLAN 上的远程适配器的 IP 地址一起传递给脚本。<p>在此示例中，该脚本在远程网络适配器 IP 地址为192.168.1.5 上传递**ifIndex**值14。

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
   >如果 RDMA 流量失败，则针对 RoCE 情况，请参阅 ToR 交换机配置，获取应该与主机设置匹配的正确 PFC/ETS 设置。 有关引用值，请参阅本文档中的 QoS 部分。

## <a name="step-7-remove-the-access-vlan-setting"></a>步骤 7： 删除访问 VLAN 设置

为创建 Hyper-v 交换机做好准备，必须删除前面安装的 VLAN 设置。  

1. 删除物理 NIC 中的访问 VLAN 设置，以防止 NIC 使用错误的 VLAN ID 自动标记传出流量。<p>删除此设置还会阻止其筛选与访问 VLAN ID 不匹配的传入流量。

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. 确认**VlanID 设置**将 VLAN ID 值显示为零。

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>步骤8。 在 Hyper-v 主机上创建 Hyper-v vSwitch

下图描述了具有 vSwitch 的 Hyper-v 主机1。

![创建 Hyper-v 虚拟交换机](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. 在 hyper-v 主机 A 上的 Hyper-v 中创建外部 Hyper-v vSwitch。 <p>在此示例中，开关名为 VMSTEST。 此外，参数**AllowManagementOS**将创建一个继承物理 NIC 的 MAC 地址和 IP 地址的主机 vNIC。

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**后果**_


   |  名称   | SwitchType |      NetAdapterInterfaceDescription      |
   |---------|------------|------------------------------------------|
   | VMSTEST |  外部  | Mellanox ConnectX 3 Pro 以太网适配器 |

   ---

2. 查看网络适配器属性。

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**后果**_


   |         名称          |        InterfaceDescription         | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |-----------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet \(VMSTEST @ no__t-1 | Hyper-v 虚拟以太网适配器 #2 |   27    |   向上   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---

3. 采用以下两种方式之一管理主机 vNIC。 

   - **Get-netadapter**视图的操作基于 "vEthernet \(VMSTEST @ no__t-2" 名称。 并非所有网络适配器属性都显示在此视图中。
   - **VMNetworkAdapter**视图删除 "vEthernet" 前缀，只使用 vmswitch 名称。 （建议） 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**后果**_


   |         名称         | IsManagementOs |        VMName        |  SwitchName  | macAddress | 状态 | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | 公司外部-交换机 |      True      | 公司外部-交换机 | 001B785768AA |    正常    | &nbsp; |             |
   |       VMSTEST        |      True      |       VMSTEST        | E41D2D074071 |    正常    | &nbsp; |             |

   ---

4. 测试连接。

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**后果**_ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. 指定和查看网络适配器 VLAN 设置。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**后果**_


   | VMName | VMNetworkAdapterName |  模式  | VlanList |
   |--------|----------------------|--------|----------|
   | &nbsp; |       VMSTEST        | 访问 |   101    |

   ---  

6. 测试连接。<p>可能需要几秒钟时间才能成功 ping 另一个适配器。  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**后果**_

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>步骤 9： 测试 Hyper-v 虚拟交换机 RDMA （模式2）

下图描述了 Hyper-v 主机（包括 Hyper-v 主机1上的 vSwitch）的当前状态。

![测试 Hyper-v 虚拟交换机](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. 在主机 vNIC 上设置优先级标记。

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  

   _**后果**_

    名称：VMSTEST IeeePriorityTag :开


2. 查看网络适配器 RDMA 信息。 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**后果**_


   |         名称          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST @ no__t-1 | Hyper-v 虚拟以太网适配器 #2 |  False  |

   ---

   >[!NOTE]
   >如果**启用**的参数的值**为 False**，则表示未启用 RDMA。


3. 查看网络适配器信息。

   ```PowerShell
   Get-NetAdapter
   ```

   _**后果**_   


   |        名称         |        InterfaceDescription         | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet （VMSTEST） | Hyper-v 虚拟以太网适配器 #2 |   27    |   向上   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---


4. 在主机 vNIC 上启用 RDMA。

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**后果**_


   |         名称          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST @ no__t-1 | Hyper-v 虚拟以太网适配器 #2 |  True   |

   ---

   >[!NOTE]
   >如果**启用**的参数的值**为 True**，则表示已启用 RDMA。

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

此输出中的最后一行 "RDMA 流量测试成功：RDMA 流量已发送到为192.168.1.5，"显示已成功在适配器上配置了汇聚 NIC。

## <a name="related-topics"></a>相关主题
- [汇聚 NIC 分组 NIC 配置](cnic-datacenter.md)
- [汇聚 NIC 的物理交换机配置](cnic-app-switch-config.md)
- [聚合 NIC 配置疑难解答](cnic-app-troubleshoot.md)

---
title: 聚合的 NIC 成组 NIC 配置 （数据中心） 中
description: 在本主题中，我们为您提供部署聚合 NIC 成组 NIC 配置与交换机嵌入式组合 (SET) 中的说明。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: 58c4483c092c30a892ea6bdde20794270340fa8e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447023"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>聚合的 NIC 成组 NIC 配置 （数据中心） 中

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在本主题中，我们为你提供说明使用交换机嵌入式组合的成组 NIC 配置中部署聚合 NIC\(设置\)。 

本主题中的示例配置介绍了两个 HYPER-V 主机**HYPER-V 主机 1**并**HYPER-V 主机 2**。 这两个主机具有两个网络适配器。 在每个主机上，一个适配器连接至 192.168.1.x/24 子网和一个适配器连接至 192.168.2.x/24 子网。

![Hyper-V 主机](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>步骤 1： 测试源和目标之间的连接

确保物理 NIC 可以连接到目标主机。  此测试演示通过使用第 3 层连接\(L3\) -或 IP 层-，以及第 2 层\(L2\)虚拟局域网\(VLAN\)。

1. 查看第一个网络适配器属性。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**结果：** _


   |    名称    |           InterfaceDescription           | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |------------|------------------------------------------|---------|--------|-------------------|-----------|
   | 测试-40 G-1 | Mellanox ConnectX 3 Pro 以太网适配器 |   11    |   向上   | E4-1D-2D-07-43-D0 |  40 Gbps  |

   ---

2. 查看第一个适配器，其中包括 IP 地址的其他属性。 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**结果：** _


   |   参数    |    ReplTest1    |
   |----------------|-------------|
   |   IPAddress    | 192.168.1.3 |
   | InterfaceIndex |     11      |
   | InterfaceAlias | 测试-40 G-1  |
   | AddressFamily  |    IPv4     |
   |      在任务栏的搜索框中键入      |   单播   |
   |  PrefixLength  |     24      |

   ---

3. 查看第二个网络适配器属性。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```

   _**结果：** _


   |    名称    |          InterfaceDescription           | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | TEST-40G-2 | Mellanox ConnectX 3 Pro 以太网 A...#2 |   13    |   向上   | E4-1D-2D-07-40-70 |  40 Gbps  |

   ---

4. 查看第二个适配器，其中包括 IP 地址的其他属性。

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**结果：** _


   |   参数    |    ReplTest1    |
   |----------------|-------------|
   |   IPAddress    | 192.168.2.3 |
   | InterfaceIndex |     13      |
   | InterfaceAlias | TEST-40G-2  |
   | AddressFamily  |    IPv4     |
   |      在任务栏的搜索框中键入      |   单播   |
   |  PrefixLength  |     24      |

   ---

5. 验证该其他 NIC 组或一组成员 pNICs 具有有效的 IP 地址。<p>使用单独的子网\(xxx.xxx。**2**.xxx vs xxx.xxx。**1**.xxx\)，以便将此适配器发送到目标。 否则为如果同一子网上找到这两个 pNICs，请在接口中实现 Windows TCP/IP 堆栈负载平衡，简单的验证变得更加复杂。


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>步骤 2： 确保源和目标可以进行通信

在此步骤中，我们使用**Test-netconnection** Windows PowerShell 命令，但如果可以使用**ping**命令如果您更喜欢。 

1. 验证的双向通信。

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


4. 有关其他 Nic 验证的连接。 对 NIC 或一组团队中包含的所有后续 pNICs 重复前面的步骤。

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**结果：** _


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | 测试-40 G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0 毫秒     |

   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>步骤 3： 为安装在 HYPER-V 主机中的 Nic 配置 VLAN Id

许多网络配置进行使用的 Vlan，并且如果想要在网络中使用 Vlan，则必须重复前面的测试 vlan 配置。

对于此步骤的 Nic 位于**访问**模式。 但是，当创建 HYPER-V 虚拟交换机\(vSwitch\)更高版本在本指南中，VLAN 属性在 vSwitch 端口级别应用。 

由于交换机可以托管多个 Vlan，因此有必要为机架顶部\(ToR\)物理交换机以具有与该主机已连接到在 Trunk 模式下配置的端口。

>[!NOTE]
>有关如何在交换机上配置的 Trunk 模式的说明，请参阅 ToR 交换机文档。

下图显示了具有两个网络适配器具有 VLAN 101 和 VLAN 102 网络适配器属性中配置每个的两个 HYPER-V 主机。

![配置虚拟局域网](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>根据电气和电子工程师协会\(IEEE\)网络服务质量标准\(QoS\)物理 NIC 中的属性处理嵌入的 802.1p 标头在 802.1Q \(VLAN\)标头时配置 VLAN id。

1. 在第一个 NIC，测试 40 G 1 配置 VLAN ID。

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**结果：** _   


   |    名称    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-1 |   VLAN ID   |     101      |     VlanID      |     {101}     |

   ---

2. 重新启动的网络适配器应用 VLAN id。

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```

3. 确保状态为**向上**。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**结果：** _


   |    名称    |          InterfaceDescription           | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | 测试-40 G-1 | Mellanox ConnectX 3 Pro 以太网 Ada... |   11    |   向上   | E4-1D-2D-07-43-D0 |  40 Gbps  |

   ---

4. 在第二个 NIC，测试 40 G 2 配置 VLAN ID。

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**结果：** _


   |    名称    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-2 |   VLAN ID   |     102      |     VlanID      |     {102}     |

   ---

5. 重新启动的网络适配器应用 VLAN id。

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```

6. 确保状态为**向上**。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**结果：** _


   |    名称    |          InterfaceDescription           | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | 测试-40 G-2 | Mellanox ConnectX 3 Pro 以太网 Ada... |   11    |   向上   | E4-1D-2D-07-43-D1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >可能需要几秒钟，让设备重新启动并使其在网络上可用。 

7. 验证的连接的第一个 nic，测试-40 G-1。<p>如果连接失败，检查交换机 VLAN 配置或目标参与同一 VLAN 中。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**结果：** _   


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | 测试-40 G-1  |
   |      SourceAddress       | 192.168.1.5 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0 毫秒     |

   ---

8. 第一个 nic，测试 40 G 2 验证的连接。<p>如果连接失败，检查交换机 VLAN 配置或目标参与同一 VLAN 中。

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**结果：** _    


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | 测试-40 G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0 毫秒     |

   ---

   >[!IMPORTANT]
   >不罕见**Test-netconnection**或 ping 故障发生在执行后立即**重新启动 NetAdapter**。  因此请等待网络适配器，若要完全初始化，并再试一次。
   >
   >如果 VLAN 101 连接起作用，但 VLAN 102 连接不，问题可能是此开关需要配置为允许所需的 VLAN 上的端口流量。 可以通过暂时将故障适配器设置为 VLAN 101 并重复执行连接测试对此进行检查。


   已成功配置 Vlan 之后下, 图显示的 HYPER-V 主机。

   ![配置服务质量](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)


## <a name="step-4-configure-quality-of-service-qos"></a>步骤 4： 配置服务质量\(QoS\)

>[!NOTE]
>必须用于相互通信的所有主机上执行所有以下 DCB 和 QoS 配置步骤。

1. 安装数据中心桥接\(DCB\)每台 HYPER-V 主机上。

   - **可选**用于使用 iWarp 的网络配置。
   - **所需**的网络配置，使用 RoCE\(任何版本\)RDMA 服务。

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**结果：** _


   | 成功 | 需要重启 | 退出代码 |     功能结果     |
   |---------|----------------|-----------|------------------------|
   |  True   |       否       |  成功  | {数据中心桥接} |

   ---

2. 为 SMB Direct 设置 QoS 策略：

   - **可选**用于使用 iWarp 的网络配置。
   - **所需**的网络配置，使用 RoCE\(任何版本\)RDMA 服务。

   在以下示例命令中，"3"的值是任意的。 只要您一直使用相同的值在整个配置 QoS 策略，可以使用 1 到 7 之间的任何值。

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**结果：** _


   |   参数    |          ReplTest1           |
   |----------------|--------------------------|
   |      名称      |           SMB            |
   |     所有者      | 组策略\(机\) |
   | NetworkProfile |           全部            |
   |   优先级   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. 该接口上设置附加的 QoS 策略，针对其他流量。   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**结果：** _   


   |   参数    |          ReplTest1           |
   |----------------|--------------------------|
   |      名称      |         默认值          |
   |     所有者      | 组策略\(机\) |
   | NetworkProfile |           全部            |
   |   优先级   |           127            |
   |    模板    |         默认          |
   |   JobObject    |          &nbsp;          |
   | PriorityValue  |            0             |

   ---

4. 开启**优先级流控制**对于 SMB 流量，这不需要 iWarp。

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

   >**重要**如果您的结果不匹配这些结果，因为 3 以外的项具有已启用值为 True，则必须禁用**FlowControl**这些类。
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >在更复杂的配置，其他流量类可能需要流控制，但这些方案是本指南的作用域之外。


5. 为第一个 NIC，测试 40 G 1 启用 QoS。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**功能**:_   


   |      参数      |   硬件   |   当前    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     无     |     无     |
   | NumTCs(Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**:_    


   | TC |  TSA   | 带宽 | 优先级 |
   |----|--------|-----------|------------|
   | 0  | “严格” |  &nbsp;   |    0-7     |

   ---

   _**OperationalFlowControl**:_  

   优先级为 3 已启用  

   _**OperationalClassifications**:_  


   | Protocol  | 端口/类型 | Priority |
   |-----------|-----------|----------|
   |  默认  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

6. 为第二个 NIC，测试 40 G 2 中启用 QoS。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**功能**:_ 


   |      参数      |   硬件   |   当前    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     无     |     无     |
   | NumTCs(Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**:_  


   | TC |  TSA   | 带宽 | 优先级 |
   |----|--------|-----------|------------|
   | 0  | “严格” |  &nbsp;   |    0-7     |

   ---

    _**OperationalFlowControl**:_  

    优先级为 3 已启用  

   _**OperationalClassifications**:_  


   | Protocol  | 端口/类型 | Priority |
   |-----------|-----------|----------|
   |  默认  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---


7. 保留到 SMB 直通该带宽的一半\(RDMA\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```

   _**结果：** _  


   | 名称 | 算法 | Bandwidth(%) | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      50      |    3     |  全局   | &nbsp;  | &nbsp;  |

   ---

8. 查看带宽保留设置：   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```

   _**结果：** _  


   |   名称    | 算法 | Bandwidth(%) | Priority | PolicySet | ifIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | [默认] |    ETS    |      50      | 0-2,4-7  |  全局   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      50      |    3     |  全局   | &nbsp;  | &nbsp;  |

   ---

9. （可选）创建租户 IP 通信的两个额外的流量类。 

   >[!TIP]
   >可以省略"IP1"和"IP2"值。

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**结果：** _


   | 名称 | 算法 | Bandwidth(%) | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP1  |    ETS    |      10      |    1     |  全局   | &nbsp;  | &nbsp;  |

   ---

   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**结果：** _


   | 名称 | 算法 | Bandwidth(%) | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP2  |    ETS    |      10      |    2     |  全局   | &nbsp;  | &nbsp;  |

   ---

10. 查看 QoS 通信类。

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```

    _**结果：** _


    |   名称    | 算法 | Bandwidth(%) | Priority | PolicySet | ifIndex | IfAlias |
    |-----------|-----------|--------------|----------|-----------|---------|---------|
    | [默认] |    ETS    |      30      |  0,4-7   |  全局   | &nbsp;  | &nbsp;  |
    |    SMB    |    ETS    |      50      |    3     |  全局   | &nbsp;  | &nbsp;  |
    |    IP1    |    ETS    |      10      |    1     |  全局   | &nbsp;  | &nbsp;  |
    |    IP2    |    ETS    |      10      |    2     |  全局   | &nbsp;  | &nbsp;  |

    ---

11. （可选）重写调试器。<p>默认情况下，附加调试程序阻止 NetQos。 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**结果：** _  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>步骤 5： 验证 RDMA 配置\(模式 1\) 

你想要确保在创建 vSwitch，并转换为 RDMA 之前正确配置了构造\(模式 2\)。

下图显示的 HYPER-V 主机的当前状态。

![测试 RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. 验证 RDMA 配置。

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```

   _**结果：** _


   |    名称    |        InterfaceDescription        | Enabled |
   |------------|------------------------------------|---------|
   | TEST-40G-1 | Mellanox ConnectX 4 VPI 适配器 #2 |  True   |
   | TEST-40G-2 |  Mellanox ConnectX 4 VPI 适配器   |  True   |

   ---

2. 确定**ifIndex**目标适配器的值。<p>运行下载的脚本时，可以在后续步骤中使用此值。   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**结果：** _


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |   TEST-40G-1   |       14       | {192.168.1.3} |
   |   TEST-40G-2   |       13       | {192.168.2.3} |

   ---

3. 下载[DiskSpd.exe 实用工具](https://aka.ms/diskspd)并将其解压缩到 C:\TEST\.

4. 下载[测试 RDMA PowerShell 脚本](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)到本地驱动器中，例如，C:\TEST 上的测试文件夹\.

5. 运行**测试 Rdma.ps1** PowerShell 脚本，以将 ifIndex 值传递给脚本，以及与同一 VLAN 上的第一个远程适配器的 IP 地址。<p>在此示例中，脚本会将**ifIndex**上的远程网络适配器 IP 地址为 192.168.1.5 14 的值。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**结果：** _ 

   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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

6. 运行**测试 Rdma.ps1** PowerShell 脚本，以将 ifIndex 值传递给脚本，以及与同一 VLAN 上的第二个远程适配器的 IP 地址。<p>在此示例中，脚本会将**ifIndex**值上的远程网络适配器 IP 地址 192.168.2.5 13。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**结果：** _ 

   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ``` 

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>步骤 6： 上的 HYPER-V 主机创建 HYPER-V vSwitch


下图显示的 HYPER-V 主机 1 与 vSwitch。

![创建 HYPER-V 虚拟交换机](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. 中的 HYPER-V 主机 1 上的设置模式创建的 vSwitch。

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```

   _**结果：** _


   |  名称   | SwitchType | NetAdapterInterfaceDescription |
   |---------|------------|--------------------------------|
   | VMSTEST |  外部  |        成组接口        |

   ---

2. 在集中查看物理适配器组。

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```

   _**结果：** _  

   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```

3. 显示两个主机 vNIC 视图

   ```PowerShell
    Get-NetAdapter
   ```

   _**结果：** _


   |        名称         |        InterfaceDescription         | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Hyper V 虚拟以太网适配器 #2 |   28    |   向上   | E4-1D-2D-07-40-71 |  80 Gbps  |

   ---

4. 查看主机 vNIC 的其他属性。 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**结果：** _


   |  名称   | IsManagementOs | VMName  |  SwitchName  | MacAddress | 状态 | IPAddresses |
   |---------|----------------|---------|--------------|------------|--------|-------------|
   | VMSTEST |      True      | VMSTEST | E41D2D074071 |    {确定}    | &nbsp; |             |

   ---


5. 测试与远程 VLAN 101 适配器的网络连接。

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```

   _**结果：** _  

   ```
   WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable

   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (CORP-External-Switch)
   SourceAddress  : 10.199.48.170
   PingSucceeded  : False
   PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-7-remove-the-access-vlan-setting"></a>步骤 7： 删除访问 VLAN 设置

在此步骤中，从物理 NIC，并设置使用 vSwitch VLANID 删除访问 VLAN 设置。

必须删除访问 VLAN 设置，以避免这两个自动标记不正确的 VLAN id 的传出流量，并从筛选入站流量的不匹配访问 VLAN id。

1. 删除设置。

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. 设置 VLAN id。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```

   _**结果：** _  

   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```


3. 测试网络连接。

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

   >**重要**如果您的结果不是类似于示例结果并 ping 失败，出现消息"警告：为 192.168.1.5 的 Ping 失败--状态：DestinationHostUnreachable，"确认"vEthernet (VMSTEST)"具有正确的 IP 地址。
   >
   >```PowerShell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >如果未设置 IP 地址，更正此问题。
   >
   >```PowerShell
   >New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
   >
   >IPAddress : 192.168.1.3
   >InterfaceIndex: 37
   >InterfaceAlias: vEthernet (VMSTEST)
   >AddressFamily : IPv4
   >Type  : Unicast
   >PrefixLength  : 24
   >PrefixOrigin  : Manual
   >SuffixOrigin  : Manual
   >AddressState  : Tentative
   >ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   >PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   >SkipAsSource  : False
   >PolicyStore   : ActiveStore
   >```  


4. 重命名管理 nic。

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**结果：** _ 


   |         名称         | IsManagementOs | VMName |      SwitchName      |  MacAddress  | 状态 | IPAddresses |
   |----------------------|----------------|--------|----------------------|--------------|--------|-------------|
   | CORP-External-Switch |      True      | &nbsp; | CORP-External-Switch | 001B785768AA |  {确定}  |   &nbsp;    |
   |         MGT          |      True      | &nbsp; |       VMSTEST        | E41D2D074071 |  {确定}  |   &nbsp;    |

   ---

5. 查看更多 NIC 属性。

   ```PowerShell
   Get-NetAdapter
   ```

   _**结果：** _


   |      名称       |        InterfaceDescription         | ifIndex | 状态 |    MacAddress     | LinkSpeed |
   |-----------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (MGT) | Hyper V 虚拟以太网适配器 #2 |   28    |   向上   | E4-1D-2D-07-40-71 |  80 Gbps  |

   ---

## <a name="step-8-test-hyper-v-vswitch-rdma"></a>步骤 8。 测试 HYPER-V vSwitch RDMA

下图显示了你的 HYPER-V 主机，包括 HYPER-V 主机 1 上的 vSwitch 的当前状态。

![测试的 HYPER-V 虚拟交换机](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. 设置优先级标记上主机 vNIC 补充了以前的 VLAN 设置。

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```

   _**结果：** _  

   名称：MGT  
   IeeePriorityTag:开  

2. 为 RDMA 创建两个主机 Vnic 并将其连接到 vSwitch VMSTEST。

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```

3. 查看管理 NIC 属性。

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**结果：** _ 


   |         名称         | IsManagementOs |        VMName        |  SwitchName  | MacAddress | 状态 | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-External-Switch |      True      | CORP-External-Switch | 001B785768AA |    {确定}    | &nbsp; |             |
   |         管理          |      True      |       VMSTEST        | E41D2D074071 |    {确定}    | &nbsp; |             |
   |         SMB1         |      True      |       VMSTEST        | 00155D30AA00 |    {确定}    | &nbsp; |             |
   |         SMB2         |      True      |       VMSTEST        | 00155D30AA01 |    {确定}    | &nbsp; |             |

   ---

## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>步骤 9： 将 IP 地址分配到 SMB 主机 Vnic vEthernet \(SMB1\) vEthernet 和\(SMB2\)

测试-40 G-1 和 2-测试-40 G 物理适配器仍然可以访问 VLAN 为 101 和 102 配置。 因此，适配器标记通信-和 ping 操作成功。 以前，配置为零，这两个 pNIC VLAN Id，然后设置 VMSTEST vSwitch 为 VLAN 101。 在此之后，就仍然能够 ping 远程 VLAN 101 适配器通过使用管理 vNIC，但当前没有 VLAN 102 成员。



1. 从以防止它在这两个自动标记不正确的 VLAN id 的出口流量并阻止它从筛选与访问 VLAN id。 不匹配的传入流量的物理 NIC 中删除访问 VLAN 设置

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**结果：** _  

   ```   
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
   ```

2. 测试远程 VLAN 102 适配器。

   ```PowerShell
   Test-NetConnection 192.168.2.5 
   ```

   _**结果：** _  

   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

3. 添加新的 IP 地址的接口 vEthernet \(SMB2\)。

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```

   _**结果：** _ 

   ```
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
   ```

4. 再次测试连接。    


5. 在预先存在的 VLAN 102 上放置 RDMA 主机 Vnic。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS

   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**结果：** _ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```

6. 检查 SMB1 和 vSwitch 设置团队下的基础物理 Nic 的 SMB2 的映射。<p>主机 vNIC 到物理 Nic 的关联是随机的并且受在创建和析构期间重新平衡。 在此情况下，可以使用间接机制来检查当前的关联。 SMB1 和 SMB2 的 MAC 地址与相关联的 NIC 团队成员测试-40 G-2。 这并不理想，因为测试 40 G 1 不具有关联的 SMB 主机 vNIC，并且不允许利用 RDMA 流量通过链接直到 SMB 主机 vNIC 映射到它。

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)

   Get-NetAdapterVmqQueue
   ```

   _**结果：** _ 

   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. 查看虚拟机网络适配器属性。

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**结果：** _ 

   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. 查看网络适配器团队映射。<p>结果应该返回的信息，因为尚未执行映射。

   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```


9. 映射 SMB1 和 SMB2 单独的物理 NIC 团队成员，并查看你的操作的结果。

   >[!IMPORTANT]
   >请确保完成此步骤中的继续操作前，或您的实现将失败。

   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"

   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**结果：** _ 

   ```   
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
   ```

10. 确认以前创建的 MAC 关联。

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**结果：** _ 

    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. 测试从远程系统的连接，因为这两个主机 Vnic 驻留在同一子网中并且具有相同的 VLAN ID \(102\)。

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**结果：** _   

    ```
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```

    ```PowerShell   
    Test-NetConnection 192.168.2.222
    ```

    _**结果：** _   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. 设置名称、 交换机名称和优先级标记。   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**结果：** _   

    ```
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On 
    Status  : {Ok}

    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    ```

13. 查看 vEthernet 网络适配器属性。

    ```PowerShell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**结果：** _   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    ```

14. 启用 vEthernet 网络适配器。  

    ```PowerShell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**结果：** _   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>步骤 10。 验证 RDMA 功能。

你想要验证从远程系统为本地系统，它具有 vSwitch，这两个的 vSwitch 将团队成员的 RDMA 功能。<p>因为这两个主机 Vnic \(SMB1 和 SMB2\)分配 VLAN 102，您可以选择在远程系统上的 VLAN 102 适配器。 <p>在此示例中，NIC 测试 40 G 2 强调 RDMA 到 SMB1 (192.168.2.111) 和 SMB2 (192.168.2.222)。

>[!TIP]
>您可能需要禁用此系统上的防火墙。  有关详细信息，请参考结构策略。
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**结果：** _ 
>   
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
>----  -----------------------   --------------- -------------  
> .
> .
>Test-40G-2VLAN ID102VlanID  {102} 
>```

1. 查看网络适配器属性。

   ```PowerShell
   Get-NetAdapter
   ```

   _**结果：** _ 

   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```

2. 查看网络适配器 RDMA 信息。

   ```PowerShell
   Get-NetAdapterRdma
   ```

   _**结果：** _  

   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. 执行第一个物理适配器的 RDMA 流量测试。

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**结果：** _ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

4. 执行第二个物理适配器的 RDMA 流量测试。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**结果：** _ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

5. 测试从本地到远程计算机的 RDMA 通信。

    ```PowerShell
    Get-NetAdapter | ft –AutoSize
    ```

    _**结果：** _ 

    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. 执行第一个虚拟适配器的 RDMA 流量测试。    

   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**结果：** _ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

7. 执行第二个虚拟适配器的 RDMA 流量测试。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**结果：** _ 

   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

此输出中的最后一行"RDMA 流量测试成功：RDMA 流量发送到 192.168.2.5，"显示已成功配置聚合 NIC 上您的适配器。

## <a name="related-topics"></a>相关主题 

- [聚合的 NIC 配置具有单个网络适配器](cnic-single.md)
- [聚合的 NIC 的物理交换机配置](cnic-app-switch-config.md)
- [故障排除聚合 NIC 配置](cnic-app-troubleshoot.md)

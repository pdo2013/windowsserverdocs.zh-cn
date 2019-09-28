---
title: 成组 NIC 配置（数据中心）中的汇聚 NIC
description: 在本主题中，我们将向你提供有关使用交换机嵌入组合（SET）在成组的 NIC 配置中部署汇聚 NIC 的说明。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: e4c305a7c8c4c4618b0df1e1b2a646356d8f821f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356118"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>成组 NIC 配置（数据中心）中的汇聚 NIC

>适用于：Windows Server（半年频道）、Windows Server 2016

在本主题中，我们将向你提供有关使用交换机嵌入组合\(集\)在成组的 nic 配置中部署汇聚 nic 的说明。 

本主题中的示例配置介绍了两个 Hyper-v 主机： **Hyper-v 主机 1**和**hyper-v 主机 2**。 两个主机具有两个网络适配器。 在每个主机上，一个适配器连接到 192.168.1/24 子网，一个适配器连接到 192.168.2/24 子网。

![Hyper-V 主机](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>步骤 1： 测试源和目标之间的连接

确保物理 NIC 可以连接到目标主机。  此测试通过使用\(第 3\)层或 IP 层和第 2 \(层二级\)虚拟局域网\(VLAN\)来演示连接性。

1. 查看第一个网络适配器属性。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**后果**_


   |    名称    |           InterfaceDescription           | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |------------|------------------------------------------|---------|--------|-------------------|-----------|
   | 测试-40G-1 | Mellanox ConnectX 3 Pro 以太网适配器 |   11    |   向上   | E4-1D-07-43-D0 |  40 Gbps  |

   ---

2. 查看第一个适配器的其他属性，包括 IP 地址。 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**后果**_


   |   参数    |    ReplTest1    |
   |----------------|-------------|
   |   IPAddress    | 192.168.1.3 |
   | InterfaceIndex |     11      |
   | InterfaceAlias | 测试-40G-1  |
   | AddressFamily  |    IPv4     |
   |      类型      |   单播   |
   |  PrefixLength  |     24      |

   ---

3. 查看第二个网络适配器属性。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```

   _**后果**_


   |    名称    |          InterfaceDescription           | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | 测试-40G-2 | Mellanox ConnectX-3 Pro 以太网 ... #2 |   13    |   向上   | E4-1D-2D-07-40-70 |  40 Gbps  |

   ---

4. 查看第二个适配器的其他属性，包括 IP 地址。

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**后果**_


   |   参数    |    ReplTest1    |
   |----------------|-------------|
   |   IPAddress    | 192.168.2.3 |
   | InterfaceIndex |     13      |
   | InterfaceAlias | 测试-40G-2  |
   | AddressFamily  |    IPv4     |
   |      类型      |   单播   |
   |  PrefixLength  |     24      |

   ---

5. 验证其他 NIC 团队或 SET 成员 pNICs 是否具有有效的 IP 地址。<p>使用单独的子网\(xxx.xxx。2.x 和 xxx.xxx。**1**. xxx\)，有助于从此适配器向目标发送。 否则，如果在同一子网上找到这两个 pNICs，则这些接口和简单验证之间的 Windows TCP/IP 堆栈负载平衡会变得更加复杂。


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>步骤 2： 确保源和目标可以通信

在此步骤中，我们将使用**Test-netconnection** Windows PowerShell 命令，但如果你愿意，可以使用**ping**命令。 

1. 验证双向通信。

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


4. 验证其他 Nic 的连接。 对 NIC 或设置团队中包含的所有后续 pNICs 重复上述步骤。

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**后果**_


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | 测试-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>步骤 3： 为安装在 Hyper-v 主机中的 Nic 配置 VLAN Id

许多网络配置利用 Vlan，如果你计划在网络中使用 Vlan，则必须重复前面的测试并配置 Vlan。

对于此步骤，Nic 处于**访问**模式。 但是，在本指南的后面部分创建 hyper-v 虚拟\(交换机\) vSwitch 时，VLAN 属性将应用于 vSwitch 端口级别。 

由于交换机可以托管多个 vlan，因此机架\(ToR\)物理交换机的顶部需要具有主机连接到的端口才能在 Trunk 模式下进行配置。

>[!NOTE]
>有关如何在交换机上配置干线模式的说明，请参阅 ToR 交换机文档。

下图显示了两个具有两个网络适配器的 Hyper-v 主机，其中每个网络适配器都在网络适配器属性中配置了 VLAN 101 和 VLAN 102。

![配置虚拟局域网](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>根据电气\(和电子工程师协会 IEEE\)网络标准，物理 NIC 中的服务\(QoS\)属性的质量在嵌入的 802.1 p 标头上起作用在配置 VLAN ID \(时\) ，802.1 q VLAN 标头中。

1. 在第一个 NIC 上配置 VLAN ID。

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**后果**_   


   |    名称    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | 测试-40G-1 |   VLAN ID   |     101      |     VlanID      |     {101}     |

   ---

2. 重新启动网络适配器以应用 VLAN ID。

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```

3. 确保状态为 "已**启动**"。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**后果**_


   |    名称    |          InterfaceDescription           | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | 测试-40G-1 | Mellanox ConnectX-3 Pro 以太网 Ada 。 |   11    |   向上   | E4-1D-07-43-D0 |  40 Gbps  |

   ---

4. 在第二个 NIC 上配置 VLAN ID。

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**后果**_


   |    名称    | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------------|-------------|--------------|-----------------|---------------|
   | 测试-40G-2 |   VLAN ID   |     102      |     VlanID      |     {102}     |

   ---

5. 重新启动网络适配器以应用 VLAN ID。

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```

6. 确保状态为 "已**启动**"。

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**后果**_


   |    名称    |          InterfaceDescription           | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | 测试-40G-2 | Mellanox ConnectX-3 Pro 以太网 Ada 。 |   11    |   向上   | E4-1D-07-43-D1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >设备可能需要几秒钟才能重新启动并在网络上变为可用。 

7. 验证第一个 NIC 的连接性（测试-40G-1）。<p>如果连接失败，请在同一 VLAN 中检查交换机 VLAN 配置或目标参与情况。 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**后果**_   


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 为192.168.1.5 |
   |      RemoteAddress       | 为192.168.1.5 |
   |      InterfaceAlias      | 测试-40G-1  |
   |      SourceAddress       | 为192.168.1.5 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---

8. 验证第一个 NIC 的连接，测试-40G-2。<p>如果连接失败，请在同一 VLAN 中检查交换机 VLAN 配置或目标参与情况。

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**后果**_    


   |        参数         |    ReplTest1    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | 测试-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0毫秒     |

   ---

   >[!IMPORTANT]
   >执行**重启-get-netadapter**后，不会立即发生**test-netconnection**或 ping 故障，这种情况并不常见。  请等待网络适配器完全初始化，然后重试。
   >
   >如果 VLAN 101 连接正常，但 VLAN 102 连接不起作用，则问题可能是需要将交换机配置为允许所需 VLAN 上的端口流量。 可以通过暂时将故障适配器设置为 VLAN 101，并重复连接测试来检查此情况。


   下图显示了成功配置 Vlan 后的 Hyper-v 主机。

   ![配置服务质量](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)


## <a name="step-4-configure-quality-of-service-qos"></a>步骤 4： 配置服务\(QoS 的质量\)

>[!NOTE]
>必须在打算彼此通信的所有主机上执行以下所有 DCB 和 QoS 配置步骤。

1. 在每个 hyper-v \(主机\)上安装数据中心桥接 DCB。

   - 对于使用 iWarp 的网络配置是**可选**的。
   - 对于使用 RoCE \(的网络配置，需要使用\) RDMA 服务的任何版本。

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**后果**_


   | 成功 | 需要重启 | 退出代码 |     功能结果     |
   |---------|----------------|-----------|------------------------|
   |  True   |       否       |  成功  | {数据中心桥接} |

   ---

2. 为 SMB Direct 设置 QoS 策略：

   - 对于使用 iWarp 的网络配置是**可选**的。
   - 对于使用 RoCE \(的网络配置，需要使用\) RDMA 服务的任何版本。

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

3. 为接口上的其他流量设置附加的 QoS 策略。   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**后果**_   


   |   参数    |          ReplTest1           |
   |----------------|--------------------------|
   |      名称      |         默认值          |
   |     所有者      | 组策略\(计算机\) |
   | NetworkProfile |           全部            |
   |   优先级   |           127            |
   |    模板    |         默认          |
   |   JobObject    |          &nbsp;          |
   | PriorityValue  |            0             |

   ---

4. 为 SMB 流量启用**优先级流控制**，这对于 iWarp 不是必需的。

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

   >**重要提示**如果你的结果不匹配这些结果，原因是3之外的项的已启用值为 True，则必须为这些类禁用**FlowControl** 。
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >在更复杂的配置下，其他流量类可能需要流控制，但这些方案不在本指南的讨论范围内。


5. 为第一个 NIC （测试-40G-1）启用 QoS。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**功能**：_   


   |      参数      |   硬件   |   当前    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     无     |     无     |
   | NumTCs （Max/ETS/PFC） |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**：_    


   | TC |  TSA   | 带宽 | 因素 |
   |----|--------|-----------|------------|
   | 0  | “严格” |  &nbsp;   |    0-7     |

   ---

   _**OperationalFlowControl**：_  

   优先级3已启用  

   _**OperationalClassifications**：_  


   | Protocol  | 端口/类型 | Priority |
   |-----------|-----------|----------|
   |  默认  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

6. 为第二个 NIC （测试-40G-2）启用 QoS。

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**功能**：_ 


   |      参数      |   硬件   |   当前    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     无     |     无     |
   | NumTCs （Max/ETS/PFC） |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses**：_  


   | TC |  TSA   | 带宽 | 因素 |
   |----|--------|-----------|------------|
   | 0  | “严格” |  &nbsp;   |    0-7     |

   ---

    _**OperationalFlowControl**：_  

    优先级3已启用  

   _**OperationalClassifications**：_  


   | Protocol  | 端口/类型 | Priority |
   |-----------|-----------|----------|
   |  默认  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---


7. 保留到 SMB 直接\(RDMA 的一半带宽\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```

   _**后果**_  


   | 名称 | 算法 | 带宽（%） | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      50      |    3     |  全局   | &nbsp;  | &nbsp;  |

   ---

8. 查看带宽预留设置：   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```

   _**后果**_  


   |   名称    | 算法 | 带宽（%） | Priority | PolicySet | ifIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | 缺省值 |    ETS    |      50      | 0-2，4-7  |  全局   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      50      |    3     |  全局   | &nbsp;  | &nbsp;  |

   ---

9. 可有可无为租户 IP 流量另外创建两个通信类。 

   >[!TIP]
   >可以省略 "IP1" 和 "IP2" 值。

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**后果**_


   | 名称 | 算法 | 带宽（%） | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP1  |    ETS    |      10      |    1     |  全局   | &nbsp;  | &nbsp;  |

   ---

   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**后果**_


   | 名称 | 算法 | 带宽（%） | Priority | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP2  |    ETS    |      10      |    2     |  全局   | &nbsp;  | &nbsp;  |

   ---

10. 查看 QoS 通信类。

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```

    _**后果**_


    |   名称    | 算法 | 带宽（%） | Priority | PolicySet | ifIndex | IfAlias |
    |-----------|-----------|--------------|----------|-----------|---------|---------|
    | 缺省值 |    ETS    |      30      |  0、4-7   |  全局   | &nbsp;  | &nbsp;  |
    |    SMB    |    ETS    |      50      |    3     |  全局   | &nbsp;  | &nbsp;  |
    |    IP1    |    ETS    |      10      |    1     |  全局   | &nbsp;  | &nbsp;  |
    |    IP2    |    ETS    |      10      |    2     |  全局   | &nbsp;  | &nbsp;  |

    ---

11. 可有可无重写调试器。<p>默认情况下，附加的调试器会阻止 NetQos。 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**后果**_  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>步骤 5： 验证 RDMA 配置\(模式1\) 

在创建 vSwitch 并过渡到 RDMA \(模式 2\)之前，你需要确保正确配置了构造。

下图显示了 Hyper-v 主机的当前状态。

![测试 RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. 验证 RDMA 配置。

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```

   _**后果**_


   |    名称    |        InterfaceDescription        | Enabled |
   |------------|------------------------------------|---------|
   | 测试-40G-1 | Mellanox ConnectX-4 VPI 适配器 #2 |  True   |
   | 测试-40G-2 |  Mellanox ConnectX-4 VPI 适配器   |  True   |

   ---

2. 确定目标适配器的**ifIndex**值。<p>运行下载的脚本时，请在后续步骤中使用此值。   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**后果**_


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |   测试-40G-1   |       14       | {192.168.1.3} |
   |   测试-40G-2   |       13       | {192.168.2.3} |

   ---

3. 下载[DiskSpd 实用工具](https://aka.ms/diskspd)，并将其解压缩到 C:\TEST\.

4. 将[测试-RDMA PowerShell 脚本](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)下载到本地驱动器上的测试文件夹，例如，C:\TEST\.

5. 运行**Test-Rdma** PowerShell 脚本，将 ifIndex 值连同同一 VLAN 上第一个远程适配器的 IP 地址一起传递给脚本。<p>在此示例中，该脚本在远程网络适配器 IP 地址为192.168.1.5 上传递**ifIndex**值14。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**后果**_ 

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
   >如果 RDMA 流量失败，则针对 RoCE 情况，请参阅 ToR 交换机配置，获取应该与主机设置匹配的正确 PFC/ETS 设置。 有关引用值，请参阅本文档中的 QoS 部分。

6. 运行**Test-Rdma** PowerShell 脚本，将 ifIndex 值连同同一 VLAN 上第二个远程适配器的 IP 地址一起传递给脚本。<p>在此示例中，该脚本在远程网络适配器 IP 地址192.168.2.5 上传递**ifIndex**值13。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**后果**_ 

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

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>步骤 6： 在 Hyper-v 主机上创建 Hyper-v vSwitch


下图显示了具有 vSwitch 的 Hyper-v 主机1。

![创建 Hyper-v 虚拟交换机](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. 在 Hyper-v 主机1上以集模式创建 vSwitch。

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```

   _**输出**_


   |  名称   | SwitchType | NetAdapterInterfaceDescription |
   |---------|------------|--------------------------------|
   | VMSTEST |  外部  |        成组-接口        |

   ---

2. 查看集中的物理适配器组。

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```

   _**后果**_  

   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```

3. 显示主机 vNIC 的两个视图

   ```PowerShell
    Get-NetAdapter
   ```

   _**后果**_


   |        名称         |        InterfaceDescription         | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet （VMSTEST） | Hyper-v 虚拟以太网适配器 #2 |   28    |   向上   | E4-1D-2D-07-40-71 |  80 Gbps  |

   ---

4. 查看主机 vNIC 的其他属性。 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**后果**_


   |  名称   | IsManagementOs | VMName  |  SwitchName  | macAddress | 状态 | IPAddresses |
   |---------|----------------|---------|--------------|------------|--------|-------------|
   | VMSTEST |      True      | VMSTEST | E41D2D074071 |    正常    | &nbsp; |             |

   ---


5. 测试与远程 VLAN 101 适配器的网络连接。

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```

   _**后果**_  

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

在此步骤中，将从物理 NIC 中删除访问 VLAN 设置，并使用 vSwitch 来设置 VLANID。

你必须删除访问 VLAN 设置，以防止使用错误的 VLAN ID 自动标记传出流量，并通过筛选入口流量而不匹配访问 VLAN ID。

1. 删除设置。

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. 设置 VLAN ID。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```

   _**后果**_  

   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```


3. 测试网络连接。

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

   >**重要提示**如果结果与示例结果不类似，并且 ping 失败并出现消息 "警告：Ping 到为192.168.1.5 失败-状态：DestinationHostUnreachable "确认" vEthernet （VMSTEST） "具有正确的 IP 地址。
   >
   >```PowerShell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >如果未设置 IP 地址，请更正此问题。
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


4. 重命名管理 NIC。

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**后果**_ 


   |         名称         | IsManagementOs | VMName |      SwitchName      |  macAddress  | 状态 | IPAddresses |
   |----------------------|----------------|--------|----------------------|--------------|--------|-------------|
   | 公司外部-交换机 |      True      | &nbsp; | 公司外部-交换机 | 001B785768AA |  正常  |   &nbsp;    |
   |         管理          |      True      | &nbsp; |       VMSTEST        | E41D2D074071 |  正常  |   &nbsp;    |

   ---

5. 查看其他 NIC 属性。

   ```PowerShell
   Get-NetAdapter
   ```

   _**后果**_


   |      名称       |        InterfaceDescription         | ifIndex | 状态 |    macAddress     | LinkSpeed |
   |-----------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet （进行） | Hyper-v 虚拟以太网适配器 #2 |   28    |   向上   | E4-1D-2D-07-40-71 |  80 Gbps  |

   ---

## <a name="step-8-test-hyper-v-vswitch-rdma"></a>步骤8。 测试 Hyper-v vSwitch RDMA

下图显示了 Hyper-v 主机（包括 Hyper-v 主机1上的 vSwitch）的当前状态。

![测试 Hyper-v 虚拟交换机](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. 在主机 vNIC 上设置优先级标记，以补充以前的 VLAN 设置。

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```

   _**后果**_  

   路径名管理  
   IeeePriorityTag :开  

2. 为 RDMA 创建两个主机 Vnic，并将其连接到 vSwitch VMSTEST。

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```

3. 查看管理 NIC 属性。

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**后果**_ 


   |         名称         | IsManagementOs |        VMName        |  SwitchName  | macAddress | 状态 | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | 公司外部-交换机 |      True      | 公司外部-交换机 | 001B785768AA |    正常    | &nbsp; |             |
   |         管理          |      True      |       VMSTEST        | E41D2D074071 |    正常    | &nbsp; |             |
   |         SMB1         |      True      |       VMSTEST        | 00155D30AA00 |    正常    | &nbsp; |             |
   |         SMB2         |      True      |       VMSTEST        | 00155D30AA01 |    正常    | &nbsp; |             |

   ---

## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>步骤 9： 将 IP 地址分配到 SMB 主机 vnic vEthernet \(SMB1\)和 vEthernet \(SMB2\)

测试-40G-1 和测试-40G-2 物理适配器的访问 VLAN 配置为101和102。 因此，适配器将对流量进行标记，并成功进行 ping 操作。 以前，你已将 pNIC VLAN Id 配置为零，并将 VMSTEST vSwitch 设置为 VLAN 101。 之后，仍可以使用 "vNIC" 对远程 VLAN 101 适配器进行 ping 操作，但目前没有 VLAN 102 成员。



1. 删除物理 NIC 中的访问 VLAN 设置，以防止它使用错误的 VLAN ID 自动标记传出流量，并防止它筛选出与访问 VLAN ID 不匹配的流量。

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**后果**_  

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

   _**后果**_  

   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

3. 为接口 vEthernet \(SMB2\)添加新的 IP 地址。

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```

   _**后果**_ 

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

4. 重新测试连接。    


5. 将 RDMA 主机 Vnic 置于预先存在的 VLAN 102 上。

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS

   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**后果**_ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```

6. 检查在 vSwitch 集团队下的底层物理 Nic 之间的 SMB1 和 SMB2 映射。<p>主机 vNIC 与物理 Nic 之间的关联是随机的，在创建和销毁期间可能会重新平衡。 在这种情况下，可以使用间接机制来检查当前关联。 SMB1 和 SMB2 的 MAC 地址与 NIC 团队成员测试-40G-2 关联。 这并不理想，因为测试-40G-1 没有关联的 SMB 主机 vNIC，因此在 SMB 主机 vNIC 映射到该链接之前，将不允许通过该链接利用 RDMA 流量。

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)

   Get-NetAdapterVmqQueue
   ```

   _**后果**_ 

   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. 查看 VM 网络适配器属性。

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**后果**_ 

   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. 查看网络适配器团队映射。<p>结果不应返回信息，因为尚未执行映射。

   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```


9. 将 SMB1 和 SMB2 映射到单独的物理 NIC 组成员，并查看操作的结果。

   >[!IMPORTANT]
   >请确保在继续操作之前完成此步骤，否则你的实现将失败。

   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"

   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**后果**_ 

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

10. 确认先前创建的 MAC 关联。

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**后果**_ 

    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. 测试远程系统的连接，因为两个主机 vnic 位于同一子网，并且具有相同的 VLAN ID \(102\)。

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**后果**_   

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

    _**后果**_   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. 设置名称、开关名称和优先级标记。   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**后果**_   

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

    _**后果**_   

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

    _**后果**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>步骤10。 验证 RDMA 功能。

你需要从远程系统向 vSwitch 集团队的两个成员验证 RDMA 功能，该系统具有 vSwitch。<p>由于主机 vnic \(SMB1 和 SMB2\)都分配到 vlan 102，因此你可以选择远程系统上的 vlan 102 适配器。 <p>在此示例中，NIC 测试-40G-2 可以 RDMA 到 SMB1 （192.168.2.111）和 SMB2 （192.168.2.222）。

>[!TIP]
>你可能需要在此系统上禁用防火墙。  有关详细信息，请参阅构造策略。
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**后果**_ 
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

   _**后果**_ 

   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```

2. 查看网络适配器 RDMA 信息。

   ```PowerShell
   Get-NetAdapterRdma
   ```

   _**后果**_  

   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. 在第一个物理适配器上执行 RDMA 流量测试。

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**后果**_ 

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

4. 在第二个物理适配器上执行 RDMA 流量测试。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**后果**_ 

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

    _**后果**_ 

    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. 对第一个虚拟适配器执行 RDMA 流量测试。    

   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**后果**_ 

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

7. 在第二个虚拟适配器上执行 RDMA 流量测试。

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**后果**_ 

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

此输出中的最后一行 "RDMA 流量测试成功：RDMA 流量已发送到192.168.2.5，"显示已成功在适配器上配置了汇聚 NIC。

## <a name="related-topics"></a>相关主题 

- [使用单个网络适配器汇聚 NIC 配置](cnic-single.md)
- [汇聚 NIC 的物理交换机配置](cnic-app-switch-config.md)
- [聚合 NIC 配置疑难解答](cnic-app-troubleshoot.md)

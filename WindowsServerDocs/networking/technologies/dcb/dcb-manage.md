---
title: 管理数据中心桥接（DCB）
description: 本主题提供有关如何使用 Windows PowerShell 命令在 Windows Server 2016 中管理数据中心桥接的说明。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d635f96516040fcb30504f752c8194b0323c63f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405777"
---
# <a name="manage-data-center-bridging-dcb"></a>管理数据中心桥接（DCB）

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题提供有关如何在运行的计算机上使用 Windows PowerShell 命令在运行的 DCB \(\-兼容\)的网络适配器上配置数据中心桥接 DCB 的说明Windows Server 2016 或 Windows 10。

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>在 Windows Server 2016 或 Windows 10 中安装 DCB

有关使用和如何安装 DCB 的先决条件信息，请参阅[在 Windows Server 2016 或 windows 10 中安装数据中心桥接（DCB）](dcb-install.md)。


## <a name="dcb-configurations"></a>DCB 配置 

在 Windows Server 2016 之前，所有 DCB 配置都广泛应用于支持 DCB 的所有网络适配器。 

在 Windows Server 2016 中，你可以将 DCB 配置应用于全局策略存储或单个策略存储\(。\) 应用各个策略时，它们会覆盖所有全局策略设置。

在执行以下操作之前，系统级别上的流量类、PFC 和应用程序优先级分配的配置不会应用于网络适配器。

1. 将 DCBX 设置为 false

2. 在网络适配器上启用 DCB。 请参阅[在网络适配器上启用和显示 DCB 设置](#bkmk_enabledcb)。

>[!NOTE]
>如果要通过 DCBX 从开关配置 DCB，请参阅[DCBX 设置](#dcb-configuration-on-network-adapters)。

DCB 规范中描述了 DCBX 的位。 如果设备上的 "愿意" 位设置为 "true"，则设备愿意通过 DCBX 接受远程设备的配置。 如果设备上的 "愿意" 位设置为 "false"，则设备将拒绝远程设备的所有配置尝试，并仅强制实施本地配置。

如果 Windows Server 2016 中未安装 DCB，则 "愿意" 位的值与操作系统相关，因为操作系统没有 "本地设置" 适用于网络适配器。 安装 DCB 后，"愿意位" 的默认值为 true。 此设计允许网络适配器保留它们可能从其远程对等端接收的任何配置。

如果网络适配器不支持 DCBX，它将永远不会从远程设备接收配置。 它确实会从操作系统接收配置，但只有在将 DCBX 的位设置为 false 之后。

## <a name="set-the-willing-bit"></a>设置愿意位

若要在网络适配器上强制执行流量类、PFC 和应用程序优先级分配的操作系统配置，或者只是替代远程设备的配置（如果有），可以运行以下命令。

>[!NOTE]
>DCB Windows PowerShell 命令名称在名称字符串中包含 "QoS" 而不是 "DCB"。 这是因为 QoS 和 DCB 集成在 Windows Server 2016 中，以提供无缝的 QoS 管理体验。

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

若要显示相应的位设置的状态，可以使用以下命令：

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>网络适配器上的 DCB 配置

在网络适配器上启用 DCB 可查看从交换机传播到网络适配器的配置。

DCB 配置包括以下步骤。

1.  在系统级别配置 DCB 设置，其中包括：

    a. 流量类管理
    
    b. 优先级流控制（PFC）设置
    
    c. 应用程序优先级分配
    
    d. DCBX 设置

2. 在网络适配器上配置 DCB。



##  <a name="dcb-traffic-class-management"></a>DCB 流量类管理

下面是用于通信类管理的示例 Windows PowerShell 命令。

### <a name="create-a-traffic-class"></a>创建流量类

可以使用**get-netqostrafficclass**命令创建流量类。

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

默认情况下，所有 802.1 p 值都映射到默认的流量类，该流量类的物理链路带宽为 100%。 **Get-netqostrafficclass**命令将创建一个新的流量类，其中标记有 802.1 p 优先级值4的任何数据包。 传输选择算法 \(TSA @ no__t-1 为 ETS，其带宽为 30%。

最多可以创建7个新的通信类。 包括默认通信类，系统中最多可以有8个通信类。 但是，支持 DCB 的网络适配器可能不支持硬件中的多个通信类。 如果创建的流量类比网络适配器可容纳的流量类多，并且在该网络适配器上启用 DCB，则微型端口驱动程序会向操作系统报告错误。 错误记录在事件日志中。

所有已创建的流量类的带宽预留的总和不得超过 99% 的带宽。 默认流量类始终具有为其本身预留的带宽的至少 1%。

### <a name="display-traffic-classes"></a>显示流量类

可以使用**get-netqostrafficclass**命令查看流量类。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>修改流量类

可以使用**get-netqostrafficclass**命令创建流量类。 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

然后，可以使用**get-netqostrafficclass**命令来查看设置。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

创建流量类之后，可以单独更改其设置。 您可以更改的设置包括：

1. 带宽分配\(-BandwidthPercentage\)

2. TSA （\-算法\)

3. 优先级映射\(-优先级\)

### <a name="remove-a-traffic-class"></a>删除流量类

可以使用**get-netqostrafficclass**命令删除流量类。

>[!IMPORTANT]
>不能删除默认的通信类。


    Remove-NetQosTrafficClass -Name SMB

然后，可以使用**get-netqostrafficclass**命令来查看设置。
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

删除流量类后，映射到该流量类的 802.1 p 值将重新映射到默认流量类。 删除流量类时，为流量类保留的任何带宽都将返回到默认的流量类分配。

## <a name="per-network-interface-policies"></a>每网络接口策略

上述所有示例均设置全局策略。 下面是有关如何设置和获取每个 NIC 策略的示例。 

"PolicySet" 字段从 Global 更改为 AdapterSpecific。 显示 AdapterSpecific 策略时，还会显示接口\(索引\) ifIndex 和接口\(名称\) ifAlias。

```
PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              AdapterSpecific  4       M1


PS C:\> New-NetQosTrafficClass -Name SMBGlobal -BandwidthPercentage 30 -Priority 4 -Algorithm ETS

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBGlobal   ETS       30           4                Global


PS C:\> New-NetQosTrafficClass -Name SMBforM1 -BandwidthPercentage 30 -Priority 4 -Algorithm ETS -Interfac


Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          Global
SMBGlobal   ETS       30           4                Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          AdapterSpecific  4       M1
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


```

## <a name="priority-flow-control-settings"></a>优先级流控制设置：

下面是优先级流控制设置的命令示例。 还可以为单个适配器指定这些设置。

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>启用并显示全局和接口特定用例的优先级流控制

```
PS C:\> Enable-NetQosFlowControl -Priority 4
PS C:\> Enable-NetQosFlowControl -Priority 3 -InterfaceAlias M1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          True       Global
5          False      Global
6          False      Global
7          False      Global

PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          True       AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1  

```


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>禁用优先级流控制（特定于全局和接口）

```
PS C:\> Disable-NetQosFlowControl -Priority 4
PS C:\> Disable-NetQosFlowControl -Priority 3 -InterfaceAlias m1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          False      Global
5          False      Global
6          False      Global
7          False      Global


PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          False      AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1  

```

##  <a name="application-priority-assignment"></a>应用程序优先级分配

下面是优先级分配的示例。

### <a name="create-qos-policy"></a>创建 QoS 策略

```
PS C:\> New-NetQosPolicy -Name "SMB Policy" -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

上一个命令为 SMB 创建新策略。 – SMB 是与 TCP 端口445（为 SMB 保留）匹配的收件箱筛选器。 如果将数据包发送到 TCP 端口445，则在将数据包传递到网络微型端口驱动程序之前，将使用 802.1 p 值为4的操作系统标记该数据包。

除– SMB 外，其他默认筛选器包括– iSCSI （匹配 tcp 端口3260）、-NFS （匹配 tcp 端口2049）、-LiveMigration （匹配的 tcp 端口6600）、-FCOE （匹配 EtherType 0x8906）和– NetworkDirect。

NetworkDirect 是我们在网络适配器上的任何 RDMA 实现之上创建的抽象层。 – NetworkDirect 必须后跟网络直接端口。

除了默认筛选器外，还可以按应用程序的可执行文件名称（如下面的第一个示例所示）或 IP 地址、端口或协议（如第二个示例中所示）对流量进行分类：

**按可执行文件名称**

```
PS C:\> New-NetQosPolicy -Name background -AppPathNameMatchCondition "C:\Program files (x86)\backup.exe" -PriorityValue8021Action 1

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

```


**按 IP 地址端口或协议**

```
PS C:\> New-NetQosPolicy -Name "Network Management" -IPDstPrefixMatchCondition 10.240.1.0/24 -IPProtocolMatchCondition both -NetworkProfile all -PriorityValue8021Action 7

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

```

### <a name="display-qos-policy"></a>显示 QoS 策略

```
PS C:\> Get-NetQosPolicy

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

### <a name="modify-qos-policy"></a>修改 QoS 策略

你可以修改 QoS 策略，如下所示。


```
PS C:\> Set-NetQosPolicy -Name "Network Management" -IPSrcPrefixMatchCondition 10.235.2.0/24 -IPProtocolMatchCondition both -PriorityValue8021Action 7
PS C:\> Get-NetQosPolicy

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPSrcPrefix    : 10.235.2.0/24
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7


```

### <a name="remove-qos-policy"></a>删除 QoS 策略

```
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y  

```

## <a name="dcb-configuration-on-network-adapters"></a>网络适配器上的 DCB 配置

网络适配器上的 DCB 配置与上面所述的系统级别上的 DCB 配置无关。 

不管是否在 Windows Server 2016 中安装了 DCB，你始终可以运行以下命令。 

如果从交换机配置 DCB，并依赖 DCBX 将配置传播到网络适配器，则可以在网络适配器上启用 DCB 后，检查在网络适配器上接收和强制执行的配置。

###  <a name="bkmk_enabledcb"></a>启用和显示网络适配器上的 DCB 设置

```
PS C:\> Enable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos

Name                       : M1
Enabled                    : True
Capabilities               :                       Hardware     Current
                                                   --------     -------
                             MacSecBypass        : NotSupported NotSupported
                             DcbxSupport         : None         None
                             NumTCs(Max/ETS/PFC) : 8/8/8        8/8/8

OperationalTrafficClasses  : TC TSA    Bandwidth Priorities
                             -- ---    --------- ----------
                              0 ETS    70%       0-3,5-7
                              1 ETS    30%       4

OperationalFlowControl     : All Priorities Disabled
OperationalClassifications : Protocol  Port/Type Priority
                             --------  --------- --------
                             Default             1


```

### <a name="disable-dcb-on-network-adapters"></a>在网络适配器上禁用 DCB

```
PS C:\> Disable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos M1

Name         : M1
Enabled      : False
Capabilities :                       Hardware     Current
                                     --------     -------
               MacSecBypass        : NotSupported NotSupported
               DcbxSupport         : None         None
               NumTCs(Max/ETS/PFC) : 8/8/8        0/0/0  

```
## <a name="bkmk_wps"></a>适用于 DCB 的 Windows PowerShell 命令

对于 Windows Server 2016 和 Windows Server 2012 R2，有 DCB 的 Windows PowerShell 命令。 你可以使用 windows Server 2016 中 Windows Server 2012 R2 的所有命令。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>适用于 DCB 的 windows Server 2016 Windows PowerShell 命令

以下适用于 windows Server 2016 的主题提供 windows PowerShell cmdlet 说明和语法，适用于所有\(数据\)中心桥接\(DCB\)Service QoS\-特定 cmdlet 的质量。 本参考按 cmdlet 开头动词的字母顺序列出了这些 cmdlet。

- [DcbQoS 模块](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>适用于 DCB 的 windows Server 2012 R2 Windows PowerShell 命令

适用于 windows Server 2012 R2 的以下主题提供了 windows PowerShell cmdlet 说明和语法，适用于\(所有\)数据中心桥\(接\)DCB Service QoS\-特定的 cmdlet。 本参考按 cmdlet 开头动词的字母顺序列出了这些 cmdlet。

- [Windows PowerShell 中的数据中心桥接（DCB）服务质量（QoS） Cmdlet](https://technet.microsoft.com/library/hh967440.aspx)

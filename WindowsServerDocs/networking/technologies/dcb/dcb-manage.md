---
title: 管理数据中心桥接 (DCB)
description: 本主题提供如何使用 Windows PowerShell 命令来管理 Windows Server 2016 中的数据中心桥接的说明。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daed746fe798ae253956d0977827d0e205bb8b3e
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034575"
---
# <a name="manage-data-center-bridging-dcb"></a>管理数据中心桥接 (DCB)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题提供有关如何使用 Windows PowerShell 命令来配置数据中心桥接的说明\(DCB\)上的 DCB\-兼容的网络适配器安装在运行的计算机Windows Server 2016 或 Windows 10。

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>在 Windows Server 2016 或 Windows 10 安装 DCB

有关使用和如何安装 DCB 的先决条件的信息，请参阅[安装的数据中心桥接 (DCB) 在 Windows Server 2016 或 Windows 10 中](dcb-install.md)。


## <a name="dcb-configurations"></a>DCB 配置 

在 Windows Server 2016 之前所有 DCB 已配置都应用通用于所有支持 DCB 的网络适配器。 

在 Windows Server 2016 中，您可以 DCB 将配置应用到全局策略存储区或单个策略存储区\(s\)。 单个策略时保护它们重写所有全局策略设置。

直到您执行以下网络适配器上未应用在系统级别的流量类、 PFC 和应用程序优先级的分配的配置。

1. 打开 DCBX 愿意位设置为 false

2. 启用网络适配器上的 DCB。 请参阅[启用和显示网络适配器上的 DCB 设置](#bkmk_enabledcb)。

>[!NOTE]
>如果你想要将 DCB 配置从通过 DCBX 开关，请参阅[DCBX 设置](#dcb-configuration-on-network-adapters)。

DCBX 愿意位是 DCB 规范中所述。 如果在设备上的愿意位设置为 true，该设备是愿意接受从通过 DCBX 远程设备的配置。 如果在设备上的愿意位设置为 false，设备将拒绝所有配置尝试从远程设备，并强制实施仅本地配置。

如果 DCB 不安装在 Windows Server 2016 愿意位的值是不相关，操作系统是而言，因为操作系统不具有任何本地设置将应用于网络适配器。 DCB 安装后，愿意位的默认值为 true。 此设计允许网络适配器，以使它们可能已经从其远程对等方收到的任何配置。

如果网络适配器不支持 DCBX，它将永远不会接收从远程设备的配置。 它会从操作系统接收配置，但仅在 DCBX 后愿意位设置为 false。

## <a name="set-the-willing-bit"></a>设置愿意位

若要强制实施流量类、 PFC 和应用程序优先级分配网络适配器上的操作系统配置或只需重写从远程设备的配置 \-如果存在任何 \-可以运行以下命令。

>[!NOTE]
>DCB Windows PowerShell 命令名称中的名称字符串包含"QoS"而不是"DCB"。 这是因为在 Windows Server 2016 提供无缝的 QoS 管理体验集成在 QoS 和 DCB。

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

若要显示的设置的愿意位的状态，可以使用以下命令：

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>网络适配器上的 DCB 配置

网络适配器上启用 DCB 可看到从一个开关传播到网络适配器的配置。

DCB 配置包括以下步骤。

1.  在系统级别，其中包括配置 DCB 设置：

    a. 流量类管理
    
    b. 优先级流控制 (PFC) 设置
    
    c. 应用程序优先级分配
    
    d. DCBX 设置

2. 配置网络适配器上的 DCB。



##  <a name="dcb-traffic-class-management"></a>DCB 流量类管理

以下是示例 Windows PowerShell 命令为流量类管理。

### <a name="create-a-traffic-class"></a>创建流量类

可以使用**新建 NetQosTrafficClass**命令以创建流量类。

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

默认情况下，所有的 802.1p 值映射到默认流量类，它具有 100%的物理链路的带宽。 **新建 NetQosTrafficClass**命令将创建一个新的流量类中，映射到的任何数据包的 802.1p 优先级标记值 4。 传输选择算法\(TSA\) ETS，且是 30%的带宽。

您可以创建最多 7 新流量类。 包括默认流量类，可以有最多 8 流量类系统中。 但是，DCB 网络适配器可能不支持很多流量的硬件中的类。 如果创建的网络适配器可以容纳更多的流量类并在该网络适配器上启用 DCB，微型端口驱动程序将向操作系统报告错误。 事件日志中记录错误。

创建的流量的所有类的带宽保留项的总和不能超过 99%的带宽。 默认流量类始终有至少 1%的带宽保留为其自身。

### <a name="display-traffic-classes"></a>显示通信类别

可以使用**Get-netqostrafficclass**命令查看通信类。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>修改流量类

可以使用**集 NetQosTrafficClass**命令以创建流量类。 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

然后，可以使用**Get-netqostrafficclass**命令查看设置。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

创建流量类后，可以更改其设置独立。 您可以更改这些设置包括：

1. 带宽分配\(-BandwidthPercentage\)

2. TSA (\-Algorithm\)

3. 优先级映射\(-优先级\)

### <a name="remove-a-traffic-class"></a>删除流量类

可以使用**删除 NetQosTrafficClass**命令以删除流量类。

>[!IMPORTANT]
>不能删除默认通信类。


    Remove-NetQosTrafficClass -Name SMB

然后，可以使用**Get-netqostrafficclass**命令查看设置。
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

删除流量类后的 802.1p 值映射到流量类重新映射到默认通信类。 删除流量类时，流量类保留任何带宽被返回到默认流量类分配。

## <a name="per-network-interface-policies"></a>每个网络接口策略

上面的示例中的所有设置的全局策略。 以下是如何设置和获取每个 NIC 策略的示例。 

"PolicySet"字段从全局更改为 AdapterSpecific。 AdapterSpecific 策略时显示，接口索引\(ifIndex\)和接口名称\(ifAlias\)也会显示。

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

以下是优先级流控制设置的命令示例。 此外可以为各个适配器指定这些设置。

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>启用和显示全局和特定接口的优先级流控制用例

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


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>禁用优先级流控制 (全局和特定的接口)

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

以下是优先级分配的示例。

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

为 SMB 前, 一个命令创建新策略。 – SMB 是收件箱匹配筛选器的 TCP 端口 445 （smb 保留）。 如果将数据包发送到 TCP 端口 445 会标记的 802.1p 值 4 操作系统之前将数据包传递给网络微型端口驱动程序。

– 除 SMB 以外，其他默认筛选器包括 – iSCSI （匹配 TCP 端口 3260）、-NFS （匹配 TCP 端口 2049年）、-LiveMigration （匹配 TCP 端口 6600 匹配）、-FCOE （匹配 EtherType 0x8906） 和 – NetworkDirect。

NetworkDirect 是任何网络适配器上的 RDMA 实现的顶部，我们创建一个抽象层。 – NetworkDirect 后面必须跟 Network Direct 的端口。

除了默认筛选器，你可以分类流量 （以下第一个示例所示），应用程序的可执行文件名称或 IP 地址、 端口或协议 （如第二个示例中所示）：

**通过可执行文件名称**

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


**通过 IP 地址端口或协议**

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

您可以修改 QoS 策略，如下所示。


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

独立于在系统级别上面所述的 DCB 配置网络适配器上的 DCB 配置。 

无论是否 DCB 安装在 Windows Server 2016 中，始终可以运行以下命令。 

如果从交换机配置 DCB，并依赖于 DCBX 传播给网络适配器的配置，您可以检查接收和网络适配器上启用 DCB 后从操作系统端的网络适配器上强制执行哪些配置。

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

### <a name="disable-dcb-on-network-adapters"></a>禁用网络适配器上的 DCB

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
## <a name="bkmk_wps"></a>DCB 的 Windows PowerShell 命令

有用于 Windows Server 2016 和 Windows Server 2012 R2 DCB Windows PowerShell 命令。 对于 Windows Server 2016 中的 Windows Server 2012 R2，您可以使用的所有命令。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016 的 DCB 的新 Windows PowerShell 命令

Windows Server 2016 的以下主题提供 Windows PowerShell cmdlet 说明和语法的所有数据中心桥接\(DCB\)服务质量\(QoS\)\-特定的 cmdlet。 本参考按 cmdlet 开头动词的字母顺序列出了这些 cmdlet。

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>有关 DCB 的 Windows Server 2012 R2 的 Windows PowerShell 命令

Windows Server 2012 R2 的以下主题提供 Windows PowerShell cmdlet 说明和语法的所有数据中心桥接\(DCB\)服务质量\(QoS\)\-特定的 cmdlet。 本参考按 cmdlet 开头动词的字母顺序列出了这些 cmdlet。

- [数据中心桥接 (DCB) 服务质量 (QoS) Cmdlet 在 Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)

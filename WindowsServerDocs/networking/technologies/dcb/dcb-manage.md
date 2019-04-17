---
title: 管理数据中心桥 (DCB)
description: 本主题提供有关如何使用 Windows PowerShell 命令管理数据中心桥 Windows Server 2016 中的说明进行操作。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbe9e5e4af2ddd834b5b8f38e9ffd1b403e92793
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="manage-data-center-bridging-dcb"></a>管理数据中心桥 (DCB)

>适用于：Windows Server（半年通道），Windows Server 2016

本主题提供有关如何使用 Windows PowerShell 命令配置数据中心桥 \(DCB\) DCB\ 兼容的网络适配器安装在计算机上运行的 Windows Server 2016 或 Windows 10 上的说明进行操作。

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Windows Server 2016 或 Windows 10 中安装 DCB

使用和如何安装 DCB 先决条件的信息，请参阅[安装数据中心桥 (DCB) 在 Windows Server 2016 或 Windows 10 中](dcb-install.md)。


## <a name="dcb-configurations"></a>DCB 配置 

Windows Server 2016 中之前, 所有 DCB 配置已都应用经常到 DCB 受支持的所有网络适配器。 

在 Windows Server 2016，可以向全球策略应用商店或个别策略 Store\(s\) 应用 DCB 配置。 应用个别策略时他们覆盖所有策略全球设置。

之前，请执行以下网络适配器上的系统级别的交通类、PFC 和应用程序优先级分配配置不应用。

1. 打开为 false DCBX 愿意付出一点

2. 启用网络适配器 DCB。 请参阅[启用和在网络适配器显示 DCB 设置](#bkmk_enabledcb)。

>[!NOTE]
>如果你想要从 DCBX 通过一个开关配置 DCB，请参阅[DCBX 设置](#BKMK_DCBX_Settings)

DCBX 愿意付出一点 DCB 规范所述。 如果愿意付出一点在设备上的设置为如此，此设备处于接受从设备远程通过 DCBX 配置。 如果愿意付出一点在设备上的设置为 false，设备将拒绝所有配置尝试从设备远程和履行只本地配置。

如果未安装在 Windows Server 2016 DCB 愿意付出一点值无关就而言操作系统，因为操作系统有没有本地设置应用到网络适配器。 DCB 安装后，愿意付出一点的默认值为 true。 此设计，以确保他们从对方远程收到任何配置的网络适配器。

如果网络适配器不支持 DCBX，它将永远不会收到配置远程设备。 它配置接收操作系统，但仅次于 DCBX 愿意付出一点设置为 false。

## <a name="set-the-willing-bit"></a>设置愿意付出一点

强制操作系统和配置的交通类、PFC，网络适配器上的应用程序优先级分配，或只需覆盖从设备远程配置 \，如果有任何 \，你可以通过运行以下命令。

>[!NOTE]
>DCB Windows PowerShell 命令名称字符串名称中包含"QoS"，而不是"DCB"。 这是因为 QoS 和 DCB 在 Windows Server 2016 为了提供无缝 QoS 管理体验集成。

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

若要显示设置愿意付出一点的状态，你可以使用以下命令：

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>DCB 配置，打开网络适配器

启用网络适配器上的 DCB 允许你看到一个交换机用来从传播到该网络适配器的配置。

DCB 配置包含以下步骤。

1.  在系统级别，其中包括配置 DCB 设置：

    。 路况类管理
    
    b。 优先级流控制 (PFC) 设置
    
    c。 应用程序优先级分配
    
    d。 DCBX 设置

2. 将 DCB 配置上的网络适配器。



##  <a name="dcb-traffic-class-management"></a>DCB 通信类管理

下面是进行通信类管理示例 Windows PowerShell 命令。

### <a name="create-a-traffic-class"></a>创建交通类

你可以使用**新建 NetQosTrafficClass**创建交通类命令。

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

默认情况下，所有 802.1 p 值映射到默认交通类别，它有物理链接带宽 100%。 **新建 NetQosTrafficClass**命令将创建一个新的交通类、的任何使用 802.1 p 优先级已标记的数据包重视 4 映射。 传输选择算法 \(TSA\) ETS，并且拥有带宽 30%。

你可以创建 7 最多的新交通类。 默认交通类别，包括还可能存在最 8 通信类系统中。 但是，DCB 功能的网络适配器可能不支持许多通信中硬件类。 如果您启用了该网络适配器上的 DCB 创建不是可容纳网络适配器上的更多流量类，微型端口驱动程序将报告的操作系统的错误。 在事件日志记录错误。

所有创建的交通类带宽预订和可能不能超过带宽 99%。 默认交通类始终具有至少为本身保留带宽 1%。

### <a name="display-traffic-classes"></a>显示通信类

你可以使用**获取 NetQosTrafficClass**命令以查看交通类。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>修改交通类

你可以使用**组 NetQosTrafficClass**创建交通类命令。 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

然后，你可以使用**获取 NetQosTrafficClass**命令以查看设置。

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

创建交通类后，你可以更改其设置独立。 你可以更改这些设置包括：

1. 带宽分配 \(-BandwidthPercentage\)

2. TSA (\-Algorithm\)

3. 优先级映射 \(-Priority\)

### <a name="remove-a-traffic-class"></a>删除交通类

你可以使用**删除 NetQosTrafficClass**删除交通类命令。

>[!IMPORTANT]
>你无法删除默认交通类别。


    Remove-NetQosTrafficClass -Name SMB

然后，你可以使用**获取 NetQosTrafficClass**命令以查看设置。
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

删除交通类后，802.1 p 值映射到通信类重新，映射为默认路况类别。 删除交通类时，已交通类保留的任何带宽将返回到默认交通类分配。

## <a name="per-network-interface-policies"></a>每个网络接口策略

设置策略全球所有上方的示例。 以下是有关如何设置并获取每个 NIC 策略的示例。 

在"PolicySet"字段从全球变为 AdapterSpecific。 当显示 AdapterSpecific 策略时，也会显示接口索引 \(ifIndex\) 和接口名称 \(ifAlias\)。

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

## <a name="priority-flow-control-settings"></a>优先级流控制的设置：

以下是命令示例的优先级排列控制设置。 此外可以单独适配器指定这些设置。

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>启用和显示优先级排列全球和控制界面特定使用情况

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


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>禁用优先级流控制 (全球和接口特定)

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

上一个命令 SMB 创建一个新的策略。 -SMB 是与 TCP 端口（保留了 SMB）445 匹配的收件箱筛选器。 如果数据包 TCP 端口它将使用 802.1 p 值 4 之前的操作系统标记的 445 数据包将传递到网络微型端口驱动程序。

除了 – SMB，其他默认 filters 包括 – iSCSI（匹配 TCP 端口 3260）、-NFS（匹配 TCP 端口 2049 年）、-LiveMigration（匹配 TCP 端口 6600）、-（匹配以太网类型 0x8906）FCOE 和 – NetworkDirect。

NetworkDirect 是我们在网络适配器上的任何 RDMA 实现顶部创建的抽象层。 -NetworkDirect 必须跟直接网络端口。

除了默认的筛选器，你可以通过应用程序的可执行文件名称（如下所示的第一个示例下面），或 IP 地址、端口或协议（第二个示例所示）分类交通：

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

## <a name="dcb-configuration-on-network-adapters"></a>DCB 配置，打开网络适配器

网络适配器上的 DCB 配置上述系统级别的 DCB 配置无关。 

无论是否 DCB 已安装在 Windows Server 2016 中，您可以始终运行以下命令。 

如果你从一个交换机用来配置 DCB 并依赖 DCBX 传播到网络适配器配置，你可以检查接收和在网络适配器上启用 DCB 后在网络适配器，来自操作系统端上强制执行何种配置。

###  <a name="bkmk_enabledcb"></a>启用和在网络适配器显示 DCB 设置

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

### <a name="disable-dcb-on-network-adapters"></a>禁用 DCB 上网络适配器

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
## <a name="bkmk_wps"></a>Windows PowerShellDCB 的命令

有 DCB Windows PowerShell 适用于 Windows Server 2016 和 Windows Server 2012 R2 的命令。 你可以对 Windows Server 2016 中的 Windows Server 2012 R2 使用的所有命令。

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server 2016Windows PowerShellDCB 的命令

Windows Server 2016 的以下主题提供 Windows PowerShell cmdlet 说明和语法所有数据中心桥 \(DCB\) 服务质量 \ (QoS\) \-specific cmdlet。 列出了这些 cmdlet 基于 cmdlet 开头的动词按字母顺序。

- [DcbQoS 模块](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server 2012 对于 DCB R2 Windows PowerShell 命令

Windows Server 2012 R2 的以下主题提供 Windows PowerShell cmdlet 说明和语法所有数据中心桥 \(DCB\) 服务质量 \ (QoS\) \-specific cmdlet。 列出了这些 cmdlet 基于 cmdlet 开头的动词按字母顺序。

- [数据中心中的 Windows PowerShell 服务 (QoS) Cmdlet 过渡 (DCB) 质量](https://technet.microsoft.com/library/hh967440.aspx)

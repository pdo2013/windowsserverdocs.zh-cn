---
ms.assetid: ''
title: 配置系统以获得高准确性
description: Windows 10 和 Windows Server 2016 中的时间同步已大幅提高。  在合理的操作条件下，可将系统配置为维持1ms （毫秒）的准确性或更好（与 UTC 相关）。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: b7cd256fdbbdbe7432e5b5d5b16254314132560f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405199"
---
# <a name="configuring-systems-for-high-accuracy"></a>配置系统以获得高准确性
>适用于：Windows Server 2016 和 Windows 10 版本1607或更高版本

Windows 10 和 Windows Server 2016 中的时间同步已大幅提高。  在合理的操作条件下，可将系统配置为维持1ms （毫秒）的准确性或更好（与 UTC 相关）。

以下指南将帮助你将系统配置为实现高准确性。  本文介绍了以下要求：

- 支持的操作系统
- 系统配置 

> [!WARNING]
> **之前的操作系统准确性目标**<br>
>Windows Server 2012 R2 及更低不能满足相同的高准确性目标。 不支持这些操作系统的高准确性。
>
>在这些版本中，Windows 时间服务满足以下要求：
>
> - 提供了满足 Kerberos 版本5身份验证要求所需的时间准确性。
> - 为加入到公共 Active Directory 林的 Windows 客户端和服务器提供了松散准确的时间。
>
>2012 R2 及更高版本的容差超出了 Windows 时间服务的设计规范。

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Windows 10 和 Windows Server 2016 默认配置

虽然在 Windows 10 或 Windows Server 2016 上的1ms 支持准确性，但大多数客户不需要非常准确的时间。

因此，**默认配置**旨在满足与之前的操作系统相同的要求：

- 提供必要的时间准确性，以满足 Kerberos 版本5身份验证要求。
- 为加入到公共 Active Directory 林的 Windows 客户端和服务器提供松散准确的时间。

## <a name="how-to-configure-systems-for-high-accuracy"></a>如何配置系统以获得高准确性

>[!IMPORTANT]
>**有关高准确性系统的支持的说明**<br>
> 时间准确性需要从权威时间源到结束设备的准确时间的端到端分发。  在此路径中添加 assymetry 的任何内容都将对准确性产生负面影响，从而影响设备的准确性。
>
>出于此原因，我们记录了[支持边界，用于为高准确性环境配置 Windows 时间服务](support-boundary.md)，其中概述了实现高准确性目标时必须满足的环境要求。

### <a name="operating-system-requirements"></a>操作系统要求

高准确性配置需要 Windows 10 或 Windows Server 2016。  时间拓扑中的所有 Windows 设备都必须满足此要求，包括更高层次的 Windows 时间服务器，以及在虚拟化方案中运行区分时间的虚拟机的 Hyper-v 主机。 所有这些设备必须至少为 Windows 10 或 Windows Server 2016。

在下面所示的插图中，需要高准确性的虚拟机正在运行 Windows 10 或 Windows Server 2016。  同样，虚拟机所在的 Hyper-v 主机以及上游 Windows 时间服务器还必须运行 Windows Server 2016。

![时间拓扑-1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**确定 Windows 版本**<br>
> 你可以在命令提示符处运行命令 `winver` 来验证 OS 版本是否为1607（或更高版本），操作系统版本为14393（或更高版本），如下所示：
>
> ![Winver-2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>系统配置

达到高准确度目标需要系统配置。  可以通过多种方式来执行此配置，包括直接在注册表中或通过组策略。  有关这些设置的详细信息，请参阅 Windows 时间服务技术参考– [Windows 时间服务工具](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools)。

#### <a name="windows-time-service-startup-type"></a>Windows 时间服务启动类型

Windows 时间服务（W32Time）必须连续运行。  为此，请将 Windows 时间服务的启动类型配置为 "自动" 启动。

![自动配置](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>累积单向网络延迟

随着网络延迟的增加，测量的不确定性和 "干扰" creeps。  因此，网络延迟必须在合理的边界内。  具体的要求取决于你的目标准确性，并在[为高准确性环境配置 Windows 时间服务](support-boundary.md)一文中概述了支持边界。

若要计算累积单向网络延迟，请在时间拓扑中添加 NTP 客户端-服务器节点对之间的单个单向延迟，从目标开始，到高准确性第1次源结束。

例如：请考虑一个时间同步层次结构，其中包含高度准确的源、两个中间 NTP 服务器 A 和 B，以及目标计算机。 若要获取目标和源之间的累计网络延迟，请在以下之间测量每个 NTP 往返时间（RTTs）的平均值：

- 目标和时间服务器 B
- Time server B 和 time server A
- 时间服务器 A 和源

可以使用收件箱 w32tm 工具获取此度量。  为此，请执行以下操作:

1. 从目标和时间服务器 B 执行计算。
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. 从时间服务器 b （指向）时间服务器 a 执行计算。
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. 针对源对 time server a 执行计算。
 
4. 接下来，添加在上一步中测量的平均 RoundTripDelay 并除以2，以获取目标和源之间的累计网络延迟。

#### <a name="registry-settings"></a>注册表设置

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
配置 log2 秒内允许用于系统轮询的最小间隔。

|  |  | 
|---------|---------|
|密钥位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|设置    | 6        |
|结果 | 最小轮询间隔现在为64秒。 |

以下命令将通知 Windows 时间以获取更新的设置：

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
配置系统轮询允许的 log2 秒的最大时间间隔。

|  |  |  
|---------|---------|
|密钥位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|设置    | 6        |
|结果 | 最大轮询间隔现在为64秒。  |

以下命令将通知 Windows 时间以获取更新的设置：

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
阶段更正调整之间的时钟计时周期数。

|  |  |  
|---------|---------|
|密钥位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|设置    | 100        |
|结果 | 阶段更正调整之间的时钟计时周期数现在为100。 |

以下命令将通知 Windows 时间以获取更新的设置：

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
配置启用 SpecialInterval 0x1 标志后的轮询间隔（以秒为单位）。

|  |  |  
|---------|---------|
|密钥位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|设置    | 64        |
|结果 | 轮询间隔现在为64秒。 |

以下命令将重新启动 Windows 时间以选取更新的设置：

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|密钥位置     | HKLM： \ SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|设置    | 2        |


---

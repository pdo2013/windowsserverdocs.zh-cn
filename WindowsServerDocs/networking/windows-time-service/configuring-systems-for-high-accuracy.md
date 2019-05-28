---
ms.assetid: ''
title: 配置系统的高精度
description: 显著改进了 Windows 10 和 Windows Server 2016 中的时间同步。  在合理的操作情况下可以配置系统维护 1 毫秒 （毫秒） 或 （相对于 UTC) 的更好的准确性。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 2a5a7a6bd6313f7a4eadd827e3d754c1e467c3bc
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63745415"
---
# <a name="configuring-systems-for-high-accuracy"></a>配置系统的高精度
>适用于：Windows Server 2016 和 Windows 10 版本 1607年或更高版本

显著改进了 Windows 10 和 Windows Server 2016 中的时间同步。  在合理的操作情况下可以配置系统维护 1 毫秒 （毫秒） 或 （相对于 UTC) 的更好的准确性。

以下指南将帮助您配置您的系统以实现高精度。  本文讨论了以下要求：

- 支持的操作系统
- 系统配置 

> [!WARNING]
> **以前版本的操作系统准确性目标**<br>
>Windows Server 2012 R2 时，下面可以达到相同的高精度目标。 这些操作系统不支持的高精度。
>
>在这些版本中，Windows 时间服务满足以下要求：
>
> - 提供必要的时间准确性，以满足 Kerberos 版本 5 身份验证要求。
> - Windows 客户端和服务器加入到常见的 Active Directory 林提供松散准确的时间。
>
>在 2012 R2 及更低的更大容差是外部的 Windows 时间服务的设计规范。

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Windows 10 和 Windows Server 2016 默认配置

虽然我们在 Windows 10 或 Windows Server 2016 上支持多达 1 毫秒的准确性，但是大多数客户不需要高度准确的时间。

在这种情况下，**默认配置**旨在满足与以前版本的操作系统是到相同的要求：

- 提供必要的时间准确性，以满足 Kerberos 版本 5 身份验证要求。
- Windows 客户端和服务器加入到常见的 Active Directory 林提供松散准确的时间。

## <a name="how-to-configure-systems-for-high-accuracy"></a>如何配置系统的高精度

>[!IMPORTANT]
>**还要注意的非常准确的系统可支持性：**<br>
> 时间准确性时，需要的端到端分布的准确的时间从权威时间源到最终设备。  添加 assymetry 中此路径上的度量值将产生负面影响准确性的任何内容将影响可在设备上实现的准确性。
>
>出于此原因，我们记录[支持边界来配置 Windows 时间服务对于高准确性环境](support-boundary.md)大纲显示还必须满足才能达到高精度目标的环境要求。

### <a name="operating-system-requirements"></a>操作系统要求

高精度配置需要 Windows 10 或 Windows Server 2016。  时间拓扑中的所有 Windows 设备必须满足此要求包括更高层次 Windows 时间服务器，并在虚拟化方案中，运行时间敏感型虚拟机的 HYPER-V 主机。 所有这些设备必须至少为 Windows 10 或 Windows Server 2016。

在图中如下所示，Windows 10 或 Windows Server 2016 运行虚拟机需要高精度。  同样，在其驻留在虚拟机，请在 HYPER-V 主机和上游 Windows 时间服务器也必须运行 Windows Server 2016。

![时间拓扑-1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**确定 Windows 版本**<br>
> 可以运行命令`winver`在命令提示符下以验证操作系统版本 1607年 （或更高版本），操作系统内部版本 14393 （或更高版本），如下所示：
>
> ![Winver-2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>系统配置

达到高精度目标要求的系统配置。  有多种方式来执行此配置，包括直接在注册表中或通过组策略。  每个设置的详细信息可在 Windows 时间服务技术参考 – [Windows 时间服务工具](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools)。

#### <a name="windows-time-service-startup-type"></a>Windows 时间服务启动类型

Windows 时间服务 (W32Time) 必须连续运行。  若要执行此操作，配置 Windows 时间服务的启动类型设置为自动启动。

![自动配置](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>累积单向网络延迟

度量不确定性和"噪声"就会渗入网络滞后时间增加时。  在这种情况下，它是命令性网络延迟是合理的边界内。  依赖于您的目标更精确的特定要求和中所述[支持边界来配置 Windows 时间服务对于高准确性环境](support-boundary.md)一文。

若要计算的累积单向网络延迟，请添加 NTP 时间拓扑，与目标开始和结束时间高准确性第 1 时间源中的客户端-服务器节点对之间的单个单向延迟问题。

例如：请考虑具有非常准确的源、 两个中间 NTP 服务器 A 和 B，按此顺序在目标计算机的时间同步层次结构。 若要获取的目标和源之间的累积网络延迟，测量单个 NTP 往返时间 (RTTs) 之间的平均值：

- 目标和时间服务器 B
- 时间服务器 B 和时间服务器 A
- 时间服务器 A 和源

可以使用收件箱 w32tm.exe 工具获取此测量值。  要实现此目的，请执行以下操作：
<!-- Use PowerShell to import the CSV then average the RTT Column -->

1. 从目标和时间服务器 b。 执行计算
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. 执行计算从针对时间服务器 b （指向） 时间服务器。
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. 从时间服务器中执行计算针对源。
 
4. 接下来，添加上一步中测量平均 RoundTripDelay 并除以 2 以获取目标和源之间的累积网络延迟。

#### <a name="registry-settings"></a>注册表设置

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
在 log2 数秒内允许的系统轮询配置的最小时间间隔。

|  |  | 
|---------|---------|
|密钥位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|设置    | 6        |
|结果 | 最小的轮询间隔现在为 64 秒。 |

以下命令向发出信号 Windows 时间才能反映更新的设置：

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
在 log2 数秒内允许的系统轮询配置的最大时间间隔。

|  |  |  
|---------|---------|
|密钥位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|设置    | 6        |
|结果 | 最大轮询间隔现在为 64 秒。  |

以下命令向发出信号 Windows 时间才能反映更新的设置：

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
阶段更正调整之间的时钟计时周期数。

|  |  |  
|---------|---------|
|密钥位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|设置    | 100        |
|结果 | 阶段更正调整之间的时钟计时周期数现在为 100 的刻度。 |

以下命令向发出信号 Windows 时间才能反映更新的设置：

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
在数秒内启用 SpecialInterval 0x1 标志时配置的轮询时间间隔。

|  |  |  
|---------|---------|
|密钥位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|设置    | 64        |
|结果 | 轮询间隔现在为 64 秒。 |

以下命令重启 Windows 时间才能反映更新的设置：

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|密钥位置     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|设置    | 2        |


---

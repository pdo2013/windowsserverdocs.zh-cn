---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows 时间服务
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: a86c2bdde9c65878b228f153009734da8f03960d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856828"
---
# <a name="windows-time-service-w32time"></a>Windows 时间服务 (W32Time)

>适用于：Windows Server 2016 中，Windows Server 2012 R2，Windows Server 2012 中，Windows 10 或更高版本

Windows 时间服务 (W32Time) 同步的日期和时间在 Active Directory 域服务 (AD DS) 中运行的所有计算机。 时间同步对于许多 Windows 服务和业务 (LOB) 应用程序的正常运行至关重要。 Windows 时间服务使用网络时间协议 (NTP) 来同步计算机时钟在网络上。 NTP 可确保，准确的时钟值或时间戳，可分配给网络验证和资源的访问请求。

Windows 时间服务 (W32Time) 主题中有可用的以下内容：
- **[Windows Server 2016 准确的时间](accurate-time.md)。** Windows Server 2016 中的时间同步准确性已大幅改进了，同时保持完全向后与早期 Windows 版本的 NTP 兼容。 可以在合理的操作情况下维护一个 1 ms 相对于 UTC 或适用于 Windows Server 2016 和 Windows 10 周年更新域成员的更好的准确性。
- **[对于高准确性环境支持边界](support-boundary.md)。** 本文介绍在需要高精确和稳定的系统时间的环境中的 Windows 时间服务 (W32Time) 的支持范围。
- **[配置系统的高精度](configuring-systems-for-high-accuracy.md)。** 显著改进了 Windows 10 和 Windows Server 2016 中的时间同步。  在合理的操作情况下可以配置系统维护 1 毫秒 （毫秒） 或 （相对于 UTC) 的更好的准确性。
- **[Windows 时间可跟踪性](windows-time-for-traceability.md)。** 在多个扇区的法规要求系统可追溯到 UTC。  这意味着，可以相对于 UTC 证明系统的偏移量。  若要启用法规符合性方案，Windows 10 和 Server 2016 提供了新的事件日志可提供从操作系统以形成了解系统时钟上执行的操作的角度来看图。  这些事件日志的 Windows 时间服务持续生成，可检查或存档以供日后分析。
- **[Windows 时间服务技术参考](windows-time-service-tech-ref.md)。** W32Time 服务提供网络而无需大量的配置的计算机的时钟同步。 W32Time 服务来说是必需的 Kerberos V5 身份验证成功的操作，则因此，对 AD DS 基于身份验证。
    - **[Windows 时间服务的工作原理](How-the-Windows-Time-Service-Works.md)。** 尽管 Windows 时间服务未完全实现的网络时间协议 (NTP)，它使用以确保在整个网络中的计算机上的时钟尽可能准确的 NTP 规范中定义的复杂算法套件。
    - **[Windows 时间服务工具和设置](Windows-Time-Service-Tools-and-Settings.md)。** 大多数域成员计算机具有 NT5DS，这意味着它们将从域层次结构的时间同步的时间客户端类型。 仅典型的例外是充当主域控制器 (PDC) 仿真器操作主机通常配置为与外部时间源同步时间的林根域的域控制器。


## <a name="related-topics"></a>相关主题
有关域层次结构和计分系统的详细信息，请参阅["什么是 Windows 时间服务？"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 博客文章。

Windows 时间提供程序插件模型[TechNet 上所述](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。

可以下载被 Windows 2016 精确时间项目引用的附录[此处](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

有关 Windows 时间服务的快速概述，看一看这[高级别概述视频](https://aka.ms/WS2016TimeVideo)。

<!-- In this guide
In this guide:
Windows Accurate Time
High Accuracy
Support Boundary
Configuration for High Accuracy
Traceability for Compliance
Best Practices
Technical Reference
How the Windows Time Service Works
Windows Time Service Tools and Settings
-->


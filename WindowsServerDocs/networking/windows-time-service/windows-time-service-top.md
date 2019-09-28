---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows 时间服务
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: e3dbaa188426ac81073e706db3adc6ab0a655c01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405191"
---
# <a name="windows-time-service-w32time"></a>Windows 时间服务（W32Time）

>适用于：Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows 10 或更高版本

Windows 时间服务（W32Time）为在 Active Directory 域服务（AD DS）中运行的所有计算机同步日期和时间。 对于许多 Windows 服务和业务线（LOB）应用程序的正确操作，时间同步至关重要。 Windows 时间服务使用网络时间协议（NTP）来同步网络上的计算机时钟。 NTP 确保准确的时钟值（或时间戳）可以分配到网络验证和资源访问请求。

Windows 时间服务（W32Time）主题中提供了以下内容：
- **[Windows Server 2016 准确时间](accurate-time.md)。** Windows Server 2016 中的时间同步准确性已显著提高，同时保持与早期 Windows 版本的完全向后 NTP 兼容性。 在合理的操作条件下，你可以在 Windows Server 2016 和 Windows 10 周年更新域成员的时间相对于 UTC 或更高的时间保持1毫秒的准确性。
- **[支持用于高准确性环境的边界](support-boundary.md)。** 本文介绍 Windows 时间服务（W32Time）在需要高度准确和稳定系统时间的环境中的支持边界。
- **[配置系统以获得高准确性](configuring-systems-for-high-accuracy.md)。** Windows 10 和 Windows Server 2016 中的时间同步已大幅提高。  在合理的操作条件下，可将系统配置为维持1ms （毫秒）的准确性或更好（与 UTC 相关）。
- **[Windows 时间以实现可跟踪](windows-time-for-traceability.md)性。** 许多扇区中的规章都需要将系统跟踪到 UTC。  这意味着系统相对于 UTC 的偏移量可能是证明的。  为了实现法规遵从性方案，Windows 10 和 Server 2016 提供了新的事件日志，以便从操作系统的角度提供一张图片，以形成对系统时钟所采取的操作的了解。  这些事件日志是针对 Windows 时间服务连续生成的，可以检查或存档以供以后分析。
- **[Windows 时间服务技术参考](windows-time-service-tech-ref.md)。** W32Time 服务为计算机提供网络时钟同步，而无需进行大量配置。 W32Time 服务对于成功操作 Kerberos V5 身份验证至关重要，因此，对于基于 AD DS 的身份验证至关重要。
    - **[Windows 时间服务的工作方式](How-the-Windows-Time-Service-Works.md)。** 尽管 Windows 时间服务不是网络时间协议（NTP）的确切实现，但它使用 NTP 规范中定义的复杂算法套件来确保网络中计算机上的时钟尽可能准确。
    - **[Windows 时间服务工具和设置](Windows-Time-Service-Tools-and-Settings.md)。** 大多数域成员计算机的时间客户端类型为 NT5DS，这意味着它们会同步域层次结构中的时间。 唯一的例外是，域控制器充当目录林根级域的主域控制器（PDC）仿真器操作主机，通常配置为使用外部时间源同步时间。


## <a name="related-topics"></a>相关主题
有关域层次结构和评分系统的详细信息，请参阅["什么是 Windows 时间服务？"。](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 博客文章。

[TechNet 上记录](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx)了 windows 时间提供程序插件模型。

可在[此处](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)下载 Windows 2016 准确时间文章引用的附录

有关 Windows 时间服务的快速概述，请查看此[高级概述视频](https://aka.ms/WS2016TimeVideo)。

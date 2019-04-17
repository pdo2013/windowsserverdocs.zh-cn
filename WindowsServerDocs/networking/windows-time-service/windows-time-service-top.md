---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows 时间服务
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b997d1f26e8da82e0d595b1ce13763e0a87d6d03
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="windows-time-service-w32time"></a>Windows 时间服务 (W32Time)

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

同步 Windows 时间服务 (W32Time) 的日期和时间的所有运行 Active Directory 域服务 (广告 DS) 的计算机。 时间同步至关重要的许多 Windows 服务和的业务线 (LOB) 应用正常运行。 Windows 时间服务使用的网络时协议 (NTP) 来同步网络上的计算机时钟。 NTP 确保，准确时钟值或时间戳，即可分配到网络验证和资源的访问权限请求。

在 Windows 时间服务 (W32Time) 主题中，以下内容有：
- **[Windows 2016 精确的时间](accurate-time.md)。** 在 Windows Server 2016 的时间同步准确性同时保持完整后反汇编 NTP 兼容性较旧的 Windows 版本与已已经提供了可观，得到改进。  可以在合理的操作条件下维护 1 毫秒 UTC 或 Windows Server 2016 和 Windows 10 周年更新域成员更好的准确性。
- **[Windows 时间服务技术参考](windows-time-service-tech-ref.md)。** W32Time service 提供计算机，而无需广泛的配置网络时钟的同步。 成功 Kerberos 第 V5 身份验证的操作，进而对广告基于 DS 的身份验证，则 W32Time 服务至关重要。
    - **[Windows 时间服务的工作原理](How-the-Windows-Time-Service-Works.md)。** 尽管 Windows 时间服务并不完全实现的网络时协议 (NTP)，它将使用复杂的算法套件中 NTP 规格以确保尽可能准确整个网络的计算机上时钟定义。
    - **[Windows 时服务工具和设置](Windows-Time-Service-Tools-and-Settings.md)。** 大多数域成员计算机具有时间客户端类型的 NT5DS，这意味着它们域层次从的时间进行同步。 这仅典型例外是充当的森林根域中，通常配置为与外部时间源时间进行同步主域控制器 (PDC) 仿真器操作主机域控制器。

## <a name="related-topics"></a>相关的主题
有关域层次和评分系统的详细信息，请参阅["近来可 Windows 时间服务？"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 博客文章。

Windows 时提供程序插件模型[TechNet 上记录](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。

可以下载通过 Windows 2016 精确的时间文章引用的附录[此处](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

有关 Windows 时间服务快速概述，看看这样[概括视频](https://aka.ms/WS2016TimeVideo)。

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


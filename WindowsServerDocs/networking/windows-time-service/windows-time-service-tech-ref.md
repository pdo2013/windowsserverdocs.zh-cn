---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows 时间服务技术参考
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 916ac654aeb1ca56e0283f9a18e011ef67734776
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="windows-time-service-technical-reference"></a>Windows 时间服务技术参考
W32Time service 提供计算机，而无需广泛的配置网络时钟的同步。 成功 Kerberos 第 V5 身份验证的操作，进而对广告基于 DS 的身份验证，则 W32Time 服务至关重要。 任何 Kerberos 注意到的应用程序，包括大多数安全服务，依赖参与身份验证请求计算机之间同步的时间。 广告 DS 域控制器必须还同步时钟，以帮助确保复制准确的数据。

> [!NOTE]  
> 在 Windows Server 2003，Microsoft Windows 2000 Server 目录服务名为 Active Directory 目录服务。 在 Windows Server 2008 R2 和 Windows Server 2008、 Active Directory 域服务 (广告 DS) 命名目录服务。 本主题中的其余部分指广告 DS，但也适用于 Windows Server 2016 中的 Active Directory 域服务信息。

在一个名为 W32Time.dll，默认情况下，在安装了其中的动态链接库中实现 W32Time 服务**%Systemroot%\System32**。 W32Time.dll 最初针对 Windows 2000 Server 支持规格中所需的网络，若要同步的时钟 Kerberos 第 V5 身份验证协议开发。 开始与 Windows Server 2003，W32Time.dll 提供对 Windows Server 2000 操作系统中网络时钟同步增加的准确性。 此外，在 Windows Server 2003，W32Time.dll 支持各种硬件设备使用时间提供商的网络时协议。

许多当前应用程序尽管最初旨在提供 Kerberos 身份验证的时钟同步，使用时间戳确保事务一致性，记录的重要事件和其他关键业务，时间时间敏感型信息。  这些应用程序受益于由 Windows 时间 service 提供的计算机之间同步的时间。

## <a name="importance-of-time-protocols"></a>时间协议的重要性
两台计算机若要交换时间信息，然后使用该信息同步他们时钟之间进行通信时间协议。 使用 Windows 时服务时间协议，的客户端从服务器请求时间信息，并使其时钟根据所收到的信息同步。
  
Windows 时间服务使用 NTP 帮助同步网络上的时间。 NTP 是包含不必同步时钟专业算法 Internet 时间协议。 NTP 是更精确的时间协议比简单网络时间协议 (SNTP)，可在某些版本的 Windows。但是，W32Time 仍支持 SNTP 启用与计算机运行 SNTP 基于时间的服务，如 Windows 2000 的后向兼容性。
<!-- maybe this should be its own topic under the Tech Ref section -->
## <a name="BKMK_Config"></a>在哪里可以找到 Windows 时间配置相关服务的信息  
本指南无法**不**讨论配置 Windows 时间服务。 有几个不同的主题 Microsoft TechNet 上和 Microsoft 知识库中执行说明配置 Windows 时间服务的过程。 如果你需要配置信息，以下主题应该帮助你找到相应的信息。  
<!-- should this be an if/then table -->
-   若要配置森林根主域控制器 (PDC) 仿真器的 Windows 时间服务，请参阅：  
  
    -   [配置 Windows 时间服务上 PDC 模拟器森林根域中](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [配置林时间源](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Microsoft 知识库文章 816042，[如何在 Windows Server 配置时间权威服务器](https://go.microsoft.com/fwlink/?LinkID=60402)，其中介绍配置设置为计算机运行 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003，并 Windows Server 2003 R2。  
  
-   若要任何域成员客户端或服务器或未配置为森林根 PDC 模拟器甚至该域控制器上配置 Windows 时间服务，请参阅[客户端将计算机配置为自动域时间同步](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。  
  
    > [!WARNING]  
    > 某些应用程序可能需要他们的计算机具有高准确性时服务。 如果是这种情况，你可以选择配置手动时间源，但是请注意，并非设计 Windows 时间服务为精准时间源的功能。 确保你了解有关高准确性时间环境，述 Microsoft 知识库文章 939322，支持限制[配置的 Windows 时间服务的支持边界高准确性环境](https://go.microsoft.com/fwlink/?LinkID=179459)。  
  
-   在任何基于 Windows 的客户端或服务器配置计算机在工作组中的成员而不是域成员查看配置 Windows 时间服务[配置手动时间为所选的客户端计算机源](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29)。  
  
-   配置了运行虚拟环境主计算机上的 Windows 时间服务，请参阅 Microsoft 知识库文章 816042，[如何配置中的时间权威服务器 Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). 如果你正在使用非 Microsoft 虚拟产品，请确保该产品，请参阅供应商的文档。  
  
-   若要在虚拟机上运行的域控制器上配置 Windows 时间服务，建议你部分禁用在主机系统和来宾操作系统作为域控制器之间同步的时间。 这使你的访客域控制器要同步的域层次的时间，但防止它时遇到扭曲如果地进行还原从保存状态的时间。 有关详细信息，请参阅 Microsoft 知识库文章 976924，[接收 24 29 日，38 Windows Server 2008 年基于承载服务器提供 Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236)和[部署注意事项虚拟化域控制器](https://go.microsoft.com/fwlink/?LinkID=192235)。  
  
-   要作为还在虚拟的电脑运行的森林根 PDC 模拟器域控制器上配置 Windows 时间服务，请按照相同说明物理计算机中所述[上 PDC 配置 Windows 时间服务森林根域中的模拟器](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。  
  
-   若要配置为虚拟电脑运行的成员服务器上的 Windows 时间服务，使用域时间层次中所述 ([客户端将计算机配置为自动域时间同步](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。


> [!IMPORTANT]  
> Windows Server 2016 中之前, W32Time 服务并非设计以满足时间敏感型应用程序需要。  但是，对 Windows Server 2016 更新现在允许你实施的 1ms 解决方案域中的准确性。  请参阅[Windows 2016 精确的时间](accurate-time.md)和[配置 Windows 时间为高准确性环境服务的支持边界](https://go.microsoft.com/fwlink/?LinkID=179459)详细信息。

## <a name="related-topics"></a>相关的主题  
[Windows 时间服务的工作原理](How-the-Windows-Time-Service-Works.md)  
[Windows 时服务工具和设置](Windows-Time-Service-Tools-and-Settings.md)  
[Microsoft 知识库文章 902229](https://go.microsoft.com/fwlink/?LinkId=186066)
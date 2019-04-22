---
title: 配置网络接口顺序
description: 本主题是适用于 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 18bc9a268b4e69e4b87b6b1e310f1162473adb10
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821768"
---
# <a name="configure-the-order-of-network-interfaces"></a>配置网络接口顺序

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在 Windows Server 2016 和 Windows 10 中，可以使用接口跃点数以配置网络接口的顺序。

这是在以前版本的 Windows 和 Windows Server，这允许您通过使用用户界面或命令配置网络适配器的绑定顺序不同于**INetCfgComponentBindings::MoveBefore**并**INetCfgComponentBindings::MoveAfter**。 这两种方法进行排序的网络接口不是在 Windows Server 2016 和 Windows 10 中可用。

相反，可用于新的方法来配置每个适配器的接口跃点数设置网络适配器的枚举的顺序。 可以通过使用配置的接口跃点数[集 NetIPInterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) Windows PowerShell 命令。

当网络流量路由选择和配置**InterfaceMetric**的参数**集 NetIPInterface**命令，用于确定接口的整体指标首选项为路由跃点数和接口跃点数的总和。 通常情况下，接口跃点数将优先考虑到特定的接口，例如，使用有线如果同时有线和无线都可用。

以下 Windows PowerShell 命令示例显示了使用此参数。

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

适配器在列表中出现的顺序确定的 IPv4 或 IPv6 接口跃点数。  有关详细信息，请参阅[GetAdaptersAddresses 函数](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)。

本指南中的所有主题的链接，请参阅[网络子系统性能调整](net-sub-performance-top.md)。

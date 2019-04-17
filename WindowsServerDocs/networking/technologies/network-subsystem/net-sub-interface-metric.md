---
title: 配置网络接口的订单
description: 本主题介绍 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2edf9d312e50a0fd8485e0032dee8413675367cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-order-of-network-interfaces"></a>配置网络接口的订单

>适用于：Windows Server（半年通道），Windows Server 2016

在 Windows Server 2016 和 Windows 10 中，你可以使用界面指标配置网络接口的订单。

这是不同于以前版本的 Windows 和 Windows Server，这允许你使用的用户界面或命令配置网络适配器的绑定顺序**INetCfgComponentBindings::MoveBefore**和**INetCfgComponentBindings::MoveAfter**。 Windows Server 2016 和 Windows 10 中不可用于订购网络接口这两种方法。

相反，你可以用于配置每个适配器界面指标设置网络适配器枚举的订单使用的新方法。 你可以通过使用配置界面指标[组 NetIPInterface](https://docs.microsoft.com/en-us/powershell/module/nettcpip/set-netipinterface) Windows PowerShell 命令。

选择网络通信的路线，并且配置**InterfaceMetric**参数**组 NetIPInterface**命令，用于确定接口的偏好随意选择整体指标是路线指标和界面指标的总和。 通常，界面指标将提供特定界面，如使用有线如果同时有线和无线有可用的偏好随意选择。

下面的 Windows PowerShell 命令示例显示了使用此参数。

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

由 IPv4 或 IPv6 界面指标确定的顺序适配器显示的列表中。  有关详细信息，请参阅[GetAdaptersAddresses 函数](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)。

本指南中的所有主题的链接，请参阅[网络子系统性能优化](net-sub-performance-top.md)。

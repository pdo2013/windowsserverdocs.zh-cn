---
title: 配置网络接口顺序
description: 本主题是 Windows Server 2016 的网络子系统性能优化指南的一部分。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4368d0709a22e9c122245c6a0131c5c9bee894a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405538"
---
# <a name="configure-the-order-of-network-interfaces"></a>配置网络接口顺序

>适用于：Windows Server（半年频道）、Windows Server 2016

在 Windows Server 2016 和 Windows 10 中，可以使用接口跃点来配置网络接口的顺序。

这与以前版本的 Windows 和 Windows Server 不同，后者允许使用用户界面或命令**INetCfgComponentBindings：： MoveBefore**配置网络适配器的绑定顺序，以及**INetCfgComponentBindings：： MoveAfter**。 Windows Server 2016 和 Windows 10 中不提供排序网络接口的两种方法。

相反，你可以使用新的方法来通过配置每个适配器的接口指标来设置网络适配器的枚举顺序。 可以使用[Get-netipinterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) Windows PowerShell 命令来配置接口跃点数。

如果选择了网络流量路由，并且已配置**get-netipinterface**命令的**InterfaceMetric**参数，则用于确定接口首选项的总体指标是路由指标和接口跃点数。 通常，接口跃点为特定接口（如有线和无线都可用）提供首选项。

下面的 Windows PowerShell 命令示例显示了此参数的用法。

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

适配器在列表中的显示顺序取决于 IPv4 或 IPv6 接口指标。  有关详细信息，请参阅[GetAdaptersAddresses 函数](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)。

有关本指南中的所有主题的链接，请参阅[网络子系统性能优化](net-sub-performance-top.md)。

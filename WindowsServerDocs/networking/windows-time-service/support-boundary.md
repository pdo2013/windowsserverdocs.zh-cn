---
ms.assetid: ''
title: 支持高精度时间边界
description: 本文介绍 Windows 时间（W32Time）服务在需要高度准确和稳定系统时间的环境中的支持边界。
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 212b9c79bc2e43e966180b928c865a9053332c3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405261"
---
# <a name="support-boundary-for-high-accuracy-time"></a>支持高精度时间边界

>适用于：Windows Server 2016 和 Windows 10 版本1607或更高版本

本文介绍 Windows 时间服务（W32Time）在需要高度准确和稳定系统时间的环境中的支持边界。

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Windows 8.1 和 2012 R2 （或更高）的高准确性支持

Windows 的早期版本（Windows 10 1607 或 Windows Server 2016 1607 之前）无法保证非常准确的时间。 这些系统上的 Windows 时间服务：

-   提供了满足 Kerberos 版本5身份验证要求所需的时间准确性

-   为加入到公共 Active Directory 林的 Windows 客户端和服务器提供了松散准确的时间

在这些操作系统上的 Windows 时间服务的设计规范之外，更严格的准确性要求不受支持。

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 和 Windows Server 2016

Windows 10 和 Windows Server 2016 中的时间准确性经过了大幅改进，同时保持与早期 Windows 版本的完全向后 NTP 兼容性。 在正确的操作条件下，运行 Windows 10 或 Windows Server 2016 及更高版本的系统可以提供1秒、50ms （毫秒）或1ms 准确性。

>[!IMPORTANT]
>**高度准确的时间源**<br>
>拓扑中的生成时间准确性非常依赖于使用准确的稳定根（层次1）时间源。 第三方供应商销售的是基于 Windows 和非 Windows 的高准确性、Windows 兼容的 NTP 时间源硬件。 请与供应商核实其产品的准确性。

>[!IMPORTANT]
>**时间准确性**<br>
>时间准确性需要从高度准确的权威时间源到最终设备的准确时间的端到端分发。 引入网络不对称的任何内容都将对准确性产生负面影响，例如，物理网络设备或目标系统上的高 CPU 负载。

## <a name="high-accuracy-requirements"></a>高准确度要求

本文档的其余部分概述了支持相应的高准确性目标所必须满足的环境要求。

### <a name="target-accuracy-1-second-1s"></a>目标准确性：1秒（1）

与高度精确的时间源相比，在特定目标计算机上实现1的精度：

-   目标系统必须运行 Windows 10、Windows Server 2016。

-   目标系统必须从时间服务器的 NTP 层次结构中同步时间，culminating 在高度准确、Windows 兼容的 NTP 时间源中。

-   上述 NTP 层次结构中的所有 Windows 操作系统都必须配置为 "[配置系统以获得高准确性](configuring-systems-for-high-accuracy.md)" 文档中所述的配置。

-   目标和源之间的累积单向网络延迟不能超过100ms。 通过在层次结构中的 NTP 客户端-服务器节点对之间添加单个单向延迟，从目标开始，到源结束。 有关详细信息，请查看高准确性时间同步文档。

### <a name="target-accuracy-50-milliseconds"></a>目标准确性：50毫秒

@No__t-0Target 准确性一节中列出的所有要求：1秒 @ no__t-0 适用，只不过此部分中有更严格的控制。

实现特定目标系统的50ms 准确性的其他要求包括：

-   目标计算机的时间源之间的网络延迟必须比5ms 更好。

-   对于高度准确的时间源，目标系统不得超过层次5

    >[!Note]
    >从命令行运行 "w32tm/query/status" 查看层次。

-   目标系统必须在高度准确的时间源内6个或更少的网络跃点内

-   所有 stratums 上的一天平均 CPU 使用率不得超过 90%

-   对于虚拟化系统，主机的一天平均 CPU 使用率不得超过 90%

### <a name="target-accuracy-1-millisecond"></a>目标准确性：1毫秒

@No__t-0Target 准确性部分中列出的所有要求：1秒 @ no__t-0 和 **Target 准确性：50毫秒 @ no__t-0 适用，只不过此部分中概述了更严格的控件。

为特定目标系统实现1个 ms 准确性的其他要求包括：

-   目标计算机的时间源之间的网络延迟必须大于 0.1 ms

-   对于高度准确的时间源，目标系统不得超过层次5

    >[!Note]
    >从命令行运行 "w32tm/query/status" 查看层次

-   目标系统必须在高度准确的时间源内4个或更少的网络跃点内

-   每个层次上的一天平均 CPU 使用率不得超过 80%

-   对于虚拟化系统，主机的一天平均 CPU 使用率不得超过 80%

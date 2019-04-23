---
ms.assetid: ''
title: 支持高精度时间边界
description: 本文介绍需要高精确和稳定的系统时间的环境中的 Windows 时间 (W32Time) 服务的支持边界。
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 991bf4502546771dae9f092c6d5732f96b1278ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866268"
---
# <a name="support-boundary-for-high-accuracy-time"></a>支持高精度时间边界

>适用于：Windows Server 2016 和 Windows 10 版本 1607年或更高版本

本文介绍在需要高精确和稳定的系统时间的环境中的 Windows 时间服务 (W32Time) 的支持范围。

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>对于 Windows 8.1 和 2012 R2 （或之前） 的高准确性支持

早期版本的 Windows （之前 Windows 10 1607年或 Windows Server 2016 1607年） 不能保证高度准确的时间。 在这些系统上将 Windows 时间服务：

-   提供必要的时间准确性，以满足 Kerberos 版本 5 身份验证要求

-   Windows 客户端和服务器加入到常见的 Active Directory 林提供松散准确的时间

更紧密的准确性要求是外部的这些操作系统上的 Windows 时间服务的设计规范和不支持。

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 和 Windows Server 2016

在 Windows 10 和 Windows Server 2016 中的时间准确性进行了显著改进，同时保持完全向后与早期 Windows 版本的 NTP 兼容。 操作条件的权限，在运行 Windows 10 或 Windows Server 2016 和更高版本的系统可以提供 1 的第二个，50 毫秒 （毫秒） 或 1 毫秒准确性。

>[!IMPORTANT]
>**高度准确的时间源**<br>
>在您的拓扑中生成的时间准确性是高度依赖于使用准确的稳定的根 （第 1） 时间源。 有 Windows 基于和非 Windows 基于非常准确，Windows 兼容，NTP 时间源硬件售出的第三方供应商。 请与你的供应商在其产品的准确性。

>[!IMPORTANT]
>**时间准确性**<br>
>时间准确性时，需要的端到端分布的准确的时间非常准确的权威时间源的数据到最终设备。 引入了网络不对称的任何内容将产生负面影响的准确性，例如物理网络设备或目标系统上的高 CPU 负载。

## <a name="high-accuracy-requirements"></a>高精度要求

此文档的其余部分概述了必须满足才能支持相应的高精度目标的环境要求。

### <a name="target-accuracy-1-second-1s"></a>目标准确性：1 秒 (1)

若要实现 1 s 准确性的特定目标机器相比到高度准确的时间源：

-   在目标系统必须运行 Windows 10 中，Windows Server 2016。

-   在目标系统必须同步时间来自 NTP 层次结构的时间服务器，最终在非常准确，Windows 兼容 NTP 时间源。

-   必须配置上述 NTP 层次结构中所有 Windows 操作系统，如中所述[配置系统上的高精度](configuring-systems-for-high-accuracy.md)文档。

-   目标和源之间的累积单向网络延迟不能超过 100 毫秒。 通过添加对单个测量累积的网络延迟与目标开始和结束在源层次结构中的 NTP 客户端-服务器节点对之间的单向延迟。 有关详细信息，请查看高精度时间同步文档。

### <a name="target-accuracy-50-milliseconds"></a>目标准确性：50 毫秒

部分中所述的所有要求**目标准确性：1 第二个**应用，除非在本部分概述了更严格的控制。

若要实现特定目标系统的 50 毫秒准确性的其他要求如下：

-   目标计算机必须具有其时间源之间的优于 5 毫秒的网络延迟时间。

-   必须在目标系统不超过第 5 从高度准确的时间源

    >[!Note]
    >从命令行以查看第运行"w32tm /query /status"。

-   在目标系统必须在 6 个月或更少网络跃点从高度准确的时间源

-   在所有 stratums 的为期一天平均 CPU 利用率不能超过 90%

-   对于虚拟化系统的主机的为期一天平均 CPU 使用率必须不超过 90%

### <a name="target-accuracy-1-millisecond"></a>目标准确性：1 毫秒

部分中所述的所有要求**目标准确性：1 秒**和**目标准确性：50 毫秒**应用，除非在本部分概述了更严格的控制。

若要实现的特定目标系统的 1 ms 准确性的其他要求如下：

-   目标计算机必须优于 0.1 毫秒的网络延迟之间它的时间源

-   必须在目标系统不超过第 5 从高度准确的时间源

    >[!Note]
    >从命令行以查看第运行 w32tm /query /status

-   在目标系统必须在高度准确的时间源的 4 个或更少网络跃点

-   每个第一天平均 CPU 使用率不能超过 80%

-   对于虚拟化系统，该主机的为期一天平均 CPU 利用率不能超过 80%

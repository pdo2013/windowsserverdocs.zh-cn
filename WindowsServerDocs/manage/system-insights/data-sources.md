---
title: 系统 Insights 数据源
description: 时系统 Insights 中编写的新功能，可以指定本地收集和分析的现有或新数据源。 本主题介绍了时注册的新功能，可以选择的数据源。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 9b46db90787d24a173ffa472ec1ecb8eaffe054b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845408"
---
# <a name="system-insights-data-sources"></a>系统 Insights 数据源

>适用于：Windows Server 2019

系统 Insights 引入了可扩展的数据收集功能。 在编写时的新功能，可以指定本地收集和分析的现有或新数据源。 本主题介绍了时注册的新功能，可以选择的数据源。

## <a name="data-sources"></a>数据源
在编写时的新功能，必须确定要收集的每个功能的特定数据源。 您指定的数据源将收集和保留直接在你的计算机，并可以从三种类型的数据源选择：

- **性能计数器**: 
    - 指定计数器路径、 名称和实例，然后系统 Insights 将收集这些性能计数器报告的相关数据。 

- **系统事件**:
    - 指定通道名称和事件 ID，并将系统 Insights 将记录多少次该事件已发生。

- **熟知的系列**
    - 系统 Insights 收集几个时，明确定义的资源在计算机上的一些基本信息。 这些系列使用的默认功能，但它们还可以使用的任何自定义功能。 这些收集以下信息：

        - **磁盘**: 
            - *属性*:Guid
            - *数据*:大小
        - **卷**:
            - *属性*:UniqueId，驱动器号，FileSystemLabel 大小
            - *数据*:已用的大小
        - **网络适配器**:
            - *属性*:InterfaceGuid，InterfaceDescription，速度
            - *数据*:Bytes Received/sec，发送字节/秒，字节总数/秒
        - **CPU**: 
            - *属性*:-
            - *数据*： 处理器时间百分比

    - 指定一个已知的序列，并且系统 Insights 将返回该系列收集的数据。 


## <a name="retention-timelines-and-collection-intervals"></a>保留期时间线和集合的时间间隔
上面的数据源具有不同的保留时间线和集合的时间间隔。 下表显示每个数据源收集的长时间和方式通常：

| 数据源 | 保留期时间线 | 收集间隔 |
| --------------- | --------------- | ----------- |
| 性能计数器 | 3 个月 | 15 分钟 |
| 系统事件 | 3 个月 | 15 分钟 |
| 熟知的系列 | 1 年 | 1 小时 |


### <a name="aggregation-types"></a>聚合类型
因为每个序列的每个收集间隔记录只有一个数据点，每个序列具有聚合类型关联它。 下表描述了数据源和相应的聚合类型：

>[!NOTE]
>对于性能计数器，可以选择从几种不同的聚合类型。

| 数据源 | 聚合类型 |
| --------------- | --------------- |
| 性能计数器 | 总和、 平均值、 最大、 最小值 |
| 系统事件 | Count |
| 磁盘的已知系列 | 最后一个 （最新值中的收集间隔） |
| 卷的已知系列 | 最后一个 （最新值中的收集间隔） |
| CPU 的已知系列 | 平均值 |
| 网络的已知系列 | 平均值 |

## <a name="data-footprint"></a>数据占用的空间

系统 Insights C 驱动器 （c:） 上本地收集的所有数据。 一般情况下，系统 Insights 数据占用的空间是适度。 它依赖于类型和数量的数据源指定每项功能，直接和下表详细介绍每种数据类型的存储使用情况：

| 数据源 | 最大内存占用 |
| --------------- | --------------- |
| 性能计数器 | 240 KB |
| 系统事件 | 200 KB |
| 磁盘的已知系列 | 每个磁盘的 200 KB |
| 卷的已知系列 | 每个卷的 300 KB |
| CPU 的已知系列 | 100 KB |
| 网络的已知系列 | 每个网络适配器的 300 KB |

>[!NOTE]
>**预测功能默认情况下，最大占用空间应小于 10 MB 的大多数的独立计算机。** 

## <a name="see-also"></a>请参阅
若要了解有关系统 Insights 的详细信息，请参阅以下资源：

- [系统 Insights 概述](overview.md)
- [了解功能](understanding-capabilities.md)
- [管理功能](managing-capabilities.md)
- [添加和开发功能](adding-and-developing-capabilities.md)
- [系统 Insights 常见问题](faq.md)

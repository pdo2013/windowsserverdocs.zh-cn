---
title: 缓存和内存管理器改进
description: 缓存和 Windows Server 2016 中的内存管理器改进
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bd15cfc0714d1992e6b15d716b6b96ce259786da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868578"
---
# <a name="cache-and-memory-manager-improvements"></a>缓存和内存管理器改进

本主题介绍 Windows Server 2012 和 2016年中的缓存管理器和内存管理器改进。

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Windows Server 2016 中的缓存管理器改进
缓存管理器还添加了对于 true 异步缓存读取的支持。
如果严重依赖于异步缓存读取，这可能无法提高应用程序的性能。  大多数内置文件系统有支持异步缓存读取的一段时间，而是通常由于与线程池和文件系统的内部工作队列的处理相关的各种设计选择的性能限制。  利用内核正确支持，缓存管理器现在可隐藏所有线程池和工作队列管理复杂性，使其能够有效地处理异步文件系统从缓存读取。缓存管理器具有一组控件 datastructures （系统支持最大值） 的每个 VHD 嵌套级别，以最大化并行度。


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Windows Server 2012 中的缓存管理器改进
除了缓存管理器增强功能，以读取顺序工作负荷，一个新的 API 的预逻辑[CcSetReadAheadGranularityEx](https://msdn.microsoft.com/library/windows/hardware/hh406341.aspx)为了让文件系统驱动程序，例如 SMB、 更改其读取预参数。 它通过发送多个小型预读请求而不是发送单个较大读取预请求允许远程文件方案更好的吞吐量。 仅内核组件，如文件系统驱动程序，以编程方式可以在每个文件的基础上配置这些值。

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Windows Server 2012 中的内存管理器改进
启用页面组合可能会降低其具有相同的内容与专用的可分页页数挺多的服务器上的内存使用率。 例如，运行相同的占用大量内存的应用或适用于高度重复数据的单个应用程序的多个实例的服务器可能是很好的候选对象尝试组合的页。 启用页面组合的缺点是增加的 CPU 使用率。

下面是一些示例的服务器角色页面组合是速度可能不会带来的巨大优势：

-   文件服务器 （由文件页不是专用，因此不可组合使用的大部分内存）

-   Microsoft SQL Server 配置为使用 AWE 或大型页 （大多数内存为专用，但不可分页）

默认情况下禁用页面组合，但可以通过启用[启用 MMAgent](https://technet.microsoft.com/library/jj658954.aspx) Windows PowerShell cmdlet。 Windows Server 2012 中添加页面组合。

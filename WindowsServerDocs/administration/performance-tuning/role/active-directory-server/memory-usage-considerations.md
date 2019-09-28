---
title: AD DS 性能优化中的内存使用注意事项
description: 运行 Windows Server 2012 R2、2016和2019的域控制器上的 Lsass.exe 进程的内存使用率。
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 55ac47d835874ddb8e160603f08cbafa985aad2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370286"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>AD DS 性能优化的内存使用注意事项

本文介绍了本地安全机构子系统服务（LSASS，也称为 Lsass.exe 进程）的一些基础知识、适用于 LSASS 配置的最佳实践以及内存使用情况。 本文应作为对域控制器（Dc）上的 LSASS 性能和内存使用分析的指导。 如果有关于如何优化和配置服务器和 Dc 以优化此引擎的问题，本文中的信息可能会很有用。  

LSASS 负责管理本地安全机构（LSA）域身份验证和 Active Directory 管理。 LSASS 同时处理客户端和服务器的身份验证，并控制 Active Directory 引擎。 LSASS 负责以下组件：  

- 本地安全机构
- NetLogon 服务
- 安全帐户管理器（SAM）服务
- LSA 服务器服务
- 安全套接字层（SSL）
- Kerberos v5 身份验证协议
- NTLM 身份验证协议
- 加载到 LSA 中的其他身份验证包

Active Directory 数据库服务（NTDSAI）使用可扩展存储引擎（ESE、ESENT）。

下面是 DC 上的 LSASS 内存使用情况示意图：

![使用 LSASS 内存的组件示意图](media/domain-controller-lsass-memory-usage.png)  

在 DC 上使用的内存量根据 Active Directory 使用量增加。 查询数据时，数据将缓存在内存中。 因此，使用的内存量大于 Active Directory 数据库文件（ntds.dit）的大小时，查看 LSASS 是正常的。

如图所示，LSASS 内存使用情况可以分为多个部分，包括 ESE 数据库缓冲区缓存、ESE 版本存储区以及其他部分。 本文的其余部分将深入了解其中的每个部分。

## <a name="ese-database-buffer-cache"></a>ESE 数据库缓冲区缓存  
LSASS 内最大的可变内存使用量是 ESE 数据库缓冲区缓存。 缓存的大小范围从小于 1 MB 到整个数据库的大小。 因为较大的缓存提高了性能，所以 Active Directory 的数据库引擎（ESENT）会尝试尽可能大的缓存。 虽然缓存的大小因计算机内存压力而异，但 ESE 数据库缓冲区缓存的最大大小*仅*受计算机中安装的物理 RAM 的限制。 只要没有任何其他内存压力，缓存就会增长到 Active Directory 的 ntds.dit 数据库文件的大小。 可以缓存的数据库越多，DC 的性能就越好。  
  
> [!NOTE]
> 由于数据库缓存算法的工作方式，在数据库大小小于可用 RAM 的64位系统上，数据库缓存可能会增长到大于30到 40% 的数据库大小。

## <a name="ese-version-store"></a>ESE 版本存储

ESE 版本存储区（上图中的红色部分）存在可变内存使用情况。 使用的内存量取决于 Windows Server 2019 或更早版本的 Windows。

- 在 Windows Server 版本 predating Windows Server 2019 中，在默认情况下，LSASS 可能会对 ESE 版本存储使用64位计算机上的最多400MB 内存（具体取决于 Cpu 数）。 有关如何使用版本存储区的详细信息，请参阅以下 ASKDS 博客文章 Ryan Ries：[调用了版本存储，它们都是存储桶](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415)。

- 在 Windows Server 2019 中，这是简化的，当 NTDS 服务首次启动时，ESE 版本存储大小现在计算为物理 RAM 的 10%，最小值为400MB，最大值为4GB。 有关此和版本存储故障排除的详细信息，请参阅 Ryan Ries 的其他精彩博客：@no__t 0Deep：服务器 2019 @ no__t 中的 ESE 版本存储更改 Active Directory。

## <a name="other-memory-use"></a>其他内存使用

最后，有代码、堆栈、堆和各种固定大小的数据结构（例如，架构缓存）。 LSASS 使用的内存量可能会有所不同，具体取决于计算机上的负载。 随着正在运行的线程数的增加，内存堆栈的数量也随之增加。 对于这些固定的组件，LSASS 平均使用 100 MB 到 300 MB 内存。 当安装的 RAM 量较大时，LSASS 可能会使用更多的 RAM 和更少的虚拟内存。

**限制或最小化域控制器上的程序数，或在合适的位置添加额外的 RAM**

为了获得最佳性能，LSASS 会在给定 DC 上使用尽可能多的 RAM。 LSASS 会将 RAM 让给为其他进程要求提供。 其思路是优化 LSASS 的性能，同时还会考虑可能在计算机上运行的其他进程。 要监视的程序的列表包括监视代理。 有些客户对各种服务器功能具有不同的代理，这可能会占用大量的 RAM 资源。 有些查询可能会发出很多 WMI 查询，我们将在下面提供一些详细信息。

因此，为了提高性能，最好限制或最大限度地减少 DC 上程序的数量。 如果没有内存请求，LSASS 将使用此内存来缓存 Active Directory 数据库，从而获得最佳性能。

当你发现 DC 存在性能问题时，还需要监视内存使用率很高的进程。 这些问题可能有问题。 它们可能包括 Microsoft 组件。 请确保保持最新的服务更新 @ no__t-0Microsoft 包含用于高内存使用率的解决方案（作为质量更新的一部分），这也可能会帮助你的 DC 性能。

有一些内置 OS 功能可能会占用大量 RAM，具体取决于使用情况配置文件：

- **文件服务器**。 Dc 也是用于 SYSVOL 和 Netlogon 共享的文件服务器，为策略和启动/登录脚本提供服务组策略和脚本。
  但是，我们会看到客户使用 Dc 为其他文件内容服务。 SMB 文件服务器随后会消耗 RAM 来跟踪活动的客户端，但最重要的是，文件内容会使 OS 文件缓存增长，并与 ESE 数据库缓存争用 RAM。  

- **WMI 查询**。 监视解决方案通常会进行很多 WMI 查询。 单个查询的执行成本可能较低。 通常，这是引发一些开销的调用量，尤其是在监视解决方案从 Windows 管理的各种事件日志中提取新事件时。  

  生成最多卷的事件日志通常是安全事件日志。 这也是安全管理员想要收集的事件日志，尤其是在 Dc 中。  

  WMI 服务使用动态内存分配方案来优化查询。 因此，WMI 服务可能会分配大量内存，并再次与 ESE 数据库缓存争用。  

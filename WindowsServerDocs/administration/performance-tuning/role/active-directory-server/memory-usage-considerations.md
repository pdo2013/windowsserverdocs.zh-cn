---
title: 在 AD DS 性能优化的内存使用情况注意事项
description: 在域控制器上运行 Windows Server 2012 R2，2016年和 2019 Lsass.exe 进程内存使用率。
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: dd33124d7d480f3670fa2781f567eaaa28abbce6
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799779"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>有关 AD DS 性能优化的内存使用情况注意事项

本文介绍的一些基础知识的本地安全机构子系统服务 (LSASS，也称为 Lsass.exe 进程)，为 LSASS，配置的最佳实践和对内存使用情况的期望。 本文应该用作域控制器 (Dc) 的 LSASS 性能和内存使用分析中的指南。 在本文中的信息可能非常有用，如果您有关于如何优化和配置服务器和域控制器来优化此引擎的问题。  

LSASS 负责管理的本地安全机构 (LSA) 域身份验证和 Active Directory 管理。 LSASS 处理身份验证的客户端和服务器，并且它还控制 Active Directory 引擎。 LSASS 负责以下组件：  

- 本地安全机构
- NetLogon 服务
- 安全帐户管理器 (SAM) 服务
- LSA 服务器服务
- 安全套接字层 (SSL)
- Kerberos v5 身份验证协议
- NTLM 身份验证协议
- 加载到 LSA 其他身份验证包

Active Directory 数据库服务 (NTDSAI.dll) 使用可扩展存储引擎 （ESE，ESENT.dll）。

下面是在 DC 上的 LSASS 内存使用情况的可视化关系图：

![使用 LSASS 内存的组件的关系图](media/domain-controller-lsass-memory-usage.png)  

在 DC 使用 LSASS 的内存量会增加根据 Active Directory 使用情况。 查询数据时，它是在内存中缓存。 因此，是内存的正常的使用的是内存的比 Active Directory 数据库文件 (NTDS.dit) 的大小更大量的 LSASS。

在关系图中所示，可以为多个部分，包括 ESE 数据库缓冲区高速缓存、 ESE 版本存储区和其他划分 LSASS 内存使用情况。 本文的其余部分提供了深入了解每个部分。

## <a name="ese-database-buffer-cache"></a>ESE 数据库缓冲区高速缓存  
LSASS 内最大的变量的内存使用率是 ESE 数据库缓冲区高速缓存。 缓存的大小范围为整个数据库的大小小于 1 MB。 更大的缓存提高了性能，因为数据库引擎的 Active Directory (ESENT) 将尝试保留缓存一样大。 虽然缓存的大小随计算机中的内存压力，ESE 数据库缓冲区高速缓存的最大大小是*仅*受计算机中安装的物理 RAM。 只要没有任何其他内存压力，缓存可以增长到 Active Directory NTDS.dit 数据库文件的大小。 更多的数据库的可缓存，更好的性能 DC 将是。  
  
> [!NOTE]
> 由于缓存算法的数据库的数据库大小小于可用 RAM 的 64 位系统工作的方式将数据库缓存可以增大到大于数据库大小 30 到 40%。

## <a name="ese-version-store"></a>ESE 版本存储区

按 ESE 版本存储区 （上图中的红色部分） 没有变量的内存使用情况。 使用的内存量取决于是否具有 Windows Server 2019 或较旧版本的 Windows。

- 在 Windows Server 2019 predating Windows Server 版本中，通过 LSASS 可以在 ESE 版本在 64 位计算机使用最多大约 400 MB 的内存 （具体取决于 Cpu 数） 的默认存储。 有关如何使用版本存储区的详细信息请参阅以下 ASKDS 博客文章由 Ryan Ries:[版本存储区调用，并且它们是所有带存储桶](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415)。

- 在 Windows Server 2019，这简化和 NTDS 服务首次启动时, 将 ESE 版本存储区大小现在为 400 MB 的最小值和最大 4 GB 的物理 RAM 的 10%计算。 有关这很好的详细信息和版本存储区进行故障排除，请参阅从 Ryan Ries 另一个好的博客：[深入探讨：Active Directory ESE 版本存储在服务器 2019年更改](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Deep-Dive-Active-Directory-ESE-Version-Store-Changes-in-Server/ba-p/400510)。

## <a name="other-memory-use"></a>其他内存使用

最后，还有代码、 堆栈、 堆和固定的大小的各种数据结构 （例如，架构缓存）。 LSASS 使用的内存量可能不同，具体取决于计算机上的负载。 随着的正在运行的线程数的增加，因此执行数量的内存堆栈。 一般情况下，LSASS 将 100 MB 到 300 MB 的内存用于这些固定的组件。 安装更大的内存时，LSASS 可以使用更多的 RAM 和虚拟内存更少。

**限制或尽量减少你的域控制器上的程序或在适当的位置添加额外的内存。**

为获得最佳性能，LSASS 给定 DC 上尽可能采用同样多的 RAM。 LSASS 放弃该 RAM，因为其他进程请求它。 其理念是优化 LSASS 的性能，同时仍考虑可能在一台计算机运行其他进程。 要注意的程序列表包括监视代理。 某些客户具有单独的代理的各种服务器功能都可以使用大量 RAM 资源。 一些可能会发出多个 WMI 查询，我们具有以下几个详细信息。

正因为如此，并以提高性能，最好限制或在 DC 上的程序的数量降至最低。 如果没有任何内存请求，LSASS 将使用此内存来缓存 Active Directory 数据库，并因此获得最佳性能。

如果您注意到是 DC 也有性能问题，还监视大量的内存利用率的进程。 这些可能存在需要进行故障排除的问题。 它们可能包括 Microsoft 组件。 请确保您及时了解最新的服务更新&mdash;Microsoft 包含解决方案的质量更新，它还可帮助您 DC 的性能的一部分的过多的内存使用率。

没有可使用大量 RAM，具体取决于使用情况配置文件的内置 OS 工具：

- **文件服务器**。 Dc 还都针对 SYSVOL 和 Netlogon 共享，维护组策略和策略和启动/登录脚本的脚本的文件服务器。
  但是，我们看到客户使用的 Dc 以服务的其他文件内容。 SMB 文件服务器便会使用 RAM 来跟踪活动的客户端，但文件内容，会使 OS 文件缓存的增长，并与 ESE 数据库缓存会争夺 RAM。  

- **WMI 查询**。 监视解决方案通常具有很多 WMI 查询。 单个查询可能会为低开销来执行。 通常它是可能会产生一些开销，电话的数量，尤其是当监视解决方案从各种 Windows 管理的事件日志中提取新的事件。  

  生成的大多数卷在事件日志通常是安全事件日志。 这也是安全管理员想要收集，尤其是从 Dc 事件日志。  

  WMI 服务使用动态内存分配方案的优化查询。 因此，WMI 服务可能会分配大量内存，再次使用 ESE 数据库缓存争用。  

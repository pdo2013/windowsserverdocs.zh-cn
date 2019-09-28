---
title: 性能优化 Web 服务器
description: 针对 Windows Server 16 上的Web 服务器的性能优化建议
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9587eb5215d2920a0146e8a697c6f36c50f19f27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370051"
---
# <a name="performance-tuning-web-servers"></a>性能优化 Web 服务器


本主题介绍适用于 Windows Server 2016 Web 服务器的性能优化方法和建议。


## <a name="selecting-the-proper-hardware-for-performance"></a>根据性能选择适当的硬件


考虑到平均负载、峰值负载、容量、增长计划和响应时间，必须选择适当的硬件以满足预期的 Web 负载。 硬件瓶颈会限制软件优化的有效性。

[针对服务器硬件的性能优化](../../hardware/index.md)一文提供了针对硬件的建议，目的是避免以下性能约束：

-   CPU 速度慢导致为 CPU 密集型工作负荷（例如 ASP、ASP.NET 和 TLS 方案）提供的处理能力受限。

-   小型的 L2 或 L3/LLC 处理器缓存可能会对性能造成负面影响。

-   内存量受限会影响可以托管的站点数、可以存储的动态内容脚本（例如 ASP.NET）的数目，以及应用程序池或工作进程的数目。

-   由于网络适配器效率低下，网络成为瓶颈。

-   由于磁盘子系统或存储器适配器效率低下，文件系统成为瓶颈。

## <a name="operating-system-best-practices"></a>操作系统最佳做法


尽可能一开始就对操作系统进行干净安装。 升级软件时，可能会保留过时的、不需要的或者非最优的注册表设置以及以前安装的服务和应用程序。在自动启动的情况下，它们会耗用资源。 如果已安装另一操作系统且该操作系统必须保留，则应将新操作系统安装在不同的分区。 否则，新的安装会覆盖 %Program Files%\\Common Files 下的设置。

为了减少磁盘访问干扰，请尽可能将系统页面文件、操作系统、Web 数据、ASP 模板缓存以及 Internet Information Services (IIS) 日志置于不同的物理磁盘上。

为了减少对系统资源的争用，请尽可能将 Microsoft SQL Server 和 IIS 安装在不同的服务器上。

避免安装不必要的服务和应用程序。 在某些情况下，可能有必要禁用系统中不需要的服务。

## <a name="ntfs-file-system-settings"></a>NTFS 文件系统设置

系统全局开关 **NtfsDisableLastAccessUpdate** (REG\_DWORD) 1 位于 **HKLM\\System\\CurrentControlSet\\Control\\FileSystem** 下，默认设置为 1。 此开关针对上一个文件或目录访问禁用日期和时间戳更新，因此可以降低磁盘 I/O 负载和延迟。 干净安装 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008 即可默认启用此设置，不需调整。 早期版本的 Windows 不设置此项。 如果服务器运行早期版本的 Windows，或者已升级到 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008，则应启用此设置。

使用包含数千目录的大型数据集（或许多主机）时，禁用更新会很有效。 如果保留此信息只是为了 Web 管理，建议改用 IIS 日志记录。

>[!Warning]
> 某些应用程序（例如增量备份实用程序）依赖此更新信息，没有它就不能正常运行。

## <a name="see-also"></a>另请参阅
- [IIS 10.0 性能优化](tuning-iis-10.md)
- [HTTP 1.1/2 优化](http-performance.md)



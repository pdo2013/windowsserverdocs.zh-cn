---
title: 硬件要求和性能建议
description: 为 MultiPoint 服务提供硬件和性能要求和建议
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 284131028b308ee86389f25102d934390ba2f16d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389111"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>硬件要求和性能建议
本主题介绍运行 MultiPoint 服务系统和支持用户应用程序方案所需的硬件。 用户方案直接影响 CPU、RAM 和网络带宽要求。  

## <a name="optimize-multipoint-services-system-performance"></a>优化 MultiPoint 服务系统性能  
MultiPoint 服务系统的性能将直接受到 CPU、GPU 和运行 MultiPoint 服务的计算机上可用的 RAM 量的影响的影响。  
  
### <a name="applications-and-internet-content"></a>应用程序和 Internet 内容  
因为 MultiPoint 服务是一种共享资源计算解决方案，所以在工作站上运行的应用程序的类型和数量可能会影响 MultiPoint 服务系统的性能。 考虑在规划系统时经常使用的程序类型，这一点很重要。 例如，图形密集型应用程序需要比应用程序（如字处理）更强大的计算机。 用图形密集型应用程序重载计算机可能会导致整个系统中出现 lag 问题。  
  
应用程序访问的内容类型也会影响系统的性能。 如果有多个工作站在使用 web 浏览器访问多媒体内容（如动态运动视频），则在对系统性能产生负面影响之前，可以连接更少的工作站。 相反，如果多个工作站使用 web 浏览器来访问静态 web 内容，则可以连接更多的工作站，而不会对性能产生显著影响。  
  
### <a name="hardware-recommendations"></a>硬件建议  
若要在不同负载下为 MultiPoint 服务系统实现良好的性能，请在规划和测试系统时使用下表中的准则。 这些是 forMultiPoint Services 的基本要求。 实际的配置大小取决于系统配置、正在运行的工作负荷和硬件功能。 应始终通过测试应用程序和硬件进行验证。  
  
> [!NOTE]  
> 2C = 2 个核心，4C = 4 个内核，6C = 6 个内核，MT = 多线程。 处理器速度应至少为2.0 千兆赫（GHz）。  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>用于运行默认 MultiPoint Server 工作站的最低建议硬件  
  
|应用程序方案|最多5个工作站|6-8 工作站|9-12 工作站|13-16 工作站|17-20 工作站|21-24 工作站|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**提高**<br /><br />Office、web 浏览、业务线应用程序|CPU：上文<br /><br />RAM：2 GB|CPU：上文<br /><br />RAM：4 GB|CPU：4C<br /><br />RAM：6 GB|CPU：4C<br /><br />RAM：8 GB|CPU：4C + MT 或6C<br /><br />RAM：10 GB| CPU：6C + MT<br /><br />RAM：12 GB|
|**混**<br /><br />某些用户偶尔使用的 Office、web 浏览、业务线应用程序和视频|CPU：上文<br /><br />RAM：2 GB|CPU：上文<br /><br />RAM：4 GB|CPU：4C<br /><br />RAM：6 GB|CPU：4C + MT 或6C<br /><br />RAM：8 GB|CPU：6C + MT<br /><br />RAM：10 GB| CPU：6C + MT<br /><br />RAM：12 GB| 
|**视频密集型**<br /><br />Office、web 浏览、业务线应用程序，以及所有用户的频繁视频使用**备注：** 视频测试是使用360p 视频以本机分辨率执行的视频。|CPU：4C + MT<br /><br />RAM：2 GB|CPU：6C + MT<br /><br />RAM：4 GB|CPU：8C + MT<br /><br />RAM：6 GB|CPU：12C + MT<br /><br />RAM：8 GB|CPU：16C + MT<br /><br />RAM：10 GB<br /><br />-瘦客户端：RemoteFX<br />-不建议使用 USB 视频| CPU：20C + MT<br /><br />RAM：12 GB<br /><br />-瘦客户端：RemoteFX<br />-不建议使用 USB 视频|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>运行完整的 Windows 10 虚拟桌面所建议的最低硬件  
为每个工作站运行完整的虚拟操作系统实例比运行默认的 MultiPoint 桌面会话更耗费计算资源，因此，每个工作站的主机硬件要求更高：  
  
1.  CPU：每个工作站1个核心或线程  
  
2.  固态硬盘（SSD）  
  
    1.  容量 > = 每个工作站的 20 gb + 40 GB，适用于 WMS 主机操作系统  
  
    2.  随机读取/写入 IOPS > = 每个工作站3K  
  
3.  RAM > = 每个工作站 2GB + 2gb 适用于 WMS 主机操作系统  
  
已将 BIOS CPU 设置配置为启用虚拟化–第二级地址转换（SLAT）  
  
有关选择最佳 MultiPoint 服务硬件满足你的需求的详细信息，请与你的硬件供应商联系。  
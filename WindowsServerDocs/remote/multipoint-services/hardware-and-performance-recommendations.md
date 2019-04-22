---
title: 硬件要求和性能建议
description: 提供有关 MultiPoint 服务的硬件和性能要求和建议
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b47f4c224d4a4f6f4aa104b6d5ee5d93590ac0c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823388"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>硬件要求和性能建议
本主题介绍运行 MultiPoint 服务系统和支持用户应用程序方案所需的硬件。 用户方案直接影响 CPU、 RAM 和网络带宽要求。  

## <a name="optimize-multipoint-services-system-performance"></a>优化 MultiPoint 服务系统性能  
CPU、 GPU 和正在运行 MultiPoint 服务计算机可用的 RAM 量的功能将直接影响你的 MultiPoint 服务系统的性能。  
  
### <a name="applications-and-internet-content"></a>应用程序和 Internet 内容  
由于 MultiPoint Services 是共享的资源计算解决方案，则类型和数量的工作站运行的应用程序可能会影响你的 MultiPoint 服务系统的性能。 请务必要考虑的定期计划您的系统时，将使用的程序类型。 例如，图形密集型应用程序需要比如字处理器的应用程序更强大的计算机。 重载具有图形密集型应用程序的计算机可能会引起在整个系统的滞后问题。  
  
由应用程序访问的内容类型还会影响系统的性能。 如果多个工作站使用 web 浏览器访问多媒体内容，如全动态视频中，可以对系统性能产生负面影响之前连接更少的工作站。 相反，如果多个工作站使用 web 浏览器来访问静态 web 内容，多个工作站可连接而无需对性能显著影响。  
  
### <a name="hardware-recommendations"></a>硬件建议  
若要实现良好的性能与 MultiPoint 服务系统在各种负载情况下，使用准则如下表中时是规划和测试您的系统。 这些是 forMultiPoint 服务的基本要求。 实际配置大小调整取决于您的系统配置、 运行的，工作负荷和硬件功能。 始终应通过测试应用程序和硬件来进行验证。  
  
> [!NOTE]  
> 2 C = 2 个核心，4 C = 4 个核心，6 C = 6 个核心，MT = 多线程处理。 处理器速度应至少 2.0 千兆赫 (GHz)。  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>建议的最低运行默认 MultiPoint Server 的工作站硬件  
  
|应用程序方案|最多 5 个工作站|6-8 工作站|9-12 工作站|13-16 工作站|17-20 工作站|21 至 24 日工作站|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**工作效率**<br /><br />Office、 web 浏览、 业务线应用程序|CPU：2C<br /><br />RAM：2 GB|CPU：2C<br /><br />RAM：4 GB|CPU：4C<br /><br />RAM：6 GB|CPU：4C<br /><br />RAM：8 GB|CPU：C + MT 4 或 6 C<br /><br />RAM：10 GB| CPU：6C+MT<br /><br />RAM：12 GB|
|**混合**<br /><br />Office、 web 浏览、 业务线应用程序和偶尔的视频使用的某些用户|CPU：2C<br /><br />RAM：2 GB|CPU：2C<br /><br />RAM：4 GB|CPU：4C<br /><br />RAM：6 GB|CPU：C + MT 4 或 6 C<br /><br />RAM：8 GB|CPU：6C+MT<br /><br />RAM：10 GB| CPU：6C+MT<br /><br />RAM：12 GB| 
|**视频密集型**<br /><br />Office、 web 浏览、 业务线应用程序和所有用户的视频使用频繁**注意：** 使用本机分辨率 360p H.264 视频执行视频测试。|CPU：4C+MT<br /><br />RAM：2 GB|CPU：6C+MT<br /><br />RAM：4 GB|CPU：8C+MT<br /><br />RAM：6 GB|CPU：12C+MT<br /><br />RAM：8 GB|CPU：16C+MT<br /><br />RAM：10 GB<br /><br />-瘦客户端：RemoteFX<br />的不建议这样做 USB 视频| CPU：20 C + MT<br /><br />RAM：12 GB<br /><br />-瘦客户端：RemoteFX<br />的不建议这样做 USB 视频|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>建议的最低硬件用于运行完整的 Windows 10 虚拟桌面  
运行每个工作站的整个虚拟操作系统实例是更多计算资源密集型比正在运行的默认 MultiPoint 桌面会话，因此，每个工作站的主机硬件要求更高版本：  
  
1.  CPU：1 个核心或每个工作站的线程  
  
2.  固态硬盘 (SSD)  
  
    1.  容量 > = 20 GB，每个工作站 + 40 GB 的 WMS 承载操作系统  
  
    2.  随机读/写 IOPS > = 3 K 每工作站  
  
3.  RAM > = 每个工作站的 2GB + 2 GB 的 WMS 承载操作系统  
  
配置 BIOS CPU 设置以启用虚拟化 – 第二个二级地址转换 (SLAT)  
  
有关选择你的需求的最佳 MultiPoint 服务硬件的详细信息，请联系硬件供应商联系。  
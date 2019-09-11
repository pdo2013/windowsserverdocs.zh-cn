---
title: 影响 MultiPoint Services 系统性能的变量
description: MultiPoint Services 的性能信息
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 23bde4a65e3bf41d8968d55bf9641ca6a44b7d96
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871488"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>影响 MultiPoint Services 系统性能的变量
有很多变量会影响 MultiPoint 服务系统的整体性能。 在设计系统时，可能需要考虑这些情况。  
  
## <a name="usage"></a>用法  
  
-   **应用程序**同时运行的应用程序的类型和数量（特别是图形\-密集型或内存密集型应用程序）将影响系统的整体性能。 有关详细信息，请参阅[应用程序和 Internet 内容](hardware-and-performance-recommendations.md#applications-and-internet-content)。  
  
-   **Internet 使用**考虑用户是否将查看使用全运动视频的多媒体内容或网页。 如果有太多用户同时查看，此类型的内容可能会重载系统。  
  
    > [!NOTE]  
    > MultiPoint Services 中的投影功能，它允许教师将其屏幕投影到学生监视器上，而不是为了投影全动作视频。 投影功能旨在为演示目的而设计，例如显示过程。  
  
-   **高速设备**如果太多用户同时使用高速设备（如 web 摄像机或 DVD 播放机），这会影响系统的整体性能。  
  
## <a name="configuration"></a>配置  
  
-   **CPU、GPU 和 RAM**有关 CPU、GPU 和 RAM 的建议，请参阅本指南中的[优化 MultiPoint 服务系统性能](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance)。  
-   **网络带宽**对于通过 LAN 的连接工作站，客户端（例如瘦客户端、台式 PC 或便携式计算机）的网络带宽和功能非常重要，尤其是在用户的会话中运行视频时。 如果使用的是 USB over 以太网零客户端，则还应考虑网络带宽。 所有设备的视频数据都通过相同的以太网连接发送，因此，在使用这些设备时，可能需要考虑设置单独的千兆以太网网络。  
-   **RemoteFX**对于通过 LAN 的连接工作站，你可以使用 RemoteFX 大幅改善高清晰多媒体内容的传送。  
-   **显示分辨率**如果使用较高的全屏视频，可能需要考虑降低监视器分辨率以最大程度地提高性能。  
-   **USB 零客户端的数目**服务器上单个根集线器上 USB 零客户端的总数将直接影响视频性能。 有关详细信息，请参阅[USB 零客户端连接工作站的布局](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations)。 受支持的 USB over 以太网零客户端工作站的数量可能略小于 USB 零客户端的数量。  
-   **USB 带宽**设计系统时，请考虑 USB 带宽。  这对于 USB 零客户端（通过 USB 连接发送视频数据）尤其重要。 若要优化带宽，请将连接到服务器上的单个 USB 端口的设备数量降至最低。 这适用于菊花链式工作站和中间集线器。 有关详细信息，请参阅[工作站中心](MultiPoint-services-Site-Planning.md#station-hubs)和[中间中心](MultiPoint-services-Site-Planning.md#intermediate-hubs)。  
  
-   **USB 类型**如果将三个以上的 USB 零客户端连接到集线器，或者使用的是高带宽 USB 设备，则使用 USB 3.0 而不是 USB 2.0 会增加服务器与中间中心之间的可用带宽。  
  
-   **工作站**工作站总数会影响性能。 如果有大量的图形、处理或视频需求，可能需要限制工作站的总数。 有关详细信息，请参阅[优化 MultiPoint 服务系统性能](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance)。
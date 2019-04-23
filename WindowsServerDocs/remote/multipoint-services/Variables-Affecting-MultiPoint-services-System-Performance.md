---
title: 影响 MultiPoint Services 系统性能的变量
description: MultiPoint 服务的性能信息
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
ms.openlocfilehash: 7a06fcc763283114dc12ad106aa7ec146502dbd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867578"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>影响 MultiPoint Services 系统性能的变量
有许多变量可能会影响你的 MultiPoint 服务系统的整体性能。 您可能想要设计您的系统时考虑这些。  
  
## <a name="usage"></a>用法  
  
-   **应用程序**类型和数量的应用程序运行在同一时间，尤其是图形\-频繁或内存密集型应用程序将影响您的系统的整体性能。 有关详细信息，请参阅[应用程序和 Internet 内容](hardware-and-performance-recommendations.md#applications-and-internet-content)。  
  
-   **Internet 使用**考虑是否将查看你的用户，多媒体内容或使用全动态视频的网页。 如果过多个用户正在同时查看此类型的内容可以重载系统。  
  
    > [!NOTE]  
    > 在 MultiPoint Services 中，它允许教师其屏幕投影到其学生监视器，该投影功能不用于项目的全动态视频。 出于演示目的，例如显示过程旨在投影功能。  
  
-   **高速设备**如果太多用户同时使用高速设备，如 web 照相机或 DVD 播放机，这会影响系统的整体性能。  
  
## <a name="configuration"></a>配置  
  
-   **CPU、 GPU 和 RAM**请参阅[优化 MultiPoint Services 系统性能](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance)CPU、 GPU 和 RAM 建议为本指南中。  
-   **网络带宽**为 RDP over LAN 连接的工作站、 网络带宽和客户端 （例如，瘦客户端、 桌面 PC 或便携式计算机） 的功能很重要，尤其是在用户的会话中运行视频。 如果使用 USB-over-以太网零客户端时，网络带宽还应是一个考虑因素。 因此您可能想要使用这些设备时，请考虑设置单独的千兆位以太网网络，将通过同一个以太网连接发送的所有设备的视频数据。  
-   **RemoteFX** RDP over LAN 连接工作站，您可能能够使用 RemoteFX，从而极大地提升的高清晰度多媒体内容交付。  
-   **显示分辨率**如果有大量的全屏视频使用情况，您可能想考虑减少监视器分辨率，以提供最佳性能。  
-   **USB 零客户端数**上在服务器上的单个根集线器的 USB 零客户端的总数会直接影响视频的性能。 有关详细信息，请参阅[布局为 USB 零客户端连接工作站](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations)。 USB-over-以太网零支持的客户端工作站数可能略有小于 USB 零客户端的数量。  
-   **USB 带宽**设计您的系统时，请考虑 USB 带宽。  这是对 USB 零客户端，通过 USB 连接发送视频数据尤其重要。 若要优化带宽，最大程度减少到服务器上的单个 USB 端口连接的设备数。 这适用于菊花链工作站和中间的中心。 有关详细信息，请参阅[工作站集线器](MultiPoint-services-Site-Planning.md#station-hubs)并[中级中心](MultiPoint-services-Site-Planning.md#intermediate-hubs)。  
  
-   **USB 类型**而不是 USB 2.0 使用 USB 3.0 会增加服务器和中级中心之间的可用带宽，如果超过三个 USB 零客户端连接到中心，或者如果正在使用高带宽 USB 设备。  
  
-   **工作站**工作站的总数会影响性能。 如果有大量图形、 处理或视频的需求，你可能想要限制工作站的总数。 有关详细信息，请参阅[优化 MultiPoint 服务系统性能](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance)。
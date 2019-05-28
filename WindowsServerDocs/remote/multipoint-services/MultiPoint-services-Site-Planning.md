---
title: MultiPoint Services 站点规划
description: 规划 MultiPoint Services 部署 Windows Server 2016 中的信息
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063783cd-d748-489e-b175-46eadc993f7a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: da27467efb842368167b7a315056506e99331e8d
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034596"
---
# <a name="multipoint-services-site-planning"></a>MultiPoint Services 站点规划
应考虑将在其中部署运行 MultiPoint 服务，其关联的工作站的一个或多个计算机的位置。  
  
正在运行 MultiPoint 服务角色的计算机应具有方便访问的电源和直接连接到它，如打印机的外围设备。 此外，运行 MultiPoint 服务的计算机必须具有网络连接到方便访问。 网络连接是需要访问 Internet，并在有些地方，LAN。  
  
其他要考虑的因素包括：  
  
-   MultiPoint 服务系统将会设置在某个特定房间或它将会设置滚动购物车或表，以便它可以移动地点？  
  
    > [!NOTE]  
    > 如果你打算使用移动设置，则可以*相关联*与每次重新连接，以便确保每个键盘和鼠标与适当的监视器关联的 MultiPoint 服务工作站。  
  
-   将主站位于旁边其他工作站，或将分开？ 例如，如果在课堂中设置 MultiPoint 服务系统，将主站教师的支持和标准工作站上定位其他位置，聊天室中？ 重新启动运行 MultiPoint 服务的计算机时，主工作站将有权启动屏幕。 如果担心此教室中的访问级别，您可能更愿意将主工作站放在教师的支持。  
  
-   多少工作站可放在聊天室中？  
  
-   是否需要网络？ 使用直接视频连接或 USB 零客户端连接工作站的单服务器解决方案不需要网络。  
  
-   有足够空间来支持所需的数量的计算机运行 MultiPoint Services 中的网络连接  
  
-   电源插座位于何处？  
  
-   你将需要一个附加的显示设备，例如投影仪？ 如果你打算使用投影仪，将它挂起从上限，或将它定位在表上？  
  
-   哪些类型的电缆将是必需的并需要多少将？  
  
-   请考虑如何您可能希望在将来扩展。 将你添加多个工作站？  
  
## <a name="station-layout-and-configuration"></a>工作站布局和配置  
你的站点的物理布局可能会影响所选的工作站类型。 有关另一站类型的更多详细信息，请参阅[MultiPoint 工作站](MultiPoint-services-Stations.md)本指南中。 在单一的 MultiPoint 服务上允许多个工作站类型。 这提供了额外的灵活性，以满足您的安装要求。  
  
### <a name="layout-for-direct-video-connected-stations"></a>布局为直接视频连接工作站的  
  
-   为直接视频连接的工作站，视频电缆长度受限制的监视器和计算机之间的距离。  
  
-   使用中间中心或菊花链工作站集线器轻松的部署支持但建议的连续中心数的最大为 3。 这意味着从计算机到工作站集线器的最大距离为 15 米，因为每个 USB 2.0 电缆有五个指标的最大长度。  
  
> [!IMPORTANT]  
> 始终应至少一个直接视频连接的工作站每台计算机充当主工作站。  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>布局 USB 零客户端连接工作站  
  
-   使用中间中心或菊花链工作站集线器轻松的部署支持但建议的连续中心数的最大为 3。 这意味着从计算机到工作站集线器的最大距离为 15 米，因为每个 USB 2.0 电缆有五个指标的最大长度。  
  
-   建议的 USB 零客户端连接到单一的中级中心数的最大为 3。  
  
    > [!NOTE]  
    > 某些计算机继续往下看泛型中心主板，其中已添加的其他中心之间的影响*根集线器*的计算机和工作站集线器。  
  
-   如果将经常使用视频，建议将不超过两个 USB 零客户端连接到服务器上的 USB 端口。 例如，如果使用中级中心时，只有两个 USB 零客户端应连接到它。 或者，如果您是菊花链 USB 零客户端，只有两个 USB 零客户端应链接在一起。 添加的每个 USB 零客户端到服务器上的 USB 端口会降低视频的可用带宽。  
  
-   如果你打算将超过三个 USB 零客户端连接到服务器上的单个 USB 端口，则建议在服务器和中级中心之间使用 USB 3.0。  
  
> [!NOTE]  
> 建议使用你的应用程序验证性能和硬件，以确定多少 USB 零客户端可以连接到服务器上的 USB 端口。  
  
![下游集线器](./media/WMS_diagram6.gif)  
  
**图 5** MultiPoint 服务系统使用三个 USB 零客户端连接到单一的中级中心  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>布局 RDP over LAN 连接工作站  
时，LAN 客户端没有物理距离限制。 只要它们是在 LAN 上，它们可以连接到 MultiPoint 服务系统。  
  
## <a name="using-additional-hubs"></a>使用其他中心  
其他中心可用来简化安装。 有三种类型的 MultiPoint 服务系统使用的中心：  
  
-   [工作站集线器](#station-hubs)  
  
-   [中间的中心](#intermediate-hubs)  
  
-   [下游集线器](#downstream-hubs)  
  
### <a name="station-hubs"></a>工作站集线器  
工作站集线器是已经与 MultiPoint 服务工作站相关联的外部中心。 至少，工作站集线器将具有键盘接通到它。 它还可能具有附加的其他外围设备。 工作站集线器可以是符合的 USB 2.0 或更高版本规范的泛型 USB 集线器。 如果高性能的设备将向其插件的工作站集线器应从外部电源。  
  
**根集线器**称为是内置于计算机的主板上的主机控制器的 USB 集线器*根集线器*。 通常插入-在运行 MultiPoint 服务的计算机上的根集线器是工作站集线器。  
  
> [!NOTE]  
> 根中心不应作为工作站集线器。 内置到计算机 USB 端口时，通常不能以确定它们在内部连接到的 USB 根集线器。 在这种情况下，如果您插入入站键盘和鼠标直接向该计算机的 USB 端口，可能实际上会插入在键盘和鼠标到不同的 USB 根集线器。 来保证在键盘和鼠标不在同一个集线器，插件到计算机的 USB 端口，以及然后插件的工作站集线器的键盘和鼠标的工作站集线器。  
  
**菊花链工作站**可能会更轻松地连接到另一个工作站集线器，而不是直接与计算机的工作站集线器。 这允许你连接到工作站集线器是已插入项以在计算机的 USB 集线器，以便可以附加到另一个工作站集线器的工作站集线器。  
  
应不超过三个 USB 零客户端或工作站集线器菊花链连续。 必须格外小心，未超过 USB 带宽时菊花链工作站集线器。  
  
![雏菊式链接站点](./media/WMS_diagram5.gif)  
  
**图 6**与菊花链工作站的 MultiPoint 服务系统  
  
### <a name="intermediate-hubs"></a>中间的中心  
这样的中级中心是在服务器和工作站集线器之间的集线器。 它通常用于以增加可用于工作站集线器的端口数或从计算机中扩展的电台的距离。 建议不超过两个中间集线器工作站集线器和服务器之间使用。  
  
中间中心必须是 USB 2.0 或更高版本，并且它们必须从外部提供支持。 如果超过三个 USB 零客户端连接到的中级中心，USB 3.0 则建议在服务器和中级中心之间。  
  
### <a name="downstream-hubs"></a>下游集线器  
下游集线器连接到工作站集线器，若要添加的工作站设备的更多的可用端口。 下游集线器可以是外部供电或总线供电，具体取决于接通到中心的设备。  
  
![多个 USB 零客户端连接](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**图 7** MultiPoint 服务系统的中级中心、 工作站集线器，与下游集线器  
  
## <a name="users-stations-and-computers"></a>用户、 工作站和计算机  
你将需要的工作站数取决于的用户必须访问运行 MultiPoint 服务的计算机数。 同样，运行 MultiPoint 服务将需要的计算机数量取决于所需的工作站的总数。 直接视频连接的工作站、 零-客户端的连接了 USB 的工作站和 RDP-over-LAN 连接工作站是所有考虑的工作站。 此外，如果使用拆分屏幕功能，每个部分将被视为工作站。  
  
## <a name="power-considerations"></a>功能的注意事项  
以下组件需要访问接线板或电源插座：  
  
-   Server  
-   监视器
-   中级中心\(如果使用\) 
-   某些 USB 零客户端  
-   驱动的 USB 设备，例如某些外部存储设备和 DVD 驱动器  
  
## <a name="sample-multipoint-services-system-layouts"></a>示例 MultiPoint 服务系统布局  
根据可用家具、 聊天室的大小、 在工作室中运行 MultiPoint 服务和工作站的计算机数量，有多种方式可以排列物理工作站。 下图显示了五个可能的替代方案。  
  
> [!NOTE]  
> 某些这些关系图显示投影仪连接到 MultiPoint 服务系统。 这只是一个示例;MultiPoint 服务系统中包括投影仪是可选的。  
  
**计算机实验室**在此设置，工作站排列周围的空间，墙纸面向墙纸的学生使用。  
  
![计算机实验室教室设置](./media/WMS_ComputerLabLayout.gif)  
  
**组**在此设置中，有三个运行 MultiPoint 服务工作站聚集围绕每台计算机的计算机。  
  
![使用服务器展舱配置的教室](./media/WMS_ClassroomPods.gif)  
  
**Lecture 聊天室**在此设置，工作站设置行中。 此设置的一个优点是学生的所有人脸讲师。  
  
![配置为讲室的教室](./media/WMS_LectureRoom.gif)  
  
**活动中心**此安装程序包含办公桌，传统的讲室布局，并且它具有与运行 MultiPoint 服务及其关联工作站一台计算机单独区域。  
  
![MultiPoint 服务活动中心](./media/WMSActivityCenter.gif)  
  
**小型企业办公室**此设置中，在运行 MultiPoint 服务的计算机放置在一个中心位置和整个办公室的用户连接到该使用局域网\(LAN\)。  
  
![USB 零客户端连接工作站](./media/Diagram1.gif)
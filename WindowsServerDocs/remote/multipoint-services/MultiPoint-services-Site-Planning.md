---
title: MultiPoint Services 站点规划
description: Windows Server 2016 中的 MultiPoint Services 部署的规划信息
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
ms.openlocfilehash: 3d49b2861d81a938fb20544c3edeb0976ac6d327
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871640"
---
# <a name="multipoint-services-site-planning"></a>MultiPoint Services 站点规划
你应该考虑将在其中部署一个或多个运行 MultiPoint 服务的计算机的位置。  
  
运行 MultiPoint 服务角色的计算机应该能够轻松地访问电源和直接连接到它的外围设备（如打印机）。 此外，运行 MultiPoint 服务的计算机必须能够方便地访问网络连接。 访问 Internet 时需要网络连接，在此情况下，可以使用 LAN。  
  
要考虑的其他因素包括：  
  
-   MultiPoint 服务系统是在特定房间内设置的，还是会在滚动购物车或表上设置，以便可以将其从位置移动到位置？  
  
    > [!NOTE]  
    > 如果你计划使用移动安装，则可以在每次重新连接时将工作站与 MultiPoint 服务*关联*，以确保每个键盘和鼠标与相应的监视器相关联。  
  
-   主工作站是位于其他工作站旁边还是单独？ 例如，如果 MultiPoint 服务系统是在课堂上设置的，则主工作站位于教师的办公桌上，而标准电台位于房间内的其他位置？ 当运行 MultiPoint 服务的计算机重新启动时，主工作站将有权访问启动屏幕。 如果你关注的是课堂设置中的这一级别的访问权限，你可能更愿意将主工作站放在教师的桌面上。  
  
-   房间内容纳多少个工作站？  
  
-   是否需要网络？ 使用直接连接视频的单一服务器解决方案或 USB 零客户端连接工作站不需要网络。  
  
-   房间内是否有足够的网络连接来支持运行 MultiPoint 服务的计算机所需的数量  
  
-   电源插座位于何处？  
  
-   是否需要附加显示设备（如投影仪）？ 如果你计划使用投影仪，它是否会从天花板中挂起，或者是否将其定位在表上？  
  
-   需要哪种类型的电缆和需要多少缆线？  
  
-   考虑将来可能需要扩展的方式。 你将添加更多工作站吗？  
  
## <a name="station-layout-and-configuration"></a>工作站布局和配置  
站点的物理布局可能会影响你选择的工作站类型。 有关不同工作站类型的更多详细信息，请参阅本指南中的[MultiPoint 电台](MultiPoint-services-Stations.md)。 单个 MultiPoint 服务允许多个工作站类型。 这为你提供了额外的灵活性，可以满足你的安装需求。  
  
### <a name="layout-for-direct-video-connected-stations"></a>直接连接视频的工作站布局  
  
-   对于直接连接到视频的工作站，监视器和计算机之间的距离受视频电缆长度的限制。  
  
-   简化部署支持使用中间集线器或菊花链式工作站集线器，但建议的最大连续集线器数量为三个。 这意味着，计算机与工作站集线器之间的最大距离为15米，因为每个 USB 2.0 电缆的最大长度为5米。  
  
> [!IMPORTANT]  
> 每台计算机始终应该至少有一个直接视频连接工作站作为主站。  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>USB 零客户端连接工作站的布局  
  
-   简化部署支持使用中间集线器或菊花链式工作站集线器，但建议的最大连续集线器数量为三个。 这意味着，计算机与工作站集线器之间的最大距离为15米，因为每个 USB 2.0 电缆的最大长度为5米。  
  
-   最大推荐连接到单个中间集线器的 USB 零客户端数量为三个。  
  
    > [!NOTE]  
    > 某些计算机附带了主板上的通用集线器，这会在计算机的*根集线器*和工作站集线器之间添加其他集线器。  
  
-   如果视频使用频繁，则建议将不超过两个 USB 零客户端连接到服务器上的 USB 端口。 例如，如果使用中间集线器，则只应将两个 USB 零客户端连接到该中心。 或者，如果要以菊花链式的形式链接 USB 零客户端，则只应将两个 USB 零客户端链接在一起。 将每个 USB 零客户端添加到服务器上的 USB 端口会减少可用的视频带宽。  
  
-   如果计划将三个以上的 USB 零客户端连接到服务器上的单个 USB 端口，则建议在服务器和中间集线器之间使用 USB 3.0。  
  
> [!NOTE]  
> 建议使用应用程序和硬件来确定可以连接到服务器上 USB 端口的 USB 零客户端数量。  
  
![下游集线器](./media/WMS_diagram6.gif)  
  
**图 5**有三个 USB 零客户端连接到单个中间集线器的 MultiPoint 服务系统  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>RDP over LAN 连接工作站的布局  
LAN 客户端没有物理距离限制。 只要它们位于 LAN 上，它们就可以连接到 MultiPoint 服务系统。  
  
## <a name="using-additional-hubs"></a>使用其他中心  
可以使用其他中心来简化安装。 有三种类型的集线器用于 MultiPoint 服务系统：  
  
-   [工作站中心](#station-hubs)  
  
-   [中间集线器](#intermediate-hubs)  
  
-   [下游中心](#downstream-hubs)  
  
### <a name="station-hubs"></a>工作站中心  
工作站集线器是已与 MultiPoint 服务工作站关联的外部集线器。 至少，工作站集线器会将键盘插入到其中。 它还可能附加了其他外设。 工作站集线器可以是符合 USB 2.0 或更高版本规范的通用 USB 集线器。 如果具有高功率的设备将插件拖放到工作站集线器，则应该对外提供电源。  
  
**根集线器**内置于计算机主板上的主机控制器的 USB 集线器称为 "*根集线器*"。 通常，工作站集线器插入到运行 MultiPoint 服务的计算机上的根集线器。  
  
> [!NOTE]  
> 根集线器不应用作工作站集线器。 当 USB 端口内置于计算机上时，通常不能确定它们内部连接到的 USB 根集线器。 同样，如果将工作站键盘和鼠标直接插入计算机的 USB 端口，则实际上可能会将键盘和鼠标插入到不同的 USB 根集线器。 若要确保键盘和鼠标位于同一集线器上，请将工作站集线器插入到计算机的 USB 端口，然后将键盘和鼠标插入到该工作站集线器。  
  
**菊花链工作站**将工作站集线器连接到其他工作站集线器（而不是直接连接到计算机）可能会更容易。 这允许你将 USB 集线器连接到已插入到计算机的工作站集线器，以便将工作站集线器连接到另一台工作站集线器。  
  
不应存在三个以上的 USB 零客户端或工作站集线器菊花链式。 必须注意，在菊花链式工作站集线器之间不会超过 USB 带宽。  
  
![雏菊式链接站点](./media/WMS_diagram5.gif)  
  
**图 6**带有菊花链工作站的 MultiPoint 服务系统  
  
### <a name="intermediate-hubs"></a>中间集线器  
中间集线器是服务器和工作站集线器之间的中心。 它通常用于增加工作站集线器可用的端口数，或延长工作站与计算机的距离。 建议工作站集线器和服务器之间不会使用两个以上的中间集线器。  
  
中间集线器必须是 USB 2.0 或更高版本，并且它们必须在外部供电。 如果要将三个以上的 USB 零客户端连接到中间集线器，则建议在服务器和中间集线器之间使用 USB 3.0。  
  
### <a name="downstream-hubs"></a>下游集线器  
下游集线器连接到工作站集线器，为工作站设备添加更多可用端口。 下游集线器可以从外部供电或总线供电，具体取决于插入到中心的设备。  
  
![多个 USB 零客户端连接](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**图 7**带有中间集线器、工作站集线器和下游集线器的 MultiPoint 服务系统  
  
## <a name="users-stations-and-computers"></a>用户、工作站和计算机  
你将需要的工作站数量取决于将同时访问运行 MultiPoint 服务的计算机的人数。 同样，运行 MultiPoint 服务的计算机的数量将取决于所需的工作站总数。 直接连接到视频的工作站、连接了 USB 零的工作站以及通过 LAN 连接的工作站都被视为工作站。 此外，如果使用的是拆分屏幕功能，则每一半都被视为工作站。  
  
## <a name="power-considerations"></a>电源注意事项  
以下组件需要访问配电盘或插座：  
  
-   Server  
-   监视器
-   使用中间\(集线器\) 
-   某些 USB 零客户端  
-   USB 设备，如某些外部存储设备和 DVD 驱动器  
  
## <a name="sample-multipoint-services-system-layouts"></a>MultiPoint Services 系统布局示例  
根据可用的家具、房间大小、运行 MultiPoint 服务的计算机的数量，以及房间中的工作站，可以通过多种方式来排列物理工作站。 以下关系图说明了五种可能的替代方法。  
  
> [!NOTE]  
> 其中一些关系图显示了连接到 MultiPoint 服务系统的投影仪。 这只是一个示例;在 MultiPoint 服务系统中包含投影仪是可选的。  
  
**计算机实验室**在此设置中，工作站围绕房间的墙壁进行排列，学生朝向墙壁。  
  
![计算机实验室教室设置](./media/WMS_ComputerLabLayout.gif)  
  
**组**在此设置中，有三台运行 MultiPoint 服务的计算机，每台计算机上都有站群集。  
  
![使用服务器展舱配置的教室](./media/WMS_ClassroomPods.gif)  
  
**讲座房间**在此设置中，将在行中设置工作站。 此设置的优点是所有学生都面临讲师。  
  
![配置为讲室的教室](./media/WMS_LectureRoom.gif)  
  
**活动中心**此安装程序包含一种传统的桌面聊天系统布局，其中包含一个单独的区域，其中包含一台运行 MultiPoint 服务及其关联的工作站的单独计算机。  
  
![MultiPoint 服务活动中心](./media/WMSActivityCenter.gif)  
  
**小型企业办公室**在此设置中，运行 MultiPoint 服务的计算机位于中心位置，并且整个办公室内的用户使用\(局域网局域网\)连接到该计算机。  
  
![USB 零客户端连接工作站](./media/Diagram1.gif)
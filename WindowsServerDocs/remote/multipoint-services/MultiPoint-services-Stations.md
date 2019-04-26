---
title: MultiPoint 工作站
description: 了解在 MultiPoint Services 中，包括用户的不同选项的工作站
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9f9d618-ccfe-41ea-a52c-00c3c7adb51a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: e747826a7cd84521bc62e48abedf3092bf6d844c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855648"
---
# <a name="multipoint--stations"></a>MultiPoint 工作站
在 MultiPoint 服务系统环境中，*工作站*是用于连接到运行 MultiPoint 服务的计算机的用户终结点。 每个工作站提供独立的 Windows 10 体验的用户。 支持以下工作站类型：  
  
-   直接视频连接工作站  
  
-   USB 零-客户端的连接的工作站 （包括 USB-over-以太网零客户端）  
  
-   （对于富客户端或瘦客户端计算机） 的 RDP-over-LAN 连接工作站  
  
已安装了多点连接器的完整 Pc 还可以监视和控制使用 MultiPoint 仪表板。 Windows 10 上可以通过 Windows 功能控制面板启用多点连接器。 

Multipoint 服务支持任意组合这些工作站类型，但建议您在一个工作站是直接视频连接工作站，可将其作为主工作站。 此建议的原因是能够以应对预期的支持方案。 例如，若要与系统交互的 BIOS 之前运行 MultiPoint 服务。  
  
## <a name="primary-stations-and-standard-stations"></a>主工作站和标准工作站  
其中的一个直接视频连接的工作站指*主站*。 剩余工作站嘿 *标准工作站*。  
  
主工作站计算机开启时显示启动屏幕。 它提供了系统配置和故障排除信息在启动期间才可用。 主工作站必须直接视频连接的工作站。 后启动时，可以像任何其他 MultiPoint 工作站使用主工作站。  
  
## <a name="direct-video-connected-stations"></a>直接视频连接工作站  
运行 MultiPoint 服务的计算机可以包含多个视频卡，其中每个可以有一个或多个视频端口。 这允许您监视多个工作站直接插入到计算机。 键盘和鼠标是通过每个监视器与相关联的 USB 集线器连接。 这些中心嘿 *工作站集线器*。 其他外围设备，如扬声器、 耳机或 USB 存储设备还可以连接到工作站集线器，并仅供该工作站的用户。  
  
> [!IMPORTANT]  
> 应至少一个*直接视频连接工作站*每个服务器充当主工作站计算机处于打开状态时显示启动过程。  
  
![基于 MultiPoint Services USB 系统布局的图像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**图 1** MultiPoint 服务系统具有四个直接视频连接工作站  
  
### <a name="BKMK_PS2stations"></a>PS/2 的工作站  
使用 MultiPoint Services 中，可以将映射到直接视频连接监视器，以创建 PS/2 工作站的 ps/2 键盘和鼠标母板上。 母板上的高清晰度模拟音频是工作站的与此类型相关联的音频。 这不适用于计算机母板上有没有 PS/2 插孔。  
  
## <a name="usb-zero-client-connected-stations"></a>USB 零-客户端的连接的工作站  
利用 USB-零-客户端连接工作站*USB 零客户端*作为工作站集线器。 USB 零客户端有时称为多功能集线器视频。 它们是通过 USB 电缆，该计算机连接的集线器和这些中心通常支持视频显示器、 鼠标和键盘 （PS/2 或 USB） 音频和其他 USB 设备。 本指南将这些专用中心称为 USB 零客户端。  
  
下图显示了具有主工作站的 MultiPoint server 系统 （直接视频连接工作站） 和两个其他 USB 零客户端连接工作站。  
  
![USB 零客户端连接工作站](./media/WMS11_diagram7.gif)  
  
**图 2**与主工作站和两个 USB 零客户端连接工作站的 MultiPoint 服务系统  
  
### <a name="usb-over-ethernet-zero-clients"></a>USB-over-以太网零客户端  
USB-over-以太网零客户端是通过 LAN 将 USB 发送到 MultiPoint 服务系统的 USB 零客户端的变体。 这些类型的 USB 零客户端功能相似，其他 USB 零客户端，但不是局限于 USB 电缆长度最大值。 USB-over-以太网零客户端不是传统的瘦客户端，并且它们将显示为 MultiPoint 服务系统上的虚拟 USB 设备。 使用这些设备时，请参阅设备制造商的特定性能和规划建议的站点。 大多数设备都有 MultiPoint 管理器，您可以将相关联并将设备连接到 MultiPoint 服务系统的第三方插件。  
  
## <a name="rdp-over-lan-connected-stations"></a>RDP over LAN 连接工作站  
瘦客户端和传统台式机、 笔记本电脑或平板电脑，可以连接到使用远程桌面协议 (RDP) 或一种专有协议和远程桌面协议通过局域网 (LAN) 运行 MultiPoint 服务的计算机提供程序。 RDP 连接提供非常类似于其他任何 MultiPoint 工作站的最终用户体验，但使用本地客户端计算机的硬件。 了解有关我们远程桌面应用程序可用的 Android、 iOS、 Mac 和 Windows 中的[远程桌面客户端](../remote-desktop-services/clients/remote-desktop-clients.md)。 
  
客户端和运行 Microsoft RemoteFX 的设备可以通过利用的处理器和视频硬件功能的本地瘦客户端或计算机通过网络提供高清晰视频提供了丰富的多媒体体验。  
  
如果您有现有的 LAN 客户端 MultiPoint 服务可以提供一种在要同时升级你的所有用户对 Windows 10 体验的快速且经济高效的方法。  
  
从部署和管理的角度看，使用 RDP-over-LAN 连接工作站时，将存在以下差异：  
  
-   不限制为物理 USB 连接距离  
  
-   可能会作为工作站重用较旧计算机硬件  
  
-   更轻松地扩展到更多的工作站。 在网络上的任何客户端可能会用作远程工作站  
  
-   通过 MultiPoint 管理器控制台进行故障排除任何硬件  
  
-   分割屏幕的功能。  
  
    有关详细信息，请参阅[拆分屏幕工作站](#a-namebkmksplitscreenstationsasplit-screen-stations)本主题中更高版本  
  
-   没有工作站重命名或通过 MultiPoint 管理器控制台配置自动登录  
  
![USB 零客户端连接工作站](./media/Diagram1.gif)  
  
**图 3**与 RDP-over-LAN 连接工作站的 MultiPoint 服务系统  
  
## <a name="additional-configuration-options"></a>其他配置选项  
  
### <a name="BKMK_SplitscreenStations"></a>拆分屏幕工作站  
MultiPoint 服务提供了一个拆分屏幕选项具有直接视频连接的工作站或 USB-零-客户端连接工作站的计算机上。 分割屏幕提供了创建每个监视器的其他工作站的能力。 而不是要求两个监视器，您可以使用一个监视器具有两个工作站集线器设置使用一个监视器创建两个工作站。 而无需购买其他监视器、 USB 零客户端或视频卡，可以快速增加可用的电台的数。  
  
使用拆分屏幕工作站的好处可以包括：  
  
-   通过适应 MultiPoint 服务系统上的更多用户，从而降低成本和空间。  
  
-   允许两个用户进行协作项目并行。  
  
-   允许教师演示一个工作站上的过程，而一名学生老师其他工作站上。  
  
任何 MultiPoint 服务工作站显示器均 1024 x 768 的分辨率或更高版本可以拆分为两个工作站屏幕。 为获得最佳拆分屏幕的用户体验，建议使用最小 1600x900 解析宽屏幕。 此外，建议最小键盘而无需数字板允许以适应监视器前面的两个键盘。  
  
若要创建拆分屏幕工作站，您设置一个直接视频连接或零的客户端的连接了 USB 的工作站。 然后，通过插入键盘和鼠标连接到服务器的 USB 集线器中添加其他工作站集线器。 使用 MultiPoint 管理器拆分屏幕并将新的中心映射到该监视器的下半部分，然后可以将工作站转换为两个工作站。 在屏幕的左半部分将成为一个工作站和右半部分将成为第二个工作站。  
  
当工作站拆分后，一个用户可以登录到左侧的工作站，同时另一个用户登录到右侧的工作站。  
  
![拆分屏幕工作站](./media/WMS_diagram3.gif)  
  
**图 4**使用拆分屏幕工作站的 MultiPoint 服务系统  
  
## <a name="BKMK_StationTypeComparison"></a>工作站类型比较  
  
||直接连接的视频|USB 零客户端连接|RDP over LAN 连接|  
|-|--------------------------|-----------------------------|----------------------------|  
|视频的性能|为了获得最佳的视频性能建议||使用有关提高视频质量较低的网络带宽在支持 RemoteFX 的瘦客户端|  
|物理限制|受视频电缆长度和 USB 集线器和电缆长度 （建议 15 计量最大长度）|USB 集线器和电缆长度 （建议 15 计量最大长度） 受限制|LAN 分发受限制|  
|允许的工作站数量 |受到的母板上的可用 PCIe 槽数乘以每个视频卡的视频端口|USB 零客户端制造商可能受限于总数 （有关详细信息，请参阅此表后面的说明）。|网络交换机上的可用端口受限制|  
|分割屏幕|是|是|否|  
|MultiPoint 管理器工作站外围设备状态，自动登录配置工作站重命名|是|是|否|  
|访问服务器启动菜单|是|否|否|  
  
> [!NOTE]  
> USB 零客户端连接到服务器的总数可能受制造商或运行 MultiPoint 服务的计算机的硬件功能。
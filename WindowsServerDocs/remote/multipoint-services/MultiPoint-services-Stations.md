---
title: MultiPoint 工作站
description: 了解 MultiPoint Services 中的工作站，包括用户的不同选项
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
ms.openlocfilehash: 4aa08f58f8fdf6d6fce816ee090275b0bf46a844
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871635"
---
# <a name="multipoint--stations"></a>MultiPoint 工作站
在 MultiPoint 服务系统环境中，*工作站*是用于连接到运行 MultiPoint 服务的计算机的用户终结点。 每个工作站为用户提供独立的 Windows 10 体验。 支持以下工作站类型：  
  
-   直接连接视频的工作站  
  
-   USB-连接到客户端的工作站（包括 USB 上的零个客户端）  
  
-   通过 LAN 连接的工作站（适用于胖客户端或瘦客户端计算机）  
  
还可以使用 MultiPoint 仪表板监视和控制已安装 MultiPoint 连接器的完整电脑。 在 Windows 10 上，可以通过 Windows 的 "控制面板" 功能启用 MultiPoint 连接器。 

Multipoint Services 支持这些工作站类型的任意组合，但建议一个工作站是直接连接视频的工作站，它可以充当主站。 此建议的原因是能够预测支持方案。 例如，要在 MultiPoint 服务运行之前与系统的 BIOS 交互。  
  
## <a name="primary-stations-and-standard-stations"></a>主工作站和标准工作站  
一个直接连接到视频的工作站定义*为主工作站*。 其余的工作站称为*标准工作站*。  
  
计算机打开时，主工作站将显示启动屏幕。 它提供对系统配置和故障排除信息的访问，仅在启动过程中可用。 主站必须是直接连接视频的工作站。 启动后，可以像使用任何其他 MultiPoint 工作站一样使用主站。  
  
## <a name="direct-video-connected-stations"></a>直接连接视频的工作站  
运行 MultiPoint 服务的计算机可以包含多个视频卡，每个视频卡可以有一个或多个视频端口。 这样，你就可以将多个工作站的监视器直接插入到计算机中。 键盘和鼠标通过与每个监视器关联的 USB 集线器进行连接。 这些中心称为*站中心*。 其他外围设备（如扬声器、耳机或 USB 存储设备）也可以连接到工作站集线器，它们仅供该工作站的用户使用。  
  
> [!IMPORTANT]  
> 每台服务器上应该至少有一个*直接连接到视频的工作站*充当主站，以便在打开计算机时显示启动进程。  
  
![基于 USB 的 MultiPoint Services 系统布局的图像](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**图 1**带有四个直接连接到视频的工作站的 MultiPoint 服务系统  
  
### <a name="BKMK_PS2stations"></a>PS/2 工作站  
使用 MultiPoint 服务，可以将主板上的 PS/2 键盘和鼠标映射到直接连接视频的监视器，以创建 PS/2 工作站。 母板上的高清晰模拟音频是与此类工作站关联的音频。 这不适用于主板上没有 PS/2 插孔的计算机。  
  
## <a name="usb-zero-client-connected-stations"></a>USB-连接到客户端的工作站  
USB 零客户端连接的工作站使用*usb 零客户端*作为工作站集线器。 USB 零客户端有时称为具有视频的多功能集线器。 它们是通过 USB 电缆连接到计算机的集线器，这些中心通常支持视频监视器、鼠标和键盘（PS/2 或 USB）、音频和其他 USB 设备。 本指南将这些专门的集线器称为 USB 零客户端。  
  
下图显示了具有主站（直接视频连接工作站）的 MultiPoint server 系统，以及两个附加的 USB 零客户端连接工作站。  
  
![USB 零客户端连接的工作站](./media/WMS11_diagram7.gif)  
  
**图 2**带有主工作站和两个 USB 零客户端连接工作站的 MultiPoint 服务系统  
  
### <a name="usb-over-ethernet-zero-clients"></a>USB over 以太网零客户端  
USB over 以太网零客户端是将 USB over LAN 发送到 MultiPoint 服务系统的 USB 零客户端的变体。 这些类型的 USB 零客户端的工作方式类似于其他 USB 零客户端，但不受 USB 电缆长度最大的限制。 USB over 以太网零客户端不是传统瘦客户端，它们显示为 MultiPoint 服务系统上的虚拟 USB 设备。 使用这些设备时，请参考设备制造商，了解具体的性能和站点规划建议。 大多数设备都有一个用于 MultiPoint 管理器的第三方插件，使你能够将设备关联并连接到 MultiPoint 服务系统。  
  
## <a name="rdp-over-lan-connected-stations"></a>通过局域网的 RDP 连接工作站  
瘦客户端和传统的台式机、便携式计算机或平板电脑可以通过局域网（LAN）连接到运行 MultiPoint 服务的计算机，方法是使用远程桌面协议（RDP）或专有协议和远程桌面协议程序. RDP 连接提供与任何其他 MultiPoint 工作站非常类似的最终用户体验，但使用本地客户端计算机的硬件。 [详细了解](../remote-desktop-services/clients/remote-desktop-clients.md)适用于适用于 Android、iOS、Mac 和 Windows 的远程桌面应用程序。 
  
运行 Microsoft RemoteFX 的客户端和设备可以利用本地瘦客户端或计算机的处理器和视频硬件功能，通过网络提供高清晰度视频，从而提供丰富的多媒体体验。  
  
如果你具有现有的 LAN 客户端 MultiPoint 服务，则可以提供快速且经济高效的方式将所有用户同时升级到 Windows 10 体验。  
  
从部署和管理的角度来看，当你使用通过 LAN 连接的工作站时，存在以下差异：  
  
-   不限于物理 USB 连接距离  
  
-   可能将较旧的计算机硬件用作工作站  
  
-   更易于扩展到更多的工作站。 网络上的任何客户端都可能用作远程工作站  
  
-   无硬件通过 MultiPoint 管理器控制台进行故障排除  
  
-   无拆分屏幕功能。  
  
    有关详细信息，请参阅本主题后面的[拆分屏幕工作站](#split-screen-stations)  
  
-   无工作站通过 MultiPoint 管理器控制台重命名或配置自动登录  
  
![USB 零客户端连接工作站](./media/Diagram1.gif)  
  
**图 3**带 RDP-TCP 连接的工作站的 MultiPoint 服务系统  
  
## <a name="additional-configuration-options"></a>其他配置选项  
  
### <a name="split-screen-stations"></a>拆分屏幕工作站  
MultiPoint 服务在具有直接连接到视频的工作站或连接了 USB 零客户端的工作站的计算机上提供了一个拆分屏幕选项。 拆分屏幕提供了为每个监视器创建额外工作站的能力。 不需要两个监视器，而是可以使用一个具有两个工作站集线器设置的监视器来创建两个具有一个监视器的工作站。 可以快速增加可用工作站的数量，无需购买额外的监视器、USB 零客户端或视频卡。  
  
使用拆分屏幕工作站的优点包括：  
  
-   通过适应 MultiPoint 服务系统中的更多用户，降低成本和节省空间。  
  
-   允许两个用户在项目上并行协作。  
  
-   允许教师在一台工作站上演示某个过程，同时在其他工作站上演示。  
  
任何分辨率为1024x768 或更高的 MultiPoint 服务工作站监视器都可以拆分为两个工作站。 为获得最佳的拆分屏幕用户体验，建议使用最小1600x900 分辨率的宽屏幕。 还建议使用不带数字键盘的迷你键盘，使这两个键盘可以放置在监视器前面。  
  
若要创建拆分屏幕电台，请设置一个连接到视频的直接连接设备或连接了 USB 的连接工作站。 然后，通过将键盘和鼠标插入连接到服务器的 USB 集线器来添加其他工作站集线器。 然后，你可以使用 MultiPoint 管理器将工作站转换为两个工作站，并将新的集线器映射到监视器的一半。 屏幕的左半部分成为一个工作站，右半部分变为另一站。  
  
在拆分工作站后，一个用户可以登录到左侧工作站，另一个用户可以登录到正确的工作站。  
  
![拆分屏幕工作站](./media/WMS_diagram3.gif)  
  
**图 4**带拆分屏幕的 MultiPoint 服务系统  
  
## <a name="BKMK_StationTypeComparison"></a>工作站类型比较  
  
||直接视频连接|USB 零客户端连接|已连接 RDP-TCP over LAN|  
|-|--------------------------|-----------------------------|----------------------------|  
|视频性能|建议为获得最佳视频性能||使用支持 RemoteFX 的瘦客户端，以更低的网络带宽提高视频质量|  
|物理限制|受视频电缆长度和 USB 集线器和电缆长度的限制（建议15米最大长度）|受 USB 集线器和电缆长度限制（建议使用15米最大长度）|受 LAN 分发限制|  
|允许的工作站数量 |受主板上每个视频端口视频卡的可用 PCIe 槽数的限制|总数可能受 USB 零客户端制造商限制（有关详细信息，请参阅此表后面的说明。）|受网络交换机上的可用端口限制|  
|拆分屏幕|是|是|否|  
|MultiPoint 管理器工作站外设状态，自动登录配置，工作站重命名|是|是|否|  
|访问服务器启动菜单|是|否|否|  
  
> [!NOTE]  
> 连接到服务器的 USB 零客户端的总数可能受运行 MultiPoint 服务的计算机的制造商或硬件功能的限制。

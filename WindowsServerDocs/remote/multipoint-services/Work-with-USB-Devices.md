---
title: 使用 USB 设备
description: 了解 USB 设备如何使用 MultiPoint 服务
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a33f2b83-bbc2-4fc1-8a94-aaa985dfe1f9
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: ce4338eccc5640f8743093649685054718f9ed2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394784"
---
# <a name="work-with-usb-devices"></a>使用 USB 设备
你可以将设备连接到 MultiPoint 服务系统中的计算机或连接到 MultiPoint 工作站集线器。 设备连接到的位置和设备的类型会影响设备是对系统上的所有用户可用，还是仅对单个用户可用，还是对任何用户都不可用。 不同连接类型的示例如下：  
  
-   如果将设备（例如打印机或 USB 大容量存储设备）直接连接到计算机，则 MultiPoint 服务系统上的所有会话用户都可以访问该设备。 虚拟桌面工作站用户将无法访问直接连接到计算机的设备。  
  
-   如果将设备（例如键盘、鼠标、音频设备或大容量存储设备）连接到工作站集线器，则该设备仅对登录到该 MultiPoint 服务工作站的用户可用。  
  
-   如果将特定类型的设备（例如键盘或鼠标）连接到计算机，则此设备对系统上的任何用户都不可用。  
  
下表显示了设备列表以及这些设备的行为方式，具体取决于他们连接到系统的位置。 有关如何连接工作站集线器的信息，请参阅使用[工作站集线器](#working-with-station-hubs)。 有关如何将视频监视器连接到工作站的详细信息，请参阅[使用视频设备](Work-with-Video-Devices.md)。  
  
|||||  
|-|-|-|-|  
|**设备**|**直接连接到计算机时的行为**|**连接到工作站时的行为**|**说明**|  
|键盘|我们不建议将键盘直接连接到计算机。|仅允许工作站用户访问。|如果键盘包含 USB 端口，则键盘内部的 USB 集线器可能是工作站集线器。 附加到该端口的其他 USB 设备仅对使用该键盘的用户可用。<br /><br />有些工作站集线器配备有 PS @ no__t-02 鼠标端口，该端口转换为集线器内的 USB 连接。|  
|鼠标|我们不建议将鼠标直接连接到计算机。|仅允许工作站用户访问。|有些工作站集线器配备有 PS @ no__t-02 鼠标端口，该端口转换为集线器内的 USB 连接。|  
|USB 集线器|请参阅使用[工作站中心](#working-with-station-hubs)。|请参阅使用[工作站中心](#working-with-station-hubs)。||  
|视频显示器|请参阅[MultiPoint Services 视频设备](work-with-video-devices.md)。|请参阅[MultiPoint Services 视频设备](work-with-video-devices.md)。||  
|音频输出设备（例如耳机）|我们不建议将音频输出设备直接连接到计算机。|仅允许工作站用户访问。|有些工作站集线器配备有模拟音频端口，该端口在集线器内部转换为 USB 音频连接。|  
|音频输入设备（例如麦克风）|我们不建议将音频输入设备直接连接到计算机。|仅允许工作站用户访问。|有些工作站集线器配备有模拟音频端口，该端口在集线器内部转换为 USB 音频连接。|  
|打印机|可供系统上的所有用户访问 *|仅允许工作站用户访问。||  
|USB 大容量存储设备|可供系统上的所有用户访问。 \*|仅允许工作站用户访问。|这些设备包括 USB 闪存驱动器、外部硬盘驱动器和数码相机。|  
|网络摄像机|可供系统上的所有用户访问 *|仅允许工作站用户访问。|一次只能有一位用户可以连接到摄像机。|  
  
\* 连接到主机计算机的设备对登录到虚拟桌面工作站的用户不可见。  
  
有关如何设置工作站的详细信息，请参阅[设置工作站](Set-Up-a-Station.md)。  
  
### <a name="working-with-station-hubs"></a>使用工作站集线器  
将 USB 集线器连接到 MultiPoint Server 系统时，有关如何使用 USB 集线器有四种解决方案。 以下每种解决方案对连接到集线器的设备分别提供不同的访问方式，具体取决于集线器的类型及其连接到系统的位置。  
  
-   连接到 MultiPoint 服务系统中的计算机，并且有附加键盘的工作站集线器可以用来创建 MultiPoint 服务工作站。 连接到工作站集线器的键盘和鼠标使用的是集线器上的可用端口。 视频显示器将连接到计算机上的视频端口或连接到工作站集线器上的视频适配器（如果可用）。 之后，键盘、鼠标和显示器就与 MultiPoint 服务工作站相关联。  
  
-   连接到 MultiPoint 服务系统中的计算机，并且没有附加键盘的 USB 集线器不能为所需设备提供足够的计算机端口时，可以用来将其他设备连接到计算机。 连接到此 USB 集线器的所有设备对 MultiPoint 服务系统的所有用户可用。 但不能将其视为 MultiPoint 服务工作站集线器。  
  
-   连接到 MultiPoint 服务系统中的计算机，用且拥有电源的 USB 集线器（也称为中间集线器）可以用来连接其他用于创建 MultiPoint 工作站的 USB 集线器。  
  
-   连接到工作站集线器的 USB 集线器可用于将其他设备连接到该工作站集线器。 必须将键盘直接连接到该工作站集线器。  
  
有关如何设置 MultiPoint 服务工作站的详细信息，请参阅[设置工作站](Set-Up-a-Station.md)。  
  
## <a name="see-also"></a>请参阅  
[使用视频设备](Work-with-Video-Devices.md)  
[管理工作站硬件](Manage-Station-Hardware.md)  
[设置工作站](Set-Up-a-Station.md)
---
ms.assetid: ''
title: Windows Server 2019 中 Windows 时间服务功能的内部预览
description: Windows Server 2019 中的新 Windows 时间服务功能
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 38682afa37a4c6882ee2e63a4abf4cd9fdbd2b27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405220"
---
# <a name="insider-preview"></a>Insider preview 


## <a name="leap-second-support"></a>Leap 第二次支持


>适用于：Windows Server 2019 和 Windows 10，版本1809

Leap 为 UTC 进行了偶尔的1秒调整。 随着地球的旋转速度慢， [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) （原子时间刻度）与其分离[平均太阳时间](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time)或天文时间。  在最多 .9 秒分离 UTC 后，将插入[leap](https://en.wikipedia.org/wiki/Leap_second) ，以保持 UTC 同步，并使用平均阳历时间。

Leap 在美国和欧盟都在满足准确性和跟踪性法规要求方面已变得非常重要。

有关详细信息，请参阅：

-  我们的[公告博客](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  面向[开发人员](https://aka.ms/Dev-LeapSecond)的验证指南

-  适用于[IT 专业人员](https://aka.ms/ITPro-LeapSecond)的验证指南


## <a name="precision-time-protocol"></a>精度时间协议

>适用于：Windows Server 2019 和 Windows 10，版本1809

Windows Server 2019 和 Windows 10 （版本1809）中包含的新的时间提供程序允许你使用精度时间协议（PTP）来同步时间。 随着网络间的时间分布，它会遇到延迟（延迟）（如果没有考虑），或者如果不是对称的，则很难理解从时间服务器发送的时间戳。 PTP 使网络设备能够将每个网络设备引入的延迟添加到计时度量，从而为 windows 客户端提供更准确的时间示例。

有关详细信息，请参阅：

-  我们的[公告博客](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  适用于[IT 专业人员](https://aka.ms/PTPValidation)的验证指南


## <a name="software-timestamping"></a>软件时间戳

>适用于：Windows Server 2019 和 Windows 10，版本1809

当通过网络从时间服务器接收计时数据包时，它必须由操作系统的网络堆栈处理，然后才能在时间服务中使用。 网络堆栈中的每个组件会引入影响计时度量准确性的可变滞后时间。

![软件时间戳](../media/Windows-Time-Service/software-timestamping.png)

为了解决此问题，软件时间戳使我们可以在上面所示的 "Windows 网络组件" 之前和之后的时间戳数据包，以考虑操作系统中的延迟。

有关详细信息，请参阅：

-  我们的[公告博客](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  适用于[IT 专业人员](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)的验证指南



---
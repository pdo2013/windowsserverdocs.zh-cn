---
ms.assetid: ''
title: 深入了解适用于 Windows 时间服务功能在 Windows Server 2019 预览版
description: Windows Server 2019 中的新 Windows 时间服务功能
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ef0ff317f5957add5ecbe9f88ef83753b805ec41
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829018"
---
# <a name="insider-preview"></a>Insider preview 


## <a name="leap-second-support"></a>闰秒支持


>适用于：Windows Server 2019 和 Windows 10，版本 1809

闰秒是偶尔 1 秒调整为 UTC。 因为地球旋转减慢， [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) （原子时间刻度） 有何区别[意味着太阳能时间](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time)或天文时间。  UTC 已分离最.9 秒，一旦[闰秒](https://en.wikipedia.org/wiki/Leap_second)插入以 UTC 中保持同步的 mean 太阳能时间。

闰秒已变得重要以满足的准确性和可跟踪性法规要求在美国和欧洲联盟中。

有关详细信息，请参阅：

-  我们[公告博客](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  验证指南[开发人员](https://aka.ms/Dev-LeapSecond)

-  验证指南[IT 专业人员](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>精度时间协议

>适用于：Windows Server 2019 和 Windows 10，版本 1809

Windows Server 2019 和 Windows 10 （版本 1809年） 中包含新的时间提供程序，可使用精度时间协议 (PTP) 的时间同步。 如通过网络分发时，遇到的延迟 （延迟），如果不考虑在内，或如果不是对称的它变得越来越困难，若要了解从时间服务器发送时间戳。 PTP 使得网络设备添加到的计时度量值，从而提供更准确的时间示例 windows 客户端的每个网络设备造成的延迟。

有关详细信息，请参阅：

-  我们[公告博客](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  验证指南[IT 专业人员](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>软件加盖时间戳

>适用于：Windows Server 2019 和 Windows 10，版本 1809

在接收时的计时数据包通过网络从时间服务器，它之前必须先处理由操作系统的网络堆栈中的时间服务已使用。 每个组件在网络堆栈中的引入了大小可变的延迟影响的计时度量准确性。

![软件加盖时间戳](../media/Windows-Time-Service/software-timestamping.png)

若要解决此问题，软件加盖时间戳，我们的时间戳数据包之前和之后"Windows 网络组件"的操作系统中的延迟如上所示。

有关详细信息，请参阅：

-  我们[公告博客](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  验证指南[IT 专业人员](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---
---
title: 远程桌面连接期间出现性能低下情况或应用程序问题
description: 排查远程桌面连接期间出现的性能低下情况或应用程序问题。
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 63e28cba6669e6a6bb604fbfd19650fb461fe575
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529907"
---
# <a name="poor-performance-or-application-problems-during-remote-desktop-connection"></a>远程桌面连接期间出现性能低下情况或应用程序问题

本文解决了用户在使用远程桌面功能时可能会遇到的多个常见问题。

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>使用 Microsoft Azure 新虚拟机时出现的间歇性问题

此问题影响最近已预配的虚拟机。 在用户连接到虚拟机后，远程桌面会话无法正确加载该用户的所有设置。

若要解决此问题，请从虚拟机断开连接，等待至少 20 分钟，然后再次连接。

若要解决此问题，请适当地将以下更新应用到虚拟机：

  - Windows 10 和 Windows Server 2016：KB 4343884 [2018 年 8 月 30 日 - KB4343884（OS 内部版本 14393.2457）](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2：KB 4343891 [2018 年 8 月 30 日 - KB4343891（月度汇总预览）](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Windows 10 版本 1709 上的视频播放问题

当用户连接到运行 Windows 10 版本 1709 的远程计算机时，会出现此问题。 当这些用户使用 VMR9（视频混合渲染器 9）编解码器播放视频时，播放器仅仅显示了一个黑屏。

这是 Windows 10 版本 1709 中的一个已知问题。 Windows 10 版本 1703 中不会出现此问题。

### <a name="desktop-sharing-issues-on-windows-10"></a>Windows 10 上的桌面共享问题

如果用户具有只读的用户配置文件和关联的注册表配置单元（例如在网亭方案中），则会出现此问题。 当此类用户连接到运行 Windows 10 版本 1803 的远程计算机时，他们无法共享其桌面。

若要解决此问题，请应用 Windows 10 更新 4340917 [2018 年 7 月 24 日 - KB4340917（OS 内部版本 17134.191）](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917)。

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>在禁用 NLA 的情况下混合 Windows 10 版本时的性能问题

在禁用 NLA 的情况下，如果运行 Windows 10 的远程桌面客户端计算机连接到运行不同 Windows 10 版本的远程桌面，则会出现此问题。 运行 Windows 10 版本 1709 或更低版本的计算机上的远程桌面客户端用户在连接到运行 Windows 10 版本 1803 或更高版本的远程桌面时，会遇到性能不佳的问题。

此问题的原因是，当禁用了 NLA 时，旧式客户端计算机在连接到 Windows 10 版本 1803 或更高版本时，会使用速度较慢的协议。

若要解决此问题，请应用 KB 4340917 [2018 年 7 月 24 日 - KB4340917（OS 内部版本 17134.191）](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917)。

### <a name="black-screen-issue"></a>黑屏问题

此问题出现在 Windows 8.0、Windows 8.1、Windows 10 RTM 和 Windows Server 2012 R2 中。 用户在远程桌面中启动多个应用程序，然后从会话断开连接。 用户定期重新连接到远程桌面与应用程序交互，然后再次断开连接。 当用户在某个时间重新连接时，远程桌面会话仅仅显示一个黑屏。 若要再次正确显示此会话，用户必须从远程计算机的控制台或 RDSH 服务器控制台终止其会话，然后停止其会话的应用程序。

若要解决此问题，请适当地应用以下更新：

  - Windows 8 和 Windows Server 2012：KB4103719 [2018 年 5 月 17 日 - KB4103719（月度汇总预览）](https://support.microsoft.com/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 和 Windows Server 2012 R2：KB4103724 [2018 年 5 月 17 日 - KB4103724（月度汇总预览）](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724)和 KB 4284863 [2018 年 6 月 21 日 - KB4284863（月度汇总预览）](https://support.microsoft.com/help/4284863/windows-81-update-kb4284863)
  - Windows 10：已在 KB4284860 [2018 年 6 月 12 日 - KB4284860（OS 内部版本 10240.17889）](https://support.microsoft.com/help/4284860/windows-10-update-kb4284860)中予以修复

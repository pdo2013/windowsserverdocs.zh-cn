---
title: 远程桌面客户端 URI 方案
description: 了解有关统一资源标识符方案为远程桌面客户端
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/11/2018
ms.localizationpriority: medium
ms.openlocfilehash: f2934fed43c8f4feec2f321d684cc3593933eb5d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297466"
---
# 远程桌面客户端通用资源标识符 (URI) 方案支持

>适用于： Windows Server 版本 1803、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2

启用统一资源标识符 (URI) 方案为 IT 专业人员和开发人员提供跨平台集成远程桌面客户端的功能的方法，并通过允许丰富的用户体验： 

- 若要启动 Microsoft 远程桌面，并开始使用预定义的设置 （提供的 URI 字符串的一部分） 的远程会话的第三方应用程序。
- 若要开始使用预配置的 Url 的远程连接的最终用户。

>[!NOTE]
> 对于 Windows 操作系统不支持使用 URI 连接到远程桌面客户端-使用 MacOS、 iOS 和 Android 设备的 URI。

Microsoft 远程桌面使用 URI 方案 rdp://query_string 存储启动客户端时使用的预配置的属性设置。 查询字符串表示单个或 RDP 提供的属性的 URL 中的设置。 

RDP 属性均由连字符 (&) 分隔。 例如，连接到电脑时，该字符串将是：

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

此表提供支持可能与 iOS、 Mac、 和 Android 的远程桌面客户端使用的属性的完整列表。 （"x"中的平台列指示支持的属性。 由 v 形 (<>) 表示的值表示的远程桌面客户端支持的值）。

| **RDP 属性**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| 允许桌面元素 = i:&lt;0 或 1&gt;                    | x       | x   | x   |
| 允许字体平滑 = i:<0 或 1&gt;                         | x       | x   | x   |
| 备用 shell = s:&lt;字符串&gt;                              | x       | x   | x   |
| [audiomode = i:&lt;0、 1 或 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [身份验证级别 = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| 连接到控制台 = i:&lt;0 或 1&gt;                           | x       | x   | x   |
| 禁用光标设置 = i:&lt;0 或 1&gt;                      | x       | x   | x   |
| 禁用全屏拖 = i:&lt;0 或 1&gt;                     | x       | x   | x   |
| 禁用菜单 anims = i:&lt;0 或 1&gt;                           | x       | x   | x   |
| 禁用主题 = i:&lt;0 或 1&gt;                               | x       | x   | x   |
| 禁用墙纸 = i:&lt;0 或 1&gt;                            | x       | x   | x   |
| [drivestoredirect = s: *](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx)（这是唯一受支持的值） | x       | x   |     |
| [desktopheight = i:&lt;以像素为单位的值&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth = i:&lt;以像素为单位的值&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [域 = s:&lt;字符串&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [完整的地址 = s:&lt;字符串&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname = s:&lt;字符串&gt;                  | x | x | x |
| [gatewayusagemethod = i:&lt;1 或 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [客户端上的凭据提示 = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo = s:&lt;字符串&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline = s:&lt;字符串&gt;         | x | x | x |
| remoteapplicationmode = i:&lt;0 或 1&gt;            | x | x | x |
| remoteapplicationprogram = s:&lt;字符串&gt;         | x | x | x |
| shell 工作目录 = s:&lt;字符串&gt;          | x | x | x |
| 使用重定向服务器名称 = i:&lt;0 或 1&gt;      | x | x | x |
| [用户名 = s:&lt;字符串&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [屏幕模式 id = i:&lt;1 或 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [会话 bpp = i:&lt;8、 15、 16、 24 或 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [使用 multimon = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |
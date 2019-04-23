---
title: 远程桌面客户端 URI 方案
description: 了解远程桌面客户端的统一资源标识符方案
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
ms.openlocfilehash: 86de6468e2fa45c976711aef43a1a274e04498d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870498"
---
# <a name="remote-desktop-client-universal-resource-identifier-uri-scheme-support"></a>远程桌面客户端通用资源标识符 (URI) 方案支持

>适用于：Windows Server 版本 1803，Windows Server 2016 中，Windows Server 2012 R2

通过启用统一资源标识符 (URI) 方案，IT 专业人员和开发人员可以跨平台集成远程桌面客户端的功能，并通过进行允许操作来丰富用户体验： 

- 第三方应用程序启动 Microsoft 远程桌面，并使用预定义设置（作为 URI 字符串的一部分提供）启动远程会话。
- 最终用户使用预配置 URL 启动远程连接。

>[!NOTE]
> 对于 Windows 操作系统不支持使用 URI 来连接到远程桌面客户端-使用 MacOS、 iOS 和 Android 设备使用的 URI。

Microsoft 远程桌面使用 URI 方案 rdp: //query_string 来存储启动客户端时使用的预配置的属性设置。 查询字符串表示 URL 中提供的单个实例或一组 RDP 特性。 

RDP 属性用与号 (&) 分隔。 例如，连接到 PC 时，该字符串是：

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

此表提供可以与 iOS、Mac 和 Android 远程桌面客户端一起使用的受支持特性的完整列表。 （平台列中的“x”指示支持特性。 通过尖括号 (<>) 指示的值表示远程桌面客户端支持的值。）

| **RDP 特性**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| 允许桌面组合 = i:&lt;0 或 1&gt;                    | x       | x   | x   |
| 允许字体平滑显示 = i: < 0 或 1&gt;                         | x       | x   | x   |
| 备用 shell = s:&lt;字符串&gt;                              | x       | x   | x   |
| [audiomode = i:&lt;0、 1 或 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [身份验证级别 = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| 连接到控制台 = i:&lt;0 或 1&gt;                           | x       | x   | x   |
| disable cursor = i:&lt;0 或 1&gt;                      | x       | x   | x   |
| 禁用全窗口拖动 = i:&lt;0 或 1&gt;                     | x       | x   | x   |
| 禁用菜单 anims = i:&lt;0 或 1&gt;                           | x       | x   | x   |
| 禁用主题 = i:&lt;0 或 1&gt;                               | x       | x   | x   |
| 禁用墙纸 = i:&lt;0 或 1&gt;                            | x       | x   | x   |
| [drivestoredirect=s:*](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) （这是唯一受支持的值） | x       | x   |     |
| [desktopheight = i:&lt;以像素为单位的值&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth = i:&lt;以像素为单位的值&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [域 = s:&lt;字符串&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [完整地址 = s:&lt;字符串&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname = s:&lt;字符串&gt;                  | x | x | x |
| [gatewayusagemethod = i:&lt;1 或 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [客户端上的凭据提示 = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo = s:&lt;字符串&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline=s:&lt;string&gt;         | x | x | x |
| remoteapplicationmode = i:&lt;0 或 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;string&gt;         | x | x | x |
| shell 工作目录 = s:&lt;字符串&gt;          | x | x | x |
| 使用重定向服务器名称 = i:&lt;0 或 1&gt;      | x | x | x |
| [用户名 = s:&lt;字符串&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [屏幕模式 id = i:&lt;1 或 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [session = i:&lt;8、 15、 16、 24、 或 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [使用多监视器 = i:&lt;0 或 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |
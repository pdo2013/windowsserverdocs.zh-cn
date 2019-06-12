---
title: 远程桌面-比较客户端应用程序
description: 了解不同的远程桌面应用程序时涉及到支持的功能和函数的比较。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0e001b590f524711185e3dd70db3bc52a9b8d9af
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447122"
---
# <a name="compare-the-client-apps"></a>比较客户端应用程序

>适用于：Windows 10 中，Windows 8.1、 Windows Server 2019、 Windows Server 2016 中，Windows Server 2012 R2

我们经常被问及不同的远程桌面客户端应用程序如何与其他进行比较。 它们都执行相同的操作执行操作？ 以下是这些问题的答案。

## <a name="redirection-support"></a>重定向支持

下表比较对设备和其他远程桌面连接应用程序、 通用应用程序、 Android 应用程序、 iOS 应用程序、 macOS 应用和 web 客户端上的重定向的支持。 这些表涉及可以访问一次在远程会话中的重定向。 

如果你远程连接到您的个人桌面，有其他可以在中配置的重定向**其他设置**会话。 如果你的远程桌面或应用程序由你的组织管理，你的管理员可以启用或禁用通过组策略设置的重定向。

### <a name="input-redirection"></a>输入重定向

| 重定向 | 远程桌面<br> 连接 | 通用 | Android | iOS | macOS |          web 客户端           |
|-------------|-------------------------------|-----------|---------|-----|-------|-------------------------------|
|  键盘   |               X               |     X     |    X    |  X  |   X   |               X               |
|    鼠标    |               X               |     X     |    X    | X\* |   X   |               X               |
|    触控    |               X               |     X     |    X    |  X  |       | X （Edge 和 IE 不使用支持） |
|    其他    |              笔              |           |         |     |       |                               |

* 查看[的远程桌面 iOS Beta 客户端支持的输入设备的列表](remote-desktop-ios.md#supported-input-devices)。

### <a name="port-redirection"></a>端口重定向   

| 重定向 | 远程桌面 <br>连接 | 通用 | Android | iOS | macOS | web 客户端 |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| 串行端口 | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

启用 USB 端口重定向时，会自动在远程会话中识别连接到 USB 端口的任何 USB 设备。

### <a name="other-redirection-devices-etc"></a>其他重定向 （设备等）



| 重定向         | 远程桌面连接 | 通用   | Android | iOS         | macOS                                    | web 客户端    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| 相机             | X                         |             |         |             |                                          |               |
| 剪贴板           | X                         | 文本、 图像 | text    | 文本、 图像 | X                                        | text          |
| 本地驱动器/存储 | X                         |             | X       |             | x                                        |               |
| Location            | X                         |             |         |             |                                          |               |
| 麦克风         | X                         |X            |         |             | X                                        |               |
| 打印机            | X                         |             |         |             | X (仅 CUP)                            | PDF 打印     |
| 扫描仪            | X                         |             |         |             |                                          |               |
| 智能卡         | X                         |             |         |             | X （不受支持的 Windows 身份验证） |               |
| 扬声器            | X                         | X           | X       | X           | X                                        | X （除 IE) |

* 对于打印机重定向-macOS 应用默认情况下支持发布者照排机打印机驱动程序。 它们不支持重定向的本机打印机驱动程序。

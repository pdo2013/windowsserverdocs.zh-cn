---
title: 远程桌面 - 比较客户端应用
description: 了解不同 RD 应用在支持的特性和功能方面的比较。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 237bb79fae6460bc3b31fb1753e2d679c8d67512
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404167"
---
# <a name="compare-the-client-apps"></a>比较客户端应用

>适用于：Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

我们经常被问及不同远程桌面客户端应用之间的比较。 它们是否都执行相同的操作？ 以下是这些问题的答案。

## <a name="redirection-support"></a>重定向支持

下表比较了在远程桌面连接应用、通用应用、Android 应用、iOS 应用、macOS 应用和 Web 客户端上对设备和其他重定向的支持。 这些表涵盖了可以在远程会话中访问一次的重定向。 

如果远程连接到个人桌面，可在会话的“其他设置”  中配置其他重定向。 如果远程桌面或应用由组织管理，则管理员可以通过“组策略”设置启用或禁用重定向。

### <a name="input-redirection"></a>输入重定向

| 重定向 | 远程桌面<br> 连接 | 通用 | Android | iOS | macOS |          Web 客户端           |
|-------------|-------------------------------|-----------|---------|-----|-------|-------------------------------|
|  键盘   |               X               |     X     |    X    |  X  |   X   |               X               |
|    鼠标    |               X               |     X     |    X    | X\* |   X   |               X               |
|    触控    |               X               |     X     |    X    |  X  |       | X（不支持 Edge 和 IE） |
|    其他    |              笔              |           |         |     |       |                               |

*请查看[远程桌面 iOS Beta 客户端的受支持的输入设备列表](remote-desktop-ios.md#supported-input-devices)。

### <a name="port-redirection"></a>端口重定向   

| 重定向 | 远程桌面 <br>连接 | 通用 | Android | iOS | macOS | Web 客户端 |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| 串行端口 | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

启用 USB 端口重定向时，会自动在远程会话中识别连接到 USB 端口的任何 USB 设备。

### <a name="other-redirection-devices-etc"></a>其他重定向（设备等）



| 重定向         | 远程桌面连接 | 通用   | Android | iOS         | macOS                                    | Web 客户端    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| 相机             | X                         |             |         |             |                                          |               |
| 剪贴板           | X                         | 文本、映像 | 文本    | 文本、映像 | X                                        | 文本          |
| 本地驱动器/存储 | X                         |             | X       |             | x                                        |               |
| 位置            | X                         |             |         |             |                                          |               |
| 麦克风         | X                         |X            |         |             | X                                        |               |
| 打印机            | X                         |             |         |             | X（仅 CUPS）                            | PDF 打印     |
| 扫描仪            | X                         |             |         |             |                                          |               |
| 智能卡         | X                         |             |         |             | X（不支持 Windows 身份验证） |               |
| 扬声器            | X                         | X           | X       | X           | X                                        | X（IE 除外） |

*对于打印机重定向 - macOS 应用默认情况下支持 Publisher Imagesetter 打印机驱动程序。 它们不支持重定向本机打印机驱动程序。

---
title: Windows Server 半年频道中对 Nano Server 所做的更改
description: 在全新的 Windows Server servicing 服务模型中，Nano Server 仅作为容器操作系统，且某个功能经过了修改。
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 05/21/2019
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: c9fede02b90e285803a8bcdbc983f264d65a4589
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976506"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Windows Server 半年频道中对 Nano Server 所做的更改

>适用于：Windows Server 半年频道

如果您已在运行 Nano Server[窗口 Server 半年频道](..\get-started-19\servicing-channels-19.md)服务模型是熟悉的因为它以前称为服务的 Current Branch for Business (CBB) 模型。 Windows Server 半年频道是相同的模型的新名称。 在此模型中，Nano Server 的功能更新发布每年会有两到三次。

但是，从 Windows Server 版本 1803，Nano Server 是仅作为**容器基本操作系统映像**。 必须将其作为容器主机中的容器来运行，如 Windows Server 的 Server Core 安装。 在此版本中基于 Nano Server 运行容器与在旧版本中运行的区别如下：

- Nano Server 已经面向 .NET Core 应用程序进行了优化。
- Nano Server 的大小甚至小于 Windows Server 2016 版本。
- 默认情况下，不再包含 PowerShell Core、.NET Core 和 WMI，但在生成容器时，可以包含 [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) 和 [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) 容器数据包。
- Nano Server 不再包含服务堆栈。 Microsoft 将更新的 Nano 容器发布到你重新部署的 Docker 中心。
- 可以通过使用 Docker 对新的 Nano 容器进行故障排查。
- 现在，可以在 IoT 核心版上运行 Nano 容器。

## <a name="related-topics"></a>相关主题

- [Windows 容器文档](http://aka.ms/windowscontainers)
- [窗口 Server 半年频道概述](..\get-started-19\servicing-channels-19.md)

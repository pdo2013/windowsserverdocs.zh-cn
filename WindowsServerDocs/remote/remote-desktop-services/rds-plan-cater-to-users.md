---
title: 远程桌面服务 - 适应不同类型的用户
description: 介绍 RDS 的不同类型的用户。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da522a18-c33f-468e-b9d6-3ad7d3cfba26
author: spatnaik
ms.author: spatnaik
ms.date: 09/23/2016
manager: scottman
ms.openlocfilehash: c04909e9e0cfbf71b6632c154ac8b9b20b5bac10
ms.sourcegitcommit: b4b0e73ce35f8b594eb467a2bb2aa91bd6d86369
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72893080"
---
# <a name="remote-desktop-services---cater-to-different-kinds-of-users"></a>远程桌面服务 - 适应不同类型的用户

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

远程桌面服务支持不同类型的工作负载。 根据每类用户的预期需求缩放部署。

## <a name="types-of-users"></a>用户类型

### <a name="knowledge-user"></a>知识用户

知识用户使用 Microsoft Word、Excel、Outlook 和 Microsoft Edge 浏览器等轻量级生产力应用程序。

### <a name="professional-user"></a>专业用户

除了支持软件开发和多媒体内容创建等操作更频繁的工作负载，专业用户还使用 Internet 浏览器和生产力应用程序。

### <a name="power-user"></a>超级用户

超级用户使用计算机辅助设计 (CAD) 和 Adobe Photoshop 等工程和图形应用程序。 对于经常使用常用图形的程序进行视频呈现、3D 设计和模拟的用户来说，GPU 通常是一个不错的选择。

要详细了解图形加速，请查看[选择你的图形呈现技术](rds-graphics-virtualization.md)。

Azure 具有其他图形加速部署选项和多种可用的 GPU VM 大小。 相关信息，请查看 [GPU 优化虚拟机大小](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu)。

## <a name="test-workload"></a>测试工作负载

建议同时使用压力测试和实际使用情况模拟来加载测试部署。 可使用 LoginVSI 等模拟工具来加载测试你的部署。 请确保系统具有足以满足用户需求的响应能力和复原能力，并记得更改负载大小以避免意外情况。

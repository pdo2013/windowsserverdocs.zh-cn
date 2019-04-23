---
title: 安装 RDS 客户端访问许可证
description: 了解如何安装远程桌面客户端的 Cal。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 2f283b51acc869704a52f09bebc228660cdfbc38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870568"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>远程桌面许可证服务器上安装 RDS 客户端访问许可证

>适用于：Windows 服务器 （半年频道），Windows Server 2016

使用以下信息在许可证服务器上安装远程桌面服务客户端访问许可证 (Cal)。 Cal 安装后，许可证服务器将发出它们向适当的用户。

请注意需要计算机运行远程桌面授权管理器，但不是运行的计算机上的许可证服务器上的 Internet 连接。

1. 在许可证服务器 （通常第一个 RD 连接代理） 上，打开远程桌面授权管理器。
2. 右键单击许可证服务器，然后依次**安装许可证**。
3. 单击**下一步**的欢迎页上。
4. 选择购买 RDS Cal，从该程序，然后单击**下一步**。 如果你的服务提供程序，请选择**服务提供商许可协议**。
5. 输入您的许可证计划的信息。 在大多数情况下，这将是许可证代码或协议号码，但这具体取决于所使用的许可证程序而异。
6. 单击“下一步” 。
7. 选择产品版本、 许可证类型和您的环境中的许可证数量，然后单击**下一步**。 许可证管理器联系 Microsoft Clearinghouse 以验证并检索你的许可证。
8.  单击“完成”以完成该过程。
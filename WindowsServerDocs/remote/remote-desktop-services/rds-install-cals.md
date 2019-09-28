---
title: 安装 RDS 客户端访问许可证
description: 了解如何安装 RD 客户端的 CAL。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 4ee26a0d9ba5ee3a94a4c569a639454e064d0a90
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387420"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>在远程桌面许可证服务器上安装 RDS 客户端访问许可证

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

使用以下信息在许可证服务器上安装远程桌面服务客户端访问许可证 (CAL)。 CAL 安装后，许可证服务器会将它们酌情颁发给用户。

请注意，你需要运行远程桌面授权管理器的计算机有 Internet 连接，但不是运行许可证服务器的计算机。

1. 在许可证服务器（通常是第一个 RD 连接代理）上，打开远程桌面授权管理器。
2. 右键单击该许可证服务器，然后单击“安装许可证”  。
3. 在“欢迎”页面上单击“下一步”  。
4. 选择从中购买 RDS CAL 的程序，然后单击“下一步”  。 如果你是服务提供商，请选择“服务提供商许可协议”  。
5. 输入你的许可证计划的信息。 在大多数情况下，这将是许可证代码或协议编号，但这因所使用的许可证程序而异。
6. 单击“下一步”  。
7. 为你的环境选择产品版本、许可证类型和许可证数量，然后单击“下一步”  。 许可证管理器联系 Microsoft Clearinghouse 验证并检索你的许可证。
8.  单击“完成”  以完成该过程。
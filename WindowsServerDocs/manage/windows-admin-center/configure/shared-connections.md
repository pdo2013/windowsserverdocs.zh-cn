---
title: 配置适用于所有用户的 Windows Admin Center 网关的共享的连接
description: 了解管理员如何配置其 Windows Admin Center (Project Honolulu) 网关一次以允许所有用户共享单个连接的列表。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1bdafa0d23934666fc0f6985249502e4960dc38e
ms.sourcegitcommit: 74d9eee13386a039186a70cdc97dc0b82e74bbf4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "9271002"
---
# 配置适用于所有用户的 Windows Admin Center 网关的共享的连接

> 适用于： Windows Admin Center 预览版

通过配置共享的连接的能力，网关管理员可以配置一次为给定的 Windows Admin Center 网关的所有用户的连接列表。 

从 Windows Admin Center 网关设置**共享连接**选项卡上，网关管理员可以添加服务器、 群集和电脑连接，就像在所有连接页中，包括标记连接的功能。 为此 Windows Admin Center 网关，在其所有连接页中的所有用户将显示任何连接和标记添加到共享的连接列表中。
    ![](../media/shared-cnxns-1.png)

当所有 Windows Admin Center 的用户都访问"所有连接"页面配置共享连接后时，他们将看到它们分为两个部分的连接： 个人和共享连接。 个人组是特定用户的连接列表，该用户的浏览器会话内持续存在。 共享连接组是相同的所有用户，并从所有连接页面不能修改。
![](../media/shared-cnxns-2.png)
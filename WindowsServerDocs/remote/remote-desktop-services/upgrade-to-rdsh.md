---
title: 将远程桌面会话主机升级到 Windows Server 2016
description: 本文介绍如何将现有的远程桌面服务部署升级到 Windows Server 2016。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: 98ed5b75680e6969a40017a27061e33449cb69e8
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743350"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>将远程桌面会话主机升级到 Windows Server 2016

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

> [!IMPORTANT]
> 所有应用程序在升级之前必须先卸载，并在升级之后重新安装，以避免升级造成任何应用兼容性问题。

## <a name="supported-os-upgrades-with-rds-role-installed"></a>支持的装有 RDS 角色的 OS 升级
仅支持从 Windows Server 2012 R2 和 Windows Server 2016 TP5 升级到 Windows Server 2016。

## <a name="upgrading-a-rds-session-based-collection"></a>升级 RDS 基于会话的集合
为了尽量减少停机时间，最好是遵循以下步骤升级 RDS 基于会话的集合：

1. 确定要升级的服务器，例如，集合中一半的服务器。
2. 将“允许新连接”设置为 false，以防与这些服务器建立新连接。 
3. 注销这些服务器上的所有会话。 
4. 从集合中删除这些服务器。
5. 将服务器升级到 Windows Server 2016。
6. 在集合中的剩余服务器上，将“允许新连接”设置为“false”。 
7. 将已升级的服务器添加回到其相应集合中。
8. 从集合中删除要升级的剩余服务器集。
9. 在集合中已升级的服务器上，将“允许新连接”设置为“true”。 
10. 现在，执行上述步骤 3 到 9，升级部署中的剩余服务器。

## <a name="upgrading-a-standalone-rd-session-host-server"></a>升级独立的 RD 会话主机服务器
随时可以升级独立的 RD 会话主机服务器。
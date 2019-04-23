---
title: 升级到 Windows Server 2016 在远程桌面会话主机
description: 本文介绍如何升级现有的远程桌面服务部署到 Windows Server 2016。
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
ms.openlocfilehash: 0cf5af29d610ba64d045e10241fd39b01d3f7024
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856058"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>升级到 Windows Server 2016 在远程桌面会话主机

>适用于：Windows 服务器 （半年频道），Windows Server 2016

> [!IMPORTANT]
> 必须在升级之前卸载和升级以避免可能会增加，因为在升级任何应用程序兼容性问题后重新安装所有应用程序。

## <a name="supported-os-upgrades-with-rds-role-installed"></a>已安装 RDS 角色支持 OS 升级
仅从 Windows Server 2012 R2 和 Windows Server 2016 TP5 支持升级到 Windows Server 2016。

## <a name="upgrading-a-rds-session-based-collection"></a>升级 RDS 基于会话的集合
为了保持在最低限度的停机时间，最好是升级 RDS 基于会话的集合时遵循以下步骤：

1. 确定要升级的服务器，一半中的服务器集合。
2. 通过设置防止新连接到这些服务器**允许新连接**为 false。
3. 在这些服务器上的所有会话中都注销。 
4. 从集合中删除这些服务器。
5. 升级到 Windows Server 2016 的服务器。
6. 设置**允许新连接**为"false"集合中剩余的服务器上。
7. 将升级后的服务器添加回其相应的集合。
8. 删除剩余的集集合中要升级服务器。
9. 设置**允许新连接**为集合中升级的服务器上的"true"。
10. 现在通过执行步骤步骤 3 到 9 上述升级部署中的剩余服务器。

## <a name="upgrading-a-standalone-rd-session-host-server"></a>升级独立 RD 会话主机服务器
可以随时升级独立 RD 会话主机服务器。
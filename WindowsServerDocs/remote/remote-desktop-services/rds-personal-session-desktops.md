---
title: 通过远程桌面服务使用个人会话桌面
description: 了解如何通过 RDS 共享个性化的已分配桌面。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/16/2016
manager: dongill
ms.openlocfilehash: 723e68bad79e7723aaa0690c312e20ccf47c47b0
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743501"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>通过远程桌面服务使用个人会话桌面

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

可以使用个人会话桌面在云计算环境中部署基于服务器的个人桌面。  （Hyper-V 结构服务器与来宾虚拟机的云计算环境有区别，例如 Microsoft Azure 云或 Microsoft 云平台。）个人会话桌面功能扩展了远程桌面服务中的基于会话的桌面部署方案，它可以创建新型会话集合，其中的每个用户将分配到其自己的个人会话主机，并拥有管理权限。 

使用以下信息创建和管理个人会话桌面集合。

## <a name="create-a-personal-session-desktop-collection"></a>创建个人会话桌面集合

使用 New-RDSessionCollection cmdlet 创建个人会话桌面集合。 以下三个参数提供个人会话桌面所需的配置信息：

- **-PersonalUnmanaged** - 指定可用于将用户分配到个人会话主机服务器的会话集合类型。 如果未指定此参数，则会以传统 RD 会话主机集合的形式创建集合，其中的用户在登录时将分配到下一个可用的会话主机。
- **-GrantAdministrativePrivilege** - 如果使用了 **-PersonalUnmanaged**，则此参数会指定要为分配到会话主机的用户授予管理特权。 如果不使用此参数，则只会为用户授权标准用户权限。
- **-AutoAssignUser** - 如果使用了 **-PersonalUnmanaged**，则此参数指定要将通过 RD 连接代理进行连接的新用户自动分配到某个未分配的会话主机。 如果集合中没有任何未分配的会话主机，则用户会看到错误消息。 如果不使用此参数，则必须在用户登录之前[手动将用户分配到会话主机](#manually-assign-a-user-to-a-personal-session-host)。

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>手动将用户分配到个人会话主机
使用 **Set-RDPersonalSessionDesktopAssignment** cmdlet 可将用户手动分配到集合中的个人会话主机服务器。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\> 

-User \<字符串\>

-Name \<字符串\>

- **-CollectionName** - 指定个人会话桌面集合的名称。 此参数是必需的。
- **-ConnectionBroker** - 指定远程桌面部署的远程桌面连接代理（RD 连接代理）服务器。 如果未提供值，该 cmdlet 将使用本地计算机的完全限定域名 (FQDN)。
- **-User** - 指定要与个人会话桌面关联的用户帐户，采用“域\用户”格式。 此参数是必需的。
- **-Name** - 指定会话主机服务器的名称。 此参数是必需的。 此处标识的会话主机必须是 **-CollectionName** 参数指定的集合的成员。

**Import-RDPersonalSessionDesktopAssignment** cmdlet 从文本文件导入用户帐户与个人会话桌面之间的关联。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\>

-Path \<字符串>

**-Path** 指定要导入的文件的路径和文件名。
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>从个人会话主机中删除用户分配
使用 **Remove-RDPersonalSessionDesktopAssignment** cmdlet 可删除个人会话桌面与用户之间的关联。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\>

-Force

-Name \<字符串\>

-User \<字符串\>

**-Force** 强制运行命令而不要求用户确认。

## <a name="query-user-assignments"></a>查询用户分配
使用 **Get-RDPersonalSessionDesktopAssignment** cmdlet 可获取个人会话桌面和关联的用户帐户的列表。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\>

-User \<字符串\>

-Name \<字符串\>

可以运行该 cmdlet 来按集合名称、用户名或桌面会话名称执行查询。 如果仅指定 **-CollectionName** 参数，该 cmdlet 将返回会话主机和关联用户的列表。 如果同时指定 **-User** 参数，则会返回与该用户关联的会话主机。 如果提供 **-Name** 参数，则会返回与该会话主机关联的用户。 


**Export-RDPersonalPersonalDesktopAssignment** cmdlet 将用户与个人虚拟桌面之间的当前关联导出到文本文件。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\>

-Path \<字符串\>


所有新的 cmdlet 都支持通用参数：-Verbose、-Debug、-ErrorAction、-ErrorVariable、-OutBuffer 和 -OutVariable。 有关详细信息，请参阅 [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216)。

## <a name="hardware-accelerated-graphics"></a>硬件加速的图形
Windows Server 2016 扩展了 RemoteFX 3D 图形适配器 (vGPU) 技术，支持 OpenGL 和单用户 Windows Server 2016 来宾 VM。 可将个人会话桌面与新的 vGPU 功能相结合，为需要加速图形的托管应用程序提供支持。 或者，也可以将个人会话桌面与新的离散设备分配 (DDA) 功能相结合，为需要加速图形的托管应用程序提供支持。

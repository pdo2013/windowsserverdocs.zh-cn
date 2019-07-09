---
title: 在 RDS 中管理个人桌面会话集合
description: 了解如何将 RDSH 和 RemoteApp 程序添加到 RDS 部署中。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 286c7ba4bd4428669d135c35c825033d22b8f40e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743511"
---
## <a name="manage-your-personal-desktop-session-collections"></a>管理个人桌面会话集合

使用以下信息来管理远程桌面服务中的个人桌面会话集合。

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>手动将用户分配到个人会话主机
使用 Set-RDPersonalSessionDesktopAssignment  cmdlet 可将用户手动分配到集合中的个人会话主机服务器。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\> 

-User \<字符串\>

-Name \<字符串\>

- -CollectionName  - 指定个人会话桌面集合的名称。 此参数是必需的。
- -ConnectionBroker  - 指定远程桌面部署的远程桌面连接代理（RD 连接代理）服务器。 如果未提供值，该 cmdlet 将使用本地计算机的完全限定域名 (FQDN)。
- -User  - 指定要与个人会话桌面关联的用户帐户，采用“域\用户”格式。 此参数是必需的。
- -Name  - 指定会话主机服务器的名称。 此参数是必需的。 此处标识的会话主机必须是 -CollectionName  参数指定的集合的成员。

Import-RDPersonalSessionDesktopAssignment  cmdlet 从文本文件导入用户帐户与个人会话桌面之间的关联。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\>

-Path \<字符串>

-Path  指定要导入的文件的路径和文件名。
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>从个人会话主机中删除用户分配
使用 Remove-RDPersonalSessionDesktopAssignment  cmdlet 可删除个人会话桌面与用户之间的关联。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\>

-Force

-Name \<字符串\>

-User \<字符串\>

-Force  强制运行命令而不要求用户确认。

### <a name="query-user-assignments"></a>查询用户分配
使用 Get-RDPersonalSessionDesktopAssignment  cmdlet 可获取个人会话桌面和关联的用户帐户的列表。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\>

-User \<字符串\>

-Name \<字符串\>

可以运行该 cmdlet 来按集合名称、用户名或桌面会话名称执行查询。 如果仅指定 -CollectionName  参数，该 cmdlet 将返回会话主机和关联用户的列表。 如果同时指定 -User  参数，则会返回与该用户关联的会话主机。 如果提供 -Name  参数，则会返回与该会话主机关联的用户。 


Export-RDPersonalPersonalDesktopAssignment  cmdlet 将用户与个人虚拟桌面之间的当前关联导出到文本文件。 该 cmdlet 支持以下参数：

-CollectionName \<字符串\>

-ConnectionBroker \<字符串\>

-Path \<字符串\>


所有新的 cmdlet 都支持通用参数：-Verbose、-Debug、-ErrorAction、-ErrorVariable、-OutBuffer 和 -OutVariable。 有关详细信息，请参阅 [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216)。

---
title: 管理在 RDS 的个人桌面会话集合
description: 了解如何添加和到 RDS 部署 RDSH 和 RemoteApp 程序。
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865718"
---
## <a name="manage-your-personal-desktop-session-collections"></a>管理您的个人桌面会话集合

使用以下信息来管理远程桌面服务中的个人桌面会话集合。

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>手动将用户分配到个人会话主机
使用**集 RDPersonalSessionDesktopAssignment** cmdlet 来手动将用户分配到集合中的个人会话主机服务器。 该 cmdlet 支持以下参数：

-CollectionName \<string\>

-ConnectionBroker \<string\> 

-用户\<字符串\>

-Name \<string\>

- **– CollectionName** -指定个人会话桌面集合的名称。 此参数是必需的。
- **– ConnectionBroker** -指定在远程桌面部署的远程桌面连接代理 （RD 连接代理） 服务器。 如果不提供一个值，该 cmdlet 将使用本地计算机的完全限定的域名 (FQDN)。
- **– 用户**-指定要与个人会话桌面，以域 \ 用户格式关联的用户帐户。 此参数是必需的。
- **– 名称**-指定会话主机服务器的名称。 此参数是必需的。 此处标识的会话主机必须是集合的成员的**CollectionName**参数指定。

**导入 RDPersonalSessionDesktopAssignment** cmdlet 从文本文件导入用户帐户和个人会话桌面之间的关联。 该 cmdlet 支持以下参数：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string>

**– 路径**指定要导入的文件路径和文件名。
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>从个人会话主机中删除的用户分配
使用**删除 RDPersonalSessionDesktopAssignment** cmdlet 可删除个人会话桌面与用户之间的关联。 该 cmdlet 支持以下参数：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Name \<string\>

-用户\<字符串\>

**– 强制**强制运行命令而不要求用户确认。

### <a name="query-user-assignments"></a>查询的用户分配
使用**Get RDPersonalSessionDesktopAssignment** cmdlet 来获取个人会话桌面和关联的用户帐户的列表。 该 cmdlet 支持以下参数：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-用户\<字符串\>

-Name \<string\>

集合名称、 用户名称，或桌面会话的名称，可以查询运行 cmdlet。 如果仅指定 **– CollectionName**参数，cmdlet 将返回的会话主机和关联的用户列表。 如果还指定了 **– 用户**参数，则返回与该用户相关联的会话主机。 如果你提供 **– 名称**参数，则返回与该会话主机关联的用户。 


**导出 RDPersonalPersonalDesktopAssignment** cmdlet 将用户与个人虚拟机之间的当前关联导出到文本文件。 该 cmdlet 支持以下参数：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string\>


所有新的 cmdlet 支持以下公共参数:-Verbose、-Debug、-ErrorAction、-ErrorVariable、-OutBuffer 和-OutVariable。 有关详细信息，请参阅 [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216)。

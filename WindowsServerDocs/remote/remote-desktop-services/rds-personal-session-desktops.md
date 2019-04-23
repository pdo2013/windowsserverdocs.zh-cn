---
title: 结合使用个人会话桌面和远程桌面服务
description: 了解如何共享个性化、 分配桌面通过 rds。
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
ms.openlocfilehash: 41f6a28c1b754a5277a0966a87dae08a6aa34e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875948"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>结合使用个人会话桌面和远程桌面服务

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以通过使用个人会话桌面部署基于服务器的云计算环境中的个人桌面。  （云计算环境有构造的 HYPER-V 服务器和来宾虚拟机，如 Microsoft Azure 云或 Microsoft 云平台之间的分隔。）个人会话桌面功能扩展了远程桌面服务来创建新类型的会话集合中的基于会话的桌面部署方案，每个用户分配到它们自己的个人会话主机具有管理权限。 

使用以下信息来创建和管理个人会话桌面集合。

## <a name="create-a-personal-session-desktop-collection"></a>创建个人会话桌面集合

新建 RDSessionCollection cmdlet 用于创建个人会话桌面集合。 以下三个参数提供适用于个人会话桌面所需的配置信息：

- **-PersonalUnmanaged** -指定的会话集合，可通过该类型将用户分配到个人会话主机服务器。 如果未指定此参数，为传统的 RD 会话主机集合，其中用户在登录时将分配给下一个可用的会话主机创建集合。
- **-GrantAdministrativePrivilege** -如果使用 **-PersonalUnmanaged**，指定分配给会话主机的用户被授予管理权限。 如果不使用此参数，只是标准用户权限将授予用户。
- **-AutoAssignUser** -如果使用 **-PersonalUnmanaged**，指定通过 RD 连接代理连接的新用户将自动分配给未分配的会话主机。 如果在集合中没有未分配的会话主机，用户将看到一条错误消息。 如果不使用此参数，必须[手动将用户分配到会话主机](#manually-assign-a-user-to-a-personal-session-host)他们在登录之前。

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>手动将用户分配到个人会话主机
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
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>从个人会话主机中删除的用户分配
使用**删除 RDPersonalSessionDesktopAssignment** cmdlet 可删除个人会话桌面与用户之间的关联。 该 cmdlet 支持以下参数：

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Name \<string\>

-用户\<字符串\>

**– 强制**强制运行命令而不要求用户确认。

## <a name="query-user-assignments"></a>查询的用户分配
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

## <a name="hardware-accelerated-graphics"></a>硬件加速的图形
Windows Server 2016 扩展了 RemoteFX 3D 图形适配器 (vGPU) 技术来支持 OpenGL 和支持单用户的 Windows Server 2016 来宾 Vm。 您可以将个人会话桌面与新的 vGPU 功能，需要加速的图形的托管应用程序提供支持。 或者，您可以将个人会话桌面与新的离散设备分配 (DDA) 功能，还需要加速的图形的托管应用程序提供支持。

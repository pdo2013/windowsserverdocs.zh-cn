---
title: 远程桌面服务-安全的数据存储
description: 关于安全地存储数据以在 rds.中使用用户配置文件磁盘 (Upd) 的规划信息
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37b7f68e-7c3a-4190-a52f-99ae96885fae
author: lizap
ms.author: elizapo
ms.date: 11/21/2016
manager: dongill
ms.openlocfilehash: 242d09e49141e9fb789a83421714366105730cc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870588"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>远程桌面服务-使用 Upd，安全数据存储

>适用于：Windows 服务器 （半年频道），Windows Server 2016

应用商店商业资源、 用户个性化设置数据和设置安全地在本地或在 Azure 中。 RD 会话主机使用 AD 身份验证，并让它们安全地在个性化环境中，需要的资源的用户。 

确保用户具有一致的体验，而不考虑终结点从其它们访问其远程资源，是管理 RDS 部署的一个重要方面。 用户配置文件磁盘 (Upd) 允许用户数据、 自定义项和应用程序设置，以按照单个集合内的用户。 UPD 是每个用户，每个集合的中央共享已装载到用户的会话时他们在登录-UPD 视为该会话的持续时间内的本地驱动器中保存的 VHD 文件。 

从用户的角度来看，UPD 提供 famililar 体验-它们，其将文档保存到他们的文档文件夹中 （上什么似乎是本地驱动器），将更改其应用设置为平常，并使其 Windows 环境的任何自定义。 所有这些数据，包括注册表配置单元，存储在 UPD 上并保持在中央网络共享。 当用户主动连接到桌面或 RemoteApp 时，Upd 才可供用户使用。 因为用户的整个 C:\Users Upd 仅可以漫游集合中&#92;\<用户名\>目录 （包括 AppData\Local） 存储在 UPD 上。

可以使用[PowerShell cmdlet](https://technet.microsoft.com/library/jj215443.aspx)指定中央共享、 每个 UPD 的大小和哪些文件夹应包含或排除用户配置文件保存到 UPD 的路径。 或者，可以通过转到启用 Upd 通过服务器管理器**远程桌面服务 > 集合 > 桌面集合 > 桌面集合属性 > 用户配置文件磁盘**。 请注意，启用或禁用 Upd，整个集合的所有用户，而不适用于该集合中的特定用户。 Upd 必须存储在其中集合中的服务器拥有完全控制权限的中央文件共享上。 

可以通过将它们存储在 Azure 中使用的 Upd 中实现高可用性[存储空间直通](rds-storage-spaces-direct-deployment.md)。 
---
title: 远程桌面服务 - 保护数据存储
description: 有关在 RDS 中使用用户配置文件磁盘 (UPD) 安全存储数据的规划信息。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8b7fa596f88f5cb361e0c681ffec3bcc72403d03
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403922"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>远程桌面服务 - 使用 UPD 保护数据存储

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

在本地或者在 Azure 中安全存储业务资源、用户个性化数据和设置。 RD 会话主机使用 AD 身份验证，并在个性化的环境中安全地为用户赋予他们所需的资源。 

无论用户从哪个终结点访问其远程资源，确保用户获得一致的体验是管理 RDS 部署的一个重要方面。 用户配置文件磁盘 (UPD) 使用户数据、自定义内容和应用程序设置符合单个集合中的用户要求。 UPD 是在中心共享中保存的基于用户、基于集合的 VHD 文件，当用户登录时，该共享会装载到用户的会话 - 在该会话的整个持续时间内，UPD 都被视为一个本地驱动器。 

从用户的角度来看，UPD 提供他们所熟悉的体验 - 他们可将文档保存到“文档”文件夹（UPD 看上去是一个本地驱动器），像平时一样更改其应用设置，以及对其 Windows 环境进行任何自定义。 所有这些数据（包括注册表配置单元）将存储在 UPD 上，并在中心网络共享中持久保存。 仅当用户有效连接到桌面或 RemoteApp 后，UPD 才可供用户使用。 UPD 只能在集合中漫游，因为用户的整个 `C:\Users\<username\>` 目录（包括 AppData\Local）存储在 UPD 上。

可以使用 [PowerShell cmdlet](https://technet.microsoft.com/library/jj215443.aspx) 指定中心共享的路径、每个 UPD 的大小，以及要在保存到 UPD 的用户配置文件中包含或排除哪些文件夹。 或者，可以在服务器管理器中转到“远程桌面服务” > “集合” > “桌面集合” > “桌面集合属性” > “用户配置文件磁盘”，来启用 UPD。      请注意，只能对整个集合的所有用户（而不能对该集合中的特定用户）启用或禁用 UPD。 UPD 必须存储在中心文件共享上，该共享上的集合中的服务器拥有完全控制权限。 

使用[存储空间直通](rds-storage-spaces-direct-deployment.md)将 UPD 存储在 Azure 中可以实现 UPD 的高可用性。 
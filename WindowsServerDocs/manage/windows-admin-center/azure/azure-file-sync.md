---
title: 使用 Azure 文件同步将文件服务器与云同步
description: 使用 Azure 文件同步在 Azure 中集中组织的文件共享，同时保持本地文件服务器的灵活性、性能和兼容性。 Azure 文件同步将 Windows Server 转换为 Azure 文件共享的快速缓存，并提供可选的云分层功能。
ms.technology: manage
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: fe0e3337962b7d9c2f025a9d4eba826f3349c1f9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357383"
---
# <a name="sync-your-file-server-with-the-cloud-by-using-azure-file-sync"></a>使用 Azure 文件同步将文件服务器与云同步

>适用于：Windows Admin Center、Windows Admin Center 预览版

使用 Azure 文件同步在 Azure 中集中组织的文件共享，同时保持本地文件服务器的灵活性、性能和兼容性。 Azure 文件同步将 Windows Server 转换为 Azure 文件共享的快速缓存，并提供可选的云分层功能。 可以使用 Windows Server 上可用的任意协议本地访问数据，包括 SMB、NFS 和 FTPS。

文件同步到云后，可以将多个服务器连接到同一个 Azure 文件共享，以便在本地同步和缓存内容，同时还会传输权限（Acl）。 Azure 文件提供了一种快照功能，可以生成 Azure 文件共享的差异快照。 甚至可以通过 SMB 将这些快照装载为只读网络驱动器，便于浏览和还原。 与云分层结合使用时，运行本地文件服务器从未简单一些。

有关详细信息，请参阅[规划 Azure 文件同步部署](https://aka.ms/afs)。
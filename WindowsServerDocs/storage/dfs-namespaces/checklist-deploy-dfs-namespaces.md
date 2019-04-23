---
title: 清单：部署 DFS 命名空间
description: 本文介绍如何配置和部署 DFS 命名空间。
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7f7cca6b67ff6fa8d81e88323381866315f07f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842118"
---
# <a name="checklist-deploy-dfs-namespaces"></a>清单：部署 DFS 命名空间

> 适用于：Windows Server 2019，Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

分布式的文件系统 (DFS) 命名空间和 DFS 复制可以用于将文档、 软件和业务线数据发布到整个组织中的用户。 尽管使用 DFS 复制就足以分发数据，可以使用 DFS 命名空间配置命名空间，以便由多个服务器，其中每个保存的已更新的副本的文件夹的托管文件夹。 这样，便可增加数据的可用性，并在服务器之间分发客户端负载。

浏览命名空间中的文件夹时，用户并不知道该文件夹由多个服务器托管。 在用户打开该文件夹时，系统会将客户端计算机自动引用到其站点上的服务器。 如果没有相同站点的服务器可用，可以配置要将客户端引用到具有最低连接成本 Active Directory 目录服务 (AD DS) 中定义的服务器的命名空间。

若要部署 DFS 命名空间，请执行以下任务：

-   查看 DFS 命名空间的概念和要求。
[DFS 命名空间概述](dfs-overview.md)
-   [选择命名空间类型](choose-a-namespace-type.md)
-   [创建 DFS 命名空间](create-a-dfs-namespace.md) 
-   将基于域的现有命名空间迁移到基于域的 Windows Server 2008 模式的命名空间。 [将基于域的 Namespace 迁移到 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   通过将命名空间服务器添加到基于域的命名空间来增加可用性。 [将 Namespace 服务器添加到基于域的 DFS Namespace](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   将文件夹添加到命名空间。 [在 DFS Namespace 中创建一个文件夹](create-a-folder-in-a-dfs-namespace.md)
-   将文件夹目标添加到命名空间中的文件夹。 [添加文件夹目标](add-folder-targets.md)
-   通过使用 DFS 复制在文件夹目标之间复制内容（可选）。 [使用 DFS 复制复制文件夹目标](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>请参阅

-   [命名空间](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [清单：优化 DFS Namespace](checklist-tune-a-dfs-namespace.md)
-   [复制](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)



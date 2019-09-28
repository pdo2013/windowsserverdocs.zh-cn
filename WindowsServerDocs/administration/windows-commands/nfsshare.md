---
title: nfsshare
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a952e247ee40f832045d39d0e2164bb2e6613c54
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373209"
---
# <a name="nfsshare"></a>nfsshare



你可以使用**nfsshare**来控制网络文件系统（NFS）共享。

## <a name="syntax"></a>语法

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>描述

如果没有参数，则**nfsshare**命令行实用程序会列出 nfs 服务器导出的所有网络文件系统（NFS）共享。 使用*共享名*作为唯一参数， **Nfsshare**会列出*共享名*标识的 NFS 共享的属性。 如果提供了*共享名*和<em>驱动器</em> **：** <em>path</em> ，则**nfsshare**会将<em>Drive</em> **：** <em>path</em>标识的文件夹导出为*共享名*。 如果使用 **/delete**选项，则指定的文件夹将不再对 NFS 客户端可用。

## <a name="options"></a>选项

**Nfsshare**命令接受以下选项和参数：


|             术语              |                                                                                                                                                                                                                      定义                                                                                                                                                                                                                       |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         -o anon = {yes          |                                                                                                                                                                                                                          不                                                                                                                                                                                                                          |
|  -o rw [= @no__t > [： <Host>] ...]  |                       提供由*主机*指定的主机或客户端组对共享目录的读写访问权限。 使用冒号（ **：** ）分隔主机名和组名。 如果未指定*Host* ，则所有主机和客户端组（使用**ro**选项指定的组除外）都具有读写访问权限。 如果不设置**ro**或**rw**选项，所有客户端都具有对共享目录的读写访问权限。                       |
|  -o ro [= \<Host > [： <Host>] ...]  | 提供由*主机*指定的主机或客户端组对共享目录的只读访问。 使用冒号（ **：** ）分隔主机名和组名。 如果未指定*Host* ，则所有客户端（除了**rw**选项指定的客户端）都具有只读访问权限。 如果为一个或多个客户端设置了**ro**选项，但未设置**rw**选项，则只有使用**ro**选项指定的客户端才能访问共享目录。 |
|       -o 编码 = {big5       |                                                                                                                                                                                                                        euc-jp                                                                                                                                                                                                                         |
|       -o anongid = \<gid >       |                                                                                     指定匿名（未映射）用户将使用*gid*作为其组标识符（gid）来访问共享目录。 默认值为-2。 在报告未映射用户拥有的文件的所有者时，即使禁用匿名访问，也将使用匿名 GID。                                                                                      |
|      -o anonuid = \<uid >       |                                                                                      指定匿名（未映射）用户将使用*uid*作为其用户标识符（uid）来访问共享目录。 默认值为-2。 在报告未映射用户拥有的文件的所有者时，即使禁用匿名访问，也将使用匿名 UID。                                                                                      |
| -o root [= @no__t > [： <Host>] ...] |                                                                         提供由*主机*指定的主机或客户端组对共享目录的根访问权限。 使用冒号（ **：** ）分隔主机名和组名。 如果未指定*主机*，则所有客户端都具有根访问权限。 如果未设置**root**选项，则没有客户端具有对共享目录的根访问权限。                                                                         |
|            /delete            |                                                                                                                                                       如果指定了*共享名*或<em>驱动器</em> **：** <em>Path</em> ，则将删除指定的共享。 如果指定 \*，将删除所有 NFS 共享。                                                                                                                                                       |

> [!NOTE]
> 若要查看此命令的完整语法，请在命令提示符下键入：</br>> **nfsshare/？**

# #

[网络文件系统服务命令参考](services-for-network-file-system-command-reference.md)另请参阅
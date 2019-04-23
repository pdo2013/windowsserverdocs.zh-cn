---
title: nfsshare
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 50df88a3fddbceb1595f328bdd4e3c6f526e3a2c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881688"
---
# <a name="nfsshare"></a>nfsshare



可以使用**nfsshare**控制网络文件系统 (NFS) 共享。

## <a name="syntax"></a>语法

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>描述

不带参数， **nfsshare**命令行实用程序列出在 nfs 服务器导出的所有网络文件系统 (NFS) 共享。 与*ShareName*作为唯一参数， **nfsshare**列出了由标识的 NFS 共享的属性*ShareName*。 当*ShareName*和*驱动器 ***:*** 路径*还提供**nfsshare**导出由标识的文件夹*驱动器 ***:*** 路径*作为*ShareName*。 当 **/delete**选项，则指定的文件夹不再可供 NFS 客户端。

## <a name="options"></a>选项

**Nfsshare**命令接受以下选项和参数：

|术语|定义|
|----|----------|
|-o anon = {是 | no}|指定匿名 （未映射） 用户是否可以访问共享的目录。 默认值是**没有**。|
|-o rw [=\<主机 > [:<Host>]...]|提供对共享目录的主机或客户端的读写访问权限指定组*主机*。 用冒号分隔主机和组的名称 (**:**)。 如果*主机*未指定，则所有主机和客户端组 (除了那些使用指定**ro**选项) 具有读写访问权限。 如果既没有**ro**也不**rw**设置选项，则所有客户端具有对共享目录的读写访问权限。|
|-o ro [=\<主机 > [:<Host>]...]|提供对共享目录的主机或客户端的只读访问权限指定组*主机*。 用冒号分隔主机和组的名称 (**:**)。 如果*主机*未指定，则所有客户端 (除了那些使用指定**rw**选项) 具有只读访问权限。 如果**ro**选项设置为一个或多个客户端，但**rw**未设置选项，使用指定的客户端仅**ro**选项有权访问共享目录。|
|-o encoding={big5|euc-jp|euc-kr|euc-tw|gb2312-80|ksc5601|shift-jis}|指定的默认编码使用的文件和目录名称，并且，如果使用，必须设置为以下值之一：</br>-   **big5** （中文版）</br>-   **euc jp** （日语）</br>-   **euc 韩国**（朝鲜语）</br>-   **euc tw** （中文版）</br>-   **gb2312-80** （简体中文）</br>-   **ksc5601** （朝鲜语）</br>-   **shift jis** （日语）</br>如果此选项未设置，默认编码方案为 ANSI 或为非英语区域设置，配置默认的编码方案的区域设置的系统上。 以下是所指示的区域设置的默认编码方案：</br>日语：SHIFT-JIS</br>-朝鲜语：KS_C_5601-1987</br>-简体中文：GB2312-80</br>中文 （繁体):BIG5|
|-o anongid=\<gid>|指定匿名 （未映射） 用户将访问共享目录使用*gid*作为组标识符 (GID)。 默认值为-2。 报告未映射的用户拥有的文件的所有者，即使禁用匿名访问时，将使用匿名 GID。|
|-o  anonuid=\<uid>|指定匿名 （未映射） 用户将访问共享目录使用*uid*作为用户标识符 (UID)。 默认值为-2。 报告未映射的用户拥有的文件的所有者，即使禁用匿名访问时，将使用匿名 UID。|
|-o 根 [=\<主机 > [:<Host>]...]|提供对共享目录的主机或客户端的根访问权限指定组*主机*。 用冒号分隔主机和组的名称 (**:**)。 如果*主机*未指定，则所有客户端具有根目录访问权限。 如果**根**未设置的选项，没有客户端具有对共享目录的根访问权限。|
|/delete|如果*ShareName*或*驱动器 ***:*** 路径*指定时，将删除指定的共享。 如果 * 指定时，将删除所有 NFS 共享。|

> [!NOTE]
> 若要查看此命令的完整语法，请在命令提示符下键入：</br>> **nfsshare /?**

##

[服务的网络文件系统命令参考](services-for-network-file-system-command-reference.md)另请参阅
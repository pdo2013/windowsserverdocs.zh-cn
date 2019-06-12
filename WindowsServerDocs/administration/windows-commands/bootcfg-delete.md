---
title: bootcfg delete
description: Windows 命令主题**bootcfg 删除**-删除 Boot.ini 文件的操作系统部分中的操作系统条目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37f30a181402fe8a74148b42398641af3c4846b9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434767"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

删除操作系统条目 [操作系统] 节的 Boot.ini 文件。

## <a name="syntax"></a>语法
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parameters

|         术语         |                                                                                             定义                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。                                          |
| /u <Domain>\\<User>  | 使用指定的用户帐户权限运行命令<User>或<Domain> \\ <User>。 默认值为当前登录的用户发出命令的计算机上的权限。 |
|    /p <Password>     |                                                        指定在指定的用户帐户的密码 **/u**参数。                                                        |
| /id <OSEntryLineNum> |        要删除的 Boot.ini 文件的 [操作系统] 部分中指定的操作系统条目行号。 [操作系统] 部分标头后的第一行是 1。        |
|          /?          |                                                                                在命令提示符下显示帮助。                                                                                 |

## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg /delete**命令：
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)

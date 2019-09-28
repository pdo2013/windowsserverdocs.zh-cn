---
title: bootcfg delete
description: 用于**bootcfg delete**的 Windows 命令主题删除 boot.ini 文件的操作系统部分中的操作系统条目。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7d2298c07af32e66a2ffcebb85ec780da762be58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380007"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

删除 Boot.ini 文件的 [操作系统] 部分中的操作系统项。

## <a name="syntax"></a>语法
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parameters

|         术语         |                                                                                             定义                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。                                          |
| /u <Domain> @ no__t-1 @ no__t-2  | 使用由 <User>or <Domain> @ no__t-2 @ no__t 指定的用户的帐户权限运行命令。 默认为发出命令的计算机上当前登录用户的权限。 |
|    /p <Password>     |                                                        指定在 **/u**参数中指定的用户帐户的密码。                                                        |
| /id <OSEntryLineNum> |        指定要删除的 Boot.ini 文件的 [操作系统] 部分中的操作系统条目行号。 [操作系统] 部分标题后面的第一行是1。        |
|          /?          |                                                                                在命令提示符下显示帮助。                                                                                 |

## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg/delete**命令：
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)

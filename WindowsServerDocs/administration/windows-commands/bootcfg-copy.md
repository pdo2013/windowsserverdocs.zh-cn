---
title: bootcfg copy
description: 适用于**bootcfg copy**的 Windows 命令主题复制-创建现有启动项的副本，您可以将命令行选项添加到该副本中。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42a408443cbe6722c25780f7c27d70b05da7eb8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380117"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

创建现有启动项的副本，您可以将命令行选项添加到该副本中。

## <a name="syntax"></a>语法
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parameters

|      参数       |                                                                                             描述                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。                                          |
| /u <Domain> @ no__t-1 @ no__t-2  | 使用由 <User>or <Domain> @ no__t-2 @ no__t 指定的用户的帐户权限运行命令。 默认为发出命令的计算机上当前登录用户的权限。 |
|    /p <Password>     |                                                        指定在 **/u**参数中指定的用户帐户的密码。                                                        |
|   /d <Description>   |                                                                    指定新操作系统项的说明。                                                                    |
| /id <OSEntryLineNum> |         指定要复制的 Boot.ini 文件的 [操作系统] 部分中的操作系统条目行号。 [操作系统] 部分标题后面的第一行是1。         |
|          /?          |                                                                                在命令提示符下显示帮助。                                                                                 |

## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg/copy**命令复制启动条目1，并输入 "\ABC Server @ no__t-1" 作为描述：
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)

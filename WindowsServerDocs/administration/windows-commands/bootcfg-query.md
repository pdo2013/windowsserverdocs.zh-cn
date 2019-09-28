---
title: bootcfg query
description: 适用于**bootcfg 查询**查询的 Windows 命令主题，并显示 boot.ini 中的 [启动加载程序] 和 [操作系统] 部分条目。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae82357cfe178343872448c2ebd46c49a797b5a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379911"
---
# <a name="bootcfg-query"></a>bootcfg query

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

查询并显示 Boot.ini 中的 [启动加载程序] 和 [操作系统] 部分条目。

## <a name="syntax"></a>语法
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>Parameters

|        术语         |                                                                                             定义                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。                                          |
| /u <Domain> @ no__t-1 @ no__t-2 | 使用由 <User>or <Domain> @ no__t-2 @ no__t 指定的用户的帐户权限运行命令。 默认为发出命令的计算机上当前登录用户的权限。 |
|    /p <Password>    |                                                        指定在 **/u**参数中指定的用户帐户的密码。                                                        |
|         /?          |                                                                                在命令提示符下显示帮助。                                                                                 |

##### <a name="remarks"></a>备注
- 下面是一个**bootcfg/query**输出示例：
  ```
  Boot Loader Settings
  ----------
  timeout: 30
  default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  Boot Entries
  ------
  Boot entry ID:   1
  Friendly Name:   ""
  path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  OS Load Options: /fastdetect /debug /debugport=com1:
  ```
- **Bootcfg 查询**输出的启动加载器设置部分显示 boot.ini 的 [boot loader] 部分中的每个条目。
- **Bootcfg 查询**输出的启动条目部分显示 boot.ini 的 [操作系统] 部分中每个操作系统项的以下详细信息：启动项 ID、友好名称、路径和 OS 加载选项。
  ## <a name="BKMK_examples"></a>示例
  下面的示例演示如何使用**bootcfg/query**命令：
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  #### <a name="additional-references"></a>其他参考
  [命令行语法项](command-line-syntax-key.md)

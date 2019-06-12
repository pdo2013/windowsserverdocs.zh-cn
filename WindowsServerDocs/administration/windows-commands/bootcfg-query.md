---
title: bootcfg query
description: Windows 命令主题**bootcfg 查询**-查询和显示 [引导加载程序] 和 [操作系统] 部分条目从 Boot.ini。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e79acc100a9ec9955f2692a3c6ee812d0310b687
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434733"
---
# <a name="bootcfg-query"></a>bootcfg query

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

查询并显示 [引导加载程序] 和 [操作系统] 部分条目从 Boot.ini。

## <a name="syntax"></a>语法
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>Parameters

|        术语         |                                                                                             定义                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。                                          |
| /u <Domain>\\<User> | 使用指定的用户帐户权限运行命令<User>或<Domain> \\ <User>。 默认值为当前登录的用户发出命令的计算机上的权限。 |
|    /p <Password>    |                                                        指定在指定的用户帐户的密码 **/u**参数。                                                        |
|         /?          |                                                                                在命令提示符下显示帮助。                                                                                 |

##### <a name="remarks"></a>备注
- 以下是示例**bootcfg /query**输出：
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
- 启动加载器设置部分**bootcfg 查询**输出 Boot.ini 的 [引导加载程序] 部分中显示每个条目。
- 启动项一部分**bootcfg 查询**输出 Boot.ini 的 [操作系统] 部分中会显示为每个操作系统条目的以下详细信息：启动项目 ID、 友好名称、 路径和 OS 加载选项。
  ## <a name="BKMK_examples"></a>示例
  下面的示例演示如何使用**bootcfg /query**命令：
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  #### <a name="additional-references"></a>其他参考
  [命令行语法项](command-line-syntax-key.md)

---
title: bootcfg debug
description: 用于**bootcfg 调试**的 Windows 命令主题-添加或更改指定操作系统项的调试设置。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6659cf2bfdf83b1b2fe6f6c811365775526768a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380050"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

添加或更改指定操作系统项的调试设置。

## <a name="syntax"></a>语法
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parameters

|                           参数                           |                                                                                                                                                                                                                    描述                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {ON &#124; OFF&#124;编辑}                   | 指定调试的值。<br /><br />**启用-通过**将/debug 选项添加到指定 <OSEntryLineNum> 来启用远程调试支持。<br /><br />**OFF** -通过删除指定 <OSEntryLineNum> 中的/debug 选项来禁用远程调试支持。<br /><br />**编辑**-通过更改与指定 <OSEntryLineNum> 的/debug 选项关联的值，允许更改端口和波特率设置。 |
|                         /s <computer>                         |                                                                                                                                                                指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。                                                                                                                                                                 |
|                      /u <Domain> @ no__t-1 @ no__t-2                      |                                                                                                                       使用 <User> 或 <Domain> @ no__t-2 @ no__t）指定的用户的帐户权限运行命令。 默认为发出命令的计算机上当前登录用户的权限。                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               指定在 **/u**参数中指定的用户帐户的密码。                                                                                                                                                                               |
|       /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4}        |                                                                                                                                                                指定用于调试的 COM 端口。 如果正在禁用调试，请不要使用 **/port**参数。                                                                                                                                                                |
| /baud {9600&#124; 19200&#124; 38400&#124; 57600&#124; 115200} |                                                                                                                                                               指定用于调试的波特率。 如果正在禁用调试，请不要使用 **/baud**参数。                                                                                                                                                                |
|                     /id <OSEntryLineNum>                      |                                                                                                               指定 Boot.ini 文件的 [操作系统] 部分中的操作系统条目行号，调试选项添加到该文件中。 [操作系统] 部分标题后面的第一行是1。                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       在命令提示符下显示帮助。                                                                                                                                                                                                        |

##### <a name="remarks"></a>备注
- 如果需要1394端口调试，请使用[bootcfg dbg1394](bootcfg-dbg1394.md)。
  ## <a name="BKMK_examples"></a>示例
  下面的示例演示如何使用**bootcfg/debug**命令：
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  #### <a name="additional-references"></a>其他参考
  [命令行语法项](command-line-syntax-key.md)

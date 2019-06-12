---
title: bootcfg debug
description: Windows 命令主题**bootcfg 调试**-添加或更改指定的操作系统条目的调试设置。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 44a1145384a62d30f055cb48fd7ed6adccd2c69b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434820"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

添加或更改指定的操作系统条目的调试设置。

## <a name="syntax"></a>语法
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parameters

|                           参数                           |                                                                                                                                                                                                                    描述                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {ON &#124; OFF&#124; edit}                   | 指定用于调试的值。<br /><br />**ON** -通过添加到指定 /debug 选项可提供远程调试支持<OSEntryLineNum>。<br /><br />**关闭**-通过删除从指定 /debug 选项来禁用远程调试支持<OSEntryLineNum>。<br /><br />**编辑**-允许通过更改与指定 /debug 选项相关联的值将对端口和波特率更改率设置<OSEntryLineNum>。 |
|                         /s <computer>                         |                                                                                                                                                                指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。                                                                                                                                                                 |
|                      /u <Domain>\\<User>                      |                                                                                                                       使用指定的用户帐户权限运行命令<User>或<Domain> \\ <User>。 默认值为当前登录的用户发出命令的计算机上的权限。                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               指定在指定的用户帐户的密码 **/u**参数。                                                                                                                                                                               |
|       /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4}        |                                                                                                                                                                指定要用于调试的 COM 端口。 不要使用 **/端口**参数如果调试被禁用。                                                                                                                                                                |
| /baud {9600&#124; 19200&#124; 38400&#124; 57600&#124; 115200} |                                                                                                                                                               指定要用于调试的波特率。 不要使用 **/baud**参数如果调试被禁用。                                                                                                                                                                |
|                     /id <OSEntryLineNum>                      |                                                                                                               调试选项会添加到其中的 Boot.ini 文件的 [操作系统] 部分中指定的操作系统条目行号。 [操作系统] 部分标头后的第一行是 1。                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       在命令提示符下显示帮助。                                                                                                                                                                                                        |

##### <a name="remarks"></a>备注
- 如果调试 1394年端口是必需的使用[bootcfg dbg1394](bootcfg-dbg1394.md)。
  ## <a name="BKMK_examples"></a>示例
  下面的示例演示如何使用**bootcfg /debug**命令：
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  #### <a name="additional-references"></a>其他参考
  [命令行语法项](command-line-syntax-key.md)

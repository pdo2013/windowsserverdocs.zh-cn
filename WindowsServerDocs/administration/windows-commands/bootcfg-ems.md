---
title: bootcfg ems
description: Windows 命令主题**bootcfg ems** -使用户可以添加或更改远程计算机的紧急管理服务控制台重定向的设置。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d971fab552b321d54899b418eac4f54c4665ccb7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434746"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用户能够添加或更改远程计算机的紧急管理服务控制台重定向的设置。 通过启用紧急管理服务，您将添加"重定向 = 端口号"的 Boot.ini 文件和一个 /redirect 选项指定的操作系统条目行到 [引导加载程序] 部分的代码行。 仅在服务器上启用紧急管理服务功能。

## <a name="syntax"></a>语法
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parameters

|                            参数                             |                                                                                                                                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                                                                                                                                              |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    {ON &#124; OFF&#124; edit}                    | 指定用于紧急管理服务重定向的值。<br /><br />**ON** -启用指定的远程输出<OSEntryLineNum>。 将 /redirect 选项添加到指定<OSEntryLineNum>和重定向 = com<X>将设置为 [引导加载程序] 部分。 Com 值<X>通过设置 **/端口**参数。<br /><br />**关闭**-禁用输出到远程计算机。 从指定删除 /redirect 选项<OSEntryLineNum>重定向 = com<X>设置从 [引导加载程序] 部分。<br /><br />**编辑**-允许通过更改重定向到端口设置更改 = com<X> [引导加载程序] 部分中的设置。 Com 值<X>重置为指定的值 **/端口**参数。 |
|                          /s <computer>                           |                                                                                                                                                                                                                                                                                                          指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。                                                                                                                                                                                                                                                                                                           |
|                       /u <Domain>\\<User>                        |                                                                                                                                                                                                                                                                 使用指定的用户帐户权限运行命令<User>或<Domain> \\ <User>。 默认值为当前登录的用户发出命令的计算机上的权限。                                                                                                                                                                                                                                                                  |
|                          /p <Password>                           |                                                                                                                                                                                                                                                                                                                         指定在指定的用户帐户的密码 **/u**参数。                                                                                                                                                                                                                                                                                                                         |
| /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4 &#124; BIOSSET}  |                                                                                                                                                                                                                              指定要用于重定向的 COM 端口。 **BIOSSET**指示紧急管理服务以获取要确定应使用哪个端口重定向的 BIOS 设置。 不要使用 **/端口**禁用远程管理的输出参数。                                                                                                                                                                                                                              |
| /baud {9600 &#124; 19200 &#124; 38400&#124; 57600 &#124; 115200} |                                                                                                                                                                                                                                                                                               指定要用于重定向的波特率。 不要使用 **/baud**禁用远程管理的输出参数。                                                                                                                                                                                                                                                                                               |
|                       /id <OSEntryLineNum>                       |                                                                                                                                                                                              指定要向其在 Boot.ini 文件的 [操作系统] 部分中添加紧急管理服务选项的操作系统条目行号。 [操作系统] 部分标头后的第一行是 1。 此参数是必需的紧急管理服务值设置为时**ON**或**OFF**。                                                                                                                                                                                              |
|                                /?                                |                                                                                                                                                                                                                                                                                                                                                 在命令提示符下显示帮助。                                                                                                                                                                                                                                                                                                                                                  |

## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg /ems**命令：
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)

---
title: bootcfg dbg1394
description: Windows 命令主题**bootcfg dbg1394** -配置 1394年调试指定的操作系统条目的端口
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b36d22cea5b7b0c0e1768736d6c80c67b3c733f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857648"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

配置指定的操作系统项的 1394年端口调试。

## <a name="syntax"></a>语法
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|{ON &#124; OFF}|指定用于 1394年调试端口的值。<br /><br />-   **ON** -启用远程调试的支持情况 /dbg1394 选项添加到指定<OSEntryLineNum>。<br />-   **关闭**-通过从指定删除 /dbg1394 选项来禁用远程调试支持<OSEntryLineNum>。|
|/s <computer>|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。|
|/u <Domain>\\<User>|使用指定的用户帐户权限运行命令<User>或<Domain> \\ <User>。 默认值为当前登录的用户发出命令的计算机上的权限。|
|/p <Password>|指定在指定的用户帐户的密码 **/u**参数。|
|/ch 通道|指定要用于调试的通道。 有效值为 1 到 64 之间的整数。 不要使用 **/ch** <Channel>参数如果 1394年端口调试被禁用。|
|/id <OSEntryLineNum>|1394 端口调试选项会添加到其中的 Boot.ini 文件的 [操作系统] 部分中指定的操作系统条目行号。 [操作系统] 部分标头后的第一行是 1。|
|/?|在命令提示符下显示帮助。|
## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg /dbg1394**命令：
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)

---
title: bootcfg copy
description: Windows 命令主题**bootcfg 复制**-生成命令行选项的一个现有的启动项目，可向其中添加的副本。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ff8cf2751b229c6358e240444de940658f2eb2fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825508"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建一个现有的启动项目，可向其中添加命令行选项的副本。

## <a name="syntax"></a>语法
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/s <computer>|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。|
|/u <Domain>\\<User>|使用指定的用户帐户权限运行命令<User>或<Domain> \\ <User>。 默认值为当前登录的用户发出命令的计算机上的权限。|
|/p <Password>|指定在指定的用户帐户的密码 **/u**参数。|
|/d <Description>|指定新的操作系统条目的描述。|
|/id <OSEntryLineNum>|要复制的 Boot.ini 文件的 [操作系统] 部分中指定的操作系统条目行号。 [操作系统] 部分标头后的第一行是 1。|
|/?|在命令提示符下显示帮助。|
## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg /copy**命令将复制启动项 1，然后输入"\ABC Server\\"作为说明：
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)

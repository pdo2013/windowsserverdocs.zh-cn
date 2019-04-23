---
title: bootcfg timeout
description: Windows 命令主题**bootcfg 超时**-更改操作系统超时值。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47c68ffaad68ff3e29e8060fdb75adf46165c982
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885518"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改操作系统超时值。

## <a name="syntax"></a>语法
```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/timeout <timeOutValue>|[引导加载程序] 部分中指定的超时值。 <timeOutValue>用户必须从中进行选择操作系统引导加载程序屏幕 NTLDR 加载默认值之前的秒数。 有效范围<timeOutValue>是 0 到 999。 如果值为 0，则 NTLDR 将不显示启动加载程序屏幕直接启动默认操作系统。|
|/s <computer>|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。|
|/u <Domain\User>|运行该命令使用指定的用户帐户权限<User>或 < 域 \ 用户 >。 默认值为当前登录的用户发出命令的计算机上的权限。|
|/p <Password>|指定<Password>中指定的用户帐户 **/u**参数。|
|/?|在命令提示符下显示帮助。|
## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg /timeout**命令：
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)

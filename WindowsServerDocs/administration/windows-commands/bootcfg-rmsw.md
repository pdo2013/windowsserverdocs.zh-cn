---
title: bootcfg rmsw
description: Windows 命令主题**bootcfg rmsw** -删除操作指定的操作系统条目的系统负载选项。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65a8ba452911eb1b46a748d4e9d639ed102421b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878818"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

删除指定的操作系统条目的操作系统加载选项。

## <a name="syntax"></a>语法
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/s <computer>|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。|
|/u <Domain>\\<User>|使用指定的用户帐户权限运行命令<User>或<Domain> \\ <User>。 默认值为当前登录的用户发出命令的计算机上的权限。|
|/p <Password>|指定在指定的用户帐户的密码 **/u**参数。|
|/mm|从指定删除 /maxmem 选项和其关联的最大内存值<OSEntryLineNum>。 /Maxmem 选项指定的最大操作系统可以使用的 RAM 量。|
|/bv|从指定删除 /basevideo 选项<OSEntryLineNum>。 /Basevideo 选项将指示要用于已安装的视频驱动程序的标准的 VGA 模式下的操作系统。|
|/so|从指定删除 /sos 选项<OSEntryLineNum>。 /Sos 选项指示要在加载时，显示设备驱动程序名称的操作系统。|
|/ng|从指定删除 /noguiboot 选项<OSEntryLineNum>。 /Noguiboot 选项将禁用之前 CTRL + ALT + del 登录提示将显示进度栏。|
|/id <OSEntryLineNum>|从中删除 OS 加载选项的 Boot.ini 文件的 [操作系统] 部分中指定的操作系统条目行号。 [操作系统] 部分标头后的第一行是 1。|
|/?|在命令提示符下显示帮助。|
## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg /rmsw**命令：
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)

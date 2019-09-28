---
title: bootcfg rmsw
description: 适用于**bootcfg rmsw**的 Windows 命令主题-删除指定操作系统项的操作系统加载选项。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 43629f2e13bb6269a43d592fa0907637135aea71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379865"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

删除指定操作系统项的操作系统加载选项。

## <a name="syntax"></a>语法
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parameters

|      参数       |                                                                                                      描述                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。                                                   |
| /u <Domain> @ no__t-1 @ no__t-2  |          使用 <User> 或 <Domain> @ no__t-2 @ no__t）指定的用户的帐户权限运行命令。 默认为发出命令的计算机上当前登录用户的权限。          |
|    /p <Password>     |                                                                 指定在 **/u**参数中指定的用户帐户的密码。                                                                  |
|         /mm          |           从指定 @no__t 中删除/maxmem 选项及其关联的最大内存值。 /Maxmem 选项指定操作系统可使用的最大 RAM 量。            |
|         /bv          |                     从指定的 @no__t 中删除/basevideo 选项。 /Basevideo 选项指示操作系统使用已安装视频驱动程序的标准 VGA 模式。                     |
|         /so          |                         从指定的 @no__t 中删除/sos 选项。 /Sos 选项指示操作系统在加载时显示设备驱动程序名称。                          |
|         /ng          |                         从指定的 @no__t 中删除/noguiboot 选项。 /Noguiboot 选项禁用出现在 CTRL + ALT + del logon 提示符之前的进度栏。                          |
| /id <OSEntryLineNum> | 在从中删除 OS 加载选项的 Boot.ini 文件的 "[操作系统]" 部分中指定操作系统条目行号。 [操作系统] 部分标题后面的第一行是1。 |
|          /?          |                                                                                          在命令提示符下显示帮助。                                                                                          |

## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg/rmsw**命令：
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)

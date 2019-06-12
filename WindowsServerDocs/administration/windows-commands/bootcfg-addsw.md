---
title: bootcfg addsw
description: Windows 命令主题**bootcfg addsw** -添加了用于指定的操作系统条目的操作系统负载选项。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a056cec15bf804dafed4c4d39a80386e58c87fea
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434882"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

添加用于指定的操作系统条目的操作系统负载选项。

## <a name="syntax"></a>语法
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parameters

|         术语         |                                                                                                            定义                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。                                                        |
| /u <Domain>\\<User>  |               使用指定的用户帐户权限运行命令<User>或<Domain> \\ <User>。 默认值为当前登录的用户发出命令的计算机上的权限。               |
|    /p <Password>     |                                                                      指定在指定的用户帐户的密码 **/u**参数。                                                                       |
|   /mm <MaximumRAM>   |                                          指定兆字节为单位，可以使用操作系统中的最大 RAM 量。 值必须等于或大于 32 兆字节为单位。                                          |
|         /bv          |                                    将添加 **/basevideo**到指定的选项<OSEntryLineNum>，将定向操作系统为已安装的视频驱动程序对使用标准的 VGA 模式。                                     |
|         /so          |                                      将添加 **/sos**到指定的选项*OSEntryLineNum*，将定向操作系统来显示正在加载设备驱动程序名称。                                      |
|         /ng          |                                         将添加 **/noguiboot**到指定的选项<OSEntryLineNum>，禁用之前 CTRL + ALT + del 登录提示出现在进度栏。                                          |
| /id <OSEntryLineNum> | 操作系统加载选项会添加到其中的 Boot.ini 文件的 [操作系统] 部分中指定的操作系统条目行号。 [操作系统] 部分标头后的第一行是 1。 |
|          /?          |                                                                                               在命令提示符下显示帮助。                                                                                               |

## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg /addsw**命令：
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)

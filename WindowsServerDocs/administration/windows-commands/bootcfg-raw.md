---
title: bootcfg raw
description: 适用于**bootcfg raw**的 Windows 命令主题-将操作系统加载选项指定为 boot.ini 文件的 **[操作系统]** 部分中的操作系统项的字符串。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb5c052f85f54656c54a9e534f867d287407d2d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379899"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将指定为字符串的操作系统加载选项添加到 Boot.ini 文件的 **[操作系统]** 部分中的操作系统项。

## <a name="syntax"></a>语法
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
## <a name="parameters"></a>Parameters

|         术语          |                                                                                                            定义                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        指定远程计算机的名称或 IP 地址（不使用反斜杠）。 默认值为本地计算机。                                                         |
| /u <Domain> \\ @ no__t-2  |               使用 <User> 或 <Domain> @ no__t-2 @ no__t）指定的用户的帐户权限运行命令。 默认为发出命令的计算机上当前登录用户的权限。                |
|     /p <Password>     |                                                                       指定在 **/u**参数中指定的用户帐户的密码。                                                                       |
| <OSLoadOptionsString> | 指定要添加到操作系统项的操作系统加载选项。 这些加载选项将替换与操作系统条目关联的任何现有加载选项。 无 @no__t 验证完成-0。 |
| /id <OSEntryLineNum>  |                       指定要更新的 Boot.ini 文件的 [操作系统] 部分中的操作系统条目行号。 [操作系统] 部分标题后面的第一行是1。                       |
|          /a           |                                                       指定要添加的操作系统选项应追加到任何现有的操作系统选项。                                                        |
|          /?           |                                                                                               在命令提示符下显示帮助。                                                                                                |

##### <a name="remarks"></a>备注
- **bootcfg raw**用于向操作系统项的末尾添加文本，并覆盖任何现有的操作系统输入选项。 此文本应包含有效的 OS 加载选项，例如 **/debug**、 **/fastdetect**、 **/nodebug**、 **/baudrate**、 **/crashdebug**和 **/sos**。 例如，下面的命令将 " **/debug/fastdetect**" 添加到第一个操作系统项的末尾，同时替换任何以前的操作系统项选项：
  ```
  bootcfg /raw "/debug /fastdetect" /id 1
  ```
  ## <a name="BKMK_examples"></a>示例
  下面的示例演示如何使用**bootcfg/raw**命令：
  ```
  bootcfg /raw "/debug /sos" /id 2
  bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 "/crashdebug " /id 2
  ```
  #### <a name="additional-references"></a>其他参考
  [命令行语法项](command-line-syntax-key.md)

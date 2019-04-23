---
title: bootcfg raw
description: Windows 命令主题**bootcfg 原始**-添加操作系统加载选项指定为一个字符串中的操作系统条目 **[操作系统]** Boot.ini 文件部分。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a68c59eb3a7018ba8a7c0c96b594f0ed68d914b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841598"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

添加操作系统加载选项指定为一个字符串中的操作系统条目 **[操作系统]** Boot.ini 文件部分。

## <a name="syntax"></a>语法
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
## <a name="parameters"></a>Parameters
|术语|定义|
|----|-------|
|/s <computer>|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。|
|/u <Domain> \\<User>|使用指定的用户帐户权限运行命令<User>或<Domain> \\ <User>。 默认值为当前登录的用户发出命令的计算机上的权限。|
|/p <Password>|指定在指定的用户帐户的密码 **/u**参数。|
|<OSLoadOptionsString>|指定要添加到的操作系统条目的操作系统加载选项。 这些加载项将替换任何现有的负载选项与操作系统条目相关联。 任何验证<OSLoadOptions>完成。|
|/id <OSEntryLineNum>|要更新的 Boot.ini 文件的 [操作系统] 部分中指定的操作系统条目行号。 [操作系统] 部分标头后的第一行是 1。|
|/a|指定要添加的操作系统选项应将追加到任何现有的操作系统选项。|
|/?|在命令提示符下显示帮助。|
##### <a name="remarks"></a>备注
-   **bootcfg 原始**用于将文本添加到操作系统条目，覆盖任何现有的操作系统条目选项的末尾。 此文本应包含有效的 OS 加载选项，例如 **/debug**， **/fastdetect**， **/nodebug**， **/baudrate**， **/crashdebug**，并 **/sos**。 例如，以下命令将添加"**/debug /fastdetect**"为第一个操作系统条目，替换任何以前的操作系统条目选项：
    ```
    bootcfg /raw "/debug /fastdetect" /id 1
    ```
## <a name="BKMK_examples"></a>示例
下面的示例演示如何使用**bootcfg / 原始**命令：
```
bootcfg /raw "/debug /sos" /id 2
bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 "/crashdebug " /id 2
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)

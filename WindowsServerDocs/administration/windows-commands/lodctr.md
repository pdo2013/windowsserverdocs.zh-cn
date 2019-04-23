---
title: lodctr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ea48067bd78d260824da7d091f94957b51b768d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852218"
---
# <a name="lodctr"></a>lodctr

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

允许你进行注册，或在文件中保存性能计数器名称和注册表设置和指定受信任的服务。
## <a name="syntax"></a>语法
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|<filename>|注册性能计数器名称设置和初始化文件的文件名中提供的说明文字。|
|/s:<filename>|保存性能计数器注册表设置，然后到文件说明文字<filename>。|
|/r|从当前注册表设置和缓存的性能与注册表相关的文件还原计数器注册表设置和说明文字。<br /><br />此选项是仅在 Windows Server 2003 操作系统中可用。|
|/r:<filename>|还原性能计数器注册表设置，然后从文件说明文字<filename>。 **警告：** 如果您使用**lodctr /r**命令时，将覆盖所有性能计数器注册表设置，并说明文字，它们替换为指定的文件中定义的配置。|
|/t:<servicename>|指示该服务<servicename>是受信任。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
如果你提供的信息包含空格，使用文本周围的引号 (例如，"<filename>")。
## <a name="BKMK_Examples"></a>示例
若要保存当前性能注册表设置和应对文件的说明文字**perf backup1.txt**:
```
lodctr /s:"perf backup1.txt"
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)


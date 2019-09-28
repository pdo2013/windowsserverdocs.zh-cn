---
title: lodctr
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 96a2818110b766071eb83822abd34d8c0d00132d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374571"
---
# <a name="lodctr"></a>lodctr

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

允许你将性能计数器名称和注册表设置注册或保存到文件中，并指定受信任的服务。
## <a name="syntax"></a>语法
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
### <a name="parameters"></a>Parameters

|    参数     |                                                                                                                                         描述                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          注册性能计数器名称设置，并说明初始化文件文件名中提供的文本。                                                                                          |
|  /s： <filename>   |                                                                                                       将性能计数器注册表设置和说明文本保存到文件 <filename>。                                                                                                       |
|        /r        |                                还原计数器注册表设置，并说明与注册表相关的当前注册表设置和缓存性能文件中的文本。<br /><br />此选项仅在 Windows Server 2003 操作系统中可用。                                |
|  /r： <filename>   | 还原性能计数器注册表设置并说明文件 <filename> 中的文本。 **警告：** 如果你使用**lodctr/r**命令，你将覆盖所有性能计数器注册表设置并说明文本，并将其替换为指定文件中定义的配置。 |
| /t： <servicename> |                                                                                                                       指示服务 <servicename> 受信任。                                                                                                                       |
|        /?        |                                                                                                                             在命令提示符下显示帮助。                                                                                                                             |

## <a name="remarks"></a>备注
如果提供的信息包含空格，请使用引号将文本括起来（例如，"<filename>"）。
## <a name="BKMK_Examples"></a>示例
若要将当前性能注册表设置和计数器说明文本保存到文件**性能备份 1**，请执行以下操作：
```
lodctr /s:"perf backup1.txt"
```
## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)


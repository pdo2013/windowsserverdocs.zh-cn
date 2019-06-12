---
title: makecab
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b120cf990abe2024fd6c96ca2f1ef11fa2350ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437528"
---
# <a name="makecab"></a>makecab

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

现有文件打包为 cab (.cab) 文件。
## <a name="syntax"></a>语法
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
### <a name="parameters"></a>Parameters

|      参数       |                                                                        描述                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     若要压缩的文件。                                                                     |
|    <destination>     | 若要为压缩的文件的文件名。 如果省略，则源文件名的最后一个字符是替换为下划线 (_)，用作目标。 |
| /f <directives_file> |                                                   包含的文件**makecab**指令 （可能重复）。                                                   |
|    /d var=<value>    |                                                          定义变量，使用指定的值。                                                           |
|       /l <dir>       |                                               放置目标位置 （默认值为当前目录）。                                               |
|       /v[<n>]        |                                                    设置调试详细级别 (0 = 无，...，3 = full)。                                                     |
|          /?          |                                                           在命令提示符下显示帮助。                                                            |

## <a name="remarks"></a>备注
-   请参阅[Microsoft cab 文件格式](https://go.microsoft.com/fwlink/?LinkId=226852)directive_file 信息的 MSDN 上。

## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)


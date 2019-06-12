---
title: regini
description: 了解如何修改注册表，从命令提示符或使用脚本。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ff18dc3-5bd8-400a-b311-fd73a3267e8c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: e3f8454572b662c9327aeb4783c5e9651ad2022b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441895"
---
# <a name="regini"></a>regini

修改注册表从命令行或脚本，并应用在一个或多个文本文件中预设的更改。 您可以创建、 修改或删除注册表项，还可以修改注册表项的权限。

格式和 Regini.exe 用于对注册表进行的更改脚本文本文件的内容的详细信息，请参阅[如何从命令行或脚本更改注册表值或权限](https://support.microsoft.com/help/264584/how-to-change-registry-values-or-permissions-from-a-command-line-or-a)。

## <a name="syntax"></a>语法

```
regini [-m \\machinename | -h hivefile hiveroot][-i n] [-o outputWidth][-b] textFiles...
```

### <a name="parameters"></a>Parameters

|参数 |说明 |

|-m \< \\\\计算机名 >|使用要修改的注册表中指定的远程计算机名称。 使用格式 **\\ \\ComputerName**。|
|---------------------|-|
|-h \<hivefile hiveroot >|指定要修改本地注册表配置单元。 必须采用格式指定配置单元文件的名称和配置单元的根**hivefile hiveroot**。|
|-i \<n>|指定要用于指示命令输出中的注册表项的树形结构的缩进级别。 **Regdmp.exe**工具 （它获取的二进制格式中的注册表项的当前权限） 的倍数 4，使用缩进，因此默认值为**4**。|
|-o \<outputwidth>|以字符为单位指定命令输出中，的宽度。 如果输出将显示命令窗口中，默认值是窗口的宽度。 如果将输出定向到某个文件，默认值是**240**字符。|
|-b|指定的**Regini.exe**输出是与以前版本的向后兼容**Regini.exe**。 请参阅备注部分了解详细信息。|
|textfiles|指定一个或多个文本包含的文件的注册表数据的名称。 可以列出任意数量的 ANSI 或 Unicode 文本文件。|

## <a name="remarks"></a>备注

以下准则适用于包含通过使用应用的注册表数据的文本文件的内容主要**Regini.exe**。
-   使用分号作为行尾注释符。 它必须是一行中的第一个非空白字符。
-   使用反斜杠来指示的行继续符。 该命令将忽略反斜杠 （但不是包括） 中的所有字符的下一行的第一个非空白字符。 如果包含多个反斜杠之前空间，则替换它被一个空格。
-   使用硬制表符来控制缩进。 这种缩进指示注册表项中; 的树结构但是，这些字符都转换为单个空格而不考虑其位置。

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
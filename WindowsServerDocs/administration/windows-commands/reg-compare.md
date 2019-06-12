---
title: reg 比较
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83bb010af4bfbf38ce41001d6a6001d5a3996090
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441922"
---
# <a name="reg-compare"></a>reg 比较



比较指定的注册表子项或条目。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

## <a name="parameters"></a>Parameters

|    参数    |                                                                                                                                                                                                                                                                                          描述                                                                                                                                                                                                                                                                                           |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<KeyName1>   |                                                               指定要进行比较的第一个子项的完整路径。 若要指定远程计算机，包括计算机名称 (采用格式\\ \\ComputerName\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 *KeyName*必须包含有效的根键。 在本地计算机的有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键包括：HKLM 和 hku 开头。                                                                |
|   \<KeyName2>   | 指定要进行比较的第二个子项的完整路径。 若要指定远程计算机，包括计算机名称 (采用格式\\ \\ComputerName\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 指定仅在计算机名称*KeyName2*要使用的路径中指定的子项的操作将导致*KeyName1*。 *KeyName*必须包含有效的根键。 在本地计算机的有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键包括：HKLM 和 hku 开头。 |
| /v \<ValueName> |                                                                                                                                                                                                                                                                     指定要在子项下进行比较的值名称。                                                                                                                                                                                                                                                                      |
|       /ve       |                                                                                                                                                                                                                                                         指定应比较具有一个为 null 的值名称的唯一项。                                                                                                                                                                                                                                                         |
|      [{/oa      |                                                                                                                                                                                                                                                                                              /od                                                                                                                                                                                                                                                                                               |
|       /oa       |                                                                                                                                                                                                                                             指定显示的所有差异和匹配项。 默认情况下，列出了仅将差异。                                                                                                                                                                                                                                             |
|       /od       |                                                                                                                                                                                                                                                          指定显示仅差异。 这是默认行为。                                                                                                                                                                                                                                                          |
|       /os       |                                                                                                                                                                                                                                                    指定显示唯一匹配项。 默认情况下，列出了仅将差异。                                                                                                                                                                                                                                                     |
|       /on       |                                                                                                                                                                                                                                                       指定不显示任何内容。 默认情况下，列出了仅将差异。                                                                                                                                                                                                                                                        |
|       /s        |                                                                                                                                                                                                                                                                         将所有子项和注册表项以递归方式进行都比较。                                                                                                                                                                                                                                                                          |
|       /?        |                                                                                                                                                                                                                                                                    显示的帮助**reg 比较**在命令提示符处。                                                                                                                                                                                                                                                                    |

## <a name="remarks"></a>备注

下表列出的返回值**reg 比较**。

|值|Description|
|-----|-----------|
|0|比较是成功，并且结果完全相同。|
|1|比较失败。|
|2|比较已成功并且发现差异。|

下表列出了结果中显示的符号。

|符号|描述|
|------|-----------|
|=|*KeyName1*数据等于*KeyName2*数据。|
|<|*KeyName1*的数据小于*KeyName2*数据。|
|>|*KeyName1*数据大于*KeyName2*数据。|

## <a name="BKMK_examples"></a>示例

在密钥下的所有值进行比较**MyApp**项下的所有值**SaveMyApp**，类型：

REG COMPARE HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp

值进行比较的项下的版本**MyCo**和项下的版本值**MyCo1**，类型：

REG COMPARE HKLM\Software\MyCo HKLM\Software\MyCo1 /v Version

若要比较的所有子项和 HKLM\Software\MyCo 下创建名为 ZODIAC 所有子项和值下 HKLM\Software\MyCo 本地计算机上的计算机上的值，请键入：

REG COMPARE \\\\ZODIAC\HKLM\Software\MyCo \\\\. /s

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
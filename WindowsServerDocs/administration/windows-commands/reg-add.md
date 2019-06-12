---
title: Reg 添加
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d46fc2df23391a1dbb782014addc68d9522d603a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441912"
---
# <a name="reg-add"></a>Reg 添加


将新子项或条目添加到注册表。

## <a name="syntax"></a>语法

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="parameters"></a>Parameters

|      参数      |                                                                                                                                                                                                                                                                   描述                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<KeyName<em>></em> | 指定的子项或要添加的项的完整路径。 若要指定远程计算机，包括计算机名称 (采用格式\\ \\ \<ComputerName >\)作为的一部分*KeyName*。 省略\\ \\ComputerName\ 导致默认为本地计算机上的操作。 *KeyName*必须包含有效的根键。 在本地计算机的有效的根键包括：HKLM、 HKCU、 HKCR、 hku 开头和 HKCC。 如果指定远程计算机，则有效的根键包括：HKLM 和 hku 开头。 如果注册表项名称包含空格，则将用引号引起来的密钥名称。 |
|   /v \<ValueName>   |                                                                                                                                                                                                                                指定要添加到指定子项下的注册表项的名称。                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                指定添加到注册表的注册表项具有 null 值。                                                                                                                                                                                                                                |
|     /t \<Type>      |                                                                                                                                          指定的注册表项的类型。 *类型*必须是以下值之一：</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   /s \<Separator>   |                                                                                                                                                              指定要用于分隔数据的多个实例时指定 REG_MULTI_SZ 数据类型并且需要列出多个项的字符。 如果未指定，默认的分隔符是 **\0**。                                                                                                                                                              |
|     /d\<数据 >      |                                                                                                                                                                                                                                                 指定新的注册表项的数据。                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           添加注册表项，而不提示确认。                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              显示的帮助**reg 添加**在命令提示符处。                                                                                                                                                                                                                                               |

## <a name="remarks"></a>备注

-   子树不能添加一个带有此操作。 此版本的**reg**添加一个子项时不要求进行确认。
-   下表列出的返回值**reg 添加**操作。

| ReplTest1 | Description |
|-------|-------------|
|   0   |   成功   |
|   1   |   失败   |

-   对于 REG_EXPAND_SZ 密钥类型，请使用插入符号 ( **^** ) 与 **%** "内部 /d 参数

## <a name="BKMK_examples"></a>示例

若要在远程计算机 ABC 添加 HKLM\Software\MyCo 的密钥，请键入：
```
REG ADD \\ABC\HKLM\Software\MyCo
```
若要使用名为的值的注册表条目添加到 HKLM\Software\MyCo**数据**的 REG_BINARY 和数据类型**fe340ead**，类型：
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
若要使用的值名称的多值的注册表条目添加到 HKLM\Software\MyCo **MRU**的类型 REG_MULTI_SZ 和数据**fax\0mail\0\0**，类型：
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
值名称为扩展的注册表条目添加到 HKLM\Software\MyCo**路径**的类型 REG_EXPAND_SZ 和数据 **%systemroot%** ，类型：
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

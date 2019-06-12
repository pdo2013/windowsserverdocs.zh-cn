---
title: mmc
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4bdc093bd16ea08153b7dbc4a0e3380251f2ed7d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437332"
---
# <a name="mmc"></a>mmc

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

使用 mmc 命令行选项，可以打开特定**mmc**控制台中，打开**mmc**以作者模式，或指定的 32 位或 64 位版本**mmc**打开。
## <a name="syntax"></a>语法
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
### <a name="parameters"></a>Parameters

|       参数        |                                                                                                 描述                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>.msc |        启动**mmc**并打开已保存的控制台。 您需要保存的控制台文件指定完整的路径和文件名。 如果不指定控制台文件中， **mmc**将打开一个新的控制台。         |
|           /a           |                                                               以作者模式打开保存的控制台。  用于对保存的控制台进行更改。                                                                |
|          /64           |                         打开 64 位版本的**mmc** (mmc64)。 仅当您运行 Microsoft 64 位操作系统，并想要使用 64 位管理单元，请使用此选项。                          |
|          /32           | 打开 32 位版本的**mmc** (mmc32)。 时运行 Microsoft 64 位操作系统，您可以通过使用此命令行选项打开 mmc 运行 32 位管理单元，具有 32 位仅管理单元时。 |
|           /?           |                                                                                    在命令提示符下显示帮助。                                                                                     |

## <a name="remarks"></a>备注
- 使用<path> **\\** <filename> **.msc**可以使用环境变量来创建命令行或不依赖于显式的快捷方式的命令行选项控制台文件的位置。 例如，控制台文件的路径是否在系统文件夹 (例如， **mmc c:\winnt\system32\console_name.msc**)，可以使用可扩展数据字符串 **%systemroot%** 指定的位置(**mmc%systemroot%\system32\console_name.msc**)。 这可能是有用的组织中使用不同的计算机上的用户分配任务。
- 使用 **/a**命令行选项时使用此选项打开控制台时，它们以作者模式，而不考虑其默认模式打开。 这不会永久更改文件; 的默认模式设置如果省略此选项，mmc 将打开根据其默认模式设置的控制台文件。
- 打开后**mmc**或控制台文件以作者模式，可以通过单击打开任何现有的控制台**打开**上**控制台**菜单。
- 可以使用命令行创建快捷方式用于打开**mmc**和保存的控制台。 命令行命令配合**运行**命令**启动**菜单中，在任何命令提示符窗口、 快捷方式，或任何批处理文件或调用该命令的计划中。
  ## <a name="additional-references"></a>其他参考
- [命令行语法项](command-line-syntax-key.md)


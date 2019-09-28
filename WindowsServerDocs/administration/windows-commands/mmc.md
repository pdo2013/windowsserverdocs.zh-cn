---
title: mmc
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1bf9efe257e9e2b6cf20c28c1e6c0cf27230a6bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373597"
---
# <a name="mmc"></a>mmc

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

使用 mmc 命令行选项，您可以打开特定的**mmc**控制台，以作者模式打开**mmc** ，或指定打开32位或64位版本的**mmc** 。
## <a name="syntax"></a>语法
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
### <a name="parameters"></a>Parameters

|       参数        |                                                                                                 描述                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path> @ no__t-1<filename>.msc |        启动**mmc**并打开保存的控制台。 需要为保存的控制台文件指定完整的路径和文件名。 如果未指定控制台文件， **mmc**会打开一个新的控制台。         |
|           /a           |                                                               在作者模式下打开保存的控制台。  用于对保存的控制台进行更改。                                                                |
|          /64           |                         打开64位版本的**mmc** （mmc64）。 仅当你运行 Microsoft 64 位操作系统并想要使用64位管理单元时，才使用此选项。                          |
|          /32           | 打开32位版本的**mmc** （mmc32）。 运行 Microsoft 64 位操作系统时，如果仅有32位的管理单元，则可以通过使用此命令行选项打开 mmc 来运行32位管理单元。 |
|           /?           |                                                                                    在命令提示符下显示帮助。                                                                                     |

## <a name="remarks"></a>备注
- 使用 @no__t **\\** ，可以使用环境变量来创建不依赖于控制台文件的显式位置的命令行或快捷方式。 例如，如果控制台文件的路径位于系统文件夹（例如， **mmc c:\winnt\system32\console_name.msc**）中，则可以使用可扩展的数据字符串 **% systemroot%** 来指定位置（**mmc% systemroot% \ system32 \ console_名称 services.msc**）。 如果要将任务委派给组织中正在使用不同计算机的人员，这可能很有用。
- 使用此选项打开控制台时，使用 **/a**命令行选项，无论其默认模式如何，都将在作者模式下打开它们。 这不会永久更改文件的默认模式设置;如果省略此选项，mmc 将根据其默认模式设置打开控制台文件。
- 在作者模式下打开**mmc**或控制台文件后，可通过单击**控制台**菜单上的 "**打开**" 来打开任何现有控制台。
- 你可以使用命令行来创建用于打开**mmc**和保存的控制台的快捷方式。 命令行命令适用于 "**开始**" 菜单上的 "**运行**" 命令、"任何命令提示符" 窗口、"快捷方式" 或任何批处理文件或调用命令的程序。
  ## <a name="additional-references"></a>其他参考
- [命令行语法项](command-line-syntax-key.md)


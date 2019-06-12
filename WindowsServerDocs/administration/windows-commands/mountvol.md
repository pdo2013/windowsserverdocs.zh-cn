---
title: mountvol
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03e7cefc7c7a00338972fc365b7c25d9c795c83e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437279"
---
# <a name="mountvol"></a>mountvol



创建、 删除或列出卷装入点。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
mountvol [<Drive>:]<Path VolumeName>
mountvol [<Drive>:]<Path> /d
mountvol [<Drive>:]<Path> /l
mountvol [<Drive>:]<Path> /p
mountvol /r
mountvol [/n | /e]
mountvol <Drive>: /s
```

## <a name="parameters"></a>Parameters

|     参数     |                                                                                                                           描述                                                                                                                            |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                                                                                             指定装入点所在的位置的现有 NTFS 目录。                                                                                             |
|   \<VolumeName>   |                     指定目标的装入点的卷名。 卷名称使用以下语法，其中*GUID*是全局唯一标识符：</br>`\\\\?\Volume\{GUID}\`</br>括号 {} 是必需的。                      |
|        /d         |                                                                                                    从指定的文件夹中删除卷装入点。                                                                                                     |
|        /l         |                                                                                                     列出指定文件夹的已装入的卷名称。                                                                                                      |
|        /p         | 从指定的目录中删除卷装入点、 卸除基本卷并使基本卷脱机，使其不可安装。 如果其他进程正在使用该卷**mountvol**卸除该卷之前关闭任何打开的句柄。 |
|        /r         |             删除卷装入点目录和注册表设置将不再在系统中，使其自动装入和授予以前的卷装入点添加到系统时的卷。              |
|        /n         |                                                                      禁用新基本卷自动安装。 添加到系统时，新卷不会自动装载。                                                                       |
|        /e         |                                                                                                       重新启用新基本卷自动安装。                                                                                                        |
|        /s         |                                                                                在指定的驱动器上装入 EFI 系统分区。 在基于 Itanium 的计算机上可用。                                                                                |
|        /?         |                                                                                                               在命令提示符下显示帮助。                                                                                                               |

## <a name="remarks"></a>备注

-   **Mountvol**可以链接而无需驱动器号的卷。
-   通过使用卸除卷 **/p** "创建时不安装 UNTIL 一个卷装入点。"卷列表中列出 如果卷上有多个装入点，使用 **/d**删除其他装入点，然后使用才能 **/p**。 你可以基本卷处于可装入再次通过使分配的卷装入点。
-   如果需要扩展卷空间，而无需重新格式化或更换硬盘，可以将装载路径添加到另一个卷。 一个卷使用多个装入路径的好处是，您可以使用单个驱动器号访问所有本地卷 (如`C:`)。 不需要记住哪个卷对应的驱动器号，尽管仍可装入本地卷并将其分配驱动器号。

## <a name="BKMK_examples"></a>示例

若要创建装入点，请键入：
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
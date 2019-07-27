---
title: mountvol
description: '适用于 * * * * 的 Windows 命令主题 '
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
ms.openlocfilehash: 07c57f7ab9c41d6155e4a8d38322176aabf3868f
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544600"
---
# <a name="mountvol"></a>mountvol



创建、删除或列出卷装入点。

有关如何使用此命令的示例, 请参阅[示例](#BKMK_examples)。

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

|参数|描述|
|---------|-----------|
|[\<驱动器 >:]<Path>|指定装入点将驻留的现有 NTFS 目录。|
|\<VolumeName >|指定作为装入点目标的卷名称。 卷名使用以下语法, 其中*GUID*是全局唯一标识符:</br>`\\\\?\Volume\{GUID}\`</br>需要括号 {}。|
|/d|从指定的文件夹中删除卷装入点。|
|/l|列出指定文件夹的已装入卷名。|
|/p|从指定的目录中删除卷装入点, 卸载基本卷, 并使基本卷脱机, 使其不可装入。 如果其他进程正在使用该卷, 则**mountvol**会在卸载卷之前关闭任何打开的句柄。|
|/r|删除不再位于系统中的卷的卷装入点目录和注册表设置, 阻止它们被自动装载, 并在将其添加回系统后, 将其分配给以前的卷装入点。|
|/n|禁用新基本卷的自动装载。 添加到系统时, 不会自动装载新卷。|
|/e|重新启用新基本卷的自动装载。|
|/s|将 EFI 系统分区装载到指定驱动器上。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Mountvol**允许你链接卷, 而无需驱动器号。
-   使用 **/p**卸载的卷在卷列表中列出为 "未在创建卷装入点之前装入"。 如果卷有多个装入点, 请使用 **/d**在使用 **/p**之前删除其他装入点。 可以通过分配卷装入点, 使基本卷再次可装入。
-   如果你需要扩展卷空间而不重新格式化或更换硬盘驱动器, 则可以将装入路径添加到另一个卷。 使用一个具有多个装载路径的卷的好处是, 你可以使用单个驱动器号 (例如`C:`) 访问所有本地卷。 您无需记住哪个卷与哪个驱动器号相对应, 不过您仍可以装入本地卷并为它们分配驱动器号。

## <a name="BKMK_examples"></a>示例

若要创建装入点, 请键入:
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

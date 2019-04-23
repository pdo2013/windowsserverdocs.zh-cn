---
title: chkntfs
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 876c0e0c254216ac217aea7d165d5f4e3a7da9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861988"
---
# <a name="chkntfs"></a>chkntfs



显示或修改自动磁盘检查启动计算机时。 如果使用不带任何选项， **chkntfs**显示指定卷的文件系统。 如果自动文件检查的计划运行，请**chkntfs**显示指定的卷是否已更新或计划检查下一次计算机启动。

> [!NOTE]
> 若要运行**chkntfs**，您必须是 Administrators 组的成员。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
chkntfs <Volume> [...]
chkntfs [/d]
chkntfs [/t[:<Time>]]
chkntfs [/x <Volume> [...]]
chkntfs [/c <Volume> [...]]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<卷 > [...]|指定要检查当计算机启动时的一个或多个卷。 有效的卷包含 （后跟一个冒号） 的驱动器号装入点或卷名称。|
|/d|可将所有**chkntfs**默认设置，自动文件检查的倒计时时间除外。 默认情况下，所有卷时，会都检查启动计算机，并**chkdsk**运行那些已更新。|
|/t [:\<Time>]|更改为以秒为单位指定时间量 Autochk.exe 启动倒计时时间。 如果不输入时， **/t**显示当前的倒计时时间。|
|/x\<卷 > [...]|指定要从正在检查启动计算机时，即使该卷标记为要求中排除的一个或多个卷**chkdsk**。|
|/c\<卷 > [...]|计划要检查时计算机已启动并运行的一个或多个卷**chkdsk**上那些已更新。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要显示的驱动器 C 的文件系统类型，请键入：
```
chkntfs c:
```
以下输出表明 NTFS 文件系统：
```
The type of the file system is NTFS.
```

> [!NOTE]
> 如果自动文件检查计划运行，将显示其他输出，该值指示该驱动器已更新或已被手动计划要检查下一次计算机启动。

若要显示 Autochk.exe 启动倒计时时间，请键入：
```
chkntfs /t
```
例如，如果倒计时时间设置为 10 秒，将显示以下输出：
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
若要更改为 30 秒 Autochk.exe 启动倒计时时间，请键入：
```
chkntfs /t:30
```

> [!NOTE]
> 尽管可以将 Autochk.exe 启动倒计时时间设置为零，但这样做将阻止您取消可能很耗时的自动文件检查。

**/X**命令行选项不是积累。 如果不止一次键入，最新的条目将覆盖以前的条目。 若要从正在检查中排除多个卷，你必须列出每个单个命令中。 例如，若要排除 D 和 E 的卷，请键入：
```
chkntfs /x d: e:
```
**/C**是积累的命令行选项。 如果键入 **/c**不止一次，每个条目仍将保留。 若要确保仅在特定卷确认已选中，重置默认值，若要清除所有以前的命令，请从正在检查中排除的所有卷，然后安排自动检查所需的卷上的文件。

例如，若要计划自动检查 D 卷，但不是 C 或 E 卷上的文件，键入以下命令顺序：
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
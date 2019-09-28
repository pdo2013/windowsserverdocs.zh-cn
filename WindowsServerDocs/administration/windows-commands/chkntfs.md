---
title: chkntfs
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f940fe81f0e7e01495e071931059b2375b78bb22
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379336"
---
# <a name="chkntfs"></a>chkntfs



在计算机启动时显示或修改自动磁盘检查。 如果使用时没有选项， **chkntfs**将显示指定卷的文件系统。 如果计划运行自动文件检查，则**chkntfs**会显示指定的卷是否已更新，或是否计划在下次启动计算机时进行检查。

> [!NOTE]
> 若要运行**chkntfs**，你必须是 Administrators 组的成员。

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
|\<Volume > [...]|指定在计算机启动时要检查的一个或多个卷。 有效的卷包括驱动器号（后跟冒号）、装入点或卷名。|
|/d|还原所有**chkntfs**默认设置，但自动文件检查的倒计时时间除外。 默认情况下，当计算机启动时，所有卷都处于选中状态，并且**chkdsk**在那些更新的计算机上运行。|
|/t [： \<Time >]|将 Autochk 初始倒计时时间更改为指定的时间量（以秒为单位）。 如果未输入时间，则 **/t**将显示当前倒计时时间。|
|/x \<Volume > [...]|指定在计算机启动时要排除的一个或多个卷，即使卷被标记为需要**chkdsk**。|
|/c \<Volume > [...]|在计算机启动时计划要检查的一个或多个卷，并在那些已更新的卷上运行**chkdsk** 。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要显示驱动器 C 的文件系统类型，请键入：
```
chkntfs c:
```
以下输出指示 NTFS 文件系统：
```
The type of the file system is NTFS.
```

> [!NOTE]
> 如果计划运行自动文件检查，则将显示其他输出，以指示驱动器是否处于脏状态，或是否已手动计划在下次启动计算机时进行检查。

若要显示 Autochk 初始倒计时时间，请键入：
```
chkntfs /t
```
例如，如果倒计时时间设置为10秒，将显示以下输出：
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
若要将 Autochk 初始倒计时时间更改为30秒，请键入：
```
chkntfs /t:30
```

> [!NOTE]
> 虽然您可以将 Autochk 初始倒计时时间设置为零，但这样做将阻止您取消可能需要花费大量时间的自动文件检查。

**/X**命令行选项不可累积总计。 如果多次键入，最新的条目将覆盖以前的条目。 若要排除多个卷的选中状态，必须在单个命令中列出每个卷。 例如，若要排除 D 和 E 卷，请键入：
```
chkntfs /x d: e:
```
**/C**命令行选项为累积总计。 如果多次键入 **/c** ，则每个条目都将保留。 若要确保只检查特定的卷，请重置默认值以清除所有以前的命令，排除所有卷的检查，然后在所需的卷上计划自动文件检查。

例如，若要在 D 卷上而不是 C 或 E 卷上计划自动文件检查，请按顺序键入以下命令：
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
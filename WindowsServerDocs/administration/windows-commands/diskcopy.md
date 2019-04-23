---
title: diskcopy
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/07/2018
ms.openlocfilehash: 5b9343dc2f6b4c74da5a9d89a2ea804b702248cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841168"
---
# <a name="diskcopy"></a>diskcopy



源驱动器中的软盘的内容复制到目标驱动器中格式化或非格式化的软盘。 如果使用不带参数， **diskcopy**为源磁盘和目标磁盘将使用当前驱动器。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

> [!NOTE]
> 此命令不包括在 Windows 10。

## <a name="syntax"></a>语法

```
diskcopy [<Drive1>: [<Drive2>:]] [/v]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive1>|指定包含源磁盘的驱动器。|
|\<Drive2>|指定包含目标磁盘的驱动器。|
|/v|验证已正确复制信息。 此选项将降低复制过程。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   使用磁盘

    **Diskcopy**仅适用于可移动磁盘如软盘，必须为相同的类型。 不能使用**diskcopy**的硬盘。 如果指定的硬盘驱动器*Drive1*或*驱动器 2*， **diskcopy**显示以下错误消息：  
    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```  
    **Diskcopy**命令将提示您插入源和目标磁盘，等待您键盘上按任意键，然后再继续。

    复制磁盘后, **diskcopy**会显示以下消息：  
    ```
    Copy another diskette (Y/N)?
    ```  
    如果您按 Y **diskcopy**将提示您插入的下一步的复制操作的源和目标磁盘。 若要停止**diskcopy**处理，请按**N**。

    如果要将复制到中未格式化的软盘*驱动器 2*， **diskcopy**格式具有相同数量的边和每个音轨中的磁盘上的扇区磁盘*Drive1*。 **Diskcopy**对磁盘进行格式化并将文件复制对其显示以下消息：  
    ```
    Formatting while copying
    ```  
-   磁盘序列号

    如果源磁盘的卷序列号**diskcopy**创建新的目标磁盘卷序列号和复制操作完成时显示的数目。
-   省略驱动器参数

    如果省略*驱动器 2*参数， **diskcopy**使用当前驱动器作为目标驱动器。 如果省略这两个驱动器参数， **diskcopy**两个使用当前驱动器。 如果当前驱动器等同于*Drive1*， **diskcopy**会提示你根据需要交换磁盘。
-   使用一个驱动器进行复制

    运行**diskcopy**从软盘驱动器以外的驱动器，例如 C 驱动器。 如果软盘*Drive1*和软盘*驱动器 2*相同， **diskcopy**会提示您若要切换的磁盘。 如果磁盘包含超过可用内存可容纳的量的详细信息**diskcopy**无法读取的所有信息在一次。 **Diskcopy**从源磁盘读取、 写入目标磁盘，并提示您重新插入源磁盘。 此过程持续复制整个磁盘。
-   避免磁盘碎片整理

    碎片是较小的区域的磁盘上的现有文件之间的未使用的磁盘空间存在。 零碎的源磁盘可以减缓查找、 读取或写入文件的过程。

    因为**diskcopy**使目标磁盘上的源磁盘、 源磁盘上的任何碎片的精确副本传输到目标磁盘。 若要避免碎片之间传输的一个磁盘，请使用**副本**或**xcopy**以复制你的磁盘。 因为**副本**并**xcopy**复制文件按顺序，新磁盘碎片不多。

> [!NOTE]
> 不能使用**xcopy**要复制的启动磁盘。
-   了解**diskcopy**退出代码

    下表说明了每个退出代码。  
    |退出代码|描述|
    |---------|-----------|
    |0|复制操作已成功|
    |1|出现非致命的读/写错误|
    |3|发生致命硬件错误|
    |4|出现初始化错误|

    若要处理返回的退出代码**diskcomp**，可以使用*ERRORLEVEL*上的环境变量**如果**命令行中的批处理程序。

## <a name="BKMK_examples"></a>示例

若要将该磁盘在驱动器 B 复制到磁盘驱动器 A 中，键入：
```
diskcopy b: a:
```
若要使用软盘驱动器 A 复制到另一张软盘，请先切换到 C 驱动器，然后键入：

diskcopy 答： 答：

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
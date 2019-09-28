---
title: 格式
ms.prod: windows-server
manager: dongill
ms.author: JGerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: JasonGerend
ms.date: 10/16/2017
ms.openlocfilehash: 7db57ac8115d99327fea72c12695a2ca6d3bc05f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377038"
---
# <a name="format"></a>格式
> 适用于：Windows 10、Windows Server 2016

格式化磁盘以接受 Windows 文件。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
format <Volume> [/fs:{FAT|FAT32|NTFS}] [/v:<Label>] [/q] [/a:<UnitSize>] [/c] [/x] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/f:<Size>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/t:<Tracks> /n:<Sectors>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/p:<Passes>]
format <Volume> [/q]
```

## <a name="parameters"></a>Parameters

|   参数    |                                                                                                                                                                                                                    描述                                                                                                                                                                                                                     |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<Volume >    |                                                                                         指定要格式化的驱动器的装入点、卷名或驱动器号（后跟冒号）。 如果未指定任何以下命令行选项，则**format**将使用卷类型来确定磁盘的默认格式。                                                                                         |
|    /fs： {FAT    |                                                                                                                                                                                                                       FAT32                                                                                                                                                                                                                        |
|  /v： \<Label >   |                           指定卷标。 如果省略了 **/v**命令行选项或在未指定卷标的情况下使用它，则格式化完成后，**格式**会提示你输入卷标。 使用语法 **/v:** 以防提示输入卷标。 如果使用单个 **format** 命令格式化多个磁盘，则会为所有磁盘指定相同的卷标。                            |
| /a： \<UnitSize > | 指定要在 FAT、FAT32 或 NTFS 卷上使用的分配单元大小。 如果未指定 *UnitSize*，则会基于卷大小进行选择。 对于常规使用，强烈建议采用默认设置。 以下列表显示了 NTFS、FAT 和 FAT32 *UnitSize* 的有效值：</br>512</br>1024</br>2048</br>4096</br>8192</br>16K</br>32K</br>64K</br>对于大小超过 512 字节的扇区，FAT 和 FAT32 还支持 128k 和 256k。 |
|       /q       |                                                       执行快速格式化。 删除以前格式化的卷的文件表和根目录，但不会对错误区域执行逐个扇区扫描。 你应使用 **/q**命令行选项来仅设置你知道的状态良好的以前格式化的卷的格式。 请注意， **/q** 可替代 **/p**。                                                       |
|   /f： \<Size >   |                                                         指定要格式化的软盘的大小。 如果可能，请使用此命令行选项，而不是 **/t**和 **/n**命令行选项。 Windows 可接受的大小值如下：</br>-   1440 或 1440k 或 1440kb</br>-   1.44 或 1.44m 或 1.44mb</br>-1.44-MB，双面，四密度，3.5-英寸磁盘                                                         |
|  /t： \<Tracks >  |                                                    指定磁盘上的磁道数。 如果可能，请改用 **/f**命令行选项。 如果使用 **/t** 选项，则还必须使用 **/n** 选项。 这些选项共同提供了指定正在格式化的磁盘大小的另一种方法。 此选项对 **/f** 选项无效。                                                     |
| /n： \<Sectors >  |                                                         指定每个磁道的扇区数。如果可能，请使用 **/f**命令行选项，而不是 **/n**。 如果使用 **/n**，则还必须使用 **/t**。 这两个选项共同提供了指定正在格式化的磁盘大小的另一种方法。 此选项对 **/f** 选项无效。                                                         |
|  /p： \<Passes >  |                                                                                                                                                               归零卷上的每个扇区的指定操作数量。 此选项对 **/q** 选项无效。                                                                                                                                                                |
|       /c       |                                                                                                                                                                                     仅 NTFS。 默认情况下，将压缩在新卷上创建的文件。                                                                                                                                                                                      |
|       /x       |                                                                                                                                                            如有必要，在格式化卷之前将其卸除。 针对卷的任何打开句柄将不再有效。                                                                                                                                                            |
|       /?       |                                                                                                                                                                                                        在命令提示符下显示帮助。                                                                                                                                                                                                        |

## <a name="remarks"></a>备注

-   管理凭据

    您必须是 Administrators 组的成员才能格式化硬盘驱动器。
-   使用**格式**

    **Format**命令为磁盘创建新的根目录和文件系统。 它还可以检查磁盘上的坏区，并可以删除磁盘上的所有数据。 若要使用新磁盘，必须先使用此命令格式化磁盘。
-   添加卷标

    格式化软盘后，**格式**将显示以下消息：

    `Volume label (11 characters, ENTER for none)?`

    若要添加卷标签，请键入最多11个字符（包括空格）。 如果你不想将卷标添加到磁盘，请按 ENTER。
-   格式化硬盘

> [!NOTE]
> 您必须是 Administrators 组的成员才能格式化硬盘。

当使用**format**命令格式化硬盘时，会显示一条类似于下面的警告消息：
```
WARNING, ALL DATA ON NON-REMOVABLE DISK 
DRIVE x: WILL BE LOST! 
Proceed with Format (Y/N)? _ 
```
若要格式化硬盘，请按 Y;如果不想格式化磁盘，请按 N。
-   单元大小

    FAT 文件系统将群集数量限制为不超过65526。 FAT32 文件系统将分类数限制为介于65527和4177917之间。

    大小超过 4096 的分配单元不支持 NTFS 压缩。

> [!NOTE]
> 如果使用指定的群集大小确定以前的要求无法满足，则**Format**会立即停止处理。
> -   **设置消息格式**

    When formatting is complete, **format** displays messages that show the total disk space, the spaces marked as defective, and the space available for your files.
- 快速格式化

  您可以使用 **/q**命令行选项加速格式设置过程。 仅在硬盘上没有损坏扇区的情况下使用此选项。
- 将 **format** 与重新分配的驱动器或网络驱动器一起使用

  不应针对使用 **subst** 命令准备就绪的驱动器使用 **format** 命令。 不能通过网络对磁盘进行格式化。
- **format** 退出代码

  下表列出了每个退出代码以及其含义的简要说明。  

  |退出代码|描述|
  |---------|-----------|
  |0|格式化操作已成功。|
  |1|提供了不正确的参数。|
  |4|出现错误（0、1或5以外的任何错误）。|
  |5|用户按了 N 以响应 "继续进行格式（Y/N）" 的提示？ 以停止此进程。|

  可以结合使用 ERRORLEVEL 环境变量和 **if** 批处理命令来检查这些退出代码。
- 在恢复控制台中使用 **format**

  可从恢复控制台获取具有不同参数的 **format** 命令。

## <a name="BKMK_examples"></a>示例

若要使用默认大小格式化驱动器 A 中的新软盘，请键入：
```
format a:
```
若要在驱动器 A 的以前格式化的软盘上执行快速格式化操作，请键入：
```
format a: /q
```
若要格式化驱动器 A 中的软盘，并为其分配卷标 "DATA"，请键入：
```
format a: /v:DATA
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](https://technet.microsoft.com/library/cc771080.aspx)